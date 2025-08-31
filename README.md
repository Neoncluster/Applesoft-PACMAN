# PAC-MAN in AppleSoft BASIC
____________________________

After countless prompts with ChatGPT and Gemini, and plenty of hands-on debugging, I managed to build a fully playable Pac-Man clone for the Apple II—entirely in AppleSoft BASIC, using only text mode. The twist? On real Apple II hardware it runs far too slow, so it’s best enjoyed in an emulator like Virtual ][ for macOS with the speed turned up.

My aim was to make it as fun and faithful as possible while sneaking in simple animations: a spinning ‘$’ bonus, pulsating power pills, and ghosts that visibly shift during frightened mode to show time running out.

<img width="792" height="575" alt="Pacman Applesoft" src="https://github.com/user-attachments/assets/4d34f8b0-929b-4b65-95f8-b1603eb91e8b" />

# Highlights:

- 19×15 maze, fast and clean gameplay
- Arcade-inspired ghost AI with chase, scatter, and frightened states
- BFS-based logic giving each ghost a distinct “personality”
- Power pellets that flip the hunt in your favor
- Smooth HUD updates and level progression
- Multiple mazes (press 2 to switch)
- Tunnel shortcuts, ghost home, and bonus items
- Three speed/difficulty settings
- Custom key controls
- High score tracking
- Press 1 to quit

# Notes from forker:
- Corrected original txt file BASIC order for CiderPress importing from TXT to BAS
- Added a po disk image for emulator to boot and run
- Play it online for average performance: 1. download po file, 2. Go [Apple2TS](https://apple2ts.com/), 3. click S6D1 floppy drive and load po file, 4. Set emulator speed to Ludicrous (16MHz), 5. Run the game, set game speed to Fast. 
