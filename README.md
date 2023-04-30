# Zelda-s-target-locking-system

This project is an object locking system made with unreal for a course during my bachelor 3.

## Sommaire :

- **Presentation** of the project.
- **Game keys** to use the mechanics.
- How to **install the project**.
- The **different features** of the project.
- **Organization and structure** of the code.

## Presentation of the project :

The goal of this project is to implement a camera **lock mechanic** on Actors in Unreal Engine 5 using the Blueprint. By pressing the `E` key, the **camera will lock onto the center-most Actor** on the screen and the player will enter a **custom movement mode**. T**he player can then move away from and towards the Actor** with the front and back keys, as well as **turn aroun**d with the left and right keys.

This project requires a version of Unreal **greater than or equal** to [`Unreal 5.1`](https://www.unrealengine.com/fr/download)

## Game keys to use the mechanics :

- *Movements*:
    - Move forward / Get closer to the target : **Z**
    - Go left / Go left of target: **Q**
    - Move downward / Move away from the target: **S**
    - Go right / Go to the right of the target: **D **D**
    - Jump: **Space**

- *Lock*: **Maintain E**.

## How to install the project :

You can download the latest version of the project [**here**.](https://github.com/LeoSery/Zelda-s-target-locking-system--UnrealEngine5-2023)

- or you can clone the git repository with the following command :
```
git@github.com:LeoSery/Zelda-s-target-locking-system--UnrealEngine5-2023.git
```
ou 
```
https://github.com/LeoSery/Zelda-s-target-locking-system--UnrealEngine5-2023.git
```

If you have the right version of Unreal engine, you just have to open the file named: **`TemplateCameraLock.uproject`**.

## The **different features** of the project :

In this project you have **2 possible movement modes**, the ***`Walking`*** mode and the ***`TargetMode`***. 

### Walking

This is the **default movement mode** of the project, **you can move** around using the keys explained above, and **you can move the camera** with your mouse like in an FPS game.

### TargetMode

***TargetMode*** allows to **lock a target**, **to follow it with the camera and to move according to it**.

In order for an object to be targeted, **it must have the `LockableObject` component**. The component can be found in: `All>Content>FirstPerson>Blueprints`. You can add this component to the BP of the actor you want to lock.

You can configure the key to lock an actor in-game by modifying the InputAction named ***`"IAFocusTarget"`*** which is located in the file ***`"IMCDefault"`*** which is located here: *`All>Content>FirstPerson>Input`*.

Once in game, when you press the button assigned to TargetMode, it **will automatically find the target closest to the center of your screen** and place the **camera towards that actor**. If your actor is moving then the camera will start **following him**. In this mode **your movements are based on your targe**t and **you no longer have control over your camera** which will start to stare at the target.

If a **target is no longer visible to the player**, after X seconds, you will lose the tracking of the actor and automatically return to "**Walking**" mode. The duration before losing the targeting can be adjusted by modifying the variable named "**MaxTimeTargetCanBeInvisible**" in the EventGraph. By default the time is set to **3 seconds**.

**If you lock an actor and it disappears** from the world you **lose the targeting** on the object, and conversely if an object is instantiated and it has the component, it can **become lockable at any time**.

## Organization and structure of the code :

The locking mechanism is organized in **4 main systems**:

- The ***"LockableObject" component*** which allows to make an actor lockable.
- The ***search for the target*** actor.
- The ***tracking of the target*** actor by the camera
- The ***movement of the player*** in relation to the target actor.

### The "LockableObject" component

Here is how the **component works**: 

![](https://i.imgur.com/Fghxifo.png)

When the game is launched, the componenet will **assign a Tag** to the object on which it is present.

To add the component to an Actor, you can simply **add it to an actor**.

![](https://i.imgur.com/1T9C9AT.png)

### The search for a target actor

When you press the button assigned to the TargetMode, it will **retrieve all actors that have the tag** assigned by the LockableObject component.

![](https://i.imgur.com/rxhu04P.png)

Then, among all the actors retrieved, the function will **define which actor will become the target** by looking at **the one closest to the center of the screen**.

![](https://i.imgur.com/4CfZZtq.png)


### The tracking of the target actor by the camera

Once an actor has been **set as a target**, the **camera will look at the target**, **if you move** or if **your target moves**, the camera will continue **to follow the actor** set as target.

![](https://i.imgur.com/YLCZOGX.png)

If the target passes **behind an obstacle**, the system will **continue to follow it** during the time configured in the variable named ***"MaxTimeTargetCanBeInvisible"***, after this time, you will lose the targeting and **return to the "Walking" mode**.

![](https://i.imgur.com/LQvDhds.png)


### The movement of the player in relation to the target actor

When you have a target that has been defined, **the moves are not calculated relative to the world but relative to the target**.

If you press the key corresponding to **Forward and Backward**, it will **move you closer to the target or further away from it**.

![](https://i.imgur.com/mRjz5dN.png)

The right and left keys will make you **turn in a circle around the target**. For this the **system calculates where it should be around the target** using the function ***"TargetPositionCalculation"***.

![](https://i.imgur.com/OIc84bM.png)

Then it moves to the **calculated location**.

![](https://i.imgur.com/Oip9eW3.png)

**More details** on the **different steps and calculation**s are given **directly in the commentary of the Blueprints**.