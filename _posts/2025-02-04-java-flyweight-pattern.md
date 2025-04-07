---
layout: post
title: Flyweight design pattern
date: 2025-02-04 14:45:00
description: Flyweight design pattern with an example in Java
tags: java-snippets course patterns structural 
categories: code
---

Objects can be expensive to create and manipulate, especially in large numbers.
Simulations often contain several 100s or even 1000s of objects.
If there are some properties that are common among all the objects, then they can be extracted out.
This pattern is called Flyweight pattern.

Imagine a vehicle simulator where there are 1000s of cars.
However, there are only two models of the car supported.
The only difference between the two models is the current position.
Rest of the stuff is common among all the 1000s of instances of cars.

```java
abstract class Car {

    ...
    private Image image;
    private Color color;
    ...

    public abstract void setPosition(double newX, double newY); // Only thing that is changing between instances.
}

class SmallCar extends Car {

    ...
    private double x;
    private double y;
    private double SPEED = 100.0;
    private carFeatures carFeatures;
    ...

    public SmallCar(String style, Color color, CarFactory carFactory) {
        this.carFeatures = carFactory.getCarFeatures(style, color);
    }

    @Override
    void setPosition(double newX, double newY) {
        this.x = newX;
        this.y = newY;
    }
}

class BigCar extends Car {

    ...
    private double x;
    private double y;
    private double SPEED = 130.0;
    private carFeatures carFeatures;
    ...

    public BigCar(String style, Color color, CarFactory carFactory) {
        this.carFeatures = carFactory.getCarWith(style, color);
    }

    @Override
    void setPosition(double newX, double newY) {
        this.x = newX;
        this.y = newY;
    }
}
```

The `CarFactory` is defined as:

```java
class CarFactory {

    ...
    private Map<String, CarFeatures> cache;
    ...

    Car getCarWith(Style style, Color color) {
        if (this.cache.contains(style)) {
            returns this.cache.get(style);
        }
        CarFeatures carFeatures = new CarFeatures(style, color);
        this.cache.put(style, carFeatures);
        return carFeatures;
    }
    ...
}
```

Suppose we create 1000 `SmallCar` and `BigCar`, then there are 2000 objects that store image and color.
This is redundant and that's why we use `CarFeatures` object.
`CarFeatures` stores the constant information that is the `Style` and `Color`.
The `CarFactory` has a cache of the objects created with a specific `Style`.
Whenever a new `Car` is created, the `CarFactory` first checks if the requested style is already created for a prior creation of `Car`.
If so, then it returns a cached value.
Otherwise, it creates a new `CarFeatures`, caches it and returns a reference to the user.

The advantage of this is that the styles can be limited but the number of instances are more.
Each `Car` instance, instead of being associated with its own `CarFeatures`, it gets a reference from the cache.
May be there 10 to 15 styles, but 1000s of cars.
The memory gains are significant with such implementation.
Here, `CarFeatures` is the flyweight.





