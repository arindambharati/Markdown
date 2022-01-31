# **Unity2D Project - Super Jumper**

Design a 2D Platformer in which a player has to combat enemies and conquer the challenges he would face to reach the goal and finish the level. Use different types of player mechanics to make the game appealing to the gamers such as double/triple jump, dash, and many more.

---

# 1. PLAYER MOVEMENT

Player Movement can be achieved in different ways in unity, depending on the specific requirement, few are:

- Changing Transform.position of the player every frame based on input direction, this process is usually best when no Rigidbody is there and the object simply needs to shift every frame, for eg, in the case of a top-down camera game like Snake
- Force - Applies a gradual force on the Object, taking mass into account. This is a literal pushing motion where the bigger the mass of the object, the slower it will speed up
- Impulse - Applies an instant force on the Object, taking mass into account. This pushes the Object using the entire force in a single frame. Again, the bigger the mass of the object, the less effect this will have. Great for recoil or jump functions
- Acceleration - Same as ForceMode.Force except that it doesn't take mass into account. No matter how big the mass of the object, it will accelerate at a constant rate
- VelocityChange - Same as ForceMode.Impulse and again doesn't take mass into account. It will literally add the force to the Object's velocity in a single frame

Understanding these different types of forces can be important to shaping the structure of the game as movement is probably the basic feature of any game. For example, the following shows how a jump action that propels the object upward based on ‘space’ key input from the keyboard can be made to work by applying a velocity change to the Rigidbody component attached to the Player GameObject. 

<p align="center">
    <img alt="velocityJump" src= ./Images/Velocity_Jump.png>
</p>
For this game, since this is a 2D platformer, you can use Transform.position changes along with the Axes (Horizontal, Vertical) to create the movement of the Player every frame, and Rigidbody velocity changes for the jump.

---

# 2. CAMERA FOLLOWING THE PLAYER

In any type of game where the camera is projecting the scene for the user, as the player moves, we need to make a design such that the scene is visible for the required range of view of the user rather than keeping the camera view of the scene static which will make the user who is watching the screen lose their player object in-game as the player keeps moving out of the range of view. As such, as we learnt that the camera is also actually a GameObject, it can be manipulated just as any other GameObject in Unity is treated and can be worked on in a similar manner. Two such ways how the camera can be kept moving along with the player object in-game is:

- Attach the camera GameObject to the Player Object, as we know Unity works using a parent-child hierarchy relationship of Objects, so any child object attached to a parent object keeps moving as well when the parent object moves, for example, as shown in the image below, the Main Camera is attached to the player object for a 2D platformer game

<p align="center">
    <img alt="parent child attach" src= ./Images/parent_child_attach.png>
</p>

- Create a script Component for the Camera Object that can be made to change its position based on movement changes of the Player GameObject by using a variable in the inspector that takes the Player object as a parameter or by finding the PlayerObject in-game using GameObject.Find.

---

# 3. COLLISION DETECTION

**Collision detection** helps in the application of physics to the objects in Unity so that they behave like real-world objects inside the game, for example, when the player touches a wall or platform they should get stuck and the player shouldn’t be able to pass through it (a typical game where the user controls a player but of course, if your game requires objects to pass through each other, this can be implemented as well). In a 2D game, any GameObject can have a collider component attached to it like a Box Collider 2D that helps in detecting collisions among objects that would have their boundaries in a rectangular shape. 

<p align="center">
    <img alt="box collider2d" src= ./Images/boxCollider2d.png>
</p>


As shown above, this collider component when added to both objects will start colliding when touching each other. 

The Collider 2D types that can be used with **Rigidbody** 2D are:

- [Circle Collider 2D](https://docs.unity3d.com/Manual/class-CircleCollider2D.html) for circular collision areas.
- [Box Collider 2D](https://docs.unity3d.com/Manual/class-BoxCollider2D.html) for square and rectangle collision areas.
- [Polygon Collider 2D](https://docs.unity3d.com/Manual/class-PolygonCollider2D.html) for freeform collision areas.
- [Edge Collider 2D](https://docs.unity3d.com/Manual/class-EdgeCollider2D.html) for freeform collision areas and areas which aren’t completely enclosed (such as rounded convex corners).
- [Capsule Collider 2D](https://docs.unity3d.com/Manual/class-CapsuleCollider2D.html) for circular or lozenge-shaped collision areas.
- [Composite Collider 2D](https://docs.unity3d.com/Manual/class-CompositeCollider2D.html) for merging **Box Collider** 2Ds and Polygon Collider 2Ds.

Coming to the second part of the Collider detection when you need the objects to pass through each other but maybe perform an action when their colliders come in touch, The Collider component provides the ‘Is Trigger’ box, which if ticked the objects will pass through each but certain functions or tasks may be performed. For example, in-game we might want the Player Object to pass through a door and go to the Next Level as shown in the below game. 

<p align="center">
    <img alt="door" src= ./Images/Door.png>
</p>

While Scripting, MonoBehaviour functions can be used - OnCollisionEnter2D/Stay2D/Exit2D() and similarly OnTriggerEnter2D/Stay2D/Exit2D() based on if it is a collider or simply a trigger.

- **Collision with enemies** - For fair gameplay, if your game consists of enemies, in the case of this 2D platformer, you would want gameplay where your enemies will cause damage to the Player on either colliding(eg, super Mario dies when touches the tortoise), or with any type of action performed (eg, bullets firing from an enemy).
After the different types of enemies have been created, you will need to apply collision detection between Enemies and the Player GameObjects to start a function OnCollisionEnter2D, for example, that deals with either decreasing the life of the Player or death, depending on your game requirements. For this game as a start, since the levels are of small sizes, you can implement three lives, on touching the enemy once, you can implement a function that reduces the number of hearts by 1, and then once all 3 lives are lost, you can implement another function that will cause the death for the Player. Following UI is an example of the lives (hearts) implementation in-game: 

<p align="center">
    <img alt="lives" src= ./Images/lives_all.png>
</p>

- **Collision with walls** - The Colliders setup on the walls will help in stopping the player from falling down from the platform. A layer can be applied to denote all the platforms, for example, a layer called Platforms, and then this layer can be setup to be static (for the platforms that won’t be moving) and collision detection can be ticked in the matrix for Player and walls, the layers here being ‘Player’ and ‘Wall’ respectively.

- **Collision with Items and Collectibles** – If a game has got some items or collectibles that can be picked up by the player, then that can provide more challenges for the Player and can help increase the level of satisfaction in playing the game. Any item placed in-game can be made to perform some function based on the requirement. For, eg, the below shows keys as items to be picked up by the Player: 

<p align="center">
    <img alt="items" src= ./Images/Items_Keys.png>
</p>

For such items, similar functionality can be applied to detect collision between the Player and the Items, when detected any function can be triggered that performs some action, for example, in the above case, the key was made to disappear (by using Destroy(gameObject)) and the score was increased each time the player picks up one. In this case, you can also use a Trigger instead of a collision, since the Player doesn’t actually need to collide with the Item, rather some action should happen as soon as the Player enters the Trigger area of the Item’s Collider.

- **Trigger/Collision for Level End Condition** – At the end of a level, a level-win condition is required to be provided. This condition would be responsible for either completing the game or starting the next level. For example, in Super Mario, we see 8 Areas/levels that Mario needs to be complete in order to finish the game. Hence, this would also need to be setup as a Trigger or Collider as required. When the player enters the trigger area, a function can be triggered that does the work of starting a new level and notifies the user that the current level/area/scene has been completed. For example, something like the Portal asset shown below can be used to trigger the level condition. It might be required that the player is shown as a layer on top of the portal or door, in which case a Trigger instead of a collision would be preferable. 

<p align="center">
    <img alt="door end" src= ./Images/Door_end.png>
</p>

---

# 4. COLLISION MATRIX

Finally, another condition may arise when we might want collisions for the GameObjects but maybe not collide with specific objects. For example, we might want enemies to pass through each other but the Player should collide with them. 



<p align="center">
    <img alt="enemies passing" src= ./Images/enemies_passing.png>
</p>

In such cases, a collision matrix can be used to uncheck collisions between specific layers as shown. below. You can find this setting under Project Settings → Physics2D (for a 2D game) or Physics for a 3D game.


<p align="center">
    <img alt="door end" src= ./Images/collision_matrix.png>
</p>

---

# 5. LEVEL COMPLETE

- Let's discuss how to conclude whether a level has been completed or not. Basically, there are 2 conditions when a level is marked completed:
    1. When the player has reached the end of the level, a trigger is fired to let the player know that the level has been completed, and then the player is redirected to the next level. 
        1. We are able to achieve this through detection of collision and trigger used to detect collision.
        2. Triggers allow the player to pass through the game object. Triggers are mainly used for tutorials and power-ups.
        
        
        <p align="center">
                <img alt="door col" src= ./Images/door_coll.png>
        </p>
        
        In the above video, a collider has been added to the door and the Is Trigger component of the collider has been checked(set to true). So now the door allows the player to pass through it and acts as a Trigger which when comes in contact with the player performs a certain operation i.e. indicates the player that the level is complete and then proceeds to move on to the next level.
        
    
    1. When the player has collected the necessary collectables to complete the level is also when we can conclude whether a level is completed or not.
        1. We do this by keeping a track of the collectables collected by the player after the player has collected one.
        
        [Loom _ Free Screen & Video Recording Software - 28 January 2022 (1).mp4](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7c421f06-74e3-4f3e-a33d-0fe5e8d260a2/Loom___Free_Screen__Video_Recording_Software_-_28_January_2022_(1).mp4)
        
        In the above sample video, we keep a track of the number of collectables we need in order for the level to get complete. Once we have collected all the stuff we can see a level complete screen pops up at the end of the video 😎
        

## 6. PLAYER LIVES

- To create a Life system we need to do things, first one is the Sprite that we will be displayed on the screen for the user, the second is a controller which decreases a life whenever the player collides with an enemy or increase a life when the user collects a power-up that restores a lost life.
- The very first step to creating a Life system is to start by creating a sprite that will be displayed on the screen for the player that will be indicating how many lives are remaining for the player.
    
    
    ![Lives.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4ccc2123-45c6-45ec-93aa-ee8c73d72884/Lives.png)
    
    ![Lives_hie.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b574c9aa-fa47-41bb-ba15-941532915e4e/Lives_hie.png)
    
- The next step is to add a method that will decrease the life by one whenever our player collides with the enemy. We can start by detecting the collision in our Enemy Controller and if the collision is with the player we decrease the life by one.
- To decrease the life in real time we will be assigning these sprites to our game objects that we have created in our script and we will simply set its visibility to false whenever the player collides with an enemy. by doing so it will appear as if the lives are being decreased but in reality, we are simply changing the values of visibility of the sprite.
    
    ![Lives_scrp.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/28b712d9-2cd8-4df4-a482-da56368700f6/Lives_scrp.png)
    
- Now, you might be thinking there might be a time when the player will run out of lives what happens in this scenario?? well basically this is where we make the game over for the player.
- Once the whole implementation is done correctly this is what the final output must look like. 🤩
    
    [Lives_decrease.mp4](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9785a991-4c4d-42a9-ab9d-1cf94637a92f/Lives_decrease.mp4)
    

## 7. POWER-UPS

If you want to make your game interesting then add the feature of Power-ups and Collectibles. Power-ups are nothing but collecting items or points which will give the player a special ability. The ability could be anything- double jump, dash, or some attacks. 

Here, you will learn how to implement Power-Ups or Collectibles.

1. Drop a game object in the scene view.
2. Use a collider and in the collider component click ✅ the “Is Trigger” option.
3. Now, while implementing the logic, use compares tag to compare with the game object it collides with and after the collision, destroy the power-up game object.
    
    ![powerups.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/80d45f39-f68a-4530-81ec-1b994bc76e08/powerups.png)
    

You can add tons of features here like, after collecting the item enabling the double jump ability to the player for some seconds. This way, the player could only double jump if he gets the power up feature and that too for a limited period of time. To do this you can add a timer or use coroutines, for which you can read in the Dash mechanism section. 

- *Refer to the clip below to get an idea of what to implement.*
    
    [2022-01-29 02-09-15.mp4](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ab3b31a6-a658-46a3-ba55-32ca9dba3f14/2022-01-29_02-09-15.mp4)
    
    Here, after getting the red game object, which is a collectable item here, the player(black square object) could move much higher and enabled the double jump ability for 5 secs. Also, observe the timer on the top-right. That’s what you have to implement and of course, go beyond this project with your imagination and implement as many as cool features you want to.  
    
    ## A. Single/Double/Triple Jump
    
    - Player Jump is one of the most crucial elements for this project. Before making the double jump or triple jump first, go through the *Player Movement* section to get an idea of how you can use physics in a particular game object to move it. Also, get yourself familiarized with the RigidBody2D, BoxCollider2D components.
        - Single Jump: If you have tried to write a code for jump mechanics then you must have seen how the game object behaves when pressing the attached input. It goes infinite at a one-time press. How to fix it? *Hint:* Limit it by using a **bool value**. Also, do not forget to read about *Collison Detection* as it is important for this task.
            
            Here is a code where you can get an idea of how to do it. 
            
            ![collision.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/23c666fa-85df-4742-b99d-472e0df090e0/collision.png)
            
            Also, this is not the end. You need to write a logic for OnCollisionExit2D as well. Do it by yourself. It will boost your confidence.
            
        - Double Jump: Can we use a counter for it? 🤔 It’s like if you press the UP Arrow key once, then it will count for 1 time and jump once. If pressed twice then it will count for 2 times and jump twice. hmmmmmm.... ????? But how to implement it?
            
            Let’s see a code and understand it. 
            
            ![doblejump.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c3b34e05-fe81-4ba4-9a3e-b9c32fc9859f/doblejump.png)
            
            Here, the counter is extraJump variable. By default, the game object has the ability to jump by once. So when extraJump is 1 then the game object will jump twice. The extraJump will only be available if the game object stands on the ground platform. The value of extraJump is decreased to zero so that when the player inputs the bound key(*UP-ARROW*) the second time, the extraJump would become 0, and the next time if the player continuously presses the key again, it wouldn't affect anything to the game object and the player won't be able to jump more than twice.
            
        - Triple Jump: Change the value of extraJump to 2.
    - *You can use any number of jumps depending upon what you want in your game.*
    
    ![jumpsss.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/071f7461-f591-444a-b546-f880588d4487/jumpsss.png)
    
    ## B. Dash mechanism
    
    Many game characters will have the ability to rocket forward or backwards with their legs trailing after them as if firing a jet-pack horizontally, though they usually aren’t wearing one. When doing this, they will be able to travel a distance many times the length of their body in a split second. This move tends to be triggered by tapping or flicking the same direction more than once but some games can also have a dedicated ‘dash button’.
    
    There are many ways to implement this feature. For this game, we will try to implement a simple Dash using a dedicated dash button, let’s say the ‘Shift’ button on the keyboard. Now, imagine the requirements for this feature, we need a duration for which the dash will be active, and then maybe a cool down duration after which the dash can happen again.
    
    A very common approach to solving this is using IEnumerators. ([https://docs.unity3d.com/ScriptReference/MonoBehaviour.StartCoroutine.html](https://docs.unity3d.com/ScriptReference/MonoBehaviour.StartCoroutine.html))
    
    In order to understand, we need to go through what is Coroutine first. [MonoBehaviour.StartCoroutine](https://docs.unity3d.com/ScriptReference/MonoBehaviour.StartCoroutine.html) returns a Coroutine. Instances of this class are only used to reference these coroutines and do not hold any exposed properties or functions.
    
    A coroutine is a function that can suspend its execution (yield) until the given [YieldInstruction](https://docs.unity3d.com/ScriptReference/YieldInstruction.html) finishes. For the dash function, we actually need to wait for duration till the dash is completed and only after the cooldown period, allow the player to re-use the dash.
    
    The following script shows how the Dash can be implemented inside the Update: 
    
    ![GetKeyDown_Shift_script.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7caafd1f-3b87-4a9c-a9d2-73eb2d9c211f/GetKeyDown_Shift_script.png)
    
    And the dashCoroutine IEnumerator: 
    
    ![Dash_IEnum.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/906a15dc-ce40-442a-a7f1-bdb15e170c5f/Dash_IEnum.png)
    
    And Finally applying the impulse force in physics for the RigidBody:  
    
    ![Dash_physics.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2ace5bbb-8909-42f9-8d3d-805edf404616/Dash_physics.png)
    
    Of course, it would also be great if a collectable is implemented upon picking up which, the player would get the ability to dash. The Cooldown time should also be set as such so that it cannot be used continuously rather only after a certain amount of time, after having used it once. In the above script, we have demonstrated that to be 2 seconds, but you should set this as required by your game. The below video shows a quick animation of how this has been implemented for a 2D platformer game : 
    
    [Ellen_Dash.mp4](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6cd7b88d-0fe6-4009-a8bd-62bb54172427/Ellen_Dash.mp4)
    
    As shown above, you can see the Player dashes as soon as ‘LShift’ key is input. It would be an additional achievement if you can use a particle system or any other way to show a trailing effect when the dash happens. Remember to lock the dash ability at the start, but only activate it once a collectable/item has been picked up.
    

## 8. FALLING PLATFORMS

You can add so many features to your game to make it challenging and engaging to the gamers. One such feature is the *falling of a platform* when the player jumps over it or stands on it. It’s not so tough to implement if you think logically. It is all about the physics mechanism that would require to make a game object (here, it is the ‘platform’) fall when the player stands on it. 

- *Refer to the clip below to understand what we want you to implement.*
    
    [2022-01-27 17-05-14.mp4](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5671e361-8279-44e9-8547-ea55dd79ba0d/2022-01-27_17-05-14.mp4)
    

*Hints:* By default, the body type of the rigidbody component is Dynamic. Make it to Kinematic. Now, get the rigidbody component of the platform game object. Compare it with the player game object when both of them collide with each other. On collision, change the body type of the platform from kinematic to dynamic.

The reason why we would want to change the body type from dynamic to kinematic and vice-versa is simple-

- The Kinematic body type ignores all the forces that act on it.
- The Dynamic body type affects by all the forces that act on it.

You can also add the ‘Game Over’ feature if the player falls to the ground and nowhere along with the platform. *Refer to the **Game Over** section to implement this feature.*

## 9. PATROLLING ENEMIES

- Basically, there are 2 ways of making enemies patrol in a specific area.
    1. Using waypoints: we can set waypoints and make the game object travel within those waypoints. Below is a piece of code for further reference.
        
        ![Enemy_Patrol_wp.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/387a3622-d8fd-4159-bfdd-9a1e67d73b5a/Enemy_Patrol_wp.png)
        
        In the above code, we are calculating the distance between the waypoint and our enemy game object. By doing so once we get the distance which is less than 0.01f we are changing the waypoint to the other one that is set by the user in the Unity inspector and we are also flipping/rotating our game object so that it is facing towards the other waypoint. So, it goes on like a loop and the game object keeps on moving within the waypoints. 😎
        
    2. Using Raycasts : A Raycast is like a laser beam fired from a point that we call as “Origin” along a particular direction to detect if our game object is still in collision with the other game object and if it is not in collision we perform certain operations. The big advantage is that every object which makes contact with the laser beam can be reported!
        
        ![Patrolling_enemy_raycast.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7179ccd4-f292-4b93-9325-230e106f0308/Patrolling_enemy_raycast.png)
        
        In the above code, RaycastHit2D returns information about an object detected by a raycast in 2D physics. The RaycastHit2D class is used by [Physics2D.Raycast](https://docs.unity3d.com/ScriptReference/Physics2D.Raycast.html) and other functions to return information about the objects detected within the range fired by raycasts. 👾
        
    

## 10. UI BASICS

- What is a UI???
    
    UI or User Interface is something which allows the User to interact with or to be specific with game development it is something that allows a Player to interact with. Unity UI is a UI toolkit for developing user interfaces for games and applications. It is a Game Object-based UI system that uses Components and the Game View to arrange, position, and style user interfaces.
    
    ![UI_1.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cedf0199-81d4-46eb-93b2-117c9c2974a1/UI_1.png)
    
    ![UI_2_2.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/57723c93-1ff2-4782-b869-7087e70c3ee1/UI_2_2.png)
    
- Above snapshots show images of Main Menus of games. These main menus are the best examples of UI used in games. Obviously any game will have a menu. When we press on the buttons say single player or exit button then it will either start the game or exit the game by the functionality that has been assigned to the button itself.
- Some of the main components of UI are:
    - Canvas
    - Buttons
    - Texts
    - Panel
    - Slider

1. Canvas
    - You might have heard that we generally use Canvas whenever we are trying to paint something. It is mostly used by professional artists to show their art on the canvas.
    - Basically, canvas is a sheet where you can draw or wrt game development canvas is a component where you can draw your UI.
    - Whenever you add a UI component Canvas will also be added along with it.
    - To insert a Canvas, right click in the Scene Hierarchy and go to Create → UI → Canvas.
    
    ![UI_2.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f097ee02-8f90-4ed6-9d1a-a15cd7f96647/UI_2.png)
    

1. Buttons
    - Anything when clicked and performs an action after it has been clicked is called a button. In real life we have a switch/button to turn the light on/off in the same we have UI buttons for certain operations.
    - In game development we can consider a mouse button which when clicked during a game might fire a weapon or do any other operation assigned to it depending upon the game.
    - To insert a button, right click in the Scene Hierarchy and go to Create → UI → Button. If you do not have an existing Canvas and an Event System, Unity will automatically create one for you, and place the button inside the Canvas
    
    ![UI_buttons.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6230ea84-ca9a-4f86-ba21-8da193ea6924/UI_buttons.png)
    

1. Texts
    - Every game has a name which is displayed in the UI and for displaying those names we use UI Texts.
    - Texts are non – interactable they’re used for displaying some information. For eg: it can be used to display instructions or it can be used to display a player’s current score in the game.
    - To insert a Text, right click in the Scene Hierarchy and go to Create → UI → Text.
    
    ![UI_text.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d096761a-2c70-4363-9c3a-d27f558f2642/UI_text.png)
    
2. Panel
    - Panel is a UI component that acts as a container in Unity.
    - It helps us in grouping together different UI elements.
    - Panel defines an area that will by default stretch to fit its parent RectTransform dimensions.
    - To insert a Panel, right click in the Scene Hierarchy and go to Create → UI → Panel.
    
    ![UI_panel.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/263cfa9a-a118-4978-8cf8-dda664c517e7/UI_panel.png)
    
3. Slider
    - The Slider UI element is commonly used where a certain value should be set between a minimum and maximum value pair.
    - One of the most common usages of this is for audio volume, screen brightness, or for doing zoom.
    - To create a slider UI, right-click on the scene hierarchy and select Game Object -> UI -> Slider. A new slider element will display on your scene.
    
    ![UI_Slider.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ed79a004-b5ac-4388-b8b6-0438eafa6a4f/UI_Slider.png)
    

## 11. GAME OVER

- Whenever our player is out of lives or we run out of time to complete a level we see a certain screen that tells us what exactly??? 🤔 That’s right it tells us that our game is over.
- There’s a certain condition after which we lose our game or we can say that our game gets over. The condition can be anything we might run out of time or we might have come in contact of enemy or we might run out of lives.
- Let me show you how to make the game over when our player gets detected by our enemy.
    
    ![Game_over.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/de2ac240-d6c4-4756-9bf1-d0e6593618ec/Game_over.png)
    
- In the above code the game gets over when the player is detected by the enemy’s raycast within a range. Once the player is detected we are freezing the time in the time which is equivalent to pausing the game. After that I am setting the visibility of my game over panel as true which is initially false before the game starts. When the game gets over this is basically what you see.
    
    ![GO.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/596e7063-68af-4fdb-a04f-e4ae462a2191/GO.png)
    

## 12. LEVEL LOCK/UNLOCK AND PLAYERPREFS

PlayerPrefs: In order to save the game progress, Unity provides something called “Player Preferences” or ‘PlayerPrefs’ in short. This is a class that stores Player preferences between game sessions. What this means is, say you exit from the game today and re-open it tomorrow, it should be able to save your session and start from where you left off and shouldn’t start from the beginning of the game, what we call as ‘SAVING’ in games. PlayerPrefs can store string, float and integer values into the user’s platform registry. The location of saving these values will depend on the type of platform being used as mentioned below.

### **Standalone Player storage location**

On macOS, PlayerPrefs are stored in 

> ~/Library/Preferences/com.ExampleCompanyName.ExampleProductName.plist
> 

Unity uses the same .plist file for projects in the Editor and standalone Players.

On Windows, PlayerPrefs are stored in HKCU\Software\ExampleCompanyName\ExampleProductNamekey.

On Linux, PlayerPrefs are stored in ~/.config/unity3d/ExampleCompanyName/ExampleProductName

On Windows Store Apps, PlayerPrefs are stored in %userprofile%\AppData\Local\Packages\[ProductPackageId]\LocalState\playerprefs.dat

.On Windows Phone 8, Unity stores PlayerPrefs data in the application's local folder. See [Directory.localFolder](https://docs.unity3d.com/ScriptReference/Windows.Directory-localFolder.html)

for more information.

On Android, PlayerPrefs are stored in /data/data/pkg-name/shared_prefs/pkg-name.v2.playerprefs.xml

. Unity stores PlayerPrefs data on the device, in [SharedPreferences](https://codelabs.developers.google.com/codelabs/android-training-shared-preferences/index.html?index=..%2F..android-training#0)

. C#, JavaScript, Android Java and native code can all access the PlayerPrefs data.

On WebGL, Unity stores PlayerPrefs data using the browser's IndexedDB API. For more information, see [IndexedDB](https://developers.google.com/web/ilt/pwa/lab-indexeddb#overview)

**In-Editor Play mode storage location**

On macOS, PlayerPrefs are stored in /Library/Preferences/[bundle identifier].plist

On Windows, PlayerPrefs are stored in HKCU\Software\Unity\UnityEditor\ExampleCompamyName\ExampleProductName key.

Windows 10 uses the application’s PlayerPrefs names. For example, Unity adds a DeckBase string and converts it into DeckBase_h3232628825. The application ignores the extension.  Unity stores PlayerPrefs in a local registry, without encryption. Do not use PlayerPrefs data to store sensitive data.

There are a lot of different data types that can be stored using this class, of course using Get and Set methods they can be fetched and set respectively. The various data types allowed are: 

Float
Int
String

Now for Locking levels, you can create an enum of different statuses as shown below, based on the requirements of your game: 

![Status_levels.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9f51741c-068b-43d0-9376-a7697222bee8/Status_levels.png)

As shown in the example above, every time a level is completed, you have the status as “Completed”, levels that are unlocked will have the “Unlocked” status, and levels that have not been played yet will be kept as “Locked”. The following shows the buttons for levels, you can design in a similar way: 

![AllLevels.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5a3e6186-1f26-4eaf-a628-ae41909ed85e/AllLevels.png)

Then, for each button, the status can be set based on its state, of course, levels such as the lobby (menu screen) and Level 1 (the very first level) needs to be kept at ‘Unlocked’ status from the beginning so that they can be accessed by the user in the Start itself. The below shows an example of a switch case statement that allows the button click function to work only when the level is in ‘Unlocked’ status: 

![Button_locks.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/23b77a46-9cd1-437c-8c0e-90ed4738fb59/Button_locks.png)

Finally, the PlayerPrefs can be set and fetched as required as shown below: 

![Prefs_levelValue.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5069f31d-0de6-4a08-956b-d749a95d5385/Prefs_levelValue.png)

You can always find the values in your local device in places, for example, the below shows the storage location on a Windows PC (in the registry editor): 

![Reg_ed.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f72132bc-453d-4ce5-b098-0ae8f36bef67/Reg_ed.png)

Now, you can come inside the storage location and delete the values manually if you want to not have the saved values for any reason (for example, When the User wants to delete all saves and start a New Game) otherwise, there are 2 other ways to clear all values: 

- Use DeleteAll or DeleteKey functions using the C# script
- Use Clear All Player Prefs option under Edit category:

![ClearAllPrefs.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d29892d4-d45c-4195-958a-bb4850f05ccd/ClearAllPrefs.png)

## 13. GAME LOOP (LEVELS/GAMEPLAY)

- Super Jumper is a level-based game. Every level will have its own complexity and toughness. As of now, we would suggest a 3 level architecture that we can add to this project but of course, you are free to design more levels as you wish to. Every level that has been completed will unlock a new feature for the player that will help the player in completing the further levels. Every level completed will show a “Level completed” text/panel and will also be displaying which ability has been unlocked for the player.
- Level 1: This will be a basic level that will allow the user to explore the level and play around with and as it is the very first level the difficulty level will be minimal 😜. Some enemies will be added in the very first level to make it a little challenging for the player, please feel free to design the level as you like with enemies and static platforms
- Level 2: Once level 1 has been completed a new feature named double/triple jump can be unlocked for the player. This feature will help the player in covering distances that is far away and which cannot be covered by a single jump. Of course, there will be collectibles that the player would have to pick up first to activate the Special Jump ability. Also, you can add moving platforms to this level to make it a little tougher for the player. 😎
- Level 3: Once level 2 has been completed a new feature called Dash/Speed booster will be unlocked for the player. This feature will help the player in covering very long distances which cannot be reached by double/triple jumps. Of course, there can be another type of collectible that the player would have to pick up first to activate the Special Dash ability 😯. All other features i.e. patrolling enemies & falling platforms can also be included in this particular level.
- You can add further levels based on your vision of your game. Remember this is your game and it has to be fun for you to play when you play it yourself. You can add interesting stuff to the game, taking references from other 2D platformer type games as well.
