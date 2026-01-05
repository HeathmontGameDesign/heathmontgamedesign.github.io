# Lesson 3 - Collision Detection

In games, objects interact when they collide. In Godot, we can detect when two objects collide using Area2D nodes and collision signals. This is called **collision detection**. In this lesson as well as using collision detection to make objects interact, we will also learn how to make use of randomness in our games.

## Randomness

In games, we often need to generate random numbers. For example, we might want to:

- Randomly place objects on the screen
- Randomly choose a direction for an enemy to move
- Randomly choose a question to ask the player
- Simulate rolling a dice, selecting a card, or some other random event

### Using RandomNumberGenerator

Godot has a built-in `RandomNumberGenerator` class that we can use to generate random numbers. Here is an example:

```gdscript
extends Node2D

var rng = RandomNumberGenerator.new()

func _ready():
    rng.randomize()  # Initialize with a random seed
    var random_number = rng.randi_range(1, 6)  # Random number between 1 and 6
    print(random_number)
```

This code creates a new RandomNumberGenerator, initializes it with a random seed (so we get different results each time), and then generates a random number between 1 and 6. The `randi_range()` function takes two arguments: the lowest number in the range, and the highest number in the range.

For simple random needs, you can also use the global functions like `randf()` for random floats or `randi()` for random integers.

## Collision Detection

In Godot, we typically use Area2D nodes for detecting collisions. An Area2D can detect when other Area2D or PhysicsBody2D nodes enter or exit its collision shape. Here is an example from the sample code:

```gdscript
extends Area2D

func _ready():
    area_entered.connect(_on_area_entered)

func _on_area_entered(area):
    if area.is_in_group("coins"):
        # Move coin to random position
        area.position = Vector2(randf_range(0, 800), randf_range(0, 600))
```

This code connects to the "area_entered" signal, which is emitted when another Area2D enters this one's collision space. If the colliding area is in the "coins" group, it moves the coin to a new random position. This means that Heath, the Heathmont Game Design Robot, can collect coins!

To set up collision detection:
1. Add a CollisionShape2D child to your Area2D
2. Select a shape for the CollisionShape2D (e.g., RectangleShape2D or CircleShape2D)
3. Adjust the shape to match your sprite
4. Connect the appropriate signals in your script

## Sample Code

You now have all the information you need to play around with collision detection and randomness in Godot. In the sample code, Heath is already able to collect coins, but he should not be able to touch fire! The fire areas are already in the game, but you need to add the collision detection code to make sure Heath doesn't touch them. You can decide what should happen when he does.

## Challenge

Create a bouncing ball game - a ball that bounces around the screen and changes direction when it hits the edge.

- Add a ball sprite that moves in a random direction.
- When the ball hits the edge of the screen, it should bounce off in a new random direction.
- Add an obstacle of some kind (e.g. a brick, wall or fence) that the ball should bounce off when it collides with it.
- Add controls for the player to move the obstacle up and down.
