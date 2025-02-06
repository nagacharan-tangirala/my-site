---
layout: post
title: Functional Decomposition Antipattern
date: 2025-02-06 09:55:00
description: Functional Decomposition Antipattern with an example in Java
tags: java-snippets course anti-patterns
categories: code
--- 

The functional approach treats everything as a function instead of the Object-oriented approach.
The problem with this is that the maintenance can be challenging and tracking changes will be difficult.
Rather than introducing as a function within the same class, it makes sense to imagine the functionality as a set of objects.
However, the challenge with object-oriented approach is the level of granularity to choose.
This requires the business case understanding to focus on the aspects that users need.

A simple example is a *vehicle simulator*.
The granularity level should match the target users.
For instance, if the users only care about how vehicles impact a certain road network, then it makes sense to model the mobility and simplify the vehicle.
Similarly, if the users want to study the electric charging mechanism, then simulating the power train model of vehicle is required.
It does not make sense to simulate power train of thousands of vehicles if the user only cares about the vehicles' impact on the road.

A simple example is shape manager in any GUI tool:

```java
class ShapeManager {

    ...
    void drawSquare() {
        // Logic to draw square.
    }

    void drawCircle() {
        // Logic to draw circle.
    }

    void resizeSquare() {
        // Logic to resize square.
    }

    void resizeCircle() {
        // Logic to resize circle.
    }
    ...
}
```

Looking at shapes as manager results in the above decomposition.
If a new shape is added, then the class further blows up.
A future requirement to change the shape may also result in additional complexities.
Hence, we decompose this to objects to make it easier to manage.

```java
interface Shape {
    ...
    void draw();
    void resize();
    ...
}

class Square implements Shape {

    ...
    @Override
    void draw() {
        // Logic to draw square.
    }

    @Override
    void resize() {
        // Logic to resize square.
    }
    ...
}

class Circle implements Shape {

    ...
    @Override
    void draw() {
        // Logic to draw circle.
    }

    @Override
    void resize() {
        // Logic to resize circle.
    }
    ...
}
```

Now the shape manager can just delegate the drawing logic to these sub-classes.
Any modifications to the properties like color or effects can also be further delegated to the respective class implementation.
