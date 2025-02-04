---
layout: post
title: Bridge design pattern
date: 2025-02-03 07:25:00
description: Bridge design pattern with an example in Java
tags: java-snippets course patterns structural
categories: code
--- 

Bridge pattern is used when the classes are growing in multiple different dimensions in terms of abstraction.
There is a bridge between *abstraction* and the *implementation*.

A classic example is the `Shape`. You can have a variety of shapes like `Square` and `Circle`.
You can also have shapes with different colors, `RedSquare`, `BlueSquare`, `RedCircle`, `BlueCircle`.
You can see the problem already.
Adding new shape requires defining two new classes, one for each available color.
Similarly for the color too.

This blowup issue is fixed by separating the `Color` attribute to a separate class and let it grow on its own.
The `Shape` class will contain an instance of `Color` and delegates any color related aspects to the instance.
`Shape` as an *abstraction* delegates color related tasks to the `Color` *implementation*.

The implementation of `Color`, for example:

```java
class Color {

    ...
    private String colorHex;

    String getColorHex() {
        return this.colorHex;
    }

    void updateColor(String newHex) {
        this.colorHex = newHex;
    }
    ...
}
```

The implementation for `Shape` can be:

```java
class Shape {

    ...
    private Color color;
    private int area;

    // Anything related to shape is handled here.
    int getArea() {
        this.area;
    }

    // Anything color related is sent *through the bridge* for the color to handle.
    String getColorHex() {
        return this.color.getColorHex();
    }
}
```

This allows to create any number of `Color` and `Shape` objects without affecting the existing classes and without exponential blowup.
