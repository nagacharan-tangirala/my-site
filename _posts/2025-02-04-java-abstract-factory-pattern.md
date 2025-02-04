---
layout: post
title: Abstract Factory design pattern
date: 2025-02-04 13:55:00
description: Abstract Factory design pattern with an example in Java
tags: java-snippets course patterns creational 
categories: code
--- 

Sometimes, we are trying to build a product that requires components of various types.
However, the mix and match of the components is not desired.
In such cases, the creation of the components can be bug prone.
This can be solved by abstract factory pattern.

Suppose there are two types of sports gear.
Athletes prefer sports gear of their preferred band but do not allow mix and match.

A sample sports gear, shorts, could be:

```java
interface Shorts {
    double getPrice();
}

class PumaShorts {

    ...
    private double price;
    ...

    @Override
    double getPrice() {
        return this.price;
    }
    ...
}

class NikeShorts {

    ...
    private double price;
    ...

    @Override
    double getPrice() {
        return this.price;
    }
    ...
}
```

Similarly, `Racket` can be defined:

```java
interface Racket {
    double getPrice();
}

class PumaRacket {

    ...
    private double size;
    ...

    @Override
    double getSize() {
        return this.size;
    }
    ...
}

class NikeRacket {

    ...
    private double size;
    ...

    @Override
    double getSize() {
        return this.size;
    }
    ...
}
```

The athletes want to buy their preferred brand, and they do not want to end up with `NikeShorts` and `PumaRackets` or vice versa.
Abstract Factory pattern can help in such cases.
Initially, we define an abstract factory that defines the structure for creation of these objects.

```java
interface SportsFactory {

    Shorts createShorts();

    Racket createRacket();
}
```

This interface can be implemented by the concrete factories.

```java
class PumaFactory implements SportsFactory {

    @Override
    Shorts createShorts() {
        return new PumaShorts();
    }

    @Override
    Racket createRacket() {
        return new PumaRacket();
    }
}

class NikeFactory implements SportsFactory {

    @Override
    Shorts createShorts() {
        return new NikeShorts();
    }

    @Override
    Racket createRacket() {
        return new NikeRacket();
    }
}
```

The sports store can exploit these abstracted factories to create concrete factories based on the user requests.

```java
class SportsStore {

    ...
    private SportsFactory sportsFactory;
    ...

    void makeSale(String preferredMake) {
        if (preferredMake == "Nike") {
            this.sportsFactory = new NikeFactory();
        } else {
            this.sportsFactory = new PumaFactory();
        }

        // Here the same brand shorts and racket is guaranteed to be created.
        Shorts shorts = this.sportsFactory.createShorts();
        Racket racket = this.sportsFactory.createRacket();

        // Code to make the sale.
    }
    ...
}
```

This will prevent mix and match as expected and the sports store does not perform additional checks to actually call appropriate brand's new methods.
This may look trivial in this case, but if the object creation is complex and needs a builder of its own, then abstract factory pattern will be highly beneficial to mask that implementation away from the application.
