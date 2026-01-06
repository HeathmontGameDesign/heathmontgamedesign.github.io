# Task 1: Draw to Screen

Main Page: [Heathmont Game Design](/) | Next task: [Task 2 - Sprites and Movement](lesson2.md)
To start with Godot, you need to know how to work with the screen and draw things. The screen is where everything happens in your game. You can draw shapes, images, and text on the screen using Godot's drawing functions.

## Coordinates and Vectors in Godot

Godot uses a coordinate system to decide where to draw things on the screen. The top-left corner is the origin (0, 0). The x-axis increases as you travel to the right, and the y-axis as you go down. This is different from the coordinate system you learned in maths, where the y-axis increases as you go up.

### Understanding Coordinates and Vector2

![Coordinate System](/assets/images/coordinates.png)

The image above shows how points on a screen become coordinates. The top-left corner is (0, 0). The magenta dot is at (300, 0). The blue dot is at (50, 150).

In Godot, positions on the screen are represented using **Vector2**. A vector is a mathematical value that can represent position, direction, or movement in space. A Vector2 is a 2D vector that stores two values: an x component and a y component. While you can think of Vector2 as a coordinate pair (x, y), it's more than just a position - it can also represent direction, velocity, or any two-dimensional quantity.

When we write `Vector2(400, 300)`:

- The first value (400) is the x component - how far right from the origin.
- The second value (300) is the y component - how far down from the origin.

Vector2 is used throughout Godot for positions, sizes, directions, and more. For now, we'll use it to specify where to draw shapes on the screen.

## Drawing Shapes

Godot has multiple ways to draw shapes on the screen. The most common approach for learning is to use a script attached to a Node2D and override the `_draw()` function. Here is an example:

```gdscript
extends Node2D

func _draw():
    draw_circle(Vector2(400, 300), 30, Color.WHITE)
```

This code draws a circle. The parameters are:

- The first parameter is the center of the circle as a Vector2. Here, `Vector2(400, 300)` means the center is 400 pixels from the left and 300 pixels from the top of the screen.
- The second parameter is the radius of the circle. Here, it is 30 pixels.
- The third parameter is the color of the circle. Here, it is Color.WHITE, which is a built-in color in Godot.

To draw a filled circle (which is the default), you can use the same `draw_circle()` function. For an outline only, you would need to draw the circle with a small radius difference or use other drawing techniques.

### Rectangles

Rectangles are drawn similarly in Godot. To draw a rectangle, we use the `draw_rect()` function. The position of a rectangle is its top-left corner, not its center. Here is an example:

```gdscript
extends Node2D

func _draw():
    var rect = Rect2(100, 100, 50, 50)
    draw_rect(rect, Color.BLUE)
```

This code draws a rectangle. The parameters are:

- The first parameter is a `Rect2` object. This object is created by providing the x and y coordinates of the top-left corner of the rectangle, followed by the width and height of the rectangle. Here, it is Rect2(100, 100, 50, 50).
- The second parameter is the color of the rectangle. Here, it is Color.BLUE, which is a built-in color in Godot.

## Sample Code

You now have all the information you need to play around with drawing in Godot. To get started:

- Download the sample code here: [Task 1: Draw to Screen](https://github.com/HeathmontGameDesign/LearningGodot/blob/main/1_Draw_to_screen/1_sample.gd)
- Create a new Scene in Godot with a Node2D as the root node
- Attach a new script to the Node2D and replace it with the sample code
- Run the scene to see what it does
- Following the comments in the code, add your own code to draw different shapes on the screen

## Challenge

Create a new Godot scene that draws a house. The house should have a roof, a door, and windows. Save your scene as `1_house.tscn`.

Main Page: [Heathmont Game Design](/) | Next task: [Task 2 - Sprites and Movement](lesson2.md)
