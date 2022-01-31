# **Unity3D Project - Achievement System**

## What you will learn in this project
 - [What is an Achievement System](#what-is-an-achievement-system)
 - [What is an Observer Pattern](#what-is-an-observer-pattern)
 - [What are Scriptable Objects](#what-are-scriptable-objects)
 - [Achievement System](#achievement-system)

 ---

# 1. What is an Achievement System

Achievement is simply a goal, and all games have goals, whether set in code or left to the player to determine. One of the ways of making the achievement system is using the observer pattern. The observer pattern is a great way to ensure that you're not polluting your code with concrete references to some arbitrary achievement manager. You might be thinking what is this pattern? 

The answer to that question is **Observer Pattern!**

---

# 2. What is an Observer pattern

An object (a.k.a Subject) maintains a list of its dependents known as observers. Then if a state is changed, the subject notifies all of its observers of changes usually by calling their methods.

<p align="center">
    <img alt="op_1" src=./Images/op_1.png>
</p>
 

The Observer pattern can also be defined as the relationship where one subject can signal its many watchers (or observers). Unintuitively, it is the subject calls out to its observers that respond, rather than the observers acting independently on the subject.

For Example, **Player Died** → This is an event/subject and other classes are observers(enemies, UI, Sound effects, etc) are observing this event. So the Observer pattern helps us to send signals to all these observers and each one of them can run their own executions after receiving the signal. 

When the player dies, the enemy will have a different logic, UI service will be doing showing some “Player Died” UI,  sound effects will also get triggered, etc. So to execute all these things when a particular event triggers, the observer pattern helps us to send the signal to all the observers in one go.

Now let's see how we can use the Observer pattern to implement an achievement system. To make an achievement system first of all we need to store the achievement data in some container. right? In order to do that you will need a scriptable object.

---

# 3. What are Scriptable objects

A ScriptableObject is a data container that you can use to save large amounts of data, independent of class instances. One of the main use cases for ScriptableObjects is to reduce your Project’s memory usage by avoiding copies of values. This is useful if your Project has a prefab that stores unchanging data in attached MonoBehaviour scripts.

You can learn more in our Scriptable objects course.

<p align="center">
    <img alt="op_2" src=./Images/op_2.png>
</p>
 


<p align="center">
    <img alt="op_3" src=./Images/op_3.png>
</p>
 

                       Example of Scriptable Objects created in Unity

Now we have learned all the necessary concepts required to make the achievement system. Let's see how we can make an achievement system.


## Creating a Scriptable Object

Let's understand the syntax of the scriptable object,

[**CreateAssetMenu(fileName** = “default filename that player wants to give”, **menuName** = “name that will be shown in asset/createMenu”)] .

**CreateAssetMenu** attribute is used to create custom assets using your class.

- **fileName** - The default file name used by newly created instances of this type.
- **menuName** - The display name for this type is shown in the Assets/Create menu.
- **order** - The position of the menu item within the Assets/Create menu.


<p align="center">
    <img alt="op_4" src=./Images/op_4.png>
</p>
 

**To create a ScriptableObject**,

1. Create a script in your application’s Assets folder
2. Make it inherit from the scriptable object class
3. Use the CreateAssetMenu attribute to make it easy to create custom assets using your class

For example:

<p align="center">
    <img alt="op_5" src=./Images/op_5.png>
</p>
 

 With the above script in your Assets folder, you can create an instance of your ScriptableObject by navigating to Assets > Create > Inventory > List in Unity.

## An achievement is a Scriptable Object:

In order to store the achievement data, we need achievement scriptable objects.


<p align="center">
    <img alt="op_6" src=./Images/op_6.png>
</p>
 

Now make a scriptable object in the unity and assign all the data to that scriptable object, it will look like this -

<p align="center">
    <img alt="op_7" src=./Images/op_7.png>
</p>
 

# 4. Achievement System

There are many achievements you can make. We will take an example of bullets fired achievement. 

There must be an event service to handle all the events of the achievement system.

like this - 



<p align="center">
    <img alt="op_8" src=./Images/op_8.png>
</p>
 

<p align="center">
    <img alt="op_9" src=./Images/op_9.png>
</p>
 

**Subscribing Event -** We are using SubscribeEvents() function to subscribe our updateBulletsFired() method to the OnBulletsFired event of the EventService script. We can subscribe to an event using the += operator. By subscribing a method to an event, we ensure that whenever the event occurs, our method will be called.

**Unsubscribing Event -** Similarly, we can unsubscribe an event using the -= operator. By unsubscribing a method, we ensure that our method is no longer called whenever the event occurs. If it is not done, then it can lead to memory leaks and errors in the game.

**Invoking an event -** We can invoke an event using the ?. operator. In the above example, it will invoke the OnBulletsFired event but it will only do it if there is something registered and it is not null.

In order to know how many bullets are fired, there must be a reference, right? As we have an event(OnBulletsFired), now we have to subscribe and unsubscribe a method to that event. Let’s see how we can do that.


<p align="center">
    <img alt="op_10" src=./Images/op_10.png>
</p>
 

Achievement System is a tool that allows developers to easily create and manage in-game achievements. Like this, you can make many different types of achievement systems for your game like- 

1. Enemy Waves
2. Scored Achievements
3. Collectible Achievements
4. Map area covered achievements
5. Player Progress Achievements, etc.

---