# Lesson 7: Creating a Basic State Machine in Godot

State machines are one of the most useful programming patterns in game development. They help you organize complex behavior by breaking it down into distinct "states" and defining how to transition between them. In this lesson, you'll learn how to create a state machine for a player character that can be idle, walking, jumping, and attacking.

## What is a State Machine?

A **state machine** (also called a Finite State Machine or FSM) is a way to organize the behavior of an object by dividing it into different states. At any given time, the object is in exactly one state, and it can transition to other states based on certain conditions.

Think about a character in a game:
- When standing still, the character is in an **Idle** state
- When moving, the character is in a **Walking** state  
- When in the air, the character is in a **Jumping** state
- When attacking, the character is in an **Attacking** state

Each state has its own behavior and rules about which states it can transition to. For example:
- From Idle, you can transition to Walking or Jumping
- From Walking, you can transition to Idle, Jumping, or Attacking
- From Jumping, you can only transition back to Idle when you land
- From Attacking, you transition back to Idle or Walking after the attack finishes

## Why Use a State Machine?

Without a state machine, you might end up with code like this:

```gdscript
func _process(delta):
    if is_jumping:
        # Handle jumping
        if is_attacking:
            # Can't attack while jumping... or can we?
            pass
    elif is_attacking:
        # Handle attacking
        if Input.is_action_pressed("ui_left"):
            # Can we move while attacking?
            pass
    elif Input.is_action_pressed("ui_left"):
        # Handle walking
        if Input.is_action_just_pressed("ui_select"):
            # Start attacking... but we're already handling walking
            pass
```

This quickly becomes a tangled mess of nested if statements that's hard to read, debug, and modify. State machines solve this by:

1. **Organizing code clearly**: Each state's behavior is separate and easy to find
2. **Preventing impossible situations**: You can't be both jumping and walking at the same time
3. **Making transitions explicit**: You define exactly which state changes are allowed
4. **Simplifying debugging**: You can easily see what state your character is in
5. **Making code reusable**: The state machine pattern can be used for enemies, UI menus, game phases, and more

## Building a State Machine Step by Step

We'll build a state machine for Heath, our Heathmont Game Design Robot. He'll have four states: Idle, Walking, Jumping, and Attacking.

### Step 1: Set Up the Project

First, let's create a new scene with Heath:

1. Create a new scene in Godot with a `CharacterBody2D` as the root node
2. Rename the root node to "Heath"
3. Add a `Sprite2D` child to Heath (load your character sprite)
4. Add a `CollisionShape2D` child to Heath (set it to a RectangleShape2D that matches your sprite)
5. Save the scene as `heath_state_machine.tscn`

We're using `CharacterBody2D` instead of `Area2D` because it's designed for player characters and includes built-in physics functions we'll use later.

### Step 2: Define the States

Attach a script to the Heath node. We'll start by defining our states using an **enum** (enumeration). An enum is a way to define a set of named constants that represent different options.

```gdscript
extends CharacterBody2D
# This script implements a state machine for the player character.
# It should be attached to the Heath (CharacterBody2D) root node.

# Define all possible states using an enum.
# Enums create named constants that are easier to read than plain numbers.
# Instead of writing "if state == 0", we can write "if state == State.IDLE"
enum State {
	IDLE,      # State 0: Standing still
	WALKING,   # State 1: Moving left or right
	JUMPING,   # State 2: In the air
	ATTACKING  # State 3: Performing an attack
}

# Current state - starts as IDLE when the game begins.
# Using the State enum makes our code self-documenting.
var current_state = State.IDLE

# Movement constants - these control how Heath moves.
# Using constants makes it easy to tune gameplay without searching through code.
const SPEED = 200.0           # Horizontal movement speed in pixels per second
const JUMP_VELOCITY = -400.0  # Initial upward velocity when jumping (negative = up)
const GRAVITY = 980.0         # Gravity acceleration in pixels per second squared

# Attack timer - tracks how long we've been attacking.
# We'll use this to automatically return to idle after the attack finishes.
var attack_timer = 0.0
const ATTACK_DURATION = 0.5   # Attack lasts half a second

func _ready():
	# _ready() is called when the node enters the scene.
	print("Starting in state: ", State.keys()[current_state])
```

**What this code does:** We define an enum called `State` with four possible values. Using an enum is a Godot best practice because it makes code more readable - `State.IDLE` is much clearer than remembering that 0 means idle. The `State.keys()` function converts the enum value back to its name as a string, which is useful for debugging.

### Step 3: Implement State-Specific Behavior

Now let's add functions to handle what happens in each state. We'll create separate functions for each state to keep the code organized:

```gdscript
# Add this after the _ready() function

func _process(delta):
	# _process() is called every frame.
	# We use it to update the attack timer and handle state transitions.
	
	# Update the attack timer if we're attacking.
	if current_state == State.ATTACKING:
		attack_timer += delta
		# If attack duration has passed, return to idle.
		if attack_timer >= ATTACK_DURATION:
			change_state(State.IDLE)
	
	# Debug output - prints current state to console.
	# Comment this out once your state machine is working.
	if Engine.get_process_frames() % 60 == 0:  # Print once per second (at 60 FPS)
		print("Current state: ", State.keys()[current_state])

func _physics_process(delta):
	# _physics_process() is called every physics frame (usually 60 times per second).
	# This is where we handle movement and physics calculations.
	# Using _physics_process() instead of _process() for physics is a Godot best practice.
	
	# Apply gravity if we're not on the ground.
	# CharacterBody2D provides is_on_floor() to check if we're touching the ground.
	if not is_on_floor():
		velocity.y += GRAVITY * delta
	
	# Handle state-specific behavior.
	# Each state has its own function to keep code organized.
	match current_state:
		State.IDLE:
			handle_idle_state()
		State.WALKING:
			handle_walking_state()
		State.JUMPING:
			handle_jumping_state()
		State.ATTACKING:
			handle_attacking_state()
	
	# Move the character based on its velocity.
	# move_and_slide() is a CharacterBody2D function that handles collision automatically.
	# This is another Godot best practice for character movement.
	move_and_slide()

func handle_idle_state():
	# Behavior when in the IDLE state.
	
	# Stop horizontal movement - standing still.
	velocity.x = 0
	
	# Check for state transitions - what can we do from idle?
	
	# Can jump if on the ground.
	if Input.is_action_just_pressed("ui_up") and is_on_floor():
		change_state(State.JUMPING)
	# Can start walking if player presses left or right.
	elif Input.is_action_pressed("ui_left") or Input.is_action_pressed("ui_right"):
		change_state(State.WALKING)
	# Can attack if player presses the attack button (Space).
	elif Input.is_action_just_pressed("ui_accept"):
		change_state(State.ATTACKING)

func handle_walking_state():
	# Behavior when in the WALKING state.
	
	# Get input direction: -1 for left, 1 for right, 0 for neither.
	# Input.get_axis() is a convenient Godot function that combines two actions.
	var direction = Input.get_axis("ui_left", "ui_right")
	
	# Move in the direction the player is pressing.
	if direction != 0:
		velocity.x = direction * SPEED
	else:
		# If no direction is pressed, stop and return to idle.
		velocity.x = 0
		change_state(State.IDLE)
	
	# Check for state transitions from walking.
	
	# Can jump while walking.
	if Input.is_action_just_pressed("ui_up") and is_on_floor():
		change_state(State.JUMPING)
	# Can attack while walking.
	elif Input.is_action_just_pressed("ui_accept"):
		change_state(State.ATTACKING)

func handle_jumping_state():
	# Behavior when in the JUMPING state.
	
	# Allow some air control - player can move left/right while jumping.
	# This makes the game feel more responsive.
	var direction = Input.get_axis("ui_left", "ui_right")
	if direction != 0:
		velocity.x = direction * SPEED
	else:
		velocity.x = 0
	
	# Check for state transitions from jumping.
	
	# Can only return to idle/walking once we land.
	if is_on_floor():
		# If still pressing a direction, go to walking. Otherwise, go to idle.
		if Input.is_action_pressed("ui_left") or Input.is_action_pressed("ui_right"):
			change_state(State.WALKING)
		else:
			change_state(State.IDLE)

func handle_attacking_state():
	# Behavior when in the ATTACKING state.
	
	# During attack, stop horizontal movement.
	# This makes the attack animation look better.
	velocity.x = 0
	
	# Note: We don't check for transitions here because the attack
	# automatically ends after ATTACK_DURATION seconds (handled in _process).
	# This prevents the player from interrupting their attack.

func change_state(new_state):
	# This function handles all state changes.
	# Centralizing state changes is a best practice - it makes debugging easier
	# and lets us add code that runs on every state change.
	
	# Exit the current state - do any cleanup needed.
	match current_state:
		State.JUMPING:
			pass  # No cleanup needed for jumping
		State.ATTACKING:
			attack_timer = 0.0  # Reset attack timer
	
	# Update to the new state.
	var old_state = current_state
	current_state = new_state
	
	# Enter the new state - do any setup needed.
	match current_state:
		State.JUMPING:
			# Set upward velocity to start the jump.
			velocity.y = JUMP_VELOCITY
		State.ATTACKING:
			# Reset timer when starting attack.
			attack_timer = 0.0
	
	# Debug output - prints state changes to console.
	print("State changed: ", State.keys()[old_state], " -> ", State.keys()[new_state])
```

**What this code does:** This is the heart of our state machine. We use the `match` statement (similar to switch/case in other languages) to run different code based on the current state. Using `match` is a Godot best practice for handling multiple conditions cleanly. Each state has its own function that handles behavior and checks for transitions. The `change_state()` function centralizes all state changes, making it easy to add debug logging or special behavior when states change.

**Key concepts:**
- `is_action_just_pressed()` detects a single press (good for jumps and attacks)
- `is_action_pressed()` detects holding a key (good for continuous movement)
- `Input.get_axis()` combines left/right input into a single value (-1, 0, or 1)
- `is_on_floor()` checks if the character is touching the ground
- `move_and_slide()` moves the character and handles collisions automatically

### Step 4: Testing the State Machine

To test your state machine:

1. Press F5 or click the Play button to run the scene
2. Press Left/Right arrow keys to walk
3. Press Up arrow to jump
4. Press Space to attack
5. Watch the console (bottom of Godot editor) to see state changes

You should see debug messages like:
```
Starting in state: IDLE
Current state: IDLE
State changed: IDLE -> WALKING
Current state: WALKING
State changed: WALKING -> JUMPING
Current state: JUMPING
```

### Step 5: Adding Visual Feedback

Right now, the state machine works but you can't see what state Heath is in (except in the console). Let's add visual feedback by changing the modulate color based on state:

```gdscript
# Add this function after change_state()

func update_visual_state():
	# Change the character's color based on current state.
	# This provides visual feedback so you can see the state machine working.
	# In a real game, you'd use different animations instead of colors.
	match current_state:
		State.IDLE:
			modulate = Color.WHITE      # Default color
		State.WALKING:
			modulate = Color.CYAN       # Light blue when walking
		State.JUMPING:
			modulate = Color.YELLOW     # Yellow when jumping
		State.ATTACKING:
			modulate = Color.RED        # Red when attacking
```

Then call this function at the end of `change_state()`:

```gdscript
func change_state(new_state):
	# Exit the current state - do any cleanup needed.
	match current_state:
		State.JUMPING:
			pass  # No cleanup needed for jumping
		State.ATTACKING:
			attack_timer = 0.0  # Reset attack timer
	
	# Update to the new state.
	var old_state = current_state
	current_state = new_state
	
	# Enter the new state - do any setup needed.
	match current_state:
		State.JUMPING:
			velocity.y = JUMP_VELOCITY
		State.ATTACKING:
			attack_timer = 0.0
	
	# Debug output.
	print("State changed: ", State.keys()[old_state], " -> ", State.keys()[new_state])
	
	# Update visual feedback.
	update_visual_state()
```

Now when you run the game, Heath will change color based on his state!

### Step 6: Adding a Floor

To see jumping work properly, let's add a floor to the scene:

1. Add a `StaticBody2D` node as a child of the scene root (not of Heath)
2. Name it "Floor"
3. Add a `CollisionShape2D` child to Floor
4. Set the CollisionShape2D to use a RectangleShape2D
5. Position the floor at the bottom of the screen (y = 550, x = 400)
6. Stretch the collision shape to be wide (800 x 50 pixels)

Optionally, add a `ColorRect` to visualize the floor:
1. Add a `ColorRect` child to Floor
2. Set its size to 800 x 50
3. Set its position to (0, -25) so it's centered on the collision shape
4. Pick a color you like

Now when you run the game, Heath will fall and land on the floor, and you can see jumping work properly!

## Understanding the State Machine Pattern

Let's break down the key components of our state machine:

### 1. States (enum)
The enum defines all possible states. This is cleaner than using strings or numbers.

### 2. Current State (variable)
A single variable tracks which state we're in right now.

### 3. State Handlers (functions)
Each state has a function that defines its behavior:
- `handle_idle_state()`
- `handle_walking_state()`
- `handle_jumping_state()`
- `handle_attacking_state()`

### 4. State Transitions (change_state function)
One centralized function handles all state changes. This makes it easy to:
- Add debug logging
- Validate transitions (prevent impossible state changes)
- Run cleanup code when exiting a state
- Run setup code when entering a state

### 5. State Machine Update (_physics_process)
The main update function:
1. Applies physics (like gravity)
2. Calls the appropriate state handler based on current_state
3. Moves the character

## State Diagram

Here's a visual representation of our state machine:

```
         [IDLE]
        /   |   \
       /    |    \
    [WALK] [JUMP] [ATTACK]
       \    |    /
        \   |   /
         [IDLE]
```

- From IDLE: Can go to WALK, JUMP, or ATTACK
- From WALK: Can go to IDLE, JUMP, or ATTACK
- From JUMP: Can only return to IDLE or WALK (when landing)
- From ATTACK: Automatically returns to IDLE (after 0.5 seconds)

## Complete Code

Here's the complete script with all components together:

```gdscript
extends CharacterBody2D
# Complete state machine implementation for player character.

# Define all possible states.
enum State {
	IDLE,
	WALKING,
	JUMPING,
	ATTACKING
}

# Current state and movement constants.
var current_state = State.IDLE
const SPEED = 200.0
const JUMP_VELOCITY = -400.0
const GRAVITY = 980.0

# Attack timing.
var attack_timer = 0.0
const ATTACK_DURATION = 0.5

func _ready():
	print("Starting in state: ", State.keys()[current_state])
	update_visual_state()

func _process(delta):
	# Update attack timer.
	if current_state == State.ATTACKING:
		attack_timer += delta
		if attack_timer >= ATTACK_DURATION:
			change_state(State.IDLE)
	
	# Debug output (once per second).
	if Engine.get_process_frames() % 60 == 0:
		print("Current state: ", State.keys()[current_state])

func _physics_process(delta):
	# Apply gravity.
	if not is_on_floor():
		velocity.y += GRAVITY * delta
	
	# Handle current state.
	match current_state:
		State.IDLE:
			handle_idle_state()
		State.WALKING:
			handle_walking_state()
		State.JUMPING:
			handle_jumping_state()
		State.ATTACKING:
			handle_attacking_state()
	
	# Move the character.
	move_and_slide()

func handle_idle_state():
	velocity.x = 0
	
	if Input.is_action_just_pressed("ui_up") and is_on_floor():
		change_state(State.JUMPING)
	elif Input.is_action_pressed("ui_left") or Input.is_action_pressed("ui_right"):
		change_state(State.WALKING)
	elif Input.is_action_just_pressed("ui_accept"):
		change_state(State.ATTACKING)

func handle_walking_state():
	var direction = Input.get_axis("ui_left", "ui_right")
	
	if direction != 0:
		velocity.x = direction * SPEED
	else:
		velocity.x = 0
		change_state(State.IDLE)
	
	if Input.is_action_just_pressed("ui_up") and is_on_floor():
		change_state(State.JUMPING)
	elif Input.is_action_just_pressed("ui_accept"):
		change_state(State.ATTACKING)

func handle_jumping_state():
	var direction = Input.get_axis("ui_left", "ui_right")
	if direction != 0:
		velocity.x = direction * SPEED
	else:
		velocity.x = 0
	
	if is_on_floor():
		if Input.is_action_pressed("ui_left") or Input.is_action_pressed("ui_right"):
			change_state(State.WALKING)
		else:
			change_state(State.IDLE)

func handle_attacking_state():
	velocity.x = 0

func change_state(new_state):
	# Exit current state cleanup.
	match current_state:
		State.JUMPING:
			pass
		State.ATTACKING:
			attack_timer = 0.0
	
	# Change state.
	var old_state = current_state
	current_state = new_state
	
	# Enter new state setup.
	match current_state:
		State.JUMPING:
			velocity.y = JUMP_VELOCITY
		State.ATTACKING:
			attack_timer = 0.0
	
	# Debug and update visuals.
	print("State changed: ", State.keys()[old_state], " -> ", State.keys()[new_state])
	update_visual_state()

func update_visual_state():
	match current_state:
		State.IDLE:
			modulate = Color.WHITE
		State.WALKING:
			modulate = Color.CYAN
		State.JUMPING:
			modulate = Color.YELLOW
		State.ATTACKING:
			modulate = Color.RED
```

## Sample Code

Complete sample code for this project can be found here: [Lesson 7: State Machine](https://github.com/HeathmontGameDesign/LearningGodot/tree/main/7_State_Machine)

The repository includes:
- Complete project files
- Heath sprite asset
- Scene with floor set up
- README with additional tips

## Godot Best Practices Used

Throughout this lesson, we've followed several Godot best practices:

1. **Using enums for states**: Makes code self-documenting and prevents typos
2. **Separating state logic**: Each state has its own handler function
3. **Centralizing state changes**: One function handles all transitions
4. **Using _physics_process for physics**: Ensures consistent physics behavior
5. **Using CharacterBody2D**: Built-in features for player movement
6. **Using constants**: Makes tuning gameplay easy
7. **Using match statements**: Cleaner than long if-else chains
8. **Using is_on_floor()**: Built-in collision detection
9. **Using move_and_slide()**: Automatic collision handling
10. **Adding debug output**: Makes development and testing easier

## Advanced State Machine Concepts

Once you've mastered the basic state machine, you can explore these advanced concepts:

### State Machine as a Node

Instead of putting the state machine logic in the character script, you can create a separate `StateMachine` node that manages states. This is more reusable.

### State Scripts

Each state could be its own script file, making it easier to manage complex states with lots of logic.

### State History

Track which state you came from, allowing features like "return to previous state" or combos.

### Nested State Machines

A state can contain its own state machine. For example, the ATTACKING state could have sub-states for different attack types.

### Parallel State Machines

Run multiple state machines at once. One for movement (idle/walking/jumping), one for actions (attacking/defending), one for status effects (normal/poisoned/stunned).

## Challenges

Once you have a working state machine, try these enhancements:

### Challenge 1: Add a Crouching State

Add a CROUCHING state that:
- Activates when the player presses Down arrow while on the ground
- Makes the character shorter (adjust the collision shape)
- Prevents jumping while crouching
- Can transition to IDLE when Down is released

### Challenge 2: Add Animation

Replace the color changes with actual animations:
- Create an AnimatedSprite2D node
- Add sprite frames for idle, walking, jumping, and attacking animations
- Play the appropriate animation in each state handler

### Challenge 3: Add a Dash Attack

Add a DASHING state that:
- Activates when the player presses a key while ATTACKING
- Moves the character forward quickly
- Only works on the ground
- Lasts for a short duration (0.3 seconds)
- Transitions back to IDLE

### Challenge 4: Add Combo Attacks

Expand the ATTACKING state to support combos:
- First attack: quick punch (0.3 seconds)
- If player presses attack again during first attack: heavy kick (0.5 seconds)
- If player presses attack during heavy kick: spin attack (0.7 seconds)
- Track which attack in the combo you're on

### Challenge 5: Create an Enemy with States

Create an enemy character with its own state machine:
- PATROL: Moves back and forth
- CHASE: Moves toward the player when close
- ATTACK: Attacks when in range
- STUNNED: Briefly stunned when hit

## When to Use State Machines

State machines are useful when:
- An object has distinct modes of behavior
- Behavior depends on the object's current condition
- You need to prevent certain actions in certain conditions
- You want clear, maintainable code for complex behavior

Examples in games:
- **Player characters**: Different movement modes, actions, and abilities
- **Enemies**: Patrol, chase, attack, retreat behaviors
- **Game managers**: Menu, playing, paused, game over states
- **UI**: Different menu screens and their transitions
- **Dialogue systems**: Talking, choosing options, ending conversation
- **Doors**: Closed, opening, open, closing, locked states

## Further Learning

The state machine pattern is fundamental to game programming. Understanding it well will help you:
- Organize complex game logic
- Debug behavior problems faster
- Create more sophisticated AI
- Build better UI systems
- Structure your overall game flow

Experiment with the state machine to see how flexible and powerful this pattern can be!
