---
layout: post
title: Strategy design pattern
date: 2025-02-03 09:35:00
description: Strategy design pattern with an example in Java
tags: java-snippets course patterns behavior
categories: code
--- 

Strategy pattern helps selecting from multiple implementations based on a certain policy.
A simple example is the contextual based search that selects from the multiple available search algorithms.
If the data is sorted, then binary search can be used.
Otherwise, a linear search is unavoidable.

We can represent the search strategy by an interface:

```java
interface SearchStrategy {
    int searchKey([T] data, T key);
}

```

There can be multiple search strategies:

```java
class BinarySearch implements SearchStrategy {

    ...
    @Override
    int searchKey([T] data, T key) {
        // Implement binary search to find the position of the key.
        return position;
    }
    ...
}

class LinearSearch implements SearchStrategy {

    ...
    @Override
    int searchKey([T] data, T key) {
        // Implement linear search to find the position of the key.
        return position;
    }
    ...
}
```

Now consider a simple `Service` that needs this search feature.
`Service` does not bother about which search strategy is being used, it just calls the search function.

```java
class Service {

    ...
    private SearchStrategy searchStrategy;
    private boolean isDataSorted;
    ...
    void searchForKey([T] data, T key) {
        this.searchStrategy.searchKey(data, key);
    }
}
```
Handling the switch between different search strategies is dependent on `Context`. 
The `Context` is not explicitly handled in the `Service`.
Instead, `Policy` is used to monitor the `Context` and accordingly initialize the `SearchStrategy`.

```java
class ServicePolicy {

    ...
    private Service service;
    ...
    ServicePolicy(Service service) {
        this.service = service;
    }

    void configure() {
        if (service.isDataSorted()) {
            service.setStrategy(new BinarySearch());
        } else {
            service.setStrategy(new LinearSearch());
        }
    ...
}
```

Each time a new `Service` is created, a corresponding `ServicePolicy` can be created. 
Depending on the state of the `Service`, an appropriate `SearchStrategy` instance is set to the `Service`.
The `Service` can then just call the `searchKey()` method and it will call the appropriate concrete implementation.

```java
main() {

    ...
    Service service = new Service();
    ServicePolicy policy = new ServicePolicy(service);
    policy.configure();
    ...
}
```
