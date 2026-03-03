+++
date = '2026-03-03T17:31:09+01:00'
draft = false
title = 'Green Snake'
+++
## Green Snake

{{< button href="/files/zw.7z"  >}} Download {{< /button >}}

### About the Program

This game was written as a tribute to the original *Oil's Well* from the 1980s — first in *Python* using *PyQt*, and later rewritten in *C++* for the *Windows* operating system.

{{% columns %}}

- {{< card title="Card" image="images/game-en-small.jpg" >}}
  Normal mode
  {{< /card >}}
- {{< card title="Card" image="images/game-frozen-en-small.jpg" >}}
  Frozen mode
  {{< /card >}}

{{% /columns %}}

### Installation

The program is distributed as a *7-Zip archive* containing a single executable file, which is fully sufficient to run the game.

Simply extract the archive anywhere you like — creating a separate directory is not required.

The default font is rather unattractive, so it is recommended to download and install **C64 Pro** (for example from: [https://style64.org/release/c64-truetype-v1.2.1-style](https://style64.org/release/c64-truetype-v1.2.1-style)).

### User Manual

You can navigate the menu using the arrow keys.
To confirm a selection, press **Enter** or choose the desired option using the left mouse button.

Language switching is possible only with the mouse.

The program does not create or read any files. User settings (the last selected game mode along with the high score table) are stored in the Windows registry, separately for each computer user.

### The Game

The player's objective is to complete all game levels (currently: six) by properly controlling the green snake so that it visits all fields containing yellow squares, purple diamonds, or gold coins.

Collecting these objects (“eating objects”) awards the player a corresponding number of points.

Collecting the last object completes the level. A limited amount of time is provided to collect all objects — the bottom status bar shows the remaining time.

If the allotted time runs out before all objects are collected, the game ends.
Each level has its own time counter, which increases with the level’s difficulty.

Movement is controlled using the arrow keys. Additionally, pressing the **Backspace** key allows the snake to move backward regardless of direction.

*Creatures* moving from left to right and vice versa can be safely consumed, but they must not touch the snake’s body (otherwise a collision occurs).

The *white-and-red ghost* is harmless to the snake’s body but must not touch its head (this also results in a collision).

**Three collisions** automatically result in **game over**.

Swallowing *blue orbs* freezes the timer for 20 seconds and stops the movement of the *creatures*. Swallowing another blue orb resets the freeze timer back to 20 seconds.

### No Time Limit Mode

* (The no time limit mode was invented by my eight-year-old son, Antoni.) *

After collecting a gold coin, a *sword* appears at a random location — collected swords are counted in the lower right corner.

In this mode, collisions do not occur and the time counter is disabled.

This mode will probably not be further developed and may even disappear from the selection menu one day.

### Contact

If you would like to share feedback, report a bug, or simply write a message, please use this email address: <marcin.bielewicz@gmail.com>

### Miscellaneous

This program would not have been created without:

- the *Emacs* editor,
- *GIMP*,
- *Inkscape*,
- the *MSYS2* toolchain,
- and my eight-year-old son, *Antoni*.

The prototype was made possible thanks to a *Raspberry Pi* computer running *Ubuntu Linux*, the *Emacs* editor, the *Python 3* programming language, the *PyQt 5* library, and the programs *GIMP* and *Inkscape*.

I no longer remember who provided the sound effects used in the game (if you are the author, please contact me!), but I personally wrote every line of code and created all graphics from scratch.

The program may be freely distributed and played without limitations.
