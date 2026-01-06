# Lesson 4 - Mouse Clicks

*Previous lesson: [Lesson 3 - Collision Detection](lesson3.md) | Main Page: [Heathmont Game Design](/index.md) | Next lesson: [Lesson 5 - Arrays and Sounds](lesson5.md)*

So far we have only used very basic keyboard input inside the `_process()` function in Godot. In this lesson, we will learn how to use the mouse to interact with our games. We will learn how to detect when the mouse is clicked, and how to use the position of the mouse to interact with sprites on the screen.

## Reminder: The Game Loop

In Godot (as with lots of game development frameworks), the game loop has three main parts:

1. **Process**: This is where we update the game state. We move sprites, check for collisions, and update the score. This happens in the `_process(delta)` or `_physics_process(delta)` functions.
2. **Draw**: Godot handles drawing automatically based on your scene tree. Each visible node draws itself according to its properties.
3. **Input**: This is where we check for player input. We can use `_input()` or `_unhandled_input()` functions to detect keyboard, mouse, and other input events.

So far, we have mainly used the `_process()` function. In this lesson, we will learn how to use the `_input()` function to detect when the mouse is clicked - which represents our first dedicated use of Input events in the game loop.

## Mouse Input in Godot

In Godot, we can detect when the mouse is clicked by using the `_input(event)` function. This function is called whenever an input event occurs (mouse, keyboard, etc.). We check what type of event it is and respond accordingly.

The simplest version of using mouse input looks like this:

```gdscript
extends Node2D

func _input(event):
    if event is InputEventMouseButton:
        if event.pressed:
            print("Mouse clicked at ", event.position)
```

This code would print the position of the mouse each time the mouse is clicked to the console. The `event.position` gives us a Vector2 with the (x, y) coordinates of where the mouse was clicked.

We can also check which mouse button was pressed using `event.button_index`, which will be values like MOUSE_BUTTON_LEFT, MOUSE_BUTTON_RIGHT, etc.

## Our Sample Code

- Download the sample code here: [4 - Mouse Clicks](https://github.com/HeathmontGameDesign/LearningGodot/tree/main/4_Mouse_Clicks)
- *Note: the tick and cross images are included in the project assets.*

In the sample code, Heath is currently moving randomly across the screen. It looks a little bit weird, but it will work fine for today's game. You need to click on Heath correctly, as often as you can. Let's take a look at what is happening at the moment.

```gdscript
extends Node2D

# Constants
const HEATH_SPEED = 20
const SCREEN_WIDTH = 800
const SCREEN_HEIGHT = 600

# References to nodes (set in _ready or through @onready)
var heath
var cross
var score = 0
```

Not much new so far - but there is one key difference here. HEATH_SPEED is written in all caps. This is a convention to show that it is a **constant** - a value that we will not change during the game. Constants are often written in all caps to make them easy to spot in the code and they are especially useful when we need to use the same value in multiple places. *Any number or value that is used more than once in the code should usually be stored as a constant.* In Godot, we use the `const` keyword to define constants.

```gdscript
func _ready():
    # Get references to the sprites in the scene
    heath = $Heath  # Assumes you have a node named "Heath" in your scene
    cross = $Cross  # Assumes you have a node named "Cross" in your scene
    
    # Set initial positions
    heath.position = Vector2(randf_range(0, SCREEN_WIDTH), randf_range(0, SCREEN_HEIGHT))
    cross.position = Vector2(-50, -50)  # Off screen
```

The `_ready()` function is called when the node enters the scene tree. We use it to get references to our sprite nodes using the `$` syntax (which is shorthand for `get_node()`). We also set initial positions for Heath and the cross.

```gdscript
func _process(delta):
    # Heath moves HEATH_SPEED pixels in a random direction
    var move_x = [HEATH_SPEED, -HEATH_SPEED].pick_random()
    var move_y = [HEATH_SPEED, -HEATH_SPEED].pick_random()
    heath.position.x += move_x
    heath.position.y += move_y
    
    # If Heath goes off the screen, move him back to somewhere random
    if heath.position.x < 0 or heath.position.x > SCREEN_WIDTH:
        heath.position.x = randf_range(0, SCREEN_WIDTH)
    if heath.position.y < 0 or heath.position.y > SCREEN_HEIGHT:
        heath.position.y = randf_range(0, SCREEN_HEIGHT)
    
    # Redraw to update the score display
    queue_redraw()
```

In the `_process()` function, we move Heath in a random direction. We use the `pick_random()` method on an array to randomly choose between moving forward or backward. This is similar to Python's `random.choice()`.

We also check if Heath goes off the screen, and if he does, we move him back to a random position.

We call `queue_redraw()` to ensure our custom drawing (like the score) gets updated each frame.

Note that we haven't included any of the code for the mouse clicks yet. We will add this in the `_input()` function.

```gdscript
func _draw():
    # Draw the score
    var font = ThemeDB.fallback_font
    var font_size = 20
    draw_string(font, Vector2(10, 30), "Score: " + str(score), HORIZONTAL_ALIGNMENT_LEFT, -1, font_size)
```

In Godot, we use the `_draw()` function to draw custom elements. Here we're drawing the score text. Godot provides `draw_string()` for text rendering. The parameters are the font, position, text, alignment, width (-1 means no limit), and font size.

Note: In a real game, you might prefer to use a Label node for displaying text rather than drawing it manually, but this demonstrates the drawing function.

```gdscript
func _input(event):
    if event is InputEventMouseButton:
        if event.pressed and event.button_index == MOUSE_BUTTON_LEFT:
            # Set the position of the cross to the position of the mouse click
            cross.position = event.position
            
            # TODO: Add code to check for collision with Heath
            # If there is a collision, increase the score by 1 and change the cross texture to a tick
            # If there is not a collision, decrease the score by 1 and change the cross texture to a cross
            
            # Schedule hiding the cross after 0.5 seconds
            get_tree().create_timer(0.5).timeout.connect(hide_cross)

func hide_cross():
    cross.position = Vector2(-50, -50)
```

The `_input()` function is already moving the cross to the position of the mouse click. It does this by checking if the event is a mouse button event, if it was pressed (not released), and if it was the left mouse button.

The `TODO` comment is asking you to add code to check for a collision between Heath and the cross. In Godot, you can check if two sprites overlap by comparing their positions and sizes, or by using collision shapes. For this simple case, you might check if the click position is within Heath's bounding rectangle.

If there is a collision, you need to:

- Increase the score by 1
- Change the texture of the cross sprite to show a tick image

If there is not a collision, you need to:

- Decrease the score by 1
- Change the texture of the cross sprite to show a cross image

The `get_tree().create_timer(0.5).timeout.connect(hide_cross)` line creates a one-shot timer that calls the `hide_cross()` function after 0.5 seconds. This ensures the cross doesn't stay on the screen after we click.

## Your Task

Complete the TODO comments in the sample code:
  
- You will need to check if the mouse click position is within Heath's bounds. A simple approach is to check the distance between the click position and Heath's position. For example: `if event.position.distance_to(heath.position) < 50:` (where 50 is an approximate radius for Heath). For a more accurate rectangle-based check, you could add an Area2D node to Heath with a CollisionShape2D and use collision detection.
- You will need to increase or decrease the `score` variable based on whether there is a collision or not with the mouse position.
- You will need to change the texture of the `cross` sprite to a tick or a cross based on whether there is a collision or not. You can do this with `cross.texture = load("res://path/to/tick.png")`.
- The score should already be displayed if you're using a Label node or the drawing function shown above.
- Test your game by running it and clicking on Heath as often as you can. Make sure the score increases when you click on Heath, and decreases when you don't.

## Challenge

- Add a timer to the game. The timer should start at 30 seconds and count down to 0. When the timer reaches 0, the game should end. You can use `get_tree().create_timer()` or a Timer node to run a function every second to update the timer. You will need to add a new variable to store the timer value, and you will need to display the timer on the screen.
- Add a game over screen that displays the final score when the timer reaches 0. You can use a variable to keep track of whether the game is over or not. When the timer reaches 0, set this variable to true and display the game over screen. You can use a Label node or the drawing function to display the final score on the game over screen.
- Add a restart button to the game over screen that allows the player to restart the game. You can use the `_input()` function to check for a click on the restart button (by checking the click position), and reset the game state if the button is clicked.

*Previous lesson: [Lesson 3 - Collision Detection](lesson3.md) | Main Page: [Heathmont Game Design](/index.md) | Next lesson: [Lesson 5 - Arrays and Sounds](lesson5.md)*
