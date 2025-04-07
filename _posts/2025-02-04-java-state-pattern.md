---
layout: post
title: State design pattern
date: 2025-02-04 09:35:00
description: State design pattern with an example in Java
tags: java-snippets course patterns behavior
categories: code
--- 

The state design pattern is used to model the state machines.
The transitions between states are usually handled in a big `switch` statement.
This results in complex `switch` statement and can lead to testing difficulties.
State pattern helps in addressing the problem using object-oriented approach.

Each state is modeled as a separate state.
For example, consider a file object.
File can be either opened, closed or deleted.
Instead of modeling the states as `enum`, each state is modeled as a concrete object.
An abstract class is used to define the common interface for all the states.
The controller that is utilizing the object just calls the common interface.

The creation of new states can be in either controller or within the states itself.
A typical `File` class is:

```java
class File {

    ...
    private FileState fileState;
    ...
    void openFile() {
        switch (fileState) {
            case OPEN: System.out.println("Already opened!");
            case CLOSED: this.fileState = OPEN;
            case DELETED: throw new FileAlreadyDeletedException();
        }
    }
    ...
}
```

In this implementation, the logic is quite simple. 
In real-world examples, these can be quite extensive with many guard rails for each state.
Hence, we remodel this using state pattern.

A `FileState` abstract class can be:

```java
abstract class FileState {

    ...
    abstract void open(File file);
    abstract void close(File file);
    abstract void delete(File file);
    ...

}
```
The `File` class will delegate all the calls to this object.

```java
class File {

    ...
    private FileState fileState;
    ...
    void openFile() {
        this.fileState.open(this);
    }

    void closeFile() {
        this.fileState.close(this);
    }

    void deleteFile() {
        this.fileState.delete(this);
    }

    // A helper method to update the fileState object.
    void updateState(FileState newState) {
        this.fileState = newState;
    }
    ...
}
```

Now what remains to do is the concrete implementations for each state.
`OpenState` can have something like this.

```java
class OpenState extends FileState {

    ...
    @Override
    void open(File file) {
        System.out.println("Already opened!");
    }

    @Override
    void close(File file) {
        file.updateState(new CloseState()); // We are updating the file with the new state.
    }

    @Override
    void delete(File file) {
        System.out.println("Cannot delete when the file is open!"); // Invalid state transitions are prohibited.
    }
    ...
}
```

Similar implementations can be done for `CloseState` and `DeleteState` as well.
In `CloseState`, we can allow file deletion and opening.
In `DeleteState`, we can throw errors for both opening and closing.

Additionally, we can add guardrails to state transitions.
For instance, `OpenState` can have a check for permissions before trying to open the file.
`File` should pass these necessary constraints to the state objects, which can then use these guard rails and do additional conditional transitions.
