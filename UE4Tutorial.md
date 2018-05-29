
# Billboarding Sprites


A billboarding sprite is a sprite which is always facing the same direction as the camera.

These are useful to have an image that is always visible to the user, as well as to fake having a more detailed environment by having a detailed background always be in view.

## Steps

 1. Add a sprite that we can use as the default by importing an image.
	
	 1. Import an image
  	 2. Right-click on the created texture, and go to `Sprite Actions -> Create Sprite`

2. Create an Actor blueprint class for the billboarding sprite named `BillboardingSprite`. This is an object that could be reused several times, so having it as an object that can be dropped in will be useful.

3. In the Blueprint, add a Paper Sprite component, and set it's source sprite to your default sprite.

4. Add a Paper Sprite variable
	- Make it publicly editable by clicking the eye icon
	- Compile the blueprint
	- Set it's default value to the sprite you used for the sprite object.

5. Add a Set Sprite node in the Construction Script setup like this:


6. Create the event graph for the class
	1. Add a `SetActorRotation` node off of the `Event Tick` node
![Step 1](https://famous1622.github.io/UE4Tutorial/Step1.PNG)
	2. Create a `Make Rotator` node of of the New Rotation pin![Step 2](https://famous1622.github.io/UE4Tutorial/Step2.PNG)
	3. Create a `Get Player Camera Manager` node
	4. Create a `Get Camera Location` node off of the Return Value pin from the `Get Player Camera Manager` node
	5. Create a `Find Look at Location` node off of the Return Value pin from the `Get Camera Location` node
	6. Create a `GetActorLocation` node off of the Target pin of the `Find Look at Rotation` node
	![Step 6](https://famous1622.github.io/UE4Tutorial/Step6.PNG)
	7. Create a `Break Rotator` node off of the `Find Look at Rotation` node's Return Value pin.
	8. Create a `float + float` node off of the Z (Yaw) pin of the `Break Rotator` node
	9. Set the other value of the `float + float` node to `90.0`![Step 9](https://famous1622.github.io/UE4Tutorial/Step9.PNG)
	10. Take the output pin of the `float + float` node and connect it to the Z (Yaw) node of the `Make Rotator` node  ![Step 10](https://famous1622.github.io/UE4Tutorial/Step10.PNG)
[Pastebin of the final blueprint](https://blueprintue.com/blueprint/dicpfkoz/)
