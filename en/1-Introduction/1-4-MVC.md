# MVC

isEngine is built using MVC (Model-View-Controller) concept. This approach allows you to divide the system into three separate components.

According to the MVC concept, these three components should be isolated from each other as much as possible. This approach provides a certain level of security and facilitates development.

## Model

A model is the same as a model in real life - a model of behavior, a model of a situation. The entire system is built from models, as from blocks.

Applied to isEngine, the underlying models are defined by the framework.

Working with a database, working with user data, calling api - all this is represented by various models.

These basic models are expanded in the kernel. And you can also extend them in your project.

## Controller

The controller acts as an intermediary between the user and the system. As the name suggests, this component is responsible for receiving and processing data from the user to the system.

It is important to understand here that the controller is the thinnest component of the system. He is only responsible for data control. All basic business logic is based on using models, calling their methods, and reading states.

In isEngine, the kernel is the controller. It interacts with incoming requests and, depending on the situation, calls up various models.

## View

Just like the controller receives data from the user, the view gives the user specific data.

A view is an interface for working with the system.

In isEngine, templates are used as views. However, an application that interacts with the system via api can be used as a view.

## View Model

When working with the system, it is very important to understand the difference between the **view model** and the **view** itself.

Again. **View** is a component in MVC concept. In isEngine, these are templates.

**View model** is part of the **isEngine** framework, one of the models.

The view model was created as a layer between the **view** and the rest of the system.

According to the MVC concept, the view should not know anything about the model or the controller. However, we cannot completely isolate it from the system. The view should receive some data about the system, and in the case of the administrative part, almost all of this data. If we give the species access to models, then we get a close relationship, as a result of which we cannot separate the two components.

> If we speak in examples, then any change in the core or framework will entail the need to change templates.

In this case, the view model is the only interface with which the view can work. Through this model, the view gets all the data it needs.

This solution also ensures that as the system is further developed and developed, the view can still interact with it, regardless of its or its internal architecture.

> Returning to the examples, this means that the template will work correctly on different versions of the system.