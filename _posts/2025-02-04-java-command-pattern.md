---
layout: post
title: Command design pattern
date: 2025-02-04 11:45:00
description: Command design pattern with an example in Java
tags: java-snippets course patterns behavior
categories: code
--- 

Command pattern is used to make elements such as UI understandable.
For instance, a GUI will have so many menus and settings.
Having a custom behavior for the settings is also common.
However, having a different interface to each command makes the command invocation difficult.

There are five main components to handle:

- **Command** interface: A common interface that is used to represent any command.
- **Concrete Commands**: These are the concrete implementations of the interface.
- **Receiver**: The entity on which the actions must be performed.
- **Invoker**: The boundary object that interacts with the *Client* and performs commands on the *Receiver*.
- **Client**: The thing or class that uses the receiver.

A light switch is a good example.
Bulb is the *Receiver*, user interacting with the light is the *Client*.
The commands are turning the light on or off.
We use the switch as an *Invoker*.
A simple interface for the switch commands can be:

```java
interface SwitchCommand {
    void execute();
}
```

Bulb is the **Receiver** that must be controlled with these commands.

```java
class Bulb {

    ...
    private State state;
    ...

    void turnOn() {
        this.state = ON;
    }

    void turnOff() {
        this.state = OFF;
    }
    ...
}
```

For various switch commands, the concrete implementations can be provided:

```java
class LightOn implements SwitchCommand {

    ...
    private Bulb bulb;
    ...

    @Override
    void execute() {
        this.bulb.turnOn(); // Actual work on the receiver is done in the concrete command implementations.
        System.out.println("Turned the light on!");
    }
}

class LightOff implements SwitchCommand {

    ...
    private Bulb bulb;
    ...

    @Override
    void execute() {
        this.bulb.turnOff(); // Actual work on the receiver is done in the concrete command implementations.
        System.out.println("Turned the light off!");
    }
}
```

The switch is the **Invoker**, that receives the commands and executes them.

```java
class LightSwitch {

    ...
    void controlLight(SwitchCommand switchCommand) {
        switchCommand.execute();
    }
    ...
}
```

Any entity can be using this as a part of the system.
Lets assume that a smart home controller has this, and this becomes our **Client**.

```java
class HomeController {

    ...
    private Bulb bulb;
    private LightSwitch lightSwitch;
    ...

    void turnLightOn() {
        SwitchCommand onCommand = new LightOn(this.bulb);
        this.lightSwitch.controlLight(onCommand);
    }

    void turnLightOff() {
        SwitchCommand offCommand = new LightOff(this.bulb);
        this.lightSwitch.controlLight(offCommand);
    }
    ...
}
```

Further commands can be added, which is merely extending the `SwitchCommand` interface with concrete implementations.
The **Invoker** needs little to no changes to support the new commands.
