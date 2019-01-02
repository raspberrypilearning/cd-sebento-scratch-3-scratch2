## Losing the game

You may have noticed that the `lose`{:class="blockmoreblocks"} **More** block on the `Player Character` sprite is empty. You’re going to fill this in and set up all the pieces needed for a nice 'Game over' screen.

+ First, find the `lose` block and complete it with the following code: 

```blocks
    define lose
    stop [other scripts in sprite v] :: control stack
    broadcast [game over v]
    go to x:(0) y:(0)
    say [Game over!] for (2) secs
    say [It's pretty much impossible to catch all the methane, right?] for (5) secs
    say [It would be better to reduce the amount produced in the first place.] for (6) secs
    say [By considering the consequences of how we produce food...] for (5) secs
    say [...we can do it in a more sustainable way that's better for everyone.] for (6) secs
    stop [all v]
```

--- collapse ---
---
title: What does the code do?
---

Whenever the `lose`{:class="blockmoreblocks"} block runs, what it does is: 

 1. Stop the physics and other game scripts on the `Player Character`
 2. Tell all the other sprites that the game is over by **broadcasting** a message so they can change based on that
 3. Move the `Player Character` to the centre of the screen and have them tell the player that the game is over
 4. Stop all scripts in the game

--- /collapse ---

Now you need to make sure all the sprites know what to do when the game is over, and how to reset themselves when the player starts a new game. **Don’t forget that any new sprites you add also might need code for this!**

### Hiding the platforms and edges

+ Start with the easy ones. The `Platforms` and `Edges` sprites both need code for appearing when the game starts and disappearing at 'Game over', so add this to each of them:

```blocks
    when I receive [game over  v]
    hide
```

```blocks
    when green flag clicked
    show
```

### Stopping the farts

Now, for something a little more tricky! If you look at the code for the `Collectable` sprite, you’ll see it works by **cloning** itself. That is, it makes copies of itself that follow the special `when I start as a clone`{:class="blockevents"} instructions. 

We’ll talk more about what makes clones special when we get to the card about making new and different collectables. For now, what you need to know is that clones can do **almost** everything a normal sprite can, including receiving `broadcast`{:class="blockevents"} messages.

+ Let’s look at how the `Collectable` sprite works. See if you can understand some of its code: 

```blocks
    when green flag clicked
    set size to (35) %
    hide
    set [collectable-value v] to [1]
    set [collectable-speed v] to [1]
    set [collectable-frequency v] to [1]
    set [create-collectables v] to [true]
    set [collectable-type v] to [1]
    repeat until <not <(create-collectables) = [true]>>
        wait (collectable-frequency) secs
        go to x: (pick random (-240) to (240)) y: (-179)
        create clone of [myself v]
    end
```

 1. First it makes the original collectable invisible.
 2. Then it sets up the control variables. We’ll come back to these later.
 3. The `create-collectables`{:class="blockdata"} variable is the on/off switch for cloning: the loop creates clones if `create-collectables`{:class="blockdata"} is `true`, and does nothing if it’s not.

+ Now you need to set up a block on the `Collectable` sprite so that it reacts to the `game over` broadcast:

```blocks
    when I receive [game over v]
    hide
    set [create-collectables v] to [false]
```

This code is similar to the code controlling the `Edges` and `Platforms` sprites. The only difference is that you’re also setting the `create-collectables`{:class="blockdata"} variable to `false` so that no new clones are created when it's 'Game over'. 
 
+ Note that you can use this variable to pass messages from one part of your code to another! 
