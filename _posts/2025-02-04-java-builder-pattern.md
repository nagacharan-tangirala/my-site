---
layout: post
title: Builder design pattern
date: 2025-02-04 15:45:00
description: Builder design pattern with an example in Java
tags: java-snippets course patterns creational 
categories: code
---

The creation of the object can sometimes get complex.
Each component may have multiple possible sub-class variants.
The builder pattern makes it easy to construct such objects.

Suppose you have complex simulation of vehicular networks that contains various components.
Each component also has several models that they would like to configure.

```java
class Simulation {

    ...
    private List<Vehicle> vehicles;
    private List<RSU> rsu;
    private List<Server> servers;
    private List<BaseStation> baseStations;
    ...
}
```

Depending on the scenario, the possible components vary.
Further, the number of components also vary from scenario to scenario.
Builder pattern will let you build such object.
A common simulation builder interface can be defined as:


```java
interface SimBuilder {

    List<Vehicle> buildVehicles();
    List<RSU> buildRSU();
    List<Server> buildServers();
    List<BaseStations> buildBaseStations();
    Simulation build();
}
```

A concrete implementation for the interface can be given as follows:

```java
class SimulationBuilder implements SimBuilder {

    ...
    @Override
    SimBuilder buildVehicles() {
        List<Vehicle> vehicles;
        // Logic to build the vehicles list. (This also includes logic to variety of vehicles)
        this.simulation.setVehicles(vehicles);
        return this;
    }

    @Override
    SimBuilder buildRSU() {
        List<RSU> rsus;
        // Logic to build the RSU list.
        this.simulation.setRSU(rsus);
        return this;
    }

    @Override
    SimBuilder buildServers() {
        List<Server> servers;
        // Logic to build the Server list.
        this.simulation.setServers(servers);
        return this;
    }

    @Override
    SimBuilder buildBaseStations() {
        List<BaseStation> baseStations;
        // Logic to build base stations list.
        this.simulation.setBaseStations(baseStations);
        return this;
    }

    @Override
    Simulation build() {
        return this.simulation;
    }
}
```

Using this builder, we can configure different simulation scenarios.
For example, a `V2RScenario` will only have `Vehicle` and `RSU` components in the simulation.

```java

class Runner {

    ...
    void runSimulation(String scenario) {
        ...
        Simulation simulation;
        if (scenario == "V2R") {
            simulation = this.createV2RScenario();
        } else {
            simulation = this.createAllScenario();
        }
        ...
        // code to run the simulation.
        ...
    }

    Simulation createV2RScenario() {
        SimBuilder builder = new SimulationBuilder();
        builder.buildVehicles()
            .buildRSU()
            .build();
    }


    Simulation createAllScenario() {
        SimBuilder builder = new SimulationBuilder();
        builder.buildVehicles()
            .buildRSU()
            .buildServers()
            .buildBaseStations()
            .build();
    }
}
```

Similarly, other combinations are also possible. 
Not just the possible components, the variants of each component can also be configured from different builder requirements.
For instance, `Vehicle` list can be populated with different types of `Vehicle` derived classes.

Each method in the builder can be supplemented with additional conditions and input files to assist more complex object creation.
The creation of each component such as `Vehicle` or `BaseStation` can be further complex, which can be built using **Factory** or **Builder** patterns.
