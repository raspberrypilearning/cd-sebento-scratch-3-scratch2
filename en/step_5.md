## Super power-ups!

Now that you have a new power-up working, it’s time to make it do something cool! Let's make it 'rain' power-ups for a few seconds, instead of just giving out an extra life.
 
For this you need to create another piece of code that will start while the `react-to-player`{:class="blockmoreblocks"} block finishes running. To make that happen, you'll use a `broadcast`{:class="blockevents"} block to send a message to another piece of code inside this sprite. 

+ Create this block for the `Collectable` sprite. Let’s call the broadcast `collectable-rain`{:class="blockevents"}.

```blocks
    when I receive [collectable-rain v]
    set [collectable-frequency v] to [0.000001]
    wait (1) secs
    set [collectable-frequency v] to [1]
```

--- collapse ---
---
title: What does the new code do?
---

This block just sets `collectable-frequency`{:class="blockdata"} to a very small number \(change it to different values and see what happens!\) and then waits a second and changes it back to `1`.

This doesn’t look like it should do much, but think about what’s happening during that second: the `when green flag clicked`{:class="blockevents"} code is still running, and the `repeat until`{:class="blockcontrol"} loop in it is looping. Look at the code in that loop: 

```blocks
    repeat until <not <(create-collectables) = [true]>>
        if < [50] = (pick random (1) to (50))> then
            set [collectable-type v] to [2]
        else
            set [collectable-type v] to [1]
        end
        wait (collectable-frequency) secs
        go to x: (pick random (-240) to (240)) y:(-179)
        create clone of [myself v]
    end
```

You can see that the `wait` block here pauses the code for the length of time set by `collectable-frequency`{:class="blockdata"}. So if the value of `collectable-frequency`{:class="blockdata"} changes to `0.000001`, the `wait` block only pauses for **one millionth** of a second, meaning that the loop will run many more times than normal. As a result, the code is going to create **a lot** more power-ups than it normally would, until `collectable-frequency`{:class="blockdata"} changes back `1`. Can you think of any problems that might cause? There’ll be a lot more super-farts…what if you kept catching them?

--- /collapse ---

Now you have the sprite ready to receive the `collectable-rain`{:class="blockevents"} broadcast block, but you haven't made code for sending the broadcast yet. 

+ This next part’s easy. Just update the `react-to-player`{:class="blockmoreblocks"} block to look like this, so it broadcasts `collectable-rain`{:class="blockevents"} when the player touches a type `2` power-up. 

```blocks
    define react-to-player (type)
    if <(type) = [1]> then
        change [points v] by (collectable-value)
    end
    if <(type) = [2]> then
        broadcast [collectable-rain v]
    end
```

### Challenge: get creative!
 
+ Based on this card and the previous one, you can now make as many different power-ups as you want! What about one that gives out 20 times the usual number of points, or adds three lives, or makes it so the player can’t run out of lives for a period of time? Come up with some cool power-ups and see if you can make them!
