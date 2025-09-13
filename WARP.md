# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

This is a Pac-Man clone written entirely in AppleSoft BASIC for the Apple II computer. The game features a 19×15 maze, arcade-inspired ghost AI with multiple behavioral states, power pellets, bonus items, and three difficulty levels. The project targets Apple II emulators rather than real hardware due to performance limitations.

## Key Files

- `Applesoft Pacman by Philip Lord.txt` - The main BASIC source code (590+ lines)
- `Applesoft Pacman by Philip Lord.po` - ProDOS disk image for Apple II emulators
- `README.md` - Project documentation with gameplay features and usage instructions

## Running and Testing

### Emulator Setup
The game is designed for Apple II emulators, specifically:
1. **Virtual ][ for macOS** - Set speed to maximum
2. **Apple2TS online emulator** - Load the .po file, set speed to "Ludicrous (16MHz)", use "Fast" game speed

### Basic Testing Commands
Since this is AppleSoft BASIC, traditional unit testing doesn't apply. Testing involves:
- Loading the program in an Apple II emulator
- Verifying game mechanics (movement, collision detection, scoring)
- Testing different maze levels (press "2" to switch)
- Checking save/restore functionality for high scores and settings

## Code Architecture

### Core Game Loop Structure
The program follows a traditional BASIC structure with numbered lines and GOTO/GOSUB control flow:

- **Lines 250-640**: Main game loop handling input, movement, collision detection
- **Lines 670-750**: Pac-Man movement logic with tunnel wrapping
- **Lines 990-3090**: Ghost AI system with BFS pathfinding
- **Lines 5000+**: Utility subroutines for HUD updates, level management, animations

### Key Systems

#### Ghost AI (Lines 990-3090)
- **Individual Personalities**: Each ghost has distinct targeting behavior
  - Blinky (Red): Direct chase
  - Pinky (Pink): Anticipates player movement 4 spaces ahead
  - Inky (Blue): Complex positioning relative to Blinky and player
  - Clyde (Orange): Alternates between chase and corner retreat
- **State Management**: Normal chase, scatter, and frightened modes
- **BFS Pathfinding**: Used for "eaten ghost" return-to-home animation

#### Memory Management
- **Persistent Storage**: Uses PEEK/POKE to memory addresses 24576+ for:
  - High scores (5 bytes)
  - Custom control keys (4 bytes)
  - Game speed setting (1 byte)
  - First-run flag (1 byte)
- **Dynamic Arrays**: Multiple DIM statements for game state, pathfinding queues

#### Level System (Lines 7500+)
- **Multiple Mazes**: 4 different maze layouts stored as DATA statements
- **Level Progression**: Automatic advancement when all dots collected
- **Maze Data Format**: String-based with characters representing walls (#), dots (.), power pills (+), ghost home (-), and empty space

### Display System
- **Text Mode Only**: Uses Apple II 40-column text mode
- **Character Mapping**: 
  - Pac-Man: `<>^v` based on direction
  - Ghosts: `BPIC` (Blinky, Pinky, Inky, Clyde)
  - Power pills: Alternating `+*` for animation
  - Bonus items: `$` with spinning animation
- **HUD Management**: Incremental updates to avoid screen flicker

## Development Notes

### BASIC Programming Constraints
- **Line Number Management**: Code uses increments of 10 with some tight spacing
- **Variable Limitations**: Single/double character variable names only
- **Memory Constraints**: Careful management of arrays and string storage
- **Performance**: Delay loops used for timing instead of hardware timers

### Key Algorithms
- **Collision Detection**: Direct coordinate comparison between Pac-Man and ghosts
- **Maze Navigation**: Grid-based movement with wall detection
- **Animation Timing**: Counter-based systems for power pill pulsing and ghost state changes
- **Pathfinding**: Breadth-First Search implementation for ghost navigation

### Game State Persistence
The game uses specific memory addresses for persistent data:
- High score and game settings survive program restarts
- Custom key mappings are preserved
- First-run detection triggers setup screens

## Common Operations

### Loading in Emulator
1. Boot Apple II emulator with the .po disk image
2. Game auto-starts, or type `RUN` if needed
3. Configure controls and speed on first run

### Code Modification
- Edit the `.txt` file (properly formatted BASIC)
- Import into emulator or convert to disk image
- Line numbers must be sequential and properly spaced
- Test all game states after modifications

### Adding New Mazes
1. Add DATA statements following the pattern at lines 11100+
2. Update level counter logic in line 7352
3. Maintain 19×15 character format with proper maze elements

### Performance Tuning
- Adjust delay loops in subroutine 630 (line S1 variable)
- Modify ghost movement frequency
- Balance animation speeds with gameplay responsiveness

## Performance Optimization

### Optimized Version Available
An optimized version (`Applesoft Pacman OPTIMIZED.txt`) addresses major performance bottlenecks:

**Key Improvements:**
- **Reduced delay loops**: Main game speed reduced from 500/250/100 to 50/20/5 iterations
- **Simplified ghost AI**: Removed complex individual personality algorithms, all ghosts use direct chase
- **Eliminated BFS pathfinding**: Replaced complex "eaten ghost" animation with simple flashing
- **Optimized power pill updates**: Only redraw every 4th frame instead of every frame
- **Faster animations**: Reduced death animation from 16 to 8 cycles, bonus animations from 4 to 2 cycles
- **Shortened countdown delays**: Reduced from 30000 to 15000 iterations per number

**Performance Impact:**
- Should run smoothly at 2-4MHz emulator speeds instead of requiring 14MHz+
- Maintains all core gameplay mechanics while dramatically improving responsiveness
- Compatible with original save states and control settings

### Original Performance Issues
The original version suffers from:
- Excessive delay loops in main game cycle (line 630)
- Complex BFS algorithm for ghost pathfinding (lines 6000-6430)
- Frequent power pill redraws causing flicker (line 3500)
- Overly detailed ghost AI with individual behaviors (lines 2380-3090)
- Long animation sequences with heavy delay loops

## Technical Constraints

- **AppleSoft BASIC limitations**: No structured programming constructs
- **Memory footprint**: Must fit in Apple II RAM constraints
- **Performance**: Original targets 14MHz+ emulators, optimized version runs at 2-4MHz
- **Display**: Limited to text characters, no high-resolution graphics
- **Input**: Keyboard polling only, no joystick support in current implementation
