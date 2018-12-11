# Writeup of BESTESTGAME's Scratch code

First things first: This project was very difficult for me. Some of the code is going to be rather ugly and sub-optimal as Scratch is a step back from what I'm used to. Second, I'm splitting this up by the entity that holds the script because there are a lot.

## General Patterns used
- Backdrops are used as a cheap way to know what state the game should be in, and to have sprites act accordingly
- Individual Sprites should be relatively self contained, passing information through global variables and events if necessary (only global (ab)use was required this time.
- `Wait until` blocks in another event "thread" used to wait for a condition and act on it.

## Scene
![](https://lh3.googleusercontent.com/a8fQkg_WF9-pX8CAJppUrnpp0ovLK6_SxMPjus1tGs2DmhHkY7LgbFFbyomdam5tJYYeWtM7YtRH "Scene Overview")

The scene has 3 distinct script blocks.

### Green Flag

![Green Flag](https://lh3.googleusercontent.com/asH1gF5yNcJ9x8wnVZKrZfSPQvgFpg54yhcYLHCGChg3Kzj573F9jSkauhVAVyw7Ww0Ci09VjqNS)

The Green Flag script block of the Scene is very simple. All it does is broadcast out a signal that tells everything to reset (which ended up barely being used as the green flag is universal), switch the backdrop to the "GAME YAY" background, and resets the Coins variable, as the Scene is in charge of the general state of the coin catching minigame.

### City Game

![City Game](https://lh3.googleusercontent.com/7TNj7kOkRXhL8lCn65m16O9JGF1ag_N81ylJwWO1UnYh7Vp7nETwVXalaHImBsvE6MPyTCfKnXFT)

The city game script is also very simple. All it does is ensure that the variable for the Coins is all cleared out, show it to the player as a "Coin counter", Wait until the player has more than 19 coins, and when that's hit it broadcasts a message to the other sprites saying that the game is over.

### On click
![Stage Clicked](https://lh3.googleusercontent.com/PMipoDsc7uNK98W8lw_XYV_uIo56dv9-xa3SMM2JCCugoE--m0sUJ4L61KokAE0jMvJYsogKReHt)

This script was mostly a lazy way to pull off the start button. It avoids adding another sprite, because sprite management is hard when you can't have a "supervising" piece of code (Scene is the closest thing we have, but there's no encapsulated way to say "hide all of x" other than making an event for it). First checks if the backdrop number is 1, which is the number of the GAME YAY start screen. If the backdrop is 1, then it asks for the name, and uses`join` operators to join the name into a string. I didn't store the name in a variable, as it quickly became apparent that the `join` operator being the only way to join strings would get quickly annoying. I would've carried the name on if there was something like Python's f-strings or a `printf` style solution for string substitution. It then switches the backdrop to sprite_1, where everything else up to the city game is managed by `CUTSCENEMAN` and the Elevator sprite. I'm not sure why the else case is there, as it's pretty much a NOP, but it's there. 

## CUTSCENEMANNNNNNN

CUTSCENEMAN is the name of the larger version of the main character shown in the cutscenes. He's also a monolithic Sprite that's in charge of all of the cutscenes, and basically calls the shots.

Green flag is just a hide block for reset, as CUTSCENEMAN does nothing idlely.

![enter image description here](https://lh3.googleusercontent.com/8RmehMBqbN-nahexKXz_DUmIuOrXki3CHzrHsV6jpG6nZg0UkUDSHxTB-YvbYrDF3VizPCnbgJxx)

This is just to hide him when the coin catcher game starts. Not sure why that change x by 10 is there, probably left over from something

![enter image description here](https://lh3.googleusercontent.com/UXwaxQTDlLd5yYfQ_yOW24BOSgPHsrAG7qpV3CwHf9-_Kc5YvvtsgX064RygBKg1QOhWPz7icina)

This resets his position after the elevator scene.

![enter image description here](https://lh3.googleusercontent.com/MM41IN_xcKaW3Twi_BN9k8C4EKMz3vlKo4yFAibgEzdg7s6sxI4aWlzuiLxqVkVmsCHRoYFsXs8L)

This is the starting cutscene. The event calls are calls to the Elevator sprite to do it's part of the cutscene. Other than that there is little code other than the switch to the `citygame` backdrop (which also tells all sprites involved to initialize their state and become visible) and the think and say blocks in charge of making it all a cutscene.

![enter image description here](https://lh3.googleusercontent.com/Owrc5w21Dbf5RS7JrfEm32vj6yPDgMlpbexLA0LyfpczQwRoDuQcYV88VNz6BFmjBNerGx8_axp1)

This is the second cutscene just before the elevator. The elevators talking sprite does most of the work for this (part of) the cutscene so there's not much here

![enter image description here](https://lh3.googleusercontent.com/oivUoGdtrdp-KsEh7WPQJFjJ5vqnVsR2gGnfMAAj40aKFD63CSP3drpIbJNObrWSrOA6FlL4eydn)

This is the elevator cutscene. The first thing we do is have `CUTSCENEMAN`scream out the "ElevatorPlease" sound. Which is a cropped down version of the elevator music, as hour long mp3 files don't mix well with Scratch. He then waits a bit for effect, and then talks about the gun. He sends out the `GotGun?`event, which tells the gun to shake around and go rainbow, then sends `GotGub` which tells the gun to disappear. The cutscene ends with him changing the backdrop to the space lobby, then hiding himself and changing the backdrop to `"theoffice"`which initializes the boss fight.

## The Helicopter (aka Sprite1)
![enter image description here](https://lh3.googleusercontent.com/2mOFeGMN4rA-XyNhuufEB8rYScONRKr1I1DzXp8QiJz2wbamecGdzXxaQmdZeMgr9dTMYFJSzbh7)

These two just hide the sprite when it shouldn't show up. Also hide the BossHealth because my code is definitely organized well.

![enter image description here](https://lh3.googleusercontent.com/RJur--rMWtTBDDXJa2KPg-IgGxcOFWtotOgFZBEXNaEYhIz48VUgiMkT3cWsb7FdWIJxSyb-mPJI) 

This is the main logic of the helicopter. We show the sprite when we start the `citygame`. then in a `forever` loop, we move anywhere from 100 to 500 steps, bouncing off the sides of the screen if need be. Then we set `dropx` to the x position of the helicopter to tell the newly spawned coin where to start, then clone a coin.

![enter image description here](https://lh3.googleusercontent.com/9QJE8roRCJy5e-IGNSs_kOn1_VCoTMnFUHYK7KMfT-1TiWqpMFSERSBH9RHk82TlMMWxzCJepXiX)

This makes the sprite essentially lifeless when the city game ends. Basically the equivalent of a `break` statement in python for the forever loop.

## The City (Sprite2)
![enter image description here](https://lh3.googleusercontent.com/I4XVfOzTQnrTuoY7U_2DUYHi4uCSZmsiCOZ66Gq825n0vcpz1rSwk78B5td6KhVXNmJdhXpeCtTt)

This just makes sure that the buildings are visible while the game is going and invisible when it's not.

## The Coin (Ball)

This same sprite is used for both the citygame and the boss fight.

![enter image description here](https://lh3.googleusercontent.com/6Ns1QTmHKjZ5D_wVXatHxybYwmZSCeiUQaTfGvG-kS38nFvC5zYOKsJS7v-9yJC3ZhhCYnJ0YDjs)

Set the drop speed to the citygame level at the start of the game.

![enter image description here](https://lh3.googleusercontent.com/zv6PlcrDtL_lpnUC6o3mTYoDRf97aKNMi3QSF9FokcBFKJI5fkYRMQ90o_TObl-1mfgS50RKHUCH)

Sets the drop speed for the boss fight

![enter image description here](https://lh3.googleusercontent.com/JP38LxsRPfpzgxQkCLHU18WWm1pkF7wTLw1A1F210f3ROJeH6Q_s8R99W-IxWcGx87giSArvgp8I)

This code block does the actual dropping of the coin. It bring the ball clone to the position of the dropx, shows it, then repeats a drop until it touchs the edge of the screen, where we kill the clone if it ever gets there.

![enter image description here](https://lh3.googleusercontent.com/e0ctue2vGyAKF3314CCBv3iyZrH9STkJhoDzfRM19voEqqho9PumSJ6DIyZNBYA6iJJShLt8OvZC)

This is the collision logic for the coin. Code is stopped as soon as a clone is killed, so if it touches the edge these blocks will never trigger, and vise versa. TheChosenOne is the player controlled sprite during the boss fight, and sprite_0 is the player controlled sprite during the coin minigame, so these two increment the variable that's part of their own section of game.

## The Elevator (Sprite3)

The Elevator is actually an invisible sprite on top of an image of an elevator on the background layer, so this sprite is always on the screen.

![enter image description here](https://lh3.googleusercontent.com/yspb7fpQX7yTm82nCt3aVsqCcj8aK7VaCp-dmbOvTmeGsFCQTHV29qgYO0b9CqkI8qJ-IT4kxRAX)

All of the elevators blocks are just simple say blocks, except the Cutscene4 section. This section has some logic to add onto a string that I ended up not using, and plays the nom sound and decrements the coin counter while the elevator eats the coins. It then changes the backdrop, which is used to retrigger CUTSCENEMAN

## The Nerf Gun (New Piskel)

This is the nerf gun sprite in the elevator. It's probably the sprite that's on screen for the least time.
![enter image description here](https://lh3.googleusercontent.com/VYTFqzwOUCEQs-o2MCe2IMgBwmQ3LhNkRzaMEVfFoBAZ7ORovRGQaIbf3ZsBPQ4dZIKyjz447Eep)

This resets the gun sprite because of all the spinning around and color changing nonsense, places it in the right sport in the elevator, and shows the sprite.

![enter image description here](https://lh3.googleusercontent.com/lrxbn563mS5Yh-kW2rMvCTOPWHtisZWu8GpsHdYOrpA2AajPU1sVZEHKobcIod2gCuArWjG1oQT8)

These are the handlers used for the cutscene. The GotGun? blocks hide move the sprite around, spin it and change it's color effect. The GotGub event hides the sprite so that CUTSCENEMAN can obtain the gun.

## TheChosenOne

![enter image description here](https://lh3.googleusercontent.com/elVqO4LHsy2CZMq_WgJ-T6WpuTvmUFw08sTeWE_QVQ5fELDgmZwMRIDZKCuLfaOHy66kR0dGjlZW)

The left and right arrow blocks just constantly move the player in a certain direction until the respective key is released

The space key block checks to make sure that this is the boss fight. Then sets the dropx so that the bullet knows where to spawn. The wait block puts time between the bullets.

## sprite_0
![enter image description here](https://lh3.googleusercontent.com/0yjgUW4EoEGiF3Wnvc5dLCnTVby7W_AVapCYKxGGAaC2db29ToIF9F-jTQCtLRnPVGXsU5HAgsyA)

Same is TheChosenOne's movement. The only difference is that if the character is touching red we try to move it out of the wall. This works most of the time.

![enter image description here](https://lh3.googleusercontent.com/dIocW_jZqWwBEEWi2oZhjZ7Nm9VPvHifqOCdXhua79YS-L_UfwWd5qatSO6vWxBUIECmuw6yWlcp)

This moves the player down if he's not touching the ground, also puts him in the right spot when the game starts.

![enter image description here](https://lh3.googleusercontent.com/NyeNEgRSD0Z6fnn1IKVLuygD9ioGFq1dkwZS4cQtGjIkoj_Swep59TTWAF5ReQIuDl6rXWqUlbXe)

This stops all of the scripts, says words, then starts the elevator eats coins cutscene.

## TheBoss
![enter image description here](https://lh3.googleusercontent.com/5u5v6DDqs_z1PxEopLbEglZXSOfYKoH2eY2SVAO0iOQJ9gy5la4011-eIo3bzdq1p07zy1Vo4AVn)

This custom block is because it makes the code easier to read.

![enter image description here](https://lh3.googleusercontent.com/Od0fOwDdXXNCuNXHKajLliSxcDJ9TyqQI-d55Laza6dnIDSILUOZXROrMQCP1prWgphEZoRAadgz)

This script is the choreography for the boss fight. It picks when the fight should progress to the next stage, then calls it. The two blocks at the bottom are unnecessary and I'm not sure why they're there.

![enter image description here](https://lh3.googleusercontent.com/sgNkSCV3qZecNnAr4yd0sbLiUayhfmIZ5AqEIcD2E7paszKEQvhiE26nXzYcwjwx1kHoNxla7LV6)

This monolithic if else if else if else if is the stage change. Each part of it does something simpler until it reaches the last block, where it stays for the rest of the fight.

![enter image description here](https://lh3.googleusercontent.com/WU4U6TUwTXgNGnOvr5l4xib58dsylfT5Ixk7zqHgcvmCrx4Ruqe-yRfbwt_LWg1Seg0wLdmNdGp1)

The first block of these two waits for the boss to die, then tells you you win, switches the backdrop to the credits, kills Deja Vu, and restarts the elevator music.
The second block makes the pig go rainbow and spin when the event is called. The boss does reset when it's spawned.

## The Nerf Bullets (Sprite4)
![enter image description here](https://lh3.googleusercontent.com/1eJOTuN-hePld59VzFpO0R-on5LZZLJyUeID5aTRHqbblSs4nj0HagAN22ZvJPUSuz6AKdpns1Lj)


This is all of the code. When the bullet is cloned it jumps to TheChosenOne's set dropx, then travels upwards until it hits something. If that something is the boss it damages it. It then deletes itself.
