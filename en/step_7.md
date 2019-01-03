## Level 2

With this card, you're going to add a new level to the game that the player can get to by just pressing a button. Later, you can change your code to make it so they need a certain number of points, or something else, to get there.

### Moving to the next level

+ First, create a new button sprite by either adding it from the library or drawing your own. I did a bit of both and came up with this: 

![The button sprite to switch levels](images/levelButton.png)

Now, the code for this button is kinda clever: it’s designed so that every time you click it, it will take you to the next level, however many levels there are.

+ Add these scripts to your button sprite: 

```blocks
    when green flag clicked
    set [max-level v] to [2]
    set [min-level v] to [1]
    set [current-level v] to [1]
```

```blocks
    when this sprite clicked
    change [current-level v] by (1)
    if <(current-level) > (max-level)> then
        set [current-level v] to (min-level)
    end
    broadcast [collectable-cleanup v]
    broadcast (join [level-](current-level))
```
 
+ `max-level`{:class="blockdata"} is the highest level
+ `min-level`{:class="blockdata"} is the lowest level
+ `current-level`{:class="blockdata"} is the level the player is on right now

+ These all need to be set by the programmer \(you!\), so if you add a third level, don’t forget to change the value of `max-level`{:class="blockdata"}!

The code uses broadcasts to tell the other sprites which level to display, and to clear up the collectables.

### Make the sprites react

Now you need to get the other sprites to respond to these broadcasts! Start with the easy one: clearing all the collectables.  

+ Just tell all the existing clones to `hide` by adding this to the `Collectable` sprite: 

```blocks
    when I receive [collectable-cleanup v]
    hide
```

Since one of the first things any new clone already does is show itself, this new code means means you don’t even have to worry about turning this behaviour off for them!

Now to switch the `Platforms` sprite. You can design your own new level later if you like, but for now let’s use the one I’ve already included — you’ll see why on the next card! 

+ Add this code to the `Platforms` sprite:

```blocks
    when I receive [level-1 v]
    switch costume to [Level 1 v]
    show
```

```blocks
    when I receive [level-2 v]
    switch costume to [Level 2 v]
    show
```

It receives the `joined`{:class="blockoperators"} messages of `level-`{:class="blockdata"} and `current-level`{:class="blockdata"} that the button sprite sends out, and responds by changing the `Platforms` costume. 

+ For the `Enemy` sprite, you just need to make sure it disappears when the player enters level 2, like this: 

```blocks
    when I receive [level-1 v]
    show
```

```blocks
    when I receive [level-2 v]
    hide
```
If you prefer, you can make the enemy move to another platform instead. In that case, you would use a `go to`{:class="blockmotion"} block instead of the `show`{:class="blocklooks"} and `hide`{:class="blocklooks"}.

Whenever a new level starts, the `Player Character` sprite needs to go to the right place for that level. To make this happen, you need to change where the sprite gets its coordinates from when it first appears on the Stage. At the moment, there are fixed `x` and `y` values in its code.

+ Begin by creating variables for the starting coordinates, `start-x`{:class="blockdata"} and `start-y`{:class="blockdata"}. Then plug them into the `go to`{:class="blockmotion"} block in `reset-character`{:class="blockmoreblocks"} instead of the `x` and `y` values:

```blocks
    define reset-character
    set [can-jump v] to [true]
    set [x-velocity v] to [0]
    set [y-velocity v] to [-0]
    go to x: (start-x) y: (start-y)
```

+ Then for each broadcast announcing the start of a level, set the right `start-x`{:class="blockdata"} and `start-y`{:class="blockdata"} coordinates in response, and add a **call** to `reset-character`{:class="blockmoreblocks"}:

```blocks
    when I receive [level-1 v]
    set [start-x v] to [-183]
    set [start-y v] to [42]
    reset-character :: custom
```

```blocks
    when I receive [level-2 v]
    set [start-x v] to [-218]
    set [start-y v] to [-143]
    reset-character :: custom
```

### Starting at Level 1

You also need to make sure that every time someone starts the game, the first level they play is Level 1.

+ Go to the `reset-game`{:class="blockmoreblocks"} script and remove the call to `reset-character`{:class="blockmoreblocks"}. In its place, broadcast the `min-level`{:class="blockdata"}. The code you've already added with this card will then set up the correct starting coordinates and call `reset-character`{:class="blockmoreblocks"}.

```blocks
    define reset-game
    set size to (35) %
    set rotation style [left-right v]
    set [jump-height v] to [15]
    set [gravity v] to [2]
    set [x-speed v] to [1]
    set [y-speed v] to [1]
    set [lives v] to [3]
    set [points v] to [0]
    broadcast (join [level-](min-level))
```

--- collapse ---
---
title: Resetting the character and resetting the game
---

Notice that the first block in the `Player Character` sprite's main green flag script is a call to the `reset-game`{:class="blockmoreblocks"} **More** block. 

This block sets up all the variables for a new game and then calls the `reset-character`{:class="blockmoreblocks"} **More** block, which places the character back in its correct starting position.

Having the `reset-character`{:class="blockmoreblocks"} code in its own block separate from `reset-game`{:class="blockmoreblocks"}  allows you to reset the character to different positions **without** having to reset the whole game.

--- /collapse ---
