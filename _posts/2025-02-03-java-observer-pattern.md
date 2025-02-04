---
layout: post
title: Observer design pattern
date: 2025-02-03 08:45:00
description: Observer design pattern with an example in Java
tags: java-snippets course patterns behavior
categories: code
--- 

Observer pattern is used to separate the state of the object from its observers.
There can be one object but many obesrvers.
To maintain consistency, the observer pattern lets every observer update their information at a time.
This is also called **Publish and Subscribe**.

There are 3 variants: 

- Notification and Pull: Notify the observers that the state is changed. 
The observers can decide whether to pull the latest state or not.

- Notification and Push: Along with the notification, the latest state is also sent to the observer.

- Periodic Pull: The observer will do a periodic pull of the latest state.

A simple example can be the state of a simulation that is logged and plotted:

```java
class VehicleSim {

    ...
    private double speed;
    private GraphTool graphTool;
    private LogTool logTool;
    ...
    void updateState(double newSpeed) {
        this.speed = newSpeed;
        this.graphTool.updateSpeed(newSpeed);
        this.logTool.updateSpeed(newSpeed);
    }
    ...
}

class GraphTool {

    ...
    @Override
    void updateSpeed(double newSpeed) {
        // Code to update the graph with the new speed.
    }
    ...
}

class LogTool {

    ...
    void updateSpeed(double newSpeed) {
        // Code to update the log file with the new speed data.
    }
    ...
}
```

At each speed update, the respective graph and log tools are called.
In an extensive simulation class, such classes can be much more and the calls will also increase.
To address this issue, we use observer pattern to model the visualization stuff.

A simple observer pattern consists of an `Observer` interface and `Subject` of interest whose state is being monitored.
The `Subject` can hold many instances of `Observer`.

```java
interface Observer {
    void onUpdate(T);
}

abstract SimulationSubject { // Subject in this example.

    ...
    private List<Observer> observers;
    ...
    void addObserver(Observer<T> observer) {
        this.observers.add(observer);
    }

    void removeObserver(Observer<T> observer) {
        this.observers.remove(observer);
    }

    void notifyObservers() {
        for (Observer<T> observer: this.observers) {
            this.observer.onUpdate(T);
        }
    }
    ...
}

```
The graph and log tools are modified to implement the `Observer` interface.

```java
class GraphTool implements Observer {

    ...
    @Override
    void onUpdate(double newSpeed) {
        // Code to update the graph with the new speed.
    }
    ...
}

class LogTool implements Observer {

    ...
    @Override
    void onUpdate(double newSpeed) {
        // Code to update the log file with the new speed data.
    }
    ...
}
```

The `VehicleSim` will now become our **Subject** by extending the `SimulationObject` class.

```java

class VehicleSim extends SimulationObject {

    ...
    private double speed;
    ...
    void updateState(double newSpeed) {
        this.speed = newSpeed;
        this.observers.notifyObservers(speed);
    }
    ...
}
```

Whenever the `updateState()` method is called in the `VehicleSim`, all the observers are notified of the speed update.
They can use the latest information to perform their respective tasks.
By having this observer as an abstract class, we reduce cluttering even in the `VehicleSim` class.
Moreover, this also reduces instances of tools getting outdated data.
