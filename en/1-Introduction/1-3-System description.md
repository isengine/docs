# System description

Before starting work, you need to clarify how we represent the system and what parts it is built from.

The parts are presented as separate repositories of the "isEngine" vendor, and after installation, as folders inside your project.

Dividing the system into these parts simplifies the process of monitoring and developing the system. It also allows for independent development of each part.

If you want to update any part of the system, you can simply replace the corresponding folder. This will not affect the rest of the parts, including your project files.

## Data

Any system is based on data, and the system itself, in fact, simply controls access to it. In an extended form, it looks like reading, writing and exchanging (receiving and transmitting), processing and outputting data to the screen of the user's device. Users are divided into groups and for different groups there are different restrictions on working with data.

When we speak for a system, we mean many rules, and the most important are rules for working with data. The framework, being a layer between the system and the programming language, provides all the necessary tools for working with data according to these rules. The converse is that by using the framework, you accept these rules.

Thus, the framework sets the rules for working with data and provides all the necessary tools.

The kernel is a system assembled on the basis of a framework with which to work. It can be changed and configured, extended and modified, various modules and libraries can be connected. It is the core that is the heart of the system.

The difference between the core and the framework is that the framework is not a finished product, but only a set of tools. It cannot be configured. But it is the framework that sets the rules for working with data that should be followed.

## Framework

By definition, a framework is a software platform that defines the structure of a system. It is a kind of framework that includes a set of components to facilitate project development.

Also, a framework is understood as a framework approach to building a program (in this case, a web application), which is built from two parts:

* a constant part that does not change from configuration to configuration,
* variable part, consisting of replaceable modules.

In the established tradition, a web framework is an almost ready-made framework for a web application, which already includes many components, but sharpened for a specific structure.

**"Framework"** isEngine is only a persistent part. A set of rules and tools to implement these rules.

To be honest, the isEngine framework is more like a library. It is also true that it can be used as a library with any other project.

If you use the existing view of the web framework, then the entire isEngine system falls under it, including the framework, core and preset settings.

## Core

The core **"Core"** is an add-on over the framework. It is the core that acts as the platform for the web application.

Unlike a framework, the core uses settings and, depending on them, can have different configurations. The behavior of the system and its structure change depending on these settings.

## Installer

The installer **"isEngine"** allows you to set the initial configuration of the system, including the default template, administration panel, etc.

It is not required for the system to work. Rather, it is an add-on designed to make it easier to start the system for the first time.

In the future, it is planned to use it to create a backup copy of the system and update it.

## Modules

Modules are not directly related to parts of the system, they are part of the view (view).

> more details about the view can be found in the chapter "MVC"

Modules are taken out as separate parts of the system because they are developed as independently as the rest of the system.

## Skeleton

Skeleton **"Skeleton"** is a skeleton for creating your own module.

It, like modules, does not apply to the parts of the system described here. And it shouldn't be present in the system at all.

However, like the parts of the system shown here, it is developed, installed and updated independently of other parts of the system.