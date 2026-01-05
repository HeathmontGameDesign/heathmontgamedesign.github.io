# Godot Built-in Function Reference

This page provides a quick reference for commonly used built-in functions in Godot 4.2+. These functions are essential for game development and are used throughout the lessons on this site.

## Quick Reference Table

| Function | Description | Official Docs |
|----------|-------------|---------------|
| [`_ready()`](#_ready) | Called when a node enters the scene tree for the first time | [View Docs](https://docs.godotengine.org/en/stable/classes/class_node.html#class-node-method-ready) |
| [`_process(delta)`](#_process) | Called every frame to update game logic | [View Docs](https://docs.godotengine.org/en/stable/classes/class_node.html#class-node-method-process) |
| [`_draw()`](#_draw) | Called when a CanvasItem needs to be redrawn | [View Docs](https://docs.godotengine.org/en/stable/classes/class_canvasitem.html#class-canvasitem-method-draw) |
| [`draw_circle()`](#draw_circle) | Draws a filled circle on the screen | [View Docs](https://docs.godotengine.org/en/stable/classes/class_canvasitem.html#class-canvasitem-method-draw-circle) |
| [`draw_rect()`](#draw_rect) | Draws a filled rectangle on the screen | [View Docs](https://docs.godotengine.org/en/stable/classes/class_canvasitem.html#class-canvasitem-method-draw-rect) |
| [`Input.is_action_pressed()`](#input_is_action_pressed) | Checks if an input action is currently pressed | [View Docs](https://docs.godotengine.org/en/stable/classes/class_input.html#class-input-method-is-action-pressed) |
| [`Vector2()`](#vector2) | Creates a 2D vector for positions, directions, or velocities | [View Docs](https://docs.godotengine.org/en/stable/classes/class_vector2.html) |
| [`queue_free()`](#queue_free) | Marks a node for deletion at the end of the current frame | [View Docs](https://docs.godotengine.org/en/stable/classes/class_node.html#class-node-method-queue-free) |
| [`randf_range()`](#randf_range) | Returns a random float within a specified range | [View Docs](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#class-globalscope-method-randf-range) |
| [`randi_range()`](#randi_range) | Returns a random integer within a specified range | [View Docs](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#class-globalscope-method-randi-range) |
| [`print()`](#print) | Outputs text to the console for debugging | [View Docs](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#class-globalscope-method-print) |
| [`connect()`](#connect) | Connects a signal to a method | [View Docs](https://docs.godotengine.org/en/stable/classes/class_object.html#class-object-method-connect) |

---

## Detailed Function Reference

### `_ready()`

**Description:** The `_ready()` function is automatically called when a node enters the scene tree for the first time. This is the perfect place to initialize variables, set starting positions, or configure your node's properties.

**Usage:**
```gdscript
extends Node2D

func _ready():
    position = Vector2(400, 300)
    print("Node is ready!")
```

**Key Points:**
- Called only once when the node first enters the scene
- Called after all child nodes are also ready
- Ideal for initialization code
- Use this instead of a constructor for node setup

**Official Documentation:** [Node._ready()](https://docs.godotengine.org/en/stable/classes/class_node.html#class-node-method-ready)

---

### `_process()`

**Description:** The `_process(delta)` function is called every frame, making it perfect for continuous game logic like movement, animations, and input checking. The `delta` parameter represents the time elapsed since the last frame in seconds.

**Usage:**
```gdscript
extends Sprite2D

func _process(delta):
    # Move sprite 100 pixels per second to the right
    position.x += 100 * delta
    
    # Check for input every frame
    if Input.is_action_pressed("ui_left"):
        position.x -= 200 * delta
```

**Parameters:**
- `delta` (float): Time elapsed since the last frame in seconds

**Key Points:**
- Called every frame (typically 60 times per second)
- Always multiply movement by `delta` to ensure frame-rate independent movement
- Can be disabled by setting `set_process(false)`
- Use for continuous updates like movement and input checking

**Official Documentation:** [Node._process()](https://docs.godotengine.org/en/stable/classes/class_node.html#class-node-method-process)

---

### `_draw()`

**Description:** The `_draw()` function is called when a CanvasItem (like Node2D or Control) needs to be redrawn. Use this function to draw custom shapes, lines, and other visual elements directly on the screen.

**Usage:**
```gdscript
extends Node2D

func _draw():
    # Draw a white circle at the center
    draw_circle(Vector2(400, 300), 50, Color.WHITE)
    
    # Draw a blue rectangle
    var rect = Rect2(100, 100, 200, 150)
    draw_rect(rect, Color.BLUE)
```

**Key Points:**
- Called automatically when the node needs to be redrawn
- Use `queue_redraw()` to request a redraw
- All drawing functions must be called from within `_draw()`
- Drawing is relative to the node's position

**Official Documentation:** [CanvasItem._draw()](https://docs.godotengine.org/en/stable/classes/class_canvasitem.html#class-canvasitem-method-draw)

---

### `draw_circle()`

**Description:** Draws a filled circle on the screen at a specified position with a given radius and color. This function can only be called from within the `_draw()` function.

**Usage:**
```gdscript
extends Node2D

func _draw():
    # Draw a red circle at (200, 200) with radius 30
    draw_circle(Vector2(200, 200), 30, Color.RED)
    
    # Draw a semi-transparent green circle
    draw_circle(Vector2(400, 300), 50, Color(0, 1, 0, 0.5))
```

**Parameters:**
- `position` (Vector2): Center point of the circle
- `radius` (float): Radius of the circle in pixels
- `color` (Color): Color of the circle

**Key Points:**
- Must be called from within `_draw()`
- Position is relative to the node's position
- Use `Color()` or built-in colors like `Color.RED`
- For an outline, use `draw_arc()` or draw a slightly smaller circle on top

**Official Documentation:** [CanvasItem.draw_circle()](https://docs.godotengine.org/en/stable/classes/class_canvasitem.html#class-canvasitem-method-draw-circle)

---

### `draw_rect()`

**Description:** Draws a filled rectangle on the screen with a specified position, size, and color. The position represents the top-left corner of the rectangle.

**Usage:**
```gdscript
extends Node2D

func _draw():
    # Draw a blue rectangle at (100, 100) with size 200x150
    var rect = Rect2(100, 100, 200, 150)
    draw_rect(rect, Color.BLUE)
    
    # Draw a yellow rectangle
    draw_rect(Rect2(300, 200, 100, 100), Color.YELLOW)
```

**Parameters:**
- `rect` (Rect2): A Rect2 object defining position (x, y) and size (width, height)
- `color` (Color): Color of the rectangle
- `filled` (bool, optional): Whether to fill the rectangle (default: true)

**Key Points:**
- Must be called from within `_draw()`
- Rect2 format: `Rect2(x, y, width, height)`
- Position is the top-left corner, not the center
- Set `filled` to false for an outline only

**Official Documentation:** [CanvasItem.draw_rect()](https://docs.godotengine.org/en/stable/classes/class_canvasitem.html#class-canvasitem-method-draw-rect)

---

### `Input.is_action_pressed()`

**Description:** Checks if a specific input action is currently being pressed. Input actions are defined in Project Settings > Input Map and can represent keyboard keys, mouse buttons, or gamepad inputs.

**Usage:**
```gdscript
extends Sprite2D

func _process(delta):
    # Check if left arrow is pressed
    if Input.is_action_pressed("ui_left"):
        position.x -= 200 * delta
    
    # Check if right arrow is pressed
    if Input.is_action_pressed("ui_right"):
        position.x += 200 * delta
```

**Parameters:**
- `action` (String): Name of the input action to check

**Common Built-in Actions:**
- `"ui_left"` - Left arrow key
- `"ui_right"` - Right arrow key
- `"ui_up"` - Up arrow key
- `"ui_down"` - Down arrow key
- `"ui_accept"` - Enter/Space key
- `"ui_cancel"` - Escape key

**Key Points:**
- Returns `true` while the action is held down
- Use `Input.is_action_just_pressed()` for single press detection
- Custom actions can be defined in Project Settings
- Works with keyboard, mouse, and gamepad inputs

**Official Documentation:** [Input.is_action_pressed()](https://docs.godotengine.org/en/stable/classes/class_input.html#class-input-method-is-action-pressed)

---

### `Vector2()`

**Description:** Creates a 2D vector that can represent positions, directions, velocities, or any two-dimensional quantity. Vector2 is one of the most fundamental types in Godot and is used extensively for 2D game development.

**Usage:**
```gdscript
extends Sprite2D

func _ready():
    # Set position using Vector2
    position = Vector2(400, 300)
    
    # Create a velocity vector
    var velocity = Vector2(100, 50)
    
    # Vector operations
    var direction = Vector2(1, 0).normalized()  # Right direction
    var distance = position.distance_to(Vector2(0, 0))
```

**Parameters:**
- `x` (float): The x component (horizontal)
- `y` (float): The y component (vertical)

**Common Vector2 Properties:**
- `vector.x` - Access the x component
- `vector.y` - Access the y component
- `vector.length()` - Get the length (magnitude) of the vector
- `vector.normalized()` - Get a unit vector in the same direction

**Key Points:**
- Represents positions on screen (x = horizontal, y = vertical)
- Can be added, subtracted, multiplied, and divided
- Use for movement, directions, and velocities
- Many Godot functions accept or return Vector2

**Official Documentation:** [Vector2](https://docs.godotengine.org/en/stable/classes/class_vector2.html)

---

### `queue_free()`

**Description:** Marks a node for deletion at the end of the current frame. This is the safe way to remove nodes from the scene tree without causing crashes or unexpected behavior.

**Usage:**
```gdscript
extends Area2D

func _on_area_entered(area):
    if area.is_in_group("bullets"):
        # Remove the bullet
        area.queue_free()
        
        # Remove this node too
        queue_free()
```

**Key Points:**
- Deletion happens at the end of the current frame
- Safer than calling `free()` directly
- The node and all its children are removed
- Signals are disconnected automatically
- Use when you want to remove an object from the game

**Official Documentation:** [Node.queue_free()](https://docs.godotengine.org/en/stable/classes/class_node.html#class-node-method-queue-free)

---

### `randf_range()`

**Description:** Returns a random floating-point number between the specified minimum and maximum values (inclusive). This is useful for creating randomness in games, such as random positions, speeds, or delays.

**Usage:**
```gdscript
extends Node2D

func _ready():
    # Random position on screen (0-800 x, 0-600 y)
    var random_x = randf_range(0, 800)
    var random_y = randf_range(0, 600)
    position = Vector2(random_x, random_y)
    
    # Random speed between 100 and 300
    var speed = randf_range(100, 300)
```

**Parameters:**
- `from` (float): Minimum value (inclusive)
- `to` (float): Maximum value (inclusive)

**Returns:** A random float between `from` and `to`

**Key Points:**
- Returns a float (decimal number)
- Both endpoints are inclusive
- Use for continuous ranges like positions or speeds
- For integers, use `randi_range()` instead

**Official Documentation:** [@GlobalScope.randf_range()](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#class-globalscope-method-randf-range)

---

### `randi_range()`

**Description:** Returns a random integer between the specified minimum and maximum values (inclusive). This is useful for dice rolls, random choices, or any situation where you need whole numbers.

**Usage:**
```gdscript
extends Node2D

var rng = RandomNumberGenerator.new()

func _ready():
    rng.randomize()  # Optional: explicit randomization / Godot 3.x compatibility
    
    # Random dice roll (1-6)
    var dice = rng.randi_range(1, 6)
    print("You rolled: ", dice)
    
    # Random choice (0-3)
    var choice = rng.randi_range(0, 3)
```

**Parameters:**
- `from` (int): Minimum value (inclusive)
- `to` (int): Maximum value (inclusive)

**Returns:** A random integer between `from` and `to`

**Key Points:**
- Returns an integer (whole number)
- Both endpoints are inclusive
- Requires RandomNumberGenerator instance
- Call `randomize()` first to initialize
- For floats, use `randf_range()` instead

**Official Documentation:** [RandomNumberGenerator.randi_range()](https://docs.godotengine.org/en/stable/classes/class_randomnumbergenerator.html#class-randomnumbergenerator-method-randi-range)

---

### `print()`

**Description:** Outputs text to the console (Output tab in Godot editor). This is essential for debugging and understanding what your code is doing.

**Usage:**
```gdscript
extends Node2D

func _ready():
    print("Game started!")
    print("Position: ", position)
    print("X: ", position.x, " Y: ", position.y)
    
func _process(delta):
    if Input.is_action_just_pressed("ui_accept"):
        print("Accept key pressed at frame: ", Engine.get_frames_drawn())
```

**Parameters:**
- Accepts any number of arguments of any type
- Arguments are converted to strings and concatenated

**Key Points:**
- Output appears in the Output tab at the bottom of Godot editor
- Can print multiple values separated by commas
- Useful for debugging and tracking variable values
- Remove or comment out prints in final game for performance
- Related functions: `print_debug()`, `push_warning()`, `push_error()`

**Official Documentation:** [@GlobalScope.print()](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#class-globalscope-method-print)

---

### `connect()`

**Description:** Connects a signal to a callable method. Signals are Godot's way of notifying when something happens, like a button being pressed or an area being entered.

**Usage:**
```gdscript
extends Area2D

func _ready():
    # Connect the area_entered signal to a function
    area_entered.connect(_on_area_entered)
    
    # Connect with additional parameters
    body_entered.connect(_on_body_entered)

func _on_area_entered(area):
    print("Area entered: ", area.name)
    
func _on_body_entered(body):
    print("Body entered: ", body.name)
```

**Parameters:**
- `signal` (Signal): The signal to connect (called on the signal itself)
- `callable` (Callable): The function to call when the signal is emitted

**Common Signals:**
- `area_entered` - When an Area2D enters this area
- `area_exited` - When an Area2D exits this area
- `body_entered` - When a physics body enters this area
- `pressed` - When a button is pressed

**Key Points:**
- Connects signals to functions for event handling
- Functions should match the signal's parameters
- Can disconnect with `disconnect()`
- Essential for collision detection and UI events
- In Godot 4, use the new syntax shown above

**Official Documentation:** [Object.connect()](https://docs.godotengine.org/en/stable/classes/class_object.html#class-object-method-connect)

---

## Additional Resources

- **Official Godot Documentation**: [docs.godotengine.org](https://docs.godotengine.org/)
- **GDScript Basics**: [GDScript Reference](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_basics.html)
- **Built-in Types**: [Built-in Types Reference](https://docs.godotengine.org/en/stable/classes/index.html#built-in-types)
- **Godot Community**: [Godot Forums](https://forum.godotengine.org/) | [Discord](https://discord.gg/godotengine)

---

[Back to Home](index.md)
