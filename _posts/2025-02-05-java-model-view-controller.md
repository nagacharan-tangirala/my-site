---
layout: post
title: Model View Controller (MVC) Architecture Pattern
date: 2025-02-05 09:35:00
description: Model View Controller (MVC) pattern with an example in Java
tags: java-snippets course patterns architecture
categories: code
--- 

UI implementations usually come with lot of coupling.
This can complicate the implementation and make the maintenance a big challenge.
Model View Controller architecture helps to disconnect the data and UI elements, thereby making the UI management seamless.
Main motivation is to decouple the data access and data presentation objects.

There are three main components, as the name indicates:

- **Model**: Contains the application domain data, the entity objects.

- **View**: The information displayed to the user through the boundary objects.

- **Controller**: Mediates the change between the presentation(View) and the data objects(Model).


View and Controller together constitute the user interface.
There are two types, pull and push variant.
In push variant, the model sends the updated data to the **View** and **Controller**.
In pull variant, the **View** and **Controller** get the data from the model.

#### Model

This is the place with the ground truth data of all the contents displayed in the UI.

#### View

View faces the user and manages the UI elements.
The main task is to show the latest and updated data contained within the model.
It also accepts inputs from the user which eventually update the model.

#### Controller

This sits ideally between **Model** and **View**.
Every view has a reference of a **Controller**.
The **Controller** also knows all the available **View** implementations.
Depending on the **Model** data, **Controller** updates the appropriate view.

### Example

Consider a simple example where there is a text field asking for user their preferred food.
The **Model** stores the input food, **View** displays the text field and **Controller** handles the **View** updates.

Observer pattern is used in this implementation to achieve this architecture design.
The pull version is shown here.
The observable in this case is the **Model** and the observer is the **View**.

```java
interface Observer {
    void update();
}

interface Observable {

    ...
    private List<Observer> observers;
    ...

    // Called everytime a `model` update happens so that the UI shows fresh data.
    private notifyObservers() {
        for (Observer observer: this.observers) {
            observer.update();
        }
    }

    public void addObserver(Observer observer) {
        // add to the observer list
    }

    public void removeObserver(Observer observer) {
        // remove observer from the observer list
    }
    ...
}

```

The UI elements are the **View** components, and hence they are always ready to observe the data changes.
A simple UI window can be:

```java
class Window implements Observer {

    ...
    private FoodController foodController;
    ...

    void display() {
        // logic to display the UI window with the text input fields and a button to save the data.
        // Two empty text fields with `name` and `food` are displayed.
    }

    // This function is triggered when the `Save` button is pressed on the UI.
    void saveChoice() {
        FoodChoice foodChoice;
        // Logic to read the latest data from the UI and update the FoodChoice with it.

        // The updated FoodChoice is then sent to the controller.
        this.foodController.updateChoice(foodChoice); 
    }

    @Override
    void update() {
        // Update the UI elements with the current `FoodChoice` data.
    }

    ...
}
```
The `FoodController` is the controller that manages the view and model.
It will have a pointer to the observable which is the `FoodDatabase` in this case.

```java
class FoodController {

    ...
    private FoodDatabase foodDatabase;
    ...
    public void updateChoice(FoodChoice foodChoice) {
        this.footDatabase.updateData(foodChoice); // Update the model about the change in data.

    }
}

```

The **Model** is quite simple in this case.
This is the observable data.
Assume that we are storing the user name and the food in a database, so we update whenever a new entry is available.

```java
class FoodDatabase implements Observable {

    ...
    private List<FoodChoice> allChoices;
    ...

    void updateData(FoodChoice newEntry) {
        // Store the new food choice into the list.
        this.allChoices.add(newEntry);

        // Notify all the observers about the data change, this will refresh the UI.
        this.foodDatabase.notifyObservers();
    }
}
```

Finally, we are done.
This looks complex, because it is.
There are too many classes in it.
The guarantee with this implementation is that the maintenance is easy.
Suppose if you want a new view, all you have to do is create a new `Observer` implementation and add it to the `Observable` as one of the observers.
It will always receive the latest data refresh without additional effort and the `View` can focus on implementing the GUI elements instead of worrying about the data.

The decoupling between **View** and the **Model** is beneficial to expand the features without worrying about breaking things.
