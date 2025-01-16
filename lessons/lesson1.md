---
title: "Lesson 1: Draw to Screen"
---

To start with PyGame Zero, you need to know how to work with the screen. The screen is where everything happens in your game. You can draw shapes, images, and text on the screen.

## Coordinates in PyGame Zero

PyGame Zero uses a coordinate system to decide where to draw things on the screen. The top-left corner is the origin (0, 0). The x-axis increases as you travel to the right, and the y-axis as you go down. This is different from the coordinate system you learned in maths, where the y-axis increases as you go up.

### Understanding Coordinates

[Coordinate System](/lessons/images/coordinates.png)

The image above shows how points on a screen become coordinates. The top-left corner is (0, 0). The magenta dot is at (300, 0). The blue dot is at (50, 150).

## Drawing Shapes

PyGame Zero has functions to draw shapes on the screen. Here is an example:

```python
def draw():
    screen.draw.circle((400, 300), 30, WHITE)
```

This code draws a circle. To fill the circle, use `screen.draw.filled_circle()`. The parameters are:

- The first parameter is the center of the circle, a tuple (x, y). Here, it is (400, 300).
- The second parameter is the radius of the circle. Here, it is 30 pixels.
- The third parameter is the color of the circle. Here, it is `WHITE`, which has been pre-defined in our code.

### Rectangles

Rectangles get treated slightly differently in PyGame Zero. To draw a rectangle, we create the rectangle and then draw it. The other difference is that the position of a rectangle is its top left corner, not its centre. Here is an example:

```python

def draw():
    screen.draw.rect(Rect(100, 100, 50, 50), BLUE)
```

This code draws a rectangle. The parameters are:

- The first parameter is a `Rect` object. This object is created by passing in the x and y coordinates of the top-left corner of the rectangle, followed by the width and height of the rectangle. Here, it is `Rect(100, 100, 50, 50)`.
- The second parameter is the color of the rectangle. Here, it is `BLUE`, which has been pre-defined in our code.

## Sample Code

You now have all the information you need to play around with drawing in PyGame Zero. To get started:

- Download the sample code here: [Lesson 1: Draw to Screen](https://github.com/HeathmontGameDesign/LearningPGZ/blob/main/1_Draw_to_screen/1_sample.py)
- Open the code in Mu and run it to see what it does.
- Underneath each "#TODO" comment, add your own code to draw different shapes on the screen.

## Challenge

Create a new PyGame Zero program that draws a house. The house should have a roof, a door, and windows. Name your program `house.py`.
