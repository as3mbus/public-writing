---
type: Article
tags:
  - Phaser
  - javascript
  - GameDevelopment
  - GameEngine
  - Technology
State: Done
---

# Background

Unlike Unity, Phaser require hard-coded user interface creation. in addition phaser also don't have canvas scaler that scale the UI element based on screen size. Thus we will require a way to provide this functionality effectively.

# Problem

Below are several problem that will be addressed in this discussion.

- how to place, Scale and layout UI Object based on Canvas Screen Size
- how to handle scaling event for responsive UI
- produce clean code that understandable and easily maintained

# Proposed Solution

## Scaling

up until now my use of sprite scaling is mostly preserve aspect ratio.

### Preserve Aspect Ratio

Scaling is easily addressable using template provided module which is `Size Extension` / `Transform Wrapped Game Object`.

### 9 Slice

previously i've tried [9 slice](https://github.com/jdotrjs/phaser3-nineslice/) provided by dotrjs but having problem with IOS browser having weird case that won't render in some unknown cases

> [!important]  
> TODO : Report 9 Slice IOS Problem from populix Project (from Chat with Aman)  

### 9 Patch (Rex UI Plugin)

New Feature from Rex UI Plugin that could be used to implement nine-slice, and nine patch. it is more maintained compared to previous 9 slice. but it haven't been tried in any quest yet.

## Scale and Placement Value

Scale and Placement value can easily be referenced through phaser `scale manager` that easily accessible from any scene, or use `Screen Utility Module` that wrap this functionality into a singleton that can be accessed everywhere as long as it's initiated at boot time. Using this value is simply by multiplying it with decimal number that represent how large scale / spacing compared to game canvas.

**Example :**

- center of the screen (position) / quarter of screen(size)
    
    ```JavaScript
    export default class MainMenuSceneController extends Phaser.Scene 
    {
    	create() {
    		// Scale Manager
    		let xCenter = this.scale.width * .5;
    		let yCenter = this.scale.height * .5
    		// Screen Utility
    		const screenUtil = ScreenUtilityController.getInstance();
    		let xCenter2 = screenUtil.centerX;
    		let yCenter2 = screenUtil.centerY;
    	}
    }
    ```
    

## Handle / Listen to Scaling Event

In order to make Responsive UI at runtime, UI element need to adjust to game scale. In Order to do this we listen to event that invoked every time phase game scale event is invoked.

To begin, tracing this event go to `src/index.js`. Near the end of file there are configured window event listener that are set when `CONFIG.AUTO_CANVAS_RESIZE` are set to `true`.

```JavaScript
if (CONFIG.AUTO_CANVAS_RESIZE)
{
    window.addEventListener('resize', () =>
    {
        const screenProfile = portraitConversion(calculateScreen());
        game.scale.resize(screenProfile.width, screenProfile.height);
        game.scale.setZoom(screenProfile.zoom);
    });
}
```

from script above we can see that `game.scale` are resized everytime window invoke `resize` event. from here we know that we can listen to `game.scale` (which is Phaser `ScaleManager`) to invoke our object scaling event.

**Example :**

```JavaScript
// Object within a Scene
export default class MainMenuSceneController extends Phaser.Scene 
{
	image;

	create() {
		this.image = this.add.image();
		this.scale.on('resize', this.onResize, this);
    this.events.once('shutdown', () => {
        this.scale.off('resize', this.onResize);
    });
	}

	onResize()
	{
		this.image(this.scale.width, this.scale.height)
	}
}
```

with script above created image will scale to fill game canvas even after resizing the window at runtime. `shutdown` event are set to remove listener when scene closes, avoiding event invocation on destroyed object.

## Clean UI Layouting Script

> [!important]  
> This section is based on writer code structuring preference. Thus this is a sector that would be great to be expanded for different people style of layouting UI Objects  

### Container Grouping

Grouping game object within a container will make object placement and display state relative to container that contain it. thus making placement and hidden toggling of complex object much easier since we don't need to modify each object, but only modify the container.

### Extending Container / Sizer

Instead of creating complex object at scene creation script. group a collection of game object into class extending container/ sizer. this way it'll make constructing, re-using, and managing object easier.

**Example**

```JavaScript
export default class RewardListItemView extends Sizer {

    textScale = 0.1;
    iconSprite;
    nameText;
    iconTextureKey;

    /**
     * @param {Phaser.Scene} scene
     * @param {number} x
     * @param {number} y
     * @param {number} width
     * @param {number} height
     * @param {RewardData} rewardData
     */
    constructor(scene, x, y, width, height, rewardData=null) {
        super(scene, x, y, width, height, 'vertical');
        this.scene.add.existing(this);

        this.iconTextureKey = UIAsset.rectangle.key;
        const icon = scene.add.sprite(0, 0, this.iconTextureKey);
        this.iconSprite = icon;
        Object.assign(icon, SizeExtension);
        this.add(icon);
        
				const nameText = scene.add.text(0, 0, 'test');
        this.add(nameText);
        this.nameText = nameText;
        nameText.setColor(TEXT_COLOR);
        nameText.setAlign('center');
        nameText.setPosition(0, height / 2);
        nameText.setFontFamily(FONT_BOLD);
        nameText.setWordWrapWidth(width, true);

        if (rewardData)
            this.setUpData(rewardData);
        else
            this.resizeComponents();
        this.layout().setPosition(x, y);
    }

    setMinSize(width, height) {
        super.setMinSize(width, height);
        this.resizeComponents();
        return this;
    }

    resizeComponents() {
        this.iconSprite?.setMaxPreferredDisplaySize(this.minWidth, this.minHeight * (1 - this.textScale));
        this.nameText?.setFontSize(this.minHeight * this.textScale);
        this.nameText?.setWordWrapWidth(this.minWidth, true);
    }

    setUpData(rewardData) {
        this.iconTextureKey = rewardData.image;
        this.iconSprite.setTexture(this.iconTextureKey);
        this.nameText.setText(rewardData.name);
        this.resizeComponents();
    }

}
```

```JavaScript
// USAGE
const item = new RewardListItemView(this, 0, 0, 500, 500);
item.setUpData(data.content[i]);
item.setMinSize(200,300);
```

### Sizer (Rex UI) and Container Tips

- Sizer Layouting at Runtime
    
    - call `layout` before `setPosition` so any child modification stored and layouted properly
    
    > It is a defect that changing position related properties (x, y, scaleX, scaleY) of child won't be stored in parent container directly  
    >   
    > ~ rex  
    
- Fix Width Sizer Containing Container
    - no problem whatsoever as long as container have a size
- Container Containing Sizer
    
    - avoid using this if possible
    - be careful with this as sizer have different placement ruleset
    - attach overriden setPosition and setActive with sizer method
    
    ```JavaScript
    export default class PowerUpInventoryView extends Phaser.GameObject.Container
    {
    		constructor(scene)
    		{
    				this.rootLayout = new Sizer(scene,0,0,300,400)
    		}
    		setVisible(value)
        {
            this.rootLayout.setVisible(value);
            return super.setVisible(value);
        }
    		setPosition(x, y, z, w)
        {
            this.rootLayout.setVisible(x,y,z,w);
            return super.setPosition(x, y, z, w);
        }
    }
    ```
    

### Implement Layouting Events

crafting responsive ui at runtime require multiple size recalculation. if done poorly it'll result duplicated line that need to keep track of every time there are any layout modifications. To avoid these problem i usually define layouting events which would get invoked every time there are data modification or any size/ placement modification. this would also ease responsive UI implementations.

```JavaScript
export default class RewardListPopUpSceneController extends Phaser.Scene {
    onClose = () => {
    };
    panelBg;
    contentSizer;
    image;
    infoText;
    okButton;

    eventNames = {
        onInteraction: 'onInteraction',
        layout: 'layout'
    };
    sizerConfig;
    infoFontSize;

    itemSpacing = 0;


    constructor() {
        super({key: SceneInfo.reward_list_popup_scene.key});
    }

    create(data) {
        this.sizerConfig = {
            space: {
                item: this.scale.height * 0.02,
            }
        };
        this.infoFontSize = this.scale.height * 0.02;

        const blackBackground = this.add.image(0, 0, UIAsset.black_background.key);
        Object.assign(blackBackground, SizeExtension);
        blackBackground.setOrigin(0, 0);
        blackBackground.setInteractive();

        this.events.on(this.eventNames.layout, () => {
            blackBackground.setMinPreferredDisplaySize(this.scale.width, this.scale.height);
        });

        const panelBg = this.add.sprite(0, 0, UIAsset.reward_list_panel.key);
        this.panelBg = panelBg;
        panelBg.setOrigin(.5,0)
        Object.assign(panelBg, SizeExtension);

        this.events.on(this.eventNames.layout, () => {
            panelBg.setMaxPreferredDisplaySize(this.scale.width, this.scale.height * .85);
            panelBg.setPosition(this.scale.width * 0.5, 0);
        });


        const contentSizer = new Sizer(this, 0, 0, 0, 0, 'vertical', this.sizerConfig);
        this.contentSizer = contentSizer;

        this.events.on(this.eventNames.layout, () => {
            contentSizer.setPosition(this.scale.width * 0.5, panelBg.displayHeight * 0.55);
            contentSizer.setMinSize(panelBg.displayWidth * 0.8, panelBg.displayHeight * 0.7);
        });

        const infoText = new TagText(this, 0, 0, 'Test');
        this.add.existing(infoText);
        this.infoText = infoText;
        contentSizer.add(infoText);
        infoText.setOrigin(0.5, 1);
        infoText.setAlign('center');
        infoText.setColor(INFO_COLOR);
        infoText.setFontFamily(FontAsset.baloo_chettan2_medium.key);
        infoText.setWrapMode('word');

        this.events.on(this.eventNames.layout, () => {
            infoText.setFontSize(this.scale.height * 0.02);
            infoText.setWrapWidth(contentSizer.minWidth, true);
        });

        const rewardSizerConfig = {
            orientation: 'x',
            align: 'center',
        };

        const rewardsSizer = new FixWidthSizer(this, 0, 0, 0, 0, rewardSizerConfig);
        contentSizer.add(rewardsSizer);
        this.events.on(this.eventNames.layout, () => {
            rewardsSizer.setMinSize(contentSizer.minWidth, contentSizer.minHeight - infoText.displayHeight,);
            this.itemSpacing = contentSizer.minWidth * 0.05;
            rewardsSizer.setItemSpacing(this.itemSpacing);
            rewardsSizer.setLineSpacing(this.itemSpacing);
        });

        if (data.content) {
            for (let i = 0; i < data.content.length; i++) {
                const item = new RewardListItemView(this, 0, 0, 500, 500);
                item.setUpData(data.content[i]);
                this.events.on(this.eventNames.layout, () => {
                    item.setMinSize(
                        rewardsSizer.minWidth / 2 - this.itemSpacing,
                        rewardsSizer.minHeight / (Math.ceil(data.content.length / 2)) - this.itemSpacing);
                });
                rewardsSizer.add(item);
            }
        }

        this.events.on(this.eventNames.layout, () => {
            contentSizer.layout();
        });
        this.events.emit(this.eventNames.layout);

        this.scale.on('resize', this.onResize, this);
        this.events.once('shutdown', () => {
            this.events.off('layout');
            this.scale.off('resize', this.onResize);
        });

        // this.contentSizer.drawBounds(this.add.graphics(), 0xff0000);

        if (isEmptyObject(data)) return;
        this.open(data);
    }

    onResize() {
        this.events.emit(this.eventNames.layout);
    }


    open(configuration) {
        this.onClose = () => {
        };

        this.infoText.setText(configuration.info ?? '');
        if (configuration.onClose)
            this.onClose = configuration.onClose;

        this.contentSizer.layout();
        this.okButton.setPosition(this.scale.width * 0.5, this.closeButtonY + this.panelBg.displayHeight * 0.01);
    }
}
```