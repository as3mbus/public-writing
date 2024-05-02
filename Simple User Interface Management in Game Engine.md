---
type: Article
State: Done
tags:
  - GameDevelopment
  - GameEngine
---
**Simple User Interface** - interface that block user from interacting with other system
Example : 
- loading screen
- game popup
# Problem
below are problem that surfaced and need to be addressed within this discussion
- Cache (memory allocation)
- Frame Lag (Complexity, and Processing Speed)
- Modularity (dependency, and re-usability)
- preference (ease-of-use)
# Discussion Point

## Caching Vs Re-Create User Interface

This point discuss on addressing Cache, and Frame Lag Problem.

- It is always preferred to cache and re-use any created object instead of destroying and re-creating it multiple time (reduce complexity, increase memory allocation)
- rarely this can be neglected with close deadline with prerequisite listed below
    - not unity project
    - scene is simple enough that re-creating object, doesn't cost much processing power

## As Object Vs As Scene

-This point address Modularity, and Preference Problem

there are 2 form simple interface can be made of :
- Object (Container, Prefab, or Unity Object)
- Scene (Phaser.Scene or UnityScene)

Below are difference that help determines which suits you better

### Object
- Object can be either miniscule with minimal overhead to entire User interface system that can be used
- it contains component that build it functionality
- require scene or world scope to contain it
- will require additional visibility API to manage

### Scene
- scene consist of various game object that form it functionality
- Require management system to call, and manage it
- exist independently
- make use of game engine built-in scene management API and call protocol
- create dependency between Simple interface, Management API and User
    - could be properly addressed with proper system architecture
    - a good Object Management API Module would be great
- the only dependency it creates are between Scene and User
    - would require more user-side code or extension method that extend scene management API
- **Pros** :
	- easily referenced and called
	- lesser handling to reset the scene state
	- more lightweight memory allocation
	- easier layering management when user interface are used to block entire game scene (loading scene, popup scene)
- **Cons** :
	- more process calculation (would create object within the scene everytime it open)
	- more coupled implementation as scene launch doesn't refer to class but instead using key name value
	    - which doesn't really matter as scene is always project limited
	- no caching capability (no pooling)
	- non-type-safe scene data
	- these cons can be easily addressed by hiding scene, and provide open method that reset the state and show the scene with updated data
	> [!info] Phaser 3 Examples  
	>  Phaser : Calling method in different scene
	>  
	> [https://labs.phaser.io/edit.html?src=src/scenes/call%20function%20in%20another%20scene.js&v=3.53.1](https://labs.phaser.io/edit.html?src=src/scenes/call%20function%20in%20another%20scene.js&v=3.53.1)  
## Game Engine Trivia
- [[Unity]] render order are based on canvas render mode, sorting order, and other configuration
- [[Phaser]] render order are based on depth
- [[Phaser]] depth are within scene scope
    - thus scene that contain simple interface should always be on top of game scene
- [[Phaser]] Render Order based on Scene Order
- [[Unreal Engine]] do this with separate scope in Viewport, HUD, and User Widget 

# Result

With proper Simple Interface Management System it would Greatly help implementing simple interface that can be re-used between project.

These system would require the following features :

- depth ordering
- object referencing (type-safe)
- abstract calls similar to scene manager
- easily implemented
# Benefit

The benefit that i hope to achieve is to be able to call popup module easily, and abstractly without specifying detail of the data.

> [!important]  
> if you have any insight regarding this topic, would really appreciate if you could spend some time to comment this topic about your insight/finding  

# Teams Insight

## Phaser 3

- scene call having frame gap
    - frame gap isn't a problem as the scene are launched additively
- it depends on how the popup object placed within the system
- can be easily done through depth changing
- why not cached beforehand ? similar to how screen utility
    - can be cached through scene sleep
- don't need enforce it. can create container module. with easier data passing (type-safe)
- "prefer to cache it within a scene that always run"
- scene separation is ok, and crafted based on problem at hand. emphasize for complex scene such as leaderboard and/or history popup.
- careful with scene launch abuse, and animation effect calls
- phaser `scene.bringToTop()` send scene to top with unknown order index
    - Tested from scene with index 3 are jumped to index 15
- phaser render sorting order based on scene, then object depth.
- there is also a way to create a mediator scene that listen to other scene request to call object within it

## Unity

- Unity Canvas re-ordering doesn't factor scene order. it only factor sort order of the canvas
- unity left more memory footprint when loading scene multiple time. (caching is more important here)