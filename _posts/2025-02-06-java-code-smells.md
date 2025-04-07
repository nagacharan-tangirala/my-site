---
layout: post
title: Code Smells in Object Oriented Approach
date: 2025-02-06 16:35:00
description: Common code smells and suggestions to fix
tags: java-snippets course code-smell
categories: code
--- 

Code smell is a debatable term, often confused with code preferences of an individual developer.
Some of the code styles are recognized to have caused problems in most cases.
We will discuss a few in this post, along with some suggestions to handle them:

- **Long methods**: Some methods are long, which make them difficult to read.
    The definition of long is subjective.
    However, if there is a recognizable routine in the method that can be extracted into its own method, it is better to do it.

- **Duplicate code**: Writing the same code in multiple places increases the bug probabilities.
    Change in one of the implementations will not propagate to the duplicate part and causes mismatch in the business logic.
    It is better to extract it into a method and reuse it.

- **Big class**: Some classes are too long, almost inching on being a god class. 
    This happens if the class starts doing more than one thing and grows too big as a result.
    The class should be broken down into multiple classes according to their functionalities.

- **Big parameter list**: Some function parameters are too long, causing confusion at the caller end.
    Too many parameters can also cause bugs in the parameter order.
    All the parameters can be easily extracted into a class and the entire class can be sent to the function instead.

- **Feature envy**: A class is too dependent on other class and its methods.
    If the other class has functionality that is highly required by the current class, it is better to include that functionality directly.

- **Lazy class**: Some classes have no interesting behavior.
    It could be due to refactoring, change in design or outdated feature.
    Such classes can be removed or turned into an attribute if they contain minimal number of members.

- **Speculative Generality**: A good foresight is required to envision future developments.
    If it is done excessively, then there are too many hierarchies and layers of inheritance.
    Such inheritance layers increase the complexity and should be removed if there is no clear indication of near future use.
    Speculating future growth and writing features for them is not recommended.

- **Refused bequest**: A derived class is sometimes mimicking the model of the parent class.
    However, the interface is not desired to have compatible API.
    This can lead to weird guard rails to check if the base class pointer has the right object or not.
    The entire situation can be avoided by making the derived class and converting it into delegation relationship.

> ###### WARN 
> ###### Like with any guideline, do not follow these recommendations blindly.
{: .block-warning}

Sometimes, an occasional duplication is fine.
Some guidelines are also at odds with each other.
For example, reducing the parameter width by creating a class can result in a simple class doing little work.
However, a class with too little responsibilities is considered as a code smell.
Hence, apply common sense and make a better judgment depending on the task at hand.

The criteria to classify something as a code smell is not objective.
Hence, it is often a good call to rely on the tribal knowledge of the team to understand the general code preferences of the team.

