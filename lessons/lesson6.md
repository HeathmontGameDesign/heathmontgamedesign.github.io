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
   - Or use Godot's built-in shapes by creating a new CircleShape2D and drawing it
5. Select the CollisionShape2D and set its shape to a CircleShape2D
6. Adjust the radius to match your sprite (around 10-15 pixels works well)

Attach a script to the Ball node:

```gdscript
extends Area2D

# Ball velocity (speed and direction)
var velocity = Vector2(300, 200)

# Screen boundaries
const SCREEN_WIDTH = 800
const SCREEN_HEIGHT = 600

func _ready():
	# Position ball in center
	position = Vector2(SCREEN_WIDTH / 2, SCREEN_HEIGHT / 2)

func _process(delta):
	# Move the ball
	position += velocity * delta
	
	# Bounce off top and bottom walls
	if position.y <= 0 or position.y >= SCREEN_HEIGHT:
		velocity.y = -velocity.y
```

This code gives the ball a starting velocity and makes it bounce off the top and bottom edges of the screen. The `delta` parameter ensures smooth movement regardless of frame rate - this is a Godot best practice for frame-rate independent movement.

### Step 3: Create the Player Paddle

Now let's create a paddle that the player can control with the keyboard.

1. Add an `Area2D` node as a child of Game and name it "PlayerPaddle"
2. Add a `Sprite2D` child to PlayerPaddle
3. Add a `CollisionShape2D` child to PlayerPaddle
4. For the Sprite2D, use a white rectangle image (you can create one in any image editor, roughly 20x100 pixels)
5. Set the CollisionShape2D to use a RectangleShape2D that matches your sprite size

Attach a script to the PlayerPaddle:

```gdscript
extends Area2D

# Paddle speed
const SPEED = 400

# Screen boundaries
const SCREEN_HEIGHT = 600
const PADDLE_HEIGHT = 100  # Adjust to match your paddle sprite

func _ready():
	# Position paddle on left side
	position = Vector2(50, SCREEN_HEIGHT / 2)

func _process(delta):
	# Move paddle based on input
	if Input.is_action_pressed("ui_up"):
		position.y -= SPEED * delta
	if Input.is_action_pressed("ui_down"):
		position.y += SPEED * delta
	
	# Keep paddle within screen boundaries
	position.y = clamp(position.y, PADDLE_HEIGHT / 2, SCREEN_HEIGHT - PADDLE_HEIGHT / 2)
```

The `clamp()` function is a built-in Godot function that keeps the paddle position within bounds. This prevents the paddle from moving off-screen - a common best practice for boundary checking.

### Step 4: Create the AI Paddle

The AI paddle will automatically follow the ball's position.

1. Duplicate the PlayerPaddle node (right-click > Duplicate)
2. Rename it to "AIPaddle"
3. In the Inspector, change its position to the right side (around x=750)

Replace the script on AIPaddle with:

```gdscript
extends Area2D

# AI paddle speed (slightly slower than player for balance)
const SPEED = 350

# Screen boundaries
const SCREEN_HEIGHT = 600
const PADDLE_HEIGHT = 100

# Reference to the ball
var ball

func _ready():
	# Position paddle on right side
	position = Vector2(750, SCREEN_HEIGHT / 2)
	# Get reference to ball node
	ball = get_parent().get_node("Ball")

func _process(delta):
	if ball:
		# Move towards ball's y position
		if position.y < ball.position.y:
			position.y += SPEED * delta
		elif position.y > ball.position.y:
			position.y -= SPEED * delta
		
		# Keep paddle within screen boundaries
		position.y = clamp(position.y, PADDLE_HEIGHT / 2, SCREEN_HEIGHT - PADDLE_HEIGHT / 2)
```

This AI is simple but effective. It gets a reference to the ball using `get_parent().get_node()` - a Godot best practice for accessing sibling nodes. The AI then moves toward the ball's vertical position.

### Step 5: Implement Ball-Paddle Collision

Now we need to make the ball bounce off the paddles. Update the Ball script:

```gdscript
extends Area2D

var velocity = Vector2(300, 200)

const SCREEN_WIDTH = 800
const SCREEN_HEIGHT = 600

func _ready():
	position = Vector2(SCREEN_WIDTH / 2, SCREEN_HEIGHT / 2)
	# Connect collision signal
	area_entered.connect(_on_area_entered)

func _process(delta):
	position += velocity * delta
	
	# Bounce off top and bottom walls
	if position.y <= 0 or position.y >= SCREEN_HEIGHT:
		velocity.y = -velocity.y

func _on_area_entered(area):
	# Check if ball hit a paddle
	if area.name == "PlayerPaddle" or area.name == "AIPaddle":
		# Reverse horizontal direction
		velocity.x = -velocity.x
		
		# Add some variation based on where ball hits paddle
		var paddle_center = area.position.y
		var hit_offset = position.y - paddle_center
		velocity.y += hit_offset * 2
```

Using signals like `area_entered` is a Godot best practice for event-driven programming. This makes the code cleaner and more maintainable. The hit offset calculation adds realistic physics - hitting the edge of the paddle sends the ball at a steeper angle.

### Step 6: Add Scoring System

Let's add score tracking. First, update the Game node (root node) with a script:

```gdscript
extends Node2D

# Scores
var player_score = 0
var ai_score = 0

# References
var ball
var player_paddle
var ai_paddle

func _ready():
	# Get references to game objects
	ball = $Ball
	player_paddle = $PlayerPaddle
	ai_paddle = $AIPaddle

func _process(_delta):
	# Check if ball went off left side (AI scores)
	if ball.position.x < 0:
		ai_score += 1
		print("AI Score: ", ai_score)
		reset_ball()
	
	# Check if ball went off right side (Player scores)
	if ball.position.x > 800:
		player_score += 1
		print("Player Score: ", player_score)
		reset_ball()

func reset_ball():
	# Reset ball to center
	ball.position = Vector2(400, 300)
	# Reverse direction so it goes to whoever didn't score
	ball.velocity.x = -ball.velocity.x
	# Reset vertical velocity
	ball.velocity.y = randf_range(-200, 200)
```

The `$` syntax is a Godot shorthand for `get_node()` - another best practice for cleaner code. Using `randf_range()` adds variety to each serve.

### Step 7: Add Visual Score Display

For a better user experience, let's add on-screen score display using Labels.

1. Add a `CanvasLayer` node as a child of Game (this keeps UI elements on top)
2. Add a `Label` node as a child of CanvasLayer and name it "PlayerScoreLabel"
3. Add another `Label` node as a child of CanvasLayer and name it "AIScoreLabel"
4. Position PlayerScoreLabel in the top-left (around x=100, y=50)
5. Position AIScoreLabel in the top-right (around x=700, y=50)
6. In the Inspector for both labels:
   - Increase font size (Theme Overrides > Font Sizes > Font Size: 48)
   - Center the text alignment if desired

Update the Game script to update the labels:

```gdscript
extends Node2D

var player_score = 0
var ai_score = 0

var ball
var player_paddle
var ai_paddle
var player_score_label
var ai_score_label

func _ready():
	ball = $Ball
	player_paddle = $PlayerPaddle
	ai_paddle = $AIPaddle
	player_score_label = $CanvasLayer/PlayerScoreLabel
	ai_score_label = $CanvasLayer/AIScoreLabel
	
	# Initialize score display
	update_score_display()

func _process(_delta):
	if ball.position.x < 0:
		ai_score += 1
		reset_ball()
		update_score_display()
	
	if ball.position.x > 800:
		player_score += 1
		reset_ball()
		update_score_display()

func update_score_display():
	player_score_label.text = str(player_score)
	ai_score_label.text = str(ai_score)

func reset_ball():
	ball.position = Vector2(400, 300)
	ball.velocity.x = -ball.velocity.x
	ball.velocity.y = randf_range(-200, 200)
```

Separating the score update into its own function (`update_score_display()`) is good practice - it makes the code more modular and easier to maintain.

### Step 8: Add Game Over Condition

Let's make the game end when someone reaches 5 points:

```gdscript
extends Node2D

var player_score = 0
var ai_score = 0
var game_over = false

const WINNING_SCORE = 5

var ball
var player_paddle
var ai_paddle
var player_score_label
var ai_score_label

func _ready():
	ball = $Ball
	player_paddle = $PlayerPaddle
	ai_paddle = $AIPaddle
	player_score_label = $CanvasLayer/PlayerScoreLabel
	ai_score_label = $CanvasLayer/AIScoreLabel
	update_score_display()

func _process(_delta):
	if game_over:
		# Check for restart input
		if Input.is_action_just_pressed("ui_accept"):
			restart_game()
		return
	
	if ball.position.x < 0:
		ai_score += 1
		check_game_over()
		if not game_over:
			reset_ball()
		update_score_display()
	
	if ball.position.x > 800:
		player_score += 1
		check_game_over()
		if not game_over:
			reset_ball()
		update_score_display()

func check_game_over():
	if player_score >= WINNING_SCORE:
		game_over = true
		player_score_label.text = str(player_score) + " - YOU WIN!"
	elif ai_score >= WINNING_SCORE:
		game_over = true
		ai_score_label.text = str(ai_score) + " - AI WINS!"
	
	if game_over:
		# Stop the ball
		ball.velocity = Vector2.ZERO

func restart_game():
	player_score = 0
	ai_score = 0
	game_over = false
	update_score_display()
	reset_ball()
	ball.velocity = Vector2(300, 200)

func update_score_display():
	player_score_label.text = str(player_score)
	ai_score_label.text = str(ai_score)

func reset_ball():
	ball.position = Vector2(400, 300)
	ball.velocity.x = -ball.velocity.x
	ball.velocity.y = randf_range(-200, 200)
```

Using `Vector2.ZERO` is cleaner than writing `Vector2(0, 0)` - embracing Godot's built-in constants is a best practice. The `return` statement in `_process()` when game is over prevents further processing, which is more efficient.

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
