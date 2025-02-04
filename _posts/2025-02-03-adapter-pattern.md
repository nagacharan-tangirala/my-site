---
layout: post
title: Adapter design pattern
date: 2025-02-03 07:55:00
description: Adapter design pattern with an example in Java
tags: java-snippets course patterns
categories: code
--- 

Adapter pattern helps *adapt* an existing functionality to a new system.
Suppose you have a visualization system that generates some graphs based on `csv` files.
If there is a new software that only gives `parquet` files, then the visualization system will not work as is.
In this case, we assume that the visualization system irreplaceable and cannot be modified too.
This is true for many legacy systems, where the replacement cost is either business-critical or too expensive.
In such cases, adapter pattern can be applied to extend its functionality.

A sample `Plottable` interface can be defined as follows: 

```java
interface Plottable {

    ...
    private PlotData data;
    PlotData getData() {
        return this.data;
    }
    ...
}
```

Using the `PlotData`, the visualization service generates an image of the plot.

```java
class Visualizer {

    ...
    Image drawGraph(Plottable plottable) {
        Data data = plottable.getData();
        Image image;
        // Logic to plot data and create an image.
        return image;
    }
    ...
}
```

Let's say we now have a new external source for visualization that only returns data in `XYZData` format.

```java
class ParquetData {

    ...
    private XYZData xyzData;
    XYZData getData() {
        return this.xyzData;
    }
    ...
}
```

This is a legacy code or an external party library and it cannot be modified.
In order to make it adaptable to our visualization service, we add an adapter.

```java
class ParquetToCsvAdapter implements PlotData {

    ...
    private ParquetData parquetData; // adaptee in this case.
    private PlotData data;

    @Override
    PlotData getData() {
        XYZData xyzData = this.parquetData.getData();
        PlotData plotData;
        // Convert XYZData data to PlotData.
        return plotData;
    }
    ...
}
```

The `ParquetToCsvAdapter` class *implements* the interface `PlotData` and contains an *adaptee* `ParquetData`.
Without changing the legacy `ParquetData` code and our visualization library, we were able to meet the data contracts of the classes.
