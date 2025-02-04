---
layout: post
title: Facade design pattern
date: 2025-02-03 08:35:00
description: Facade design pattern with an example in Java
tags: java-snippets course patterns structural
categories: code
--- 

Facade pattern is useful to hide the complex interactions between multiple coupled objects.
For example, consider a shopping mall with offering different kind of services.
Each store offers a single service or a combination of the services.
Handling every possible combination can be complex.
Facade pattern can help mask these complex interactions into one class and allow the user to only use one facade controller.

A shopping mall store can have any of the combination of the following functionalities depending on the kind of store.

```java
class PaymentController {

    ...
    void handlePayment(Item item) {
        // Take money equivalent to the item's price.
        System.out.println("Payment processed");
    }
    ...
}

class ShippingController {

    ...
    void shipItem(Item item) {
        System.out.println("Item shipped");
    }
    ...
}

class OrderController {

    ...
    void processOrder(Item item) {
        System.out.println("Order processed");
    }
    ...
}
```

Every store in the shopping mall will one or more of these these instances.
For instance, `Cinema` will only have `OrderController` and `PaymentController`.
A clothing store may have all three of the classes.

```java
class Cinema {

    ...
    private OrderController orderController;
    private PaymentController paymentController;
    ...
    void buyPopcorn(Item popcorn) {
        this.orderController.processOrder(popcorn);
    }
    ...
}

class ClothingStore {

    ...
    private OrderController orderController;
    private PaymentController paymentController;
    private ShippingController shippingController;
    ...
    void buyShirt(Item shirt) {
        this.orderController.processOrder(shirt);
    }

    void shipShirt(Item shirt) {
        this.shippingController.shipItem(shirt);
    }
    ...
}
```

Exposing all the controllers to each store can complicate the implementation.
All the store's functionality is delegated to their respective controllers.
Hence, facade pattern is used to mask this complexity.

```java
class ShoppingMallFacade {

    ...
    private OrderController orderController;
    private PaymentController paymentController;
    private ShippingController shippingController;
    ...

    void handlePayment(Item item) {
        this.paymentController.handlePayment(item);
    }

    void shipItem(Item item) {
        this.shippingController.shipItem(item);
    }

    void processOrder(Item item) {
        this.orderController.processOrder(item);
    }
    ...
}
```

The `ShoppingMallFacade` will contain all the essential controllers for a store to use.
The stores can be simplified as they can just use this facade instead of the individual controllers.


```java
class Cinema {

    ...
    private ShoppingMallFacade mallFacade;
    ...
    void buyPopcorn(Item popcorn) {
        this.mallFacade.processOrder(popcorn);
    }
    ...
}

class ClothingStore {

    ...
    private ShoppingMallFacade mallFacade;
    ...
    void buyShirt(Item shirt) {
        this.mallFacade.processOrder(shirt);
    }

    void shipShirt(Item shirt) {
        this.mallFacade.shipItem(shirt);
    }
    ...
}
```
