---
title: Using custom resources in Godot
tags:
  - draft
  - godot
  - gamedev
---

The concept of separating data and logic in [Godot](http://godotengine.org) is quite powerful: you're using Resources to store some data about a class and custom scripts to define logic. Of course, it's not as simple as that and scripts are technically resources too, but that's not the point.

### What you'll learn

In this post you'll learn how to create your own custom resources to hold data of various types that can be accessed and modified through the inspector panel and scripts.

We'll start with the most basic example and then dig into how you can take it one step further by creating a fully dynamic inspector interface that changes depending on which variables are set in the resource.

## 1. Basic example

As a basic example, we're going to create a character class resource that defines a variety of stats for different playable classes (such as Warrior, Wizard, etc...).

![structure](/static/img/godot-res-1.png)

### 1.1. Creating a resource script

Start by creating a script file called `CharacterStats.gd` in the Filesystem dock.

We want this script to extend Godot's `Resource` type. Additionally, we can register the class globally as `CharacterStats` to utilize Godot's auto-completion when we'll be referencing it from other scripts.

```gdscript
extends Resource
class_name CharacterStats
```

Next we'll define some starting variables like the name of the character class and a description.

```gdscript/3,4
extends Resource
class_name CharacterStats

var name: = "Character name"
var description: = "Character description"
```

### 1.2. Exporting script variables

TODO: remove this section and just start with empty variables

Prefixing a variable with `export` keyword serializes it in the Godot Inspector for easier editing. Godot serializes a variable based on its type, so a `String` will be show as a text field, `bool` will be a checkbox and so on.

```gdscript/3,4
extends Resource
class_name CharacterStats

export var name: = "Character name"
export var description: = "Character description"
```

![Exported variables in Godot inspector](/static/img/godot-resources-1.gif)

In this case the variable type is inferred from the value, but if we were to declare an empty variable and export it, we could write it like so:

```gdscript
export(String) var name
export(String) var description
```

This is called export hints and you can learn more about different examples in the [Godot documentation][godot export docs].

In our case, we want the `description` variable to be exported as a large text area, as we might have longer descriptions for each class. Adding a `MULTILINE` flag in the export hint makes exactly that:

```gdscript/1
export(String) var name
export(String, MULTILINE) var description
```

![](/static/img/godot-resources-2.gif)

[godot export docs]: https://godot.readthedocs.io/en/latest/getting_started/scripting/gdscript/gdscript_exports.html#introduction-to-exports

Now we can add variables for all character stats we're planning to use in our game.

```gdscript
extends Resource
class_name CharacterStats

export var name: String
export(String, MULTILINE) var description: String
export var icon: Texture

export var health: int
export var armor: int
export var damage: int
export var attack_speed: float # attacks per second
```
