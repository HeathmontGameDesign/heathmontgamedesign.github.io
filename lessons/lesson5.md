# Lesson 5 - Arrays and Sounds

*Previous lesson: [Lesson 4 - Mouse Clicks](lesson4.md) | Main Page: [Heathmont Game Design](/index.md) | Next lesson: [Lesson 6 - Creating a Simple Pong Game](lesson6.md)*

Arrays are a way to store multiple values in a single variable. They are useful for keeping track of multiple items, such as sprites in a game. In this lesson, we will learn how to create and use arrays in Godot.

## What is an array?

- An array is a collection of items all stored in a single variable.
- Arrays can store any type of data, including numbers, strings, objects, and even other arrays.
- Arrays are created using square brackets [] and items are separated by commas.
- We can also append (add to the end of) an array using the `append()` method.
- Arrays are ordered, meaning that the items in an array have a specific order and can be accessed by their index (position in the array).
  - Arrays are zero-indexed, meaning that the first item in an array is at index 0, the second item is at index 1, and so on.

## Arrays of values

We can create an array of values by enclosing them in square brackets and separating them with commas. For example, we can create an array of numbers like this:

```gdscript
var numbers = [5, 12, 3, 14, 5]
```

In this example, we have created an array called `numbers` that contains five integers. We can access the items in the array using their index:

```gdscript
print(numbers[0])  # Output: 5
print(numbers[3])  # Output: 14
```

## Looping through an array

We can loop through an array using a `for` loop. This allows us to perform an action for each item in the array. For example, we can print each number in the `numbers` array like this:

```gdscript
for number in numbers:
    print(number)
```

- `numbers` is the name of the array we want to loop through.
- `number` is a variable that will take on the value of each item in the array as we loop through it.
- The `print(number)` statement will print the value of `number` for each cycle (iteration) of the loop.
- The loop will continue until it has gone through all the items in the array.

## Arrays of sprites

In Godot, the most common use of arrays is for a set of sprite nodes. This allows us to store multiple sprites in a single variable and perform actions on all of them at once. This is especially useful when we want to create multiple sprites of the same type, such as enemies or bullets.

```gdscript
extends Node2D

var enemies = []

func _ready():
    # Create an array of enemy sprites
    for i in range(5):
        var enemy = Sprite2D.new()  # Create a new Sprite2D
        enemy.texture = load("res://assets/enemy.png")  # Load the enemy texture
        enemy.position = Vector2(randf_range(0, 800), randf_range(0, 600))  # Set random position
        add_child(enemy)  # Add the enemy to the scene tree
        enemies.append(enemy)  # Add the enemy to the array
```

In this example, we create an empty array called `enemies` and then use a `for` loop to create five enemy sprites. Each enemy is given a random position on the screen and then added to the scene tree (so it can be drawn) and to the `enemies` array.

- `enemies.append(enemy)` adds the `enemy` sprite to the end of the `enemies` array.  
- We can then loop through the `enemies` array and perform actions on each enemy:

```gdscript
func _process(delta):
    for enemy in enemies:
        enemy.position.x += 1  # Move each enemy to the right
```

In this example, we loop through the `enemies` array and move each enemy sprite to the right by 1 pixel each frame.

## Sounds

Godot has built-in support for playing sounds. Sounds add interest to your game and can help players to notice key events. We can use AudioStreamPlayer nodes to load and play sound files. The sound files should be in your project's file system.

- Godot supports WAV, OGG, and MP3 sound files.
- We can play a sound by calling the `play()` method on an AudioStreamPlayer node.
- Sounds can be added to your scene as AudioStreamPlayer, AudioStreamPlayer2D, or AudioStreamPlayer3D nodes depending on whether you need positional audio.
- For simple sound effects, AudioStreamPlayer is usually sufficient.

```gdscript
extends Node2D

var audio_player

func _ready():
    # Create and configure an audio player
    audio_player = AudioStreamPlayer.new()
    audio_player.stream = load("res://sounds/explosion.ogg")
    add_child(audio_player)

func play_explosion_sound():
    audio_player.play()
```

Alternatively, you can add an AudioStreamPlayer node to your scene through the editor, load the sound file in the Inspector, and reference it in your script:

```gdscript
extends Node2D

@onready var explosion_sound = $ExplosionSound

func play_explosion():
    explosion_sound.play()
```

*Previous lesson: [Lesson 4 - Mouse Clicks](lesson4.md) | Main Page: [Heathmont Game Design](/index.md) | Next lesson: [Lesson 6 - Creating a Simple Pong Game](lesson6.md)*
