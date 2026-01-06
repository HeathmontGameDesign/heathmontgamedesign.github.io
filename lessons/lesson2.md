# Lesson 2: Sprites and Movement

*Previous lesson: [Lesson 1 - Draw to Screen](lesson1.md) | Main Page: [Heathmont Game Design](/) | Next lesson: [Lesson 3 - Collision Detection](lesson3.md)*
Drawings are nice and all, but a game requires interactivity and movement. In Godot, we use **Sprites** (specifically Sprite2D nodes) to represent visually the objects in our game. Sprites can represent:

- a player character
- an enemy
- a bullet
- an obstacle
- or any other object that incorporates interactivity.

Sprites are usually created as child nodes (often of a `CharacterBody2D` or `Area2D` node) and they store image or animation information about an object in the game. This information gets used to draw the sprite on the screen and to move it around. Today we will create a simple sprite and make it move around the screen using keyboard input.

## Creating a Sprite

To create a sprite, we add a Sprite2D node to our scene. We can do this through the editor or through code. Here is an example of setting up a sprite in the editor and then referencing it in code:

1. In the Scene dock, click the "+" button to add a new node
2. Search for "Sprite2D" and select it
3. In the Inspector, click on the "Texture" property and load your Heath image
4. Position the sprite by changing its Position property or by dragging it around in the viewport

In your script (attached to the root node or the sprite itself), you can then reference and control the sprite:

```gdscript
extends Sprite2D

func _ready():
    position = Vector2(400, 300)
```

`position` is a built-in property of all Node2D objects (including Sprite2D). By setting the `position` as `Vector2(400, 300)`, we are changing the starting position of the sprite on the screen. The `_ready()` function is called when the node first enters the scene. **Code inside `_ready()` is executed once at the start of the game.**

## The `_process()` Function

The `_process(delta)` function is called every frame of the game. This is where we put code that changes the game state, such as moving sprites. The `delta` parameter tells us how much time has passed since the last frame, which helps keep movement smooth and consistent. **Code inside `_process()` runs continuously while the game is running.**

## Responding to Input - using if statements

We can check for keyboard input in the `_process()` function using Godot's `Input` class. We can use `if` statements to check if a key is pressed. Here is an example:

```gdscript
extends Sprite2D

func _process(delta):
    if Input.is_action_pressed("ui_left"):
        position.x -= 2
    if Input.is_action_pressed("ui_right"):
        position.x += 2
```

This code checks if the left or right arrow keys are pressed, and moves the sprite left or right accordingly. It changes the `x` position of the sprite by 2 pixels each frame (subtracting to go left, adding to go right).

Godot uses "input actions" which can be configured in Project Settings > Input Map. The built-in actions like `"ui_left"` and `"ui_right"` are already configured for arrow keys. When we create more complex games we would always define our own input actions.

## Sample Code

You now have all the information you need to play around with sprites and movement in Godot. It is also your first introduction to Heath, the Heathmont Game Design Robot!

![Heath](/assets/images/heath.png)

To get started:

- Download the sample code here: [Lesson 2: Sprites and Movement](https://github.com/HeathmontGameDesign/LearningGodot/tree/main/2_Sprites_and_Movement)
  - Download the whole folder and import it as a project in Godot. This keeps the assets in the correct place.
- Open the project in Godot and run it to see what it does.
- Following the comments in the code, modify the script to:
  - allow Heath to move up and down
  - add a ghost sprite that moves slowly across the screen (without any input)

## Challenge

Create a copy of your completed sample code and modify it so that:

- Once the ghost sprite reaches the right side of the screen, it reappears on the left side.
- Include a second ghost sprite that moves up and down. Have the second ghost go the other way when it reaches the top or bottom of the screen.
- (If you succeed with the first two) Add a third ghost sprite that moves randomly around the screen. *(Look up RandomNumberGenerator in Godot for help with this)*

Save your scene as `2_ghost.tscn`

*Previous lesson: [Lesson 1 - Draw to Screen](lesson1.md) | Main Page: [Heathmont Game Design](/) | Next lesson: [Lesson 3 - Collision Detection](lesson3.md)*
