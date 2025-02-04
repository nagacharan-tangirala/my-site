---
layout: post
title: Proxy design pattern
date: 2025-02-03 07:35:00
description: Proxy design pattern with an example in Java
tags: java-snippets course patterns structural
categories: code
--- 

Proxy pattern lets you save costs of creating an expensive object.
Sometimes, an object of use must be fetched from a database multiple times for multiple requests.
This can quickly get expensive, if the object is large and complex.

Proxy pattern allows to have a stand-in light weight counterpart for an expensive or heavy object.
The counterpart will be used by the application in most cases.
Whenever there is a necessity, and unavoidable use, the actual object is instantiated and the necessary operation is passed to the actual expensive object.

There are three main use cases:

- Caching: Used to cache the object in a proximity, if the object does not change too much. This saves frequent fetches.

- Substitute: Used as a stand-in light weight counterpart to have a virtual representation. 
Imagine zoomed out regions of the map, which need not be rendered because they are not in the user focus at the moment.

- Access control: Used to add access control capabilities and permissions related layer in the proxy before actual object is allowed to be accessed/viewed.

Lets take an access control example.
Suppose you have a workplace to restrict some websites.
You can implement this system using proxy pattern.

Define a common interface to represent browsing.

```java
interface BrowserInterface() {

    ...
    Data connectTo(Url url);
    ...

}
```

CEO browser implements the logic to connect to the given websites without any restrictions.

```java
class CEOBrowser implements BrowserInterface {

    ...
    @Override
    Data connectTo(Url url) {
        Data data;
        // Some logic to get data from the internet.
        return data;
    }
    ...
}
```

However, you don't want employees to connect to any URL.
Proxy pattern lets you add such access control.

```java
class EmployeeBrowser implements BrowserInterface {

    ...
    private List<Url> restrictedURLs;

    @Override
    Data connectTo(Url url) {
        if (this.restrictedURLs.contains(url)) {
            throw AccessDeniedException();
        }

        // Not a restricted URL so we let the user access data.
        Data data;
        // Some logic to get data from the internet.
        return data;
    }
    ...
}
```

The GUI can still show work with the `BrowserInterface` object. 
If it is an `CEOBrowser`, the `URL` will be allowed to connect without any restrictions.
However, if it is an `EmployeeBrowser`, an additional check is performed for the restrictions.
Only if it is not restricted, the `URL` is connected to the internet.

Here, the `EmployeeBrowser` is the proxy, using which the access control mechanism is added.
