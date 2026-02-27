+++
date = '2026-02-25T17:31:09+01:00'
draft = false
title = 'Spaceshooter'
+++

## Spaceshooter

{{< button href="https://pypi.org/project/spaceshooter"  >}} Install {{< /button >}}

{{< button href="https://github.com/lvajxi03/spaceshooter"  >}} GitHub project page {{< /button >}}

### About

SpaceShooter is a _side-scroller shooter_ game: a spaceship travels from left to right and wants to get through the enemies until it will beat the boss.

At first, it was inspired by _Newcomers from Planet Math_, popular Polish TV series from 80s, and especially their spaceship, which, at the end, became a boss to beat. Enemies were to stay at the board until they're beaten, but later they started moving from right to left.

{{% columns %}}

- {{< card title="Card" image="images/spaceshooter1.jpg" >}}
    Default player, medical kit
    {{< /card >}}
- {{< card title="Card" image="images/spaceshooter2.jpg" >}}
  Zeppelin, lightballs, medical kit, shield and a comet.
  {{< /card >}}

{{% /columns %}}

This program is made as a prototype, written in `Python` against `PySide` library. Target wersion -- if even created -- will be written in `C++` against `SDL` or pure `WinAPI`.

### HW requirements

Any computer, where you can run `Python 3.10` or newer.

### Installation

You need `Python` version `3.10` or newer and `pip`. Then you need to open a terminal and type:

```sh
python -m pip install spaceshooter
```

All dependencies will be also installed.

### Running

![Program menu](images/spaceshooter-en.jpg)

To start the program, type the following in the terminal:

```sh
python -m pip spaceshooter
```

The game is looking by default for a font `Commodore 64 Rounded` -- otherwise it will use the default (which is, of course, ugly); please install something better  and force using this with the option as below:

```sh
pip -m spaceshooter -f "Best font I found in the Internets"
```

>[!WARNING]
>**Warning**  
>Remember about double quotes!

On the next run, font will be used again, as it's stored in the configuration file now.

To reset all the settings (_including hiscores_, so beware!), you may use:

```sh
python -m spaceshooter -r
```

This will run the program with default settings.

>[!INFO]
>**Info**  
>Options can be combined.

### Gamers' manual

The goal is, as mentioned, to kill as much enemies, as it's possible, and their boss at the end. You need to use missiles, lightballs, TNTs or bombs. Plain missiles and bombs are unlimited, where TNT must be first caught (the player has 3 TNTs at the startup), lightballs, when you caught them, work for 10s.

Every game starts with 3 lives, where every of them contains 10 minipoints. Every shoot at the player removes one minipoint. Game ends, where no lifes are present. Minipoints can be restored by catching those medical kits, flying there or there.

There are four difficulty levels:

* easy,
* normal,
* hard,
* immortal.

Easy level means that player can collide only with enemies and missiles (also these from the guns). Normal level is when the player collides also with comets, and the hard one when it collides also with buildings. Being immortal means -- surprise, surprise! -- _you're immortal_.

Here's sample gameplay (click on the image to see it):

[![Thumbnail](https://img.youtube.com/vi/QGefyk6W11I/0.jpg)](https://www.youtube.com/watch?v=QGefyk6W11I)

Using TNT causes 3 enemies to be dead immediately.

Apart from medical kids, TNTs and lightballs you can catch ice boxes (and freeze enemies for 10s) and shields -- for 10s player can be immortal.

There are five game levels, with different scenarios and enemies. At the end you need to kill the boss. Boss is trying to kill you with three missiles at once, flying in three directions. Boss can be killed by your 10 missiles.

**Settings** section contains all the keys that can be remapped if necessary.

Latest settings, difficulty levels and hiscores are saved to `~/.spaceshooterrc`

### Kudos

Big thanks for my 10yrs old son, _Antoni_, who was improving all my game concept and created all of these flying objects (_shields_, _iceboxes_ and so on); also he reworked most of the game logic.

### Misc

First _draft_ was created at the end of 2020, when I was desperately looking for a new job. I put a post to _LinkedIn_ with my proof of concept and the message "looking for ANY job". Graphics were created with crayons on A4 paper, then scanned and used in the code.

Second iteration replaced all those crayon graphics with files made in _Inkscape_ and _GIMP_.

There is no sound, so far, but maybe one day we will record some (who knows?)

### Contact

If you want to share your impressions, report a bug, or just write something, please use <a href="mailto:marcin@iostream.pl?subject=spaceshooter">this address</a>

{{< button href="https://pypi.org/project/spaceshooter"  >}} Install {{< /button >}}



Python Package Index project page: <a href="https://pypi.org/project/spaceshooter">https://pypi.org/project/spaceshooter</a>
