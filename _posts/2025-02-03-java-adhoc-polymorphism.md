---
layout: post
title: Adhoc Polymorphism Example in Java
date: 2025-02-03 06:00:00
description: Adhoc Polymorphism
tags: java-snippets course
categories: code
--- 

## Adhoc Polymorphism

Adhoc polymorphism supports overloading the methods with the same name, which is distinguished by the parameter types.
For example, there are various types of `max` functions within the `math` library.

```java
public static int max(int a, int b)
public static long max(long a, long b)
public static double max(double a, double b)
```

The usage can be simple in the user code:

```java
int a = 3;
int b = 4;
int maxInt = Math.max(a, b);

long x = 10;
long y = 13;
int maxLong = Math.max(x, y);
```

Both the function calls will succeed without any errors.
The appropriate function call is invoked.
This process is called *Overloading*, where a single feature name denotes two or more operations.
In this case, `max` operates for various types with the same name.

Of course, the following call will not work.

```java
int maxMixed = Math.max(a, y);
```

The binding for the appropriate function happens at compile time.
Hence, the mismatched types will not be able to resolve to an appropriate function and results in a compilation error.

Another important point to note is that the matching is done purely on the function parameters and not on the return types.

### Overriding

Overriding differs slightly from the overloading aspect.
A method with the same signature can be duplicated.
The appropriate method call depends on the actual object used to call the method.
This is determined dynamically during the runtime of the program.

```java
class A {
    public int foo() {
        System.out.println("A is called!");
    }
}

class B extends A {
    public int foo() {
        System.out.println("B is called!");
    }
}
```

Which `foo()` is called depends on the caller object.

```java
A a = new A();
B b = new B();
a.foo(); // *A is called!*
b.foo(); // *B is called!*
```

This is because of the overriding, also a feature of polymorphism.


#### Summary

Overloading: Same function names with *different* parameter types, function matching at *compile* time.

Overriding: Same function names with *same* parameter types, function matching at *runtime*.
