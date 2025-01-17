# Lesson 2: Actors and Movement

Drawings are nice and all, but a game requires interactivity and movement. In PyGame Zero, we use **actors** to represent moving objects in our game. Actors can be:

- a player character
- an enemy
- a bullet
- an obstacle
- or any other object that incorporates interactivity.

Actors are variables that store information about the object, such as its position, size, and image. This information gets used to draw the actor on the screen and to move it around. Just because an object is an actor doesn't mean it has to move, or even be visible. It just means that it can be.

## Creating an Actor

To create an actor, we use the `Actor` class. Here is an example:

```python
player = Actor('player', (400, 300))
```

`player` is the name of the variable that stores the actor. `'player'` is the name of the image file that represents the actor. `(400, 300)` is the starting position of the actor on the screen. This code goes at the start of the program.

## Drawing an Actor

To draw an actor on the screen, we call the `actor.draw()` function, inside our game's draw function. That is a little bit confusing, but here is an example:

```python

def draw():
    player.draw()
```

This code tells PyGame Zero to draw the `player` actor on the screen. As with most programming, each line is called sequentially, so if we have multiple actors, the last one drawn will appear on top.

## The `update()` Function

The `update()` function is called every frame of the game. This is where we put code that changes the game state, such as moving actors.

## Responding to Input - using if statements

In later lessons we will use event-based functions to respond to input, but for now we will check for keyboard input in the `update()` function. We can use `if` statements to check if a key is pressed. Here is an example:

```python

def update():
    if keyboard.left:
        player.x = player.x - 2
    if keyboard.right:
        player.x = player.x + 2
```

This code checks if the left or right arrow keys are pressed, and moves the player actor left or right accordingly. It changes the `x` position of the player actor by 2 pixels (down to go left, up to go right). 

## Sample Code

You now have all the information you need to play around with actors and movement in PyGame Zero. It is also your first introduction to Heath, the Heathmont Game Design Robot!

![Heath](/assets/images/heath.png)

To get started:

- Download the sample code here: [Lesson 2: Actors and Movement](https://github.com/HeathmontGameDesign/LearningPGZ/tree/main/2_Actors_and_Movement)
  - Download the whole folder as a zip file and extract it to your computer. This keeps the images folder in the correct place.
- Open the code in Mu and run it to see what it does.
- Underneath each "#TODO" comment, add your own code to:
  - allow Heath to move up and down
  - add a ghost actor that moves slowly across the screen

## Challenge

Create a copy of your completed sample code and modify it so that:

- Once the ghost actor reaches the right side of the screen, it reappears on the left side.
- Include a second ghost actor that moves up and down. Have the second ghost go the other way when it reaches the top or bottom of the screen.

Name your program `2_ghost.py`
