---
layout: post
title: Golden Hammer Antipattern
date: 2025-02-06 09:35:00
description: Golden Hammer Antipattern with an example in Java
tags: java-snippets course anti-patterns
categories: code
--- 

Anti-patterns are similar to patterns but result in negative consequences instead of benefits.
Initially, they look tempting and feel like a good choice to apply.
However, they can quickly become the weakness of the system.
Maintenance and expansion gets increasingly challenging.

One such anti-pattern is Golden Hammer.
There is a saying that, when you have just a hammer with you, everything looks like a nail.
If a developer is familiar with some tool/framework/data structure, they are prone to use it in every scenario.
They can have preferences in working with certain tools/patterns.
However, using them everywhere may not be a good choice and can quickly cause problems for the rest of team.

A simple way to demonstrate is the use of `List` everywhere, as if it is a golden hammer.
Suppose that the developer who wrote this (not me) fancies `List` 

```java
class Simulator {

    ...
    private List<Integer> vehicleIds;
    private List<Vehicle> vehicles;
    ...

    double getSpeed(int vehicleId) {
        Vehicle vehicle;
        for (Integer vehId: vehicleIds) {
            if (vehicleId == vehId) {
                vehicle = this.vehicles.get(vehId);
            }
        }

        double speed = 0.0;
        if (vehicle != null) {
            speed = vehicle.getSpeed();
        }
        return speed;
    }
    ...
}
```

This can be way more simplified if a `Map` is used instead.

```java
class Simulator {

    ...
    private Map<Integer, Vehicle> vehicles;
    ...

    ...
    double getSpeed(int vehicleId) {
        if (this.vehicles.containsKey(vehicleId)) {
            return this.vehicle.get(vehicleId).getSpeed();
        }
        return 0.0;
    }
    ...
}
```

This looks simple with this example but imagine if this applies to any specific **Design Pattern** or **Architecture Pattern** then there will be more complexity introduced in the code just to fit in the preferred pattern.
It is recommended to pick the best pattern that fits the problem instead of personal choices.

