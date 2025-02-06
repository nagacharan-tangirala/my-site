---
layout: post
title: Blob Antipattern
date: 2025-02-06 11:35:00
description: Blob Antipattern
tags: java-snippets course anti-patterns
categories: code
--- 

A class breaks its single responsibility principle and tends to grow extremely large in size.
Such a class is also called god object because it has most of the components of a system within itself.
Some design patterns also tend to lead towards such blob anti-pattern.
For example, mediator pattern helps two objects with different interfaces communicate.
If the number of interactions are too large or the number of components grow, the mediator object will have references of several services.
This makes the mediator class god object.
Even Facade pattern can lead to becoming a god object.

It is important to recognize such classes and reduce their responsibilities at an earlier stage.
Otherwise, they can become pain to maintain and enhance.
Over the time, developers will be scared to modify for the fear of breaking everything down.
Typical strategy to deal with it is:

- Distribute the responsibilities to smaller classes.
- Move functionalities to the respective smaller classes.
- Remove redundant associations and interactions with other classes.

A simple example is a chess game code:

```java
class Chess {

    ...
    void capturePlayerMove() {
        // logic to capture the user interface and carry out the move.
    }

    void updateUI() {
        // Updates the UI after the move.
    }

    void performComputerMove() {
        // logic to make the computer move the next piece.
    }

    void captureStats() {
        // logic to capture the statistics.
    }

    void trackTime() {
        // logic to capture the time remaining for each player.
    }
    ...
}
```

Clearly, the class is trying to do a lot and is an example for **Blob** pattern.
The functionality can be spread out over different classes.
There can be `Player` class responsible for the player action capture.
A UI manager will take care of the game window updates.
AI player will be in its own classes.
The timing module can be in its own class.
This is how the blob is dealt with.

A general exception to such approach when dealing with legacy systems.
Sometimes, it is not possible to modify the legacy code and building a wrapper around it is the only choice.
In such cases, breaking the code responsibilities can be tricky and a blob is unavoidable.
