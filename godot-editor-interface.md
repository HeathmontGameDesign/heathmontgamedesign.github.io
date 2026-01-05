# Understanding the Godot Editor Interface

When you open a project in Godot 4.2 or higher, you'll see a powerful and comprehensive editor interface. This page will help you understand the different areas of the editor and how to use them effectively.

## Overview of the Main Areas

The Godot editor is divided into several key areas, each serving a specific purpose. As you work through the lessons, you'll become familiar with these areas and learn to navigate between them efficiently.

## 1. Main Toolbar (Top)

The toolbar at the top of the editor provides quick access to essential functions:

- **Main Menus**: File, Edit, Project, Debug, Editor, and Help menus for accessing all editor functions
- **Play Controls**: Run the current scene, run the main scene, pause, and stop buttons
- **Scene/Script Tabs**: Switch between the 2D, 3D, Script, and AssetLib workspaces
- **Search**: Quickly find and open files in your project

### Workspace Tabs

- **2D**: For working on 2D games, showing the 2D viewport
- **3D**: For working on 3D games, showing the 3D viewport
- **Script**: For writing and editing code
- **AssetLib**: For browsing and downloading assets from the Godot Asset Library

## 2. Viewport (Center)

The viewport is the largest area in the center of the editor where you visualize and edit your game scenes.

### Key Features:

- **Visual Scene Editing**: Drag and drop nodes, move objects around, and arrange your game visually
- **Camera Controls**: In 3D mode, use the mouse to orbit, pan, and zoom around your scene
- **Grid and Snapping**: Enable snapping to align objects precisely
- **Gizmos**: Visual tools for moving, rotating, and scaling objects

### Viewport Toolbar:

The toolbar above the viewport provides tools specific to the current workspace:

- **Transform Tools**: Select, move, rotate, and scale tools
- **View Options**: Toggle grid, snap settings, and camera views
- **Preview**: Test how your scene looks during gameplay

**Tip**: Use the mouse wheel to zoom in and out. Hold the middle mouse button to pan around the viewport.

## 3. Scene Dock (Top-Left)

The Scene dock shows the **node hierarchy** of your current scene. In Godot, everything in your game is organized as a tree of nodes.

### Understanding the Scene Hierarchy:

- **Parent-Child Relationships**: Nodes can have children, creating a tree structure
- **Node Types**: Different node types provide different functionality (Sprite2D, CharacterBody2D, etc.)
- **Selection**: Click a node to select it and view its properties in the Inspector

### Common Actions:

- **Add Node**: Click the "+" button or right-click to add a new node
- **Delete Node**: Select a node and press Delete or right-click and choose Delete
- **Rename Node**: Double-click a node's name or press F2
- **Reparent**: Drag and drop nodes to change their parent-child relationships
- **Organize**: Group related nodes together under parent nodes for better organization

**Important**: The order of nodes in the Scene dock affects the drawing order in 2D. Nodes lower in the list are drawn on top of nodes higher in the list.

## 4. FileSystem Dock (Bottom-Left)

The FileSystem dock shows all the files and folders in your project directory.

### File Types You'll See:

- **.gd files**: GDScript code files
- **.tscn files**: Scene files (text format)
- **.tres files**: Resource files
- **Image files**: .png, .jpg, etc. for sprites and textures
- **Audio files**: .wav, .ogg, etc. for sound effects and music
- **.godot folder**: Editor metadata (ignore this)

### Common Actions:

- **Import Assets**: Drag files from your file explorer into this dock to import them
- **Create Files**: Right-click to create new folders, scripts, or resources
- **Open Files**: Double-click to open scenes or scripts
- **Organize**: Create folders to keep your project organized
- **Search**: Use the search box to find files quickly

**Best Practice**: Keep your project organized with folders like "scenes", "scripts", "sprites", "sounds", etc.

## 5. Inspector (Right)

The Inspector shows all the **properties** of the currently selected node. This is where you configure how nodes behave.

### What You'll See in the Inspector:

- **Node Type**: Shows at the top (e.g., "Sprite2D", "CharacterBody2D")
- **Properties**: Organized into collapsible sections
- **Transform**: Position, rotation, and scale
- **Visibility**: Layer settings and visibility toggles
- **Custom Properties**: Properties specific to each node type

### Working with Properties:

- **Edit Values**: Click on a property to edit it (numbers, colors, text, etc.)
- **Drag Resources**: Drag files from the FileSystem dock to assign textures, scripts, etc.
- **Reset Values**: Right-click a property to reset it to its default value
- **Save Resources**: Some properties open sub-inspectors for complex resources

### Useful Inspector Features:

- **Color Picker**: Click on color properties to open a color picker
- **File Browser**: Click on file properties to browse for resources
- **Checkboxes**: Enable/disable boolean properties
- **Dropdowns**: Select from predefined options

**Tip**: You can hover over property names to see tooltips explaining what they do.

## 6. Bottom Panel

The bottom panel contains several tabs that provide additional functionality:

### Output Tab:

- Shows print statements from your code
- Displays messages from the editor
- Useful for debugging

### Debugger Tab:

- Shows error messages and stack traces
- Displays the call stack when your game is paused
- View and modify variables during gameplay

### Audio Tab:

- Manage audio buses for mixing sound
- Add audio effects
- Control volume levels

### Animation Tab:

- Create and edit animations
- Set up keyframes for animated properties
- Control animation playback

### Others:

- **Shader Editor**: Edit visual shaders
- **Import**: Configure import settings for assets

**Tip**: You can drag the top edge of the bottom panel to resize it or collapse it completely when not needed.

## 7. Script Editor

When you click on the "Script" tab at the top or double-click a .gd file, the Script editor opens:

### Script Editor Features:

- **Code Editor**: Write and edit GDScript code with syntax highlighting
- **Line Numbers**: Track which line you're on
- **Auto-completion**: Press Ctrl+Space (or Cmd+Space on Mac) for suggestions
- **Documentation**: Hover over functions to see documentation
- **Multiple Scripts**: Open multiple scripts in tabs
- **Search and Replace**: Find and replace text across your code

### Script Editor Toolbar:

- **File Menu**: Save, reload, and close scripts
- **Edit Menu**: Undo, redo, cut, copy, paste
- **Search Menu**: Find, replace, and navigate code
- **Goto Menu**: Jump to specific functions or lines
- **Help**: Access documentation for GDScript and Godot classes

### Useful Shortcuts:

- **Ctrl+S** (Cmd+S): Save the current script
- **Ctrl+F** (Cmd+F): Find text
- **Ctrl+H** (Cmd+H): Find and replace
- **Ctrl+D** (Cmd+D): Duplicate line
- **Ctrl+/** (Cmd+/): Toggle comment

## Tips for Working in the Godot Editor

1. **Save Often**: Use Ctrl+S (Cmd+S) to save your work regularly
2. **Use Multiple Monitors**: Drag the Script editor to a second monitor if available
3. **Customize Layout**: Rearrange docks by dragging their tabs
4. **Learn Shortcuts**: Keyboard shortcuts will speed up your workflow
5. **Read Error Messages**: The debugger provides helpful information when things go wrong
6. **Experiment**: Don't be afraid to try things - you can always undo with Ctrl+Z (Cmd+Z)

## Common Workflows

### Creating a New Scene:

1. Click Scene → New Scene (or Ctrl+N / Cmd+N)
2. Add a root node in the Scene dock
3. Add child nodes to build your scene
4. Configure properties in the Inspector
5. Save the scene with Ctrl+S (Cmd+S)

### Adding a Script to a Node:

1. Select the node in the Scene dock
2. Click the "Attach Script" button (scroll/document icon) or right-click → Attach Script
3. Choose a location and name for the script
4. Click "Create"
5. Write your code in the Script editor

### Running Your Game:

1. Make sure you've set a main scene (Project → Project Settings → Application → Run)
2. Press F5 to run the main scene or F6 to run the current scene
3. Press F8 to stop the game
4. Check the Output and Debugger tabs for any errors

## Getting Help

- **Built-in Documentation**: Press F1 or click Help → Search Help to access comprehensive documentation
- **Tooltips**: Hover over buttons and properties to see helpful tooltips
- **Official Documentation**: Visit [docs.godotengine.org](https://docs.godotengine.org/) for detailed guides
- **Community**: Ask questions on the [Godot Forums](https://forum.godotengine.org/) or [Discord](https://discord.gg/godotengine)

---

Now that you understand the Godot editor interface, you're ready to start creating games! Head back to the [lessons](index.md#activities) to begin learning how to use Godot effectively.
