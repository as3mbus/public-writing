---
Created: 2021-09-12T19:39
tags:
  - Technical
Last Edit: 2022-02-17T14:36
State: Done
type: Article
---
# Problems

- Monolithic code makes your code barely understandable nor maintainable
- Implementation of multiple module that interact with each other resulting in coupled interaction thus throwing modularity out of the window
- accessing module of a module is indecent and resulting in improper coupling
- Separation of concern and single responsibility paradigm leaves you with independent component that doesn't work together

# Solution

- an implementation logic should be separated to different segment that can be re-invented multiple time even with different module with the same purpose at mind
- Multiple module that interact with each other could have been a pattern that can be generalize into it's own module so it can be used multiple times
- Limit user access so they can't reach your inner parts

# What Is Facade Pattern

> The facade pattern (also spelled façade) is a software-design pattern that Analogue facade in an architecture, a facade is an object that serves as a front-facing interface masking more complex underlying or structural code. ~ Wikipedia

> [!important]  
> in a nutshell it is a nutshell  

  

![[Untitled 22.png|Untitled 22.png]]

Source : Wikipedia

# Real World Equivalent

Look around you. most of stuff you used have a facade hiding their inner darkness if multiple implementation

# Related Topic

- Separation of Concern leaves you with multiple module that works independently, you might use this to implement additional logic that integrate multiple module into a new module that implement those module with specific logic case
- isn't like most of your framework including unity, .net, and any other stuff, already leads you into sense of facade. giving you false sense of freedom where they have hidden parts that you might blindly use and bind yourself onto it ?

# Topic Detail

This pattern is some form of **Abstraction** that works well in a case where **Composition Over Inheritance** are more desired. It **Separates Implementation Logic** from it's actual module, keeping the module loosely coupled with other module that it might interact with.

## Abstraction

Like most Object Oriented Design pattern facade provide an abstraction options that helps with application of SOLID principle

- S : creating a facade class that only responsible in handling implementation logic that works with multiple module
- O : Making sure that every module can stay as it's own and any other implementation can re-invent the wheel dependent on it's own use-case
- L : Separating Implementation logic and module logic as different matters
- I : by having a facade we can hide other irrelevant properties and only providing access to the logic where it actually matters
- D : By separating implementation logic and their module we could always re-use the logic with different module that serve the same purpose for the logic to run

## Composition Over Inheritance

By hiding an implementation of multiple module facade pattern is more valued in condition where those component comes from it composition than it come from inheritance. Facade main purpose is to hide multiple module implementation within a abstraction layer that only contain it's implementation logic. By using composition more the implementation logic won't have additional overhead or any unused implementation that might add additional complexity within it.

## Implementation Logic Separation

good facade is a facade that can be re-used without any dependency with it's used module, thus resulting in facade that only contains implementation logic that can be used multiple time no matter whatever component that it needs to serve to.

# Study Cases

## Root Composition

Ever since knowing this method from previous sharing and actual implementation in WASKITA project, it's been a effective tool to work with.

### Main

Containing Root composition and initialization of multiple module that going to be used;

### Scene Controller

Containing Multiple Module Implementation within a game scene including :

- dependency Injection
- Initiation
- event trigger

# Lesson Learned

> [!important]  
> The more i know