# Lesson 4 - Mouse Clicks

So far we have only used very basic keyboard input inside the `update()` function in PyGame Zero. In this lesson, we will learn how to use the mouse to interact with our games. We will learn how to detect when the mouse is clicked, and how to use the position of the mouse to interact with actors on the screen.

## Reminder: The Game Loop

In PyGame Zero (as with lots of game development frameworks), the game loop has three main parts:

1. **Update**: This is where we update the game state. We move actors, check for collisions, and update the score.
2. **Draw**: This is where we draw the game state to the screen. We draw actors, backgrounds, and any other visual elements.
3. **Events**: This is where we check for player input. We check for keyboard input, mouse input, and any other events that might happen in the game.

So far, we have only used the `update()` and `draw()` functions. In this lesson, we will learn how to use the `on_mouse_down()` function to detect when the mouse is clicked - which represents our first use of Events in the game loop.

## Mouse Clicks

In PyGame Zero, we can detect when the mouse is clicked by using the `on_mouse_down()` function. This function is called whenever the mouse button is pressed. Other functions like `on_mouse_up()` and `on_mouse_move()` are also available, but we will focus on `on_mouse_down()` for now.

The function takes two arguments: the `pos` (position) of the mouse (a tuple of `(x, y)` coordinates), and the `button` that was pressed (a number representing the button that was pressed).

## Our Sample Code

In the sample code, Heath is currently moving randomly across the screen. It looks a little bit crazy, but it will work for today's game. You need to click on Heath correctly, as often as you can. Lets take a look at what is happening at the moment.

```python
import pgzrun
import random

# Set the size of the screen
WIDTH = 800
HEIGHT = 600

TITLE = '4 - Mouse Clicks'

BGCOLOUR = (255, 200, 0)

HEATH_SPEED = 20
```

Not much relevant so far - but there is an important point here. HEATH_SPEED and BGCOLOUR are not built in to PyGame Zero, but we have written them in all caps. This is a convention to show that they are **constants** - values that we will not change during the game. Constants are often written in all caps to make them easy to spot in the code and they are especially useful when we need to use the same value in multiple places. *Any number that is used more than once in the code should be a constant.*

```python
# GLOBAL VARIABLES
# Create the heath actor and place it at a random position
heath = Actor('heath', (random.randint(0, WIDTH), random.randint(0, HEIGHT)))
# Create the cross actor and place it off the screen
cross = Actor('cross', (-50, -50))

score = 0
```

We have three **global variables** here. `heath` is the actor that we are trying to click on, `cross` is the actor that will appear when we click on `heath`, and `score` is the player's score. We will use these variables to keep track of the game state.

**Global variables** are variables that can be accessed from anywhere in the program. They are defined outside of any functions, so they are available to all functions in the program. Global variables are useful for storing information that needs to be shared between functions.

To use `score` in the `on_mouse_down()` and `draw()` functions, we need to declare it as a global variable inside those functions. We will do this later in the code. We don't need to do this for our actors, because they are objects in the game, so they are automatically available to all functions.

```python
def update():
    # Heath moves HEATH_SPEED pixels in a random direction
    move_x = random.choice([HEATH_SPEED, -HEATH_SPEED])
    move_y = random.choice([HEATH_SPEED, -HEATH_SPEED])
    heath.x += move_x
    heath.y += move_y

    # If Heath goes off the screen, move him back to somewhere random
    if heath.left < 0:
        heath.left = random.randint(0, WIDTH)
    if heath.right > WIDTH:
        heath.right = random.randint(0, WIDTH)
    if heath.top < 0:
        heath.top = random.randint(0, HEIGHT)
    if heath.bottom > HEIGHT:
        heath.bottom = random.randint(0, HEIGHT)
```

In the `update()` function, we move `heath` in a random direction. We use the `HEATH_SPEED` constant to determine how far `heath` moves each frame. Notice that this time around, the movement uses `random.choice()` rather than `random.randint()`. This is because we want him to move the full distance, rather than somewhere in betwene.

We also check if `heath` goes off the screen, and if he does, we move him back to a random position.

Note that we haven't included any of the code for the mouse clicks yet. We will add this in the `on_mouse_down()` function.

```python

def draw():
    # As above, score is being used as a global variable
    global score

    screen.fill(BGCOLOUR)
    heath.draw()
    cross.draw()
    # TODO: Draw the score

```

The draw() function looks familiar, with only the `global score` being new. We are also asked to add the score to the screen - you might want to look at [PyGame Zero's text formatting page](https://pygame-zero.readthedocs.io/en/stable/ptext.html) to see how to do this.

```python

def on_mouse_down(pos):
    # Set the position of the cross to the position of the mouse click
    cross.pos = pos
    
    # TODO: Add code to check for collision with Heath. If there is a collision, increase the score by 1
    # and change the cross image to a tick. If there is not a collision, decrease the score by 1 and
    # make the cross image a cross.
    
    # This schedules the "hide_cross" function to run in 0.5 seconds, so that the cross doesn't stay on the screen.
    clock.schedule_unique(hide_cross, 0.5)

# hide_cross moves the cross off the screen
def hide_cross():
    cross.pos = (-50, -50)

pgzrun.go()
```

The on_mouse_down() function is already moving the cross to the position of the mouse click. It does this by using the `pos` argument from the function, which gives us the `(x, y)` coordinates of the mouse click.

The `TODO` comment is asking you to add code to check for a collision between `heath` and `cross`. If there is a collision, you need to increase the score by 1 (we have already seen how to add to an existing variable) and change the image of `cross` to a tick. If there is not a collision, you need to decrease the score by 1 and change the image of `cross` Actor to a cross (in case it was a tick).

The other important part is the `clock.schedule_unique()` function. This function schedules another function to run after a certain amount of time. In this case, we are scheduling the `hide_cross()` function to run after 0.5 seconds. This is so that the cross doesn't stay on the screen after we click on `heath`.

## Your Task

Complete the TODO comments in the sample code:
  
- You will need to use the `colliderect()` function to check for a collision between `heath` and `cross`. If you are not sure how to do this, look back at the collision detection code from the previous lesson.
- You will need to increase or decrease the `score` variable based on whether there is a collision or not with the mouse position.
- You will need to change the image of the `cross` Actor to a tick or a cross based on whether there is a collision or not.
- Add the score to the screen. You can use the `screen.draw.text()` function to do this. You will need to convert the `score` variable to a string using the `str()` function.
- Test your game by running it and clicking on `heath` as often as you can. Make sure the score increases when you click on `heath`, and decreases when you don't.

## Challenge

- Add a timer to the game. The timer should start at 30 seconds and count down to 0. When the timer reaches 0, the game should end. You can use the `clock.schedule_interval()` function to run a function every second to update the timer. You will need to add a new global variable to store the timer value, and you will need to add code to draw the timer on the screen.
- Add a game over screen that displays the final score when the timer reaches 0. You can use a new global variable to keep track of whether the game is over or not. When the timer reaches 0, set this variable to `True` and display the game over screen. You can use the `screen.draw.text()` function to display the final score on the game over screen.
- Add a restart button to the game over screen that allows the player to restart the game. You can use the `on_mouse_down()` function to check for a click on the restart button, and reset the game state if the button is clicked.
