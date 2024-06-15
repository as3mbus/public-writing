---
Created: 2021-09-12T19:39
tags:
  - Technical
Last Edit: 2021-10-04T19:51
State: Done
type: Article
---
Serialization = converting data that can be converted back into the object with logic (model)

Deserialization = Converting data back to object with logic (controller)

it may look different but after several cycle thinking it actually isn't that different.

MVC is basically separating serialized object (model) with the logic or rule that work with it (controller), and it's representation logic (view)

Model = Business data that is important and can be interacted in many ways

Controller = Business logic that interact with data to produce/ modify data according to defined rules

view = Representation logic that displays model and provide user a way to interact with model through controller.

this approach would benefit multiple controller controlling a single model. separating different logic layer into different part.

the difference between MVC and Composite Pattern (the one used by unity component system) is in here model dont have any relation with other entity. thus leaving model extendible to many direction. controller in the other hand act as component that can exist on it's own with the only relation is with model it interact with.

# MVC

Design Pattern that aim to separate data (model), business logic(control), and presentation logic.(view)

business logic here are any calculation / action that can be done for data.

this pattern are famous in web app implementation. below are the reason

- web app are heavily data dependent
- there are bunch of representation option that can be made from single web app.
- web app highly value stored data to the point where any data should never be removed.
- concurrent web app development are working with logic that modify, or create more data.
- the logic of each controller rarely affect each other. (shouldn't affect each other)
- usage flow of web app usually linear.

# MVC isn't really for game.

- game isn't dependent with business data.
- game is the representation layer and the logic packed in 1
    - it can be separated but the relation will still exist
- game doesn't store much data other than current player data, and game data.
- game concurrent development works with available data to represent valid gameplay, it read, and modify repeatedly. with data creation either at the end or at the beginning of the usage.
- game have highly correlating controller that read and write other controller
- usage flow of the game are concurrent. having update loop while multiple controller interacting with each other multiple time concurrently

# case where MVC Pattern Applicable within a game

- base game flow
    - game initialization (player data as model, scene as controller interacting with player data, and gameplay scene as view)
    - game data calculation
- Menu, and informational User Interface
    - this is for interface that doesn't update at rapid rate.
    - interface that simply display an static information.
- web backend services