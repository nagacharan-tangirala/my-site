---
layout: post
title: Broker Architecture Pattern
date: 2025-02-05 12:35:00
description: Broker architecture pattern with an example in Java
tags: java-snippets course patterns architecture
categories: code
--- 

Client and server talk to each other to carry out transactions.
However, the implementation of the server can be in an entirely different language with an incompatible API.
Broker pattern helps to enable communication between such diverse client and server implementations.
Further, it decouples the client and server, allowing good extensibility even during runtime.

A very dumb broker implementation is demo'd here, the real implementations are typically complex.

Phone is a client that wants to access the weather server to get latest temperature:
```java
class Phone {

    ...
    private double temperature;
    private Location location;
    private WeatherService weatherService;
    ...

    public void getTemperature() {
        this.temparature = this.weatherService.getTemperature(this.location);
    }
    ...
}
```

A simple `WeatherService` can be:

```java
class WeatherService {

    ...
    double getTemperature(Location location) {
        double temperature = 0;
        // Some logic to get the latest temperature data and update it.
        return temperature;
    }
    ...
}
```

Such an implementation is tightly coupled. 
Broker pattern allows us to decouple this.

A simple broker will contain a list of services and returns the requested service to the client.

```java
class Broker {

    ...
    private WeatherService weatherService;
    ...

    ...
    double getTemperature(Location location) {
        if (this.weatherService != null) {
            return this.weatherService.getTemperature(location);
        } else {
            return ServiceNotFoundException("Weather service is not available");
        }
    }
    ...
}
```

Client, instead of directly contacting the `WeatherService` directly, will talk to the `Broker`.
The modified `Client` will now look like this:

```java
class Phone {

    ...
    private double temperature;
    private Location location;
    private Broker broker;
    ...

    ...
    public void getTemperature() {
        this.temparature = this.broker.getTemperature(this.location);
    }
    ...
}
```

This is how `Broker` pattern sits in between `Client` and `Server` and helps to decouple them.
Expansion is also easier with this approach.
