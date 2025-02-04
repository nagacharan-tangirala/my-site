---
layout: post
title: Template Method design pattern
date: 2025-02-04 11:25:00
description: Template Method design pattern with an example in Java
tags: java-snippets course patterns behavior
categories: code
--- 

Sometimes, there are two different classes that show very similar behavior with different interfaces.
Template method pattern can be used to remodel and make these indifferent interfaces stick to a common interface.
This prevents expensive refactoring costs that require changing both the classes.

Refactoring has a checklist.
 - Identify the common and different steps that the indifferent interfaces are doing.
 - Create a new super abstract class with all the common methods defined in it.
 - Move all the common methods to this class from the individual classes.
 - Define abstract placeholder methods for the methods that are different in both the interfaces.
 - Invoke these methods from the template.
 - Convert the indifferent classes as sub-classes derived from the abstract class and override the abstract methods.

A very simple example would be file writers that can output to different formats.

```java
class CsvWriter {

    ...
    private File file;
    private CsvData csvData;
    ...

    private void openCsv() {
        this.file.open();
    }

    public void writeDataToCsv(Data data) {
        this.openCsv();
        this.formatData(data);
        this.writeToCsv(csvData);
        this.closeCsvFile();
    }

    private void writeToCsv() {
        this.file.write(this.csvData);
    }

    private void formatData(Data data) {
        // Code to format the given data into CsvData.
        this.csvData.update(data);
    }

    private void closeCsvFile() {
        this.file.close();
    }
    ...
}
```

The following is a parquet writer that reads schema before writing data.
You can see that the both the classes have some functionality that is overlapping.

```java
class ParquetWriter {

    ...
    private File file;
    private Schema schema;
    ...

    private void openParquet() {
        this.file.open();
    }

    private void readSchema() {
        // Code to read schema from the user input file.
    }

    public void writeDataToParquet(Data data) {
        this.openParquet();
        this.readSchema();
        this.writeToParquet(data);
        this.closeFile();
    }

    private void writeToParquet()
        this.file.writeWithSchema(data, this.schema);
    }

    private void closeParquetFile() {
        this.file.close();
    }
    ...
}
```

The two file writer objects do very similar tasks but have wildly different interfaces.
In case, if we want to check whether a file is being overwritten, then the check has to be added to both the classes.
This will create a possibility for bugs.
We tackle this problem using template pattern.

The common steps are opening and closing.
`ParquetWriter` has a single indifferent function that does schema reading before writing.
`CsvWriter` does the formatting step before writing.
The template method is the data writing step, which will call all the steps in the predefined order.

Now that we identified the common and indifferent methods, we create a new abstract super class.

```java
abstract class Writer {

    ...
    private File file;
    ...

    // Common functionality is generalized and moved to this abstract class.
    void openFile() {
        this.file.open();
    }

    void closeFile() {
        this.file.close();
    }

    // This is the template method pattern. It calls the same functionality for both kinds of writers.
    // It is the responsibility of the overriding classes to actually implement a corect behavior.
    public void writeOutput(Data data) {
        this.openFile();
        this.formatData(data);
        this.readSchema();
        this.writeData();
        this.closeFile();
    }

    abstract void formatData(Data data);

    abstract void readSchema();

    abstract void writeData(Data data);
    ...
}
```

With the abstract class ready, we override it and write the actual implementations.

```java
class CsvWriter extends Writer {

    ...
    private CsvData csvData;
    ...

    @Override
    void formatData(Data data) {
        // Logic to format the data into CSV format.
        this.csvData.update(data);
    }

    @Override
    void readSchema() {} // No schema in CSV, so we have an empty implementation.

    @Override
    void writeData(Data data) {
        this.file.write(CsvData); // We ignore the input data, because there is formatted data already.
    }
    ...
}
```
Similarly, we write parquet writer as well.

```java
class ParquetWriter extends Writer {

    ...
    private Schema schema;
    ...

    @Override
    void formatData(Data data) { } // We don't need formatting here, so we keep this empty.

    @Override
    void readSchema() {
        // Code to read Schema data and update the schema member.
    }

    @Override
    void writeData(Data data) {
        this.file.write(data, this.schema); // We write using the schema and data.
    }
    ...
}
```

This completes the template pattern refactoring.
The template method is the `writeOutput` method in the abstract super class `Writer`.
The extended classes do not bother about the method call sequence.
Instead, they focus on their respective ways of doing the appropriate file writing.
