---
layout: post
title: Mediator design pattern
date: 2025-02-04 12:35:00
description: Mediator design pattern with an example in Java
tags: java-snippets course patterns behavior 
categories: code
--- 

Classes with fewer responsibilities are preferred.
However, too much modularization may result in increasing interactions between the objects.
This is not preferred because it increases the chances for bugs.

The coupling between the classes should be low.
Mediator pattern helps in achieving it.
It is like an air traffic controller that ensures safe interactions for airplanes.
It is not always recommended to add this pattern, because sometimes the high coupling can be a sign of bad design requiring a total redesign.

Steps to apply mediator pattern are:

- Identify the set of tightly coupled classes that can benefit from mediator pattern.
- Declare the mediator interface with the common method that represents an interaction.
- Implement the interface with a concrete mediator.
- Rework the interactions through the mediator.
- Make sure not to create a controller class with every component in the mediator or God object.

A simple example is a chat application, where users of different types want to interact.
A tutor and student interaction, for example.
The direct coupling is not good and a mediator pattern can be helpful to add interactions.

A mediator interface can be:
```java
interface Messenger {
    void send(Person sender, String message);
    void sendTo(Person sender, String receiverName, String message);
}
```

`Student` and `Tutor` are concrete implementations of the `Person` class.

```java
class Person {

    ...
    private String name;
    private Messenger messenger;
    ...

    void receive(String message) {
        System.out.println(message);
    }

    void sendMessage() {
        String newMessage = "Hello everyone!";
        this.messenger.send(this, newMessage);
    }

    void sendMessageTo(String targetName) {
        String newMessage = "Hello friend!";
        this.messenger.send(this, targetName, newMessage);
    }
    ...
}

class Student extends Person {

    ...
    private int year;
    ...
}

class Tutor extends Person {

    ...
    private String chair;
    ...
}
```

A concrete implementation of the `Messenger` mediator interface can be:

```java
class ChatApp implements Messenger {

    ...
    private List<Person> allPersons;
    ...

    void addPerson(Person newPerson) {
        this.allPersons.add(newPerson);
    }

    @Override
    void sendMessageTo(Person sender, String receiverName, String message) {
        for (Person person: this.allPersons) {
            if (person.getName() == receiverName) {
                person.receive(message);
            }
        }
    }

    @Override
    void send(Person sender, String message) {
        for (Person person: this.allPersons) {
            if (person.getName() != sender.getName()) {
                person.receive(message);
            }
        }
    }
}
```

This mediator is passed to all the `Person` objects at the time of creation.
Similarly, the mediator is also aware of all the `Person` objects that are created (needs to be done in the application).
This facilitates the interactions between seemingly complex objects of `Person`.
