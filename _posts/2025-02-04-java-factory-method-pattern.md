---
layout: post
title: Factory method design pattern
date: 2025-02-04 12:55:00
description: Factory method design pattern with an example in Java
tags: java-snippets course patterns creational 
categories: code
--- 

Low coupling is preferred.
The implementation should depend on the super classes and interfaces, but not on the concrete implementations.
Hence, the creation of the objects should be decoupled from the object usage places.
Factory method pattern can assist in achieving it.

Suppose we are building a rental app that allows vehicle rentals.
It creates a type of the vehicle on the fly and allows it to be rented.

The interface for the `Vehicle` can be:

```java
interface Vehicle {
    String getName();
}

class Bike implements Vehicle {

    ...
    private String name;
    private boolean isRented;
    ...

    @Override
    String getName() {
        return this.name;
    }
}

class Car implements Vehicle {

    ...
    private String name;
    private boolean isRented;
    ...

    @Override
    String getName() {
        return this.name;
    }
}
```

The creation of the `Vehicle` objects is handled in the `VehicleFactory` class:

```java
class VehicleFactory {

    ...
    Vehicle createVehicle(String vType) {
        if (vType == "car") {
            return new Car();
        } else {
            return new Bike();
        }
    }
}
```

The rental app wants to rent these vehicles whenever a new request arrives.
Depending on the concrete request of the user app, appropriate `Vehicle` will be created.

```java
class Rental {

    ...
    private VehicleFactory vehicleFactory;
    boolean rent(String vehicleType) {
        Vehicle vehicle = this.vehicleFactory.createVehicle(vehicleType);
        vehicle.rent(); // Any logic required to rent.
        return true;
    }
    ...
}
```

Here, `Rental` application is free from bothering about the actual concrete types of `Vehicle`.
Instead, it can write the logic assuming that the object is of `Vehicle` interface type.
Any new extensions are easier, it just needs adding new concrete implementation of `Vehicle` and extending the `VehicleFactory` to support its creation.

