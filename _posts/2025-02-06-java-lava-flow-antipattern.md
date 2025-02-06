---
layout: post
title: Lava Flow Antipattern
date: 2025-02-06 10:35:00
description: Lava Flow Antipattern
tags: java-snippets course anti-patterns
categories: code
--- 

These are the remnants of the code blocks that are either commented out or disabled by renaming the functions.
There are several reasons for this to happen.
Team members leaving the company, change in project goals, change in requirements, technology rigidness, lack of knowledge to fix broken things etc.
All of this results in weird comments that are left in production code to not touch the code.

It is compared to lava because over the years, lava gets hardened and remains as a solid with its scars visible evidently.
An entire architecture overhaul is required to deal with such remnants of the code.
Most importantly, knowledge of the engineer is essential to identify such parts of the code.

It is difficult to show how to fix it in an example.
All I can do is show some bad comments:
These are imagined but some or the other form of such comments are found in real-world code:

```java
void sendData() {

    ...
    // No idea why we need to send empty string but it will break if we don't.
    // Don't try to change it, otherwise the rest of the code will not work.
    // This depends on some library that we don't have code for.
    Url url = this.getUrl("");
    ...
}
```

```java
// Implemented by the Lead engineer who left us last year.
// No one knows what the numbers are for but this function is essential for our workflow.
// Changing this function will break the production.
double getNumber(Entity entity) {

// Some logic written to compute numbers.
// Uses lot of constants that are not defined.

}
```
