Light Jumper 💡 - Game Overview

🎯 Core Concept

Light Jumper is a unique platformer game with a twist: all platforms are invisible. The only way to see them is by jumping. Each jump emits a pulse of light that briefly reveals the platforms around the player, creating a memory-based challenge where players must jump to see, memorize the layout, and land safely.

Global Constants

Screen Settings

SCREEN_WIDTH = 1000    # Width of the game window in pixels
SCREEN_HEIGHT = 700    # Height of the game window in pixels
FPS = 60               # Frames per second for game loop

Colors

BACKGROUND = (10, 10, 30)                 # Dark blue background
PLAYER_COLOR = (255, 215, 0)              # Gold - player character
PLATFORM_COLOR = (70, 130, 180)           # Steel blue - regular platforms
PLATFORM_HIGHLIGHT = (100, 180, 255)      # Light blue - platform glow
MOVING_PLATFORM_COLOR = (180, 70, 130)    # Purple-pink - moving platforms
MOVING_PLATFORM_HIGHLIGHT = (220, 100, 180) # Light purple - moving platform glow
GOAL_COLOR = (50, 205, 50)                # Lime green - goal door
LIGHT_PULSE_COLOR = (255, 255, 200)       # Soft white-yellow - light pulse
TEXT_COLOR = (255, 255, 255)              # White - UI text
DANGER_COLOR = (220, 20, 60)              # Crimson red - danger zones

Physics Parameters

GRAVITY = 0.8           # Downward acceleration per frame
JUMP_STRENGTH = -16     # Initial upward velocity when jumping
PLAYER_SPEED = 7        # Horizontal movement speed
LIGHT_RADIUS = 250      # How far the light pulse reaches
LIGHT_DURATION = 20     # How long light pulse lasts (frames)


Player Class

Variables

x, y: float             # Current position (top-left corner)
width, height: int      # Player dimensions
vel_x, vel_y: float     # Horizontal and vertical velocity
on_ground: bool         # True if player is standing on a platform
jumping: bool           # True during jump animation
facing_right: bool      # Direction player is facing
light_pulse: int        # Counter for light pulse effect
jump_count: int         # Total jumps made in current level
lives: int              # Remaining lives (starts at 3)
invincible: int         # Frames of invincibility after taking damage

Methods

move(platforms, dangers)

Purpose: Updates player position and handles collisions
Applies gravity to vertical velocity
Updates position based on velocity
Checks collisions with platforms and dangers
Handles screen boundaries and falling off screen
Updates invincibility timer

jump()

Purpose: Makes the player jump
Only works when player is on ground
Sets upward velocity
Triggers light pulse effect
Plays jump sound
Increments jump counter

draw(screen)

Purpose: Renders the player on screen
Draws light pulse effect when active
Draws player body with flashing effect during invincibility
Draws face and directional eyes
Adds a small light above player's head


Platform Class

Variables

x, y: float             # Current position
width, height: int      # Platform dimensions
revealed: bool          # Whether platform is currently visible
reveal_timer: int       # How long platform stays visible
is_moving: bool         # True for moving platforms
original_x, original_y: float  # Starting position for moving platforms
move_direction: int     # 1 for right, -1 for left
move_speed: int         # Pixels per frame movement speed
move_range: int         # How far platform moves from origin

Methods

update(player)

Purpose: Updates platform state
Moves platform if it's a moving type
Checks if player's light pulse reveals this platform
Updates reveal timer



draw(screen)

Purpose: Renders the platform
Draws glow effect when revealed
Draws platform body with rounded corners
Adds pattern details on top
Shows direction arrows for moving platforms


Danger Class

Variables

x, y: float             # Current position
width, height: int      # Danger zone dimensions
revealed: bool          # Whether danger is currently visible
reveal_timer: int       # How long danger stays visible
is_moving: bool         # True for moving dangers
original_x, original_y: float  # Starting position
move_direction: int     # Movement direction (1 or -1)
move_speed: int         # Movement speed
move_range: int         # Movement range from origin
pulse: float            # Animation counter for pulsing effect

Methods

update(player)

Purpose: Updates danger zone state
Moves danger if it's a moving type
Updates pulsing animation
Checks if player's light pulse reveals this danger
Updates reveal timer

draw(screen)

Purpose: Renders the danger zone
Draws red zone with warning stripes
Uses pulsing effect for visibility


Goal Class

Variables

x, y: float             # Position of goal door
width, height: int      # Goal dimensions
revealed: bool          # Whether goal is visible
reveal_timer: int       # Visibility duration
pulse: float            # Animation counter for pulsing effect

Methods


update(player)

Purpose: Updates goal state
Updates pulsing animation
Checks if player's light pulse reveals the goal
Updates reveal timer

draw(screen)

Purpose: Renders the goal
Draws glowing green door
Adds door handle detail
Creates animated light beam above door

check_collision(player)

Purpose: Checks if player reached the goal
Returns: True if player collides with goal, False otherwise


Particle Class

Variables

x, y: float             # Current position
color: tuple           # Particle color (RGB)
size: float            # Current size
speed_x, speed_y: float # Movement velocity
lifetime: int          # Frames until particle disappears

Methods

update()

Purpose: Updates particle state
Moves particle based on velocity
Applies gravity
Reduces size and lifetime
Returns: True if particle still alive, False if expired

draw(screen)

Purpose: Renders the particle
Draws as a circle with alpha transparency based on lifetime

Level Class

Variables

level_num: int          # Current level number (1-8+)
platforms: list         # List of Platform objects
dangers: list           # List of Danger objects
goal: Goal              # Goal object for this level
player_start: tuple     # (x, y) starting position for player
Methods

setup_level()

Purpose: Creates level layout based on level number

Level 1: Basic platforms for learning
Level 2: Introduces moving platforms
Level 3: Adds danger zones
Level 4: Complex layout with moving platforms and dangers
Level 5: Challenging layout with narrow platforms
Level 6+: Randomly generated levels with increasing difficulty

generate_random_level()

Purpose: Creates random level for levels beyond 5
Generates platforms with decreasing size as level increases
Adds dangers with increasing probability
Creates progressively more challenging layouts


Game Class

Variables

clock: pygame.Clock     # Game clock for FPS control
level_num: int          # Current level number
max_level: int          # Maximum level number (8)
level: Level            # Current level object
player: Player          # Player object
particles: list         # List of active particles
game_state: str         # "playing", "win", "game_over"
win_timer: int          # Timer for win screen display
level_transition_timer: int     # Timer for level transitions

Methods

reset_game()

Purpose: Resets game to initial state for current level
Creates new level object
Resets player position and stats
Clears particles
Sets game state to "playing"

next_level()

Purpose: Advances to next level
Increments level number (loops back after max level)
Plays level transition sound
Calls reset_game() for new level



handle_events()

Purpose: Processes user input
Handles window close event
Processes keyboard input (movement, jumping, restart)
Updates player velocity based on key states

update()

Purpose: Updates all game objects and logic
Updates player position and collisions
Updates platforms, dangers, and goal
Checks win/lose conditions
Manages game state transitions
Updates particles

draw()

Purpose: Renders entire game scene
Draws background with stars
Renders all game objects in correct order
Calls UI drawing method
Updates display

draw_ui()

Purpose: Renders user interface elements
Level number, jump counter, lives display
Game instructions during gameplay
Win/game over screens with statistics
Level transition messages

run()
Purpose: Main game loop
Continuous loop that handles events, updates, and draws
Maintains consistent FPS


Main Game Loop

if __name__ == "__main__":
    game = Game()        # Create game instance
game.run()           # Start main game loop

Game Flow:

Initialization: Create Game object which sets up first level

Main Loop: Continuous cycle of:

Handle user input
Update game state
Render graphics


Level Progression: Complete levels to advance

Game States:

Playing: Normal gameplay
Win: Level completed, show statistics
Game Over: No lives remaining, restart from level 1


