# Lesson 6: Creating a Simple Pong Game

Pong is one of the earliest video games and a perfect project for learning game development fundamentals. In this lesson, you'll create a complete Pong game from scratch, bringing together everything you've learned about sprites, movement, collision detection, and game logic.

## What is Pong?

Pong is a two-player game that simulates table tennis. Each player controls a paddle on opposite sides of the screen. A ball bounces back and forth between them, and players must hit the ball with their paddle to keep it in play. If a player misses, the opponent scores a point.

## Project Overview

Our Pong game will include:
- A player-controlled paddle (left side)
- An AI-controlled paddle (right side)
- A ball that bounces between paddles and walls
- Score tracking for both players
- Game reset when a point is scored

## Setting Up the Project

### Step 1: Create a New Scene

1. Create a new scene in Godot with a `Node2D` as the root node
2. Rename the root node to "Game"
3. Save the scene as `pong.tscn`

### Step 2: Create the Ball

The ball is the centerpiece of our game. We'll use an `Area2D` node with a sprite and collision shape.

1. Add an `Area2D` node as a child of Game and name it "Ball"
2. Add a `Sprite2D` child to Ball
3. Add a `CollisionShape2D` child to Ball
4. For the Sprite2D texture, you can:
   - Use a simple white circle image
   - Or draw a circle using a script with the `_draw()` function and `draw_circle()` method
5. Select the CollisionShape2D and set its shape to a CircleShape2D (in the Inspector, under Shape, choose "New CircleShape2D")
6. Adjust the radius to match your sprite (around 10-15 pixels works well)

Attach a script to the Ball node (right-click Ball node > Attach Script > Create). This script will be saved as `ball.gd` in your project folder:

```gdscript
extends Area2D
# This script controls the ball's movement and bouncing behavior.
# It should be attached directly to the Ball (Area2D) node in your scene tree.

# Ball velocity stores both speed and direction as a 2D vector.
# Positive x moves right, negative x moves left.
# Positive y moves down, negative y moves up.
var velocity = Vector2(300, 200)

# Screen boundaries - these should match your project window size.
# You can set this in Project Settings > Display > Window.
const SCREEN_WIDTH = 800
const SCREEN_HEIGHT = 600

func _ready():
	# _ready() is called once when the node enters the scene tree.
	# Position the ball in the center of the screen at start.
	position = Vector2(SCREEN_WIDTH / 2, SCREEN_HEIGHT / 2)

func _process(delta):
	# _process() is called every frame. Delta is the time elapsed since last frame.
	# Move the ball by adding velocity multiplied by delta time.
	# Using delta ensures movement is smooth regardless of frame rate.
	position += velocity * delta
	
	# Check if ball hits top or bottom walls and reverse vertical direction.
	# position.y <= 0 means ball hit the top.
	# position.y >= SCREEN_HEIGHT means ball hit the bottom.
	if position.y <= 0 or position.y >= SCREEN_HEIGHT:
		velocity.y = -velocity.y  # Reverse vertical direction (bounce)
```

**What this code does:** This script makes the ball move continuously across the screen and bounce when it hits the top or bottom edges. The ball starts in the center and moves at an angle determined by its velocity vector. The `delta` parameter ensures smooth movement regardless of frame rate - this is a Godot best practice for frame-rate independent movement.

**Where it lives:** This script is attached to the Ball node in your scene. When you run the game, you'll see the ball moving and bouncing off the top and bottom walls, but it will pass through the left and right sides (we'll fix that with scoring later).

### Step 3: Create the Player Paddle

Now let's create a paddle that the player can control with the keyboard.

1. Add an `Area2D` node as a child of Game and name it "PlayerPaddle"
2. Add a `Sprite2D` child to PlayerPaddle
3. Add a `CollisionShape2D` child to PlayerPaddle
4. For the Sprite2D, use a white rectangle image (you can create one in any image editor, roughly 20x100 pixels)
5. Set the CollisionShape2D to use a RectangleShape2D that matches your sprite size

Attach a script to the PlayerPaddle node (right-click PlayerPaddle > Attach Script > Create). This script will be saved as `player_paddle.gd`:

```gdscript
extends Area2D
# This script controls the player's paddle movement using keyboard input.
# It should be attached to the PlayerPaddle (Area2D) node in your scene tree.

# Movement speed in pixels per second.
# Higher values make the paddle move faster.
const SPEED = 400

# Screen boundaries - these should match your project settings.
const SCREEN_HEIGHT = 600
# Adjust PADDLE_HEIGHT to match your actual paddle sprite height.
# To find your sprite height: Select the Sprite2D node and check the Texture 
# section in the Inspector. Look at the image dimensions (e.g., 20x100 means 100 pixels tall).
const PADDLE_HEIGHT = 100

func _ready():
	# _ready() is called once when the node enters the scene.
	# Position paddle on the left side of screen, centered vertically.
	# x = 50 puts it near the left edge, y = SCREEN_HEIGHT / 2 centers it.
	position = Vector2(50, SCREEN_HEIGHT / 2)

func _process(delta):
	# _process() is called every frame.
	# Check for arrow key input and move paddle accordingly.
	
	# If up arrow is pressed, move paddle up (decrease y position).
	if Input.is_action_pressed("ui_up"):
		position.y -= SPEED * delta
	# If down arrow is pressed, move paddle down (increase y position).
	if Input.is_action_pressed("ui_down"):
		position.y += SPEED * delta
	
	# Keep paddle within screen boundaries using clamp().
	# clamp() restricts a value between a minimum and maximum.
	# This prevents the paddle from moving off the top or bottom of the screen.
	position.y = clamp(position.y, PADDLE_HEIGHT / 2, SCREEN_HEIGHT - PADDLE_HEIGHT / 2)
```

**What this code does:** This script allows the player to control the left paddle using the up and down arrow keys. The paddle moves at a constant speed (400 pixels per second) and is prevented from moving off-screen by the `clamp()` function. The `clamp()` function is a built-in Godot function that keeps the paddle position within bounds - a common best practice for boundary checking.

**Where it lives:** This script is attached to the PlayerPaddle node in your scene. The paddle will be positioned on the left side of the screen and respond to your keyboard input in real-time when you run the game.

### Step 4: Create the AI Paddle

The AI paddle will automatically follow the ball's position.

1. Duplicate the PlayerPaddle node (right-click > Duplicate)
2. Rename it to "AIPaddle"
3. In the Inspector, change its position to the right side (around x=750)

Replace the script on AIPaddle with a new script (right-click AIPaddle > Attach Script > Replace existing). This script will be saved as `ai_paddle.gd`:

```gdscript
extends Area2D
# This script controls the AI opponent's paddle.
# The AI automatically tracks and follows the ball's vertical position.
# It should be attached to the AIPaddle (Area2D) node in your scene tree.

# AI paddle speed - intentionally slightly slower than player for game balance.
# This gives the player a fair chance to win.
const SPEED = 350

# Screen boundaries - should match project settings.
const SCREEN_HEIGHT = 600
# Adjust PADDLE_HEIGHT to match your paddle sprite height.
# Check the Sprite2D's texture dimensions in the Inspector to find the correct height.
const PADDLE_HEIGHT = 100

# Reference to the ball node - we need this to track its position.
var ball

func _ready():
	# _ready() is called once when the node enters the scene.
	# Position paddle on the right side of screen, centered vertically.
	# x = 750 puts it near the right edge, y = SCREEN_HEIGHT / 2 centers it.
	position = Vector2(750, SCREEN_HEIGHT / 2)
	
	# Get reference to the ball node using get_parent() to access the Game node,
	# then get_node() to find the Ball child node.
	# This allows the AI to track where the ball is at all times.
	# Note: This assumes your scene structure has Ball as a sibling node.
	# If you renamed your Ball node, update "Ball" to match the new name.
	ball = get_parent().get_node("Ball")

func _process(delta):
	# _process() is called every frame.
	# Only proceed if we successfully got a reference to the ball.
	if ball:
		# Simple AI logic: move paddle toward ball's y position.
		# If paddle is above the ball, move down.
		if position.y < ball.position.y:
			position.y += SPEED * delta
		# If paddle is below the ball, move up.
		elif position.y > ball.position.y:
			position.y -= SPEED * delta
		
		# Keep paddle within screen boundaries using clamp().
		# This prevents the paddle from moving off the top or bottom.
		position.y = clamp(position.y, PADDLE_HEIGHT / 2, SCREEN_HEIGHT - PADDLE_HEIGHT / 2)
```

**What this code does:** This script creates a simple but effective AI opponent that automatically moves toward the ball's vertical position. It gets a reference to the ball using `get_parent().get_node()` - a Godot best practice for accessing sibling nodes. The AI is intentionally slightly slower than the player (350 vs 400 speed) to make the game balanced and winnable.

**Where it lives:** This script is attached to the AIPaddle node on the right side of your scene. When you run the game, you'll see the AI paddle automatically track and try to intercept the ball.

### Step 5: Implement Ball-Paddle Collision

Now we need to make the ball bounce off the paddles. Go back to the Ball node and update its script by adding collision detection. Replace the entire Ball script with this enhanced version:

```gdscript
extends Area2D
# This is the enhanced Ball script with collision detection.
# It should replace the earlier ball.gd script on the Ball (Area2D) node.

# Ball velocity stores both speed and direction.
var velocity = Vector2(300, 200)

# Screen boundaries.
const SCREEN_WIDTH = 800
const SCREEN_HEIGHT = 600

func _ready():
	# Position ball in center at start.
	position = Vector2(SCREEN_WIDTH / 2, SCREEN_HEIGHT / 2)
	
	# Connect the area_entered signal to our collision handler function.
	# This signal is emitted whenever another Area2D enters this one's collision space.
	# This is a Godot best practice for event-driven collision detection.
	area_entered.connect(_on_area_entered)

func _process(delta):
	# Move the ball every frame using delta time.
	position += velocity * delta
	
	# Bounce off top and bottom walls.
	if position.y <= 0 or position.y >= SCREEN_HEIGHT:
		velocity.y = -velocity.y

func _on_area_entered(area):
	# This function is called automatically when the ball collides with another Area2D.
	# The 'area' parameter is the node that the ball collided with.
	
	# Check if the colliding node is one of the paddles by checking its name.
	if area.name == "PlayerPaddle" or area.name == "AIPaddle":
		# Reverse the horizontal direction to make ball bounce back.
		velocity.x = -velocity.x
		
		# Add realistic physics: hitting different parts of paddle affects ball angle.
		# Calculate where on the paddle the ball hit.
		var paddle_center = area.position.y
		var hit_offset = position.y - paddle_center
		# Multiply by 2 to create noticeable angle changes.
		# If ball hits top of paddle (negative offset), it goes upward more.
		# If ball hits bottom of paddle (positive offset), it goes downward more.
		velocity.y += hit_offset * 2
```

**What this code does:** This updated Ball script adds collision detection using signals. The `area_entered.connect()` line sets up an event listener that calls `_on_area_entered()` whenever the ball touches a paddle. Using signals like `area_entered` is a Godot best practice for event-driven programming - it makes the code cleaner and more maintainable. The hit offset calculation adds realistic physics: hitting the edge of the paddle sends the ball at a steeper angle, while hitting the center sends it straighter.

**Where it lives:** This replaces the earlier ball.gd script on the Ball node. When you run the game now, the ball will bounce off both paddles, not just the walls!

### Step 6: Add Scoring System

Let's add score tracking. Attach a new script to the Game node (the root Node2D). This script will be saved as `game.gd`:

```gdscript
extends Node2D
# This is the main game controller script.
# It manages scoring and coordinates the other game objects.
# It should be attached to the Game (Node2D) root node in your scene tree.

# Score variables - track points for each player.
# These start at 0 and increment when the opponent misses the ball.
var player_score = 0
var ai_score = 0

# References to child nodes - we'll use these to access and control game objects.
var ball
var player_paddle
var ai_paddle

func _ready():
	# _ready() is called once when the scene starts.
	# Get references to our game object nodes using $ shorthand.
	# $ is short for get_node() - a Godot best practice for cleaner code.
	ball = $Ball
	player_paddle = $PlayerPaddle
	ai_paddle = $AIPaddle

func _process(_delta):
	# _process() is called every frame.
	# Note: we use _delta with underscore prefix because we don't use the delta parameter.
	# This prevents Godot from showing a warning about unused parameters.
	
	# Check if ball went off the left side of screen (player missed).
	# If so, AI scores a point.
	if ball.position.x < 0:
		ai_score += 1  # Increment AI score
		print("AI Score: ", ai_score)  # Print to console for debugging
		reset_ball()  # Reset ball to center for next round
	
	# Check if ball went off the right side of screen (AI missed).
	# If so, player scores a point.
	if ball.position.x > 800:
		player_score += 1  # Increment player score
		print("Player Score: ", player_score)  # Print to console
		reset_ball()  # Reset ball to center for next round

func reset_ball():
	# This function resets the ball to the center after someone scores.
	# Reset ball position to center of screen.
	ball.position = Vector2(400, 300)
	# Reverse horizontal direction so ball goes toward the player who just scored.
	# This is fair: if you scored, you have to return the ball.
	ball.velocity.x = -ball.velocity.x
	# Randomize vertical velocity to add variety to each serve.
	# randf_range() generates a random float between -200 and 200.
	ball.velocity.y = randf_range(-200, 200)
```

**What this code does:** This is the main game controller that manages scoring logic. It watches the ball's position every frame and checks if it went off either side of the screen. When the ball goes off the left, the AI scores; when it goes off the right, the player scores. The `$` syntax is a Godot shorthand for `get_node()` - another best practice for cleaner code. Using `randf_range()` adds variety to each serve by randomizing the ball's vertical direction.

**Where it lives:** This script is attached to the Game (root Node2D) node. It sits at the top of the scene hierarchy and coordinates all the game objects below it. When you run the game, you'll see scores printed to the console (at the bottom of the Godot editor) whenever someone scores.

### Step 7: Add Visual Score Display

For a better user experience, let's add on-screen score display using Labels. First, set up the UI nodes:

1. Add a `CanvasLayer` node as a child of Game (this keeps UI elements on top)
2. Add a `Label` node as a child of CanvasLayer and name it "PlayerScoreLabel"
3. Add another `Label` node as a child of CanvasLayer and name it "AIScoreLabel"
4. Position PlayerScoreLabel in the top-left (around x=100, y=50)
5. Position AIScoreLabel in the top-right (around x=700, y=50)
6. In the Inspector for both labels:
   - Increase font size (Theme Overrides > Font Sizes > Font Size: 48)
   - Center the text alignment if desired

Now update the Game script (game.gd) to update the labels. Replace the entire script with this enhanced version:

```gdscript
extends Node2D
# This is the enhanced game controller with visual score display.
# It should replace the earlier game.gd script on the Game (Node2D) root node.

# Score variables.
var player_score = 0
var ai_score = 0

# References to game objects.
var ball
var player_paddle
var ai_paddle
# New references to the score label UI elements.
var player_score_label
var ai_score_label

func _ready():
	# Get references to all game objects and UI elements.
	ball = $Ball
	player_paddle = $PlayerPaddle
	ai_paddle = $AIPaddle
	# Access labels through the CanvasLayer using path notation.
	# The path goes: CanvasLayer (child of Game) -> PlayerScoreLabel (child of CanvasLayer)
	player_score_label = $CanvasLayer/PlayerScoreLabel
	ai_score_label = $CanvasLayer/AIScoreLabel
	
	# Initialize score display to show "0" at game start.
	update_score_display()

func _process(_delta):
	# Check if ball went off left side (AI scores).
	if ball.position.x < 0:
		ai_score += 1
		reset_ball()
		update_score_display()  # Update UI to show new score
	
	# Check if ball went off right side (Player scores).
	if ball.position.x > 800:
		player_score += 1
		reset_ball()
		update_score_display()  # Update UI to show new score

func update_score_display():
	# This function updates the text of the score labels.
	# str() converts the integer score to a string for display.
	# Separating this into its own function is good practice - 
	# it makes the code more modular and easier to maintain.
	player_score_label.text = str(player_score)
	ai_score_label.text = str(ai_score)

func reset_ball():
	# Reset ball position and randomize direction.
	ball.position = Vector2(400, 300)
	ball.velocity.x = -ball.velocity.x
	ball.velocity.y = randf_range(-200, 200)
```

**What this code does:** This enhanced version of the game controller adds visual score display. It gets references to the Label nodes and updates their text whenever someone scores. The `update_score_display()` function uses `str()` to convert the integer score to a string that can be displayed in the label. Separating the score update into its own function is good practice - it makes the code more modular and easier to maintain.

**Where it lives:** This replaces the earlier game.gd script on the Game node. Your scene hierarchy should now look like:
- Game (Node2D) - has game.gd script
  - Ball (Area2D) - has ball.gd script
  - PlayerPaddle (Area2D) - has player_paddle.gd script
  - AIPaddle (Area2D) - has ai_paddle.gd script
  - CanvasLayer
    - PlayerScoreLabel (Label)
    - AIScoreLabel (Label)

When you run the game, you'll now see the scores displayed on screen instead of just in the console!

### Step 8: Add Game Over Condition

Let's make the game end when someone reaches 5 points and allow restarting. Replace the entire Game script one final time with this complete version:

```gdscript
extends Node2D
# This is the complete game controller with game over logic and restart.
# This is the final version of game.gd for the Game (Node2D) root node.

# Score variables.
var player_score = 0
var ai_score = 0
# Game state flag - tracks whether the game has ended.
var game_over = false

# Constant for winning score - using const is a Godot best practice.
# This makes it easy to change the winning condition later.
const WINNING_SCORE = 5

# References to all game objects and UI elements.
var ball
var player_paddle
var ai_paddle
var player_score_label
var ai_score_label

func _ready():
	# Get references to all nodes.
	ball = $Ball
	player_paddle = $PlayerPaddle
	ai_paddle = $AIPaddle
	player_score_label = $CanvasLayer/PlayerScoreLabel
	ai_score_label = $CanvasLayer/AIScoreLabel
	# Initialize display to show starting scores (0 and 0).
	update_score_display()

func _process(_delta):
	# If game is over, only check for restart input.
	if game_over:
		# Check if player pressed Space/Enter (ui_accept action).
		# is_action_just_pressed() only triggers once per keypress.
		if Input.is_action_just_pressed("ui_accept"):
			restart_game()
		# Return early to skip the rest of _process().
		# This stops the game from continuing after game over.
		return
	
	# Game logic only runs if game is not over.
	# Check if ball went off left side (AI scores).
	if ball.position.x < 0:
		ai_score += 1
		check_game_over()  # Check if AI reached winning score
		if not game_over:
			reset_ball()
			update_score_display()
	
	# Check if ball went off right side (Player scores).
	if ball.position.x > 800:
		player_score += 1
		check_game_over()  # Check if player reached winning score
		if not game_over:
			reset_ball()
			update_score_display()

func check_game_over():
	# This function checks if either player has reached the winning score.
	if player_score >= WINNING_SCORE:
		game_over = true
		# Modify the label to show victory message.
		player_score_label.text = str(player_score) + " - YOU WIN!"
	elif ai_score >= WINNING_SCORE:
		game_over = true
		# Modify the label to show defeat message.
		ai_score_label.text = str(ai_score) + " - AI WINS!"
	
	# If game is over, stop the ball from moving.
	if game_over:
		# Vector2.ZERO is a built-in constant equal to Vector2(0, 0).
		# Using built-in constants is a Godot best practice.
		ball.velocity = Vector2.ZERO

func restart_game():
	# This function resets everything to start a new game.
	player_score = 0
	ai_score = 0
	game_over = false
	# Update display to clear the win/lose messages.
	update_score_display()
	# Reset ball position and velocity.
	reset_ball()
	ball.velocity = Vector2(300, 200)

func update_score_display():
	# Update the score labels with current scores.
	player_score_label.text = str(player_score)
	ai_score_label.text = str(ai_score)

func reset_ball():
	# Reset ball to center with randomized direction.
	ball.position = Vector2(400, 300)
	ball.velocity.x = -ball.velocity.x
	ball.velocity.y = randf_range(-200, 200)
```

**What this code does:** This complete version adds a game over condition and restart functionality. It uses a `game_over` boolean flag to track the game state. When someone reaches 5 points, the game stops processing and displays a win/lose message. The `return` statement in `_process()` when game is over prevents further processing, which is more efficient. Using `Vector2.ZERO` is cleaner than writing `Vector2(0, 0)` - embracing Godot's built-in constants is a best practice. Using `const WINNING_SCORE` makes it easy to adjust game difficulty later without hunting through the code.

**Where it lives:** This is the final, complete version of game.gd on the Game node. It replaces all previous versions. When you run the game now, you'll have a fully functional Pong game with scoring, game over detection, and the ability to restart by pressing Space or Enter!

## Godot Best Practices Used

Throughout this tutorial, we've followed several Godot best practices:

1. **Using `delta` for movement**: Ensures smooth, frame-rate independent movement
2. **Signals for collision detection**: Event-driven architecture using `area_entered`
3. **Node references**: Using `$` shorthand and `get_node()` for clean code
4. **Clamp for boundaries**: Built-in function for keeping values within range
5. **Separating concerns**: Different nodes for different game objects
6. **Constants for magic numbers**: Using `const` for values like speeds and screen size
7. **Built-in constants**: Using `Vector2.ZERO` instead of creating new vectors
8. **CanvasLayer for UI**: Keeps UI elements separate from game objects
9. **Modular functions**: Breaking code into smaller, reusable functions

## Sample Code

Complete sample code for this project can be found here: [Lesson 6: Pong Game](https://github.com/HeathmontGameDesign/LearningGodot/tree/main/6_Pong_Game)

The repository includes:
- Complete project files
- Sprite assets for the ball and paddles
- A README with setup instructions

## Testing Your Game

To test your Pong game:

1. Press F5 or click the Play button to run the scene
2. Use the Up and Down arrow keys to control the left paddle
3. Try to score against the AI opponent
4. The first to 5 points wins
5. Press Space (ui_accept) to restart after game over

## Challenges

Once you have a working Pong game, try these enhancements:

### Challenge 1: Add Sound Effects
Add sound effects when:
- The ball hits a paddle
- The ball hits a wall
- A player scores
- The game ends

Look back at Lesson 5 for information on adding sounds to your game.

### Challenge 2: Increase Difficulty
Make the game progressively harder:
- Increase ball speed each time it hits a paddle
- Make the AI paddle faster as the game progresses
- Add a maximum speed limit so it doesn't get too fast

### Challenge 3: Two-Player Mode
Modify the game to allow two human players:
- Change the AI paddle to respond to different keys (like W and S)
- Add a menu to choose between 1-player and 2-player mode

### Challenge 4: Visual Polish
Enhance the game's appearance:
- Add a dotted center line
- Create a trail effect behind the ball
- Add particle effects when the ball hits something
- Change colors based on who's winning

### Challenge 5: Power-ups
Add power-ups that randomly appear:
- Paddle size increase/decrease
- Ball speed modifications
- Multi-ball mode (multiple balls at once)

## Further Learning

This Pong game demonstrates fundamental game development concepts:
- Game loops and delta time
- Player input handling
- AI opponent logic
- Collision detection
- Score tracking and game states
- UI updates

These concepts apply to many other types of games. Experiment with modifying the game mechanics to create your own variations!
