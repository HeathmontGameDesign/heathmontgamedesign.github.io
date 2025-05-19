# Lesson 5 - Lists and Sounds

Lists are a way to store multiple values in a single variable. They are useful for keeping track of multiple items, such as actors in a game. In this lesson, we will learn how to create and use lists in Pygame Zero.

## What is a list?

- A list is a collection of items all stored in a single variable.
- Lists can store any type of data, including numbers, strings, and even other lists.
- Lists are created using square brackets [] and items are separated by commas.
- We can also append (add to the end of) a list using the `append()` method.
- Lists are ordered, meaning that the items in a list have a specific order and can be accessed by their index (position in the list).
  - Lists are zero-indexed, meaning that the first item in a list is at index 0, the second item is at index 1, and so on.

## Lists of values

We can create a list of values by enclosing them in square brackets and separating them with commas. For example, we can create a list of numbers like this:

```python

numbers = [5, 12, 3, 14, 5]

```

In this example, we have created a list called `numbers` that contains five integers. We can access the items in the list using their index:

```python

print(numbers[0])  # Output: 5
print(numbers[3])  # Output: 14

```

## Looping through a list

We can loop through a list using a `for` loop. This allows us to perform an action for each item in the list. For example, we can print each number in the `numbers` list like this:

```python

for number in numbers:
    print(number)

```

- `numbers` is the name of the list we want to loop through.
- `number` is a variable that will take on the value of each item in the list as we loop through it.
- The `print(number)` statement will print the value of `number` for each cycle (iteration) of the loop.
- The loop will continue until it has gone through all the items in the list.

## Lists of actors

In Pygame Zero, the most common use of lists is for a set of actors. This allows us to store multiple actors in a single variable and perform actions on all of them at once. This is especially useful when I want to create multiple actors of the same type, such as enemies or bullets.

```python

# Create a list of actors
enemies = []
for i in range(5):
    enemy = Actor('enemy') # Create a new enemy actor
    enemy.pos = random.randint(0, WIDTH), random.randint(0, HEIGHT) # Set a random position for the enemy
    enemies.append(enemy) # Add the enemy to the list
    
```

In this example, we create an empty list called `enemies` and then use a `for` loop to create five enemy actors. Each enemy is given a random position on the screen and then added to the `enemies` list using the `append()` method.

- `enemies.append(enemy)` adds the `enemy` actor to the end of the `enemies` list.  
- We can then loop through the `enemies` list and draw each enemy on the screen:

```python
def draw():
    screen.fill(BGCOLOUR) # Clear the screen
    for enemy in enemies: # Loop through the list of enemies
        enemy.draw() # Draw each enemy on the screen
```

In this example, we loop through the `enemies` list and call the `draw()` method for each enemy actor. This will draw all the enemies on the screen at their respective positions.

## Sounds

Pygame Zero has built-in support for playing sounds. Sounds add interest to your game and can help players to notice key events. We can use the `sounds` module to load and play sound files. The sound files should be in the same directory as our Python file or in a subdirectory called `sounds`.

- Pygame Zero supports WAV and OGG sound files.
- We can play a sound using the `play()` method.
- Sounds need to be stored in the `sounds` folder in the same directory as our Python file.
  - They need to be all lowercase and have the `.wav` or `.ogg` file extension.
- Sounds are designed for short sound effects, not for long music tracks. If you try to use long music tracks they will lag and cause problems.

```python

sounds.explosion.play() # Play the sound called explosion.wav (or explosion.ogg)

```
