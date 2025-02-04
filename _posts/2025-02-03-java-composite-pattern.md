---
layout: post
title: Composite design pattern
date: 2025-02-03 07:05:00
description: Composite design pattern with an example in Java
tags: java-snippets course patterns structural
categories: code
--- 

Hierarchical objects with arbitrary depths can be used to represent using a composite pattern.
The root object will treat the sub-objects and the unit component object as the same.
The sub-objects can also be sometimes complex or an aggregation of several unit components.

An example is a directory structure, with each directory containing multiple other directories, files and so on.
Represent a unit component, which is the `FileObject`, as a general interface.
```java
interface FileObject {
    int getSize();
}
```

Both files and directories can be represented as a `FileObject`.

```java
class File implements FileObject {

    ...
    private int size;

    @Override
    int getSize() {
        return this.size;
    }
    ...

}

class Directory implements FileObject {

    ...
    private int size;

    @Override
    int getSize() {
        return this.size;
    }
    ...

}
```

Using these components, the client, which in our case can be assumed to be a `FileViewer`, can get the size of any of the components.

```java
class FileViewer {

    ...
    private List<FileObject> fileObjects;

    int getSize() {
        int totalSize = 0;
        for (FileObject fileObject: this.fileObjects) {
            totalSize += fileObject.getSize();
        }
    }
    ...
}
```

In addition to the `getSize()`, the `FileObject` interface can be expanded to other similar tasks, such as `getName()`, `getPermissions()`, etc.
This allows the `FileViewer` to treat the individual components equally, irrespective of the fact that whether they are sub-directories or files.
