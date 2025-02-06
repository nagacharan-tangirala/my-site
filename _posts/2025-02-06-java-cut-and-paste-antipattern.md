---
layout: post
title: Cut and Paste Programming Antipattern
date: 2025-02-06 12:55:00
description: Cut and Paste Programming Antipattern
tags: java-snippets course anti-patterns
categories: code
--- 

Cut and Paste programming antipattern is the result of relying too much on external sources for coding.
Earlier, it was due to pasting code snippets off of StackOverflow and other code related websites.
With the increasing AI penetration in programming, developers should be more aware of this antipattern.
The code suggestions can be often littered with subtle bugs or non-standard design practices.
As a developer, it is the responsibility to recognize such bugs and only incorporate good code.
Otherwise, the codebase will grow huge with lot of automagically generated code.
The developers will no longer be in control of the codebase and enhancements can be tricky.

Feature development is important, but developing in the future should not be compromised.
While good design practices can sometimes be time consuming, the benefits are always evident.
Cut and Paste programming can lead to exponential growth of number of lines, but with questionable reliability.
It is always recommended to be mindful of letting such code growth.

A common way of solving such antipattern growth is refactoring.
There are three main things that a developer can do:

- Common method refactoring: Refactoring out the common methods to a parent class will reduce the surface area for bugs.
- White box reuse: Subtle differences captured in the subclass to mimic the majority of superclass functionality as is and support the new differentiable feature.
- Black box reuse: Combine several fragmented classes doing similar task to one class.

```java
class Simulator {

    ...
    void moveVehicles() {
        // Logic to move the vehicles.
    }

    void moveElectricVehicles() {
        // Logic to move the electric vehicles.
    }
    ..
}
```

The logic for moving the normal vehicles is already written in `moveVehicles()`.
Hence, a new feature for electric vehicles is cut and pasted from `moveVehicles()` and modified to fit electric vehicle behavior.
Adding new vehicles will further blow up this class size.
Further, the functionality is repeated in both the functions.
This will have deviations in the behavior, causing bug fixes from one method not reaching the other function.

This can be addressed by using the proposed methods.
Applying one of the tricks, we can refactor this code.
`Vehicle` can be defined as a parent class, with `BenzinVehicle` and `ElectricVehicle` being the children classes.

```java
class Vehicle {

    ...
    private int vehicleId;
    ...

    ...
    void move() {
        // Common logic to move the vehicles.
    }
    ...
}

class BenzinVehicle extends Vehicle {

    ...

    // No need to override move() here because default is sufficient.
    ...
}

class ElectricVehicle extends Vehicle {

    ...

    @Override
    void move() {
        // Logic to move the electric vehicles because of their different drive train.
    }

    ...
}
```

The simulator can only have the logic for moving the vehicles, without being concerned about the type of vehicles.

```java
class Simulator {

    ...
    void moveVehicles() {
        // Call the respective vehicles` move function.
    }
    ...
}
```

Although this is obvious in this case, real-world codes are much more complex and can be difficult to spot such antipatterns.
While developing, the developers should be long-sighted and should not trade short-term productivity over long-term maintainability.
This is called tech debt and will be paid by the team in future if not the same developer who introduced it.
