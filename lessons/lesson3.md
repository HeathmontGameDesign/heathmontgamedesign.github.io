# Lesson 3 - Collision Detection

In games, objects interact when they collide. In PyGame Zero, we can detect when two actors collide by checking if their bounding boxes overlap. This is called **collision detection**. In this lesson as well as using collision detection to make objects interact, we will also learn how to make use of randomness in our games.

## Randomness

In games, we often need to generate random numbers. For example, we might want to:

- Randomly place objects on the screen
- Randomly choose a direction for an enemy to move
- Randomly choose a question to ask the player
- Simulate rolling a dice, selecting a card, or some other random event

### Importing a package

The `random` package is built into Python, but it doesn't get imported automatically. To use it, we need to import it at the start of our program. We do this the same way we have with `pgzrun`. Here is an example:

```python

import random

...

random_number = random.randint(1, 6)

```

This code imports the `random` package, and then generates a random number between 1 and 6. The `randint()` function takes two arguments: the lowest number in the range, and the highest number in the range. We use `int` because an integer is a whole number (rather than a decimal).

## Collision Detection

In PyGame Zero, we can check if two actors are colliding by using the `colliderect()` function. This function checks if the bounding boxes of two actors overlap. Here is an example from the sample code:

```python

if heath.colliderect(coin):
        coin.x, coin.y = (random.randint(0, WIDTH), random.randint(0, HEIGHT))

```

This code checks if the bounding box of the `heath` actor overlaps with the bounding box of the `coin` actor. If they do overlap, the condition is true, so the `coin` actor is moved to a new random position on the screen. This means that Heath, the Heathmont Game Design Robot, can collect coins!

## Sample Code

You now have all the information you need to play around with collision detection and randomness in PyGame Zero. In the sample code, Heath is already able to collect coins, but he should not be able to touch fire! The fire actors are already in the game, but you need to add the collision detection code to make sure Heath doesn't touch them. You can decide what should happen when he does.

## Challenge

Create a bouncing ball game - a ball that bounces around the screen and changes direction when it hits the edge.

- Add a ball actor that moves in a random direction.
- When the ball hits the edge of the screen, it should bounce off in a new random direction.
- Add an obstacle of some kind (e.g. a brick, wall or fence) that the ball should bounce off when it collides with it.
- Add controls for the player to move the obstacle up and down.
