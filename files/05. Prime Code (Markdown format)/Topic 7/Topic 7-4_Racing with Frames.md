---
tags: Python_English, Topic 7
---

Python Robotics Course Lesson 28

Topic 7-4
Racing with Frames
===

Program an even more advanced game!

## In This Lesson...

Use the control techniques you learned in the last lesson to program a more complex game!



:::info

Lesson Introduction

■ Lesson 28 Contents
1. A Quick Review 
2. Making a Racing Game
3. Challenge: Adjusting the Difficulty


In this lesson you'll use the programming techniques you learned in the last lesson to program a more complicated game!

In the first half of the lesson, we'll review the concepts from the last lesson and modify the code from our previous program to work in the new one.

In the second half, we'll show you how to program a racing game where you drive around the LED display as you avoid enemies trying to crash into you!



This game is complex because you'll need to make new enemy objects appear on the LED display as well as delete the enemy objects once they're off the display. We'll be using the techniques that we learned in the previous lesson as we work on this game!

For your final challenge, we'll take a look at various factors that affect the game's difficulty as you find a difficulty level that suits you best!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!

:::

## A Quick Review

In the last lesson we took a look at the way images are processed in films and video games, using the same concept to control multiple objects at the same time. We put these objects on a timeline which moved at a framerate we set! 

##### What You've Learned

* How to set up a frame with multiple objects in it.
* Frames are measured in **fps**, or **frames per second**!
* Every object in a frame has its own timeline.
* You can register an object's methods in advance as events on the object's timeline. These events run when the object reaches that point on its timeline!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-4/7-4_1_E.png"/>

To make this work, you defined two classes called `Ticker` and `TickObj`. The `Ticker` class controls when the frame updates, and `TickObj` is a superclass which provides basic features for the objects in the frame. We then made classes for each object as subclasses of `TickObj`!


##### Defining Topic 7-3's Classes
```python==1
import time

class Ticker:   
    def __init__(self, fps):
        self.fps = fps
        self.ticks_time_ms = round(1000/fps)
        self.objs = []
    
    def register(self, *objs):
        for obj in objs:
            self.objs.append(obj)
    
    def ticks(self):
        time.sleep_ms(self.ticks_time_ms)
        for obj in self.objs:
            obj.ticks()

class TickObj:
    def __init__(self, end, repeat=True):
        self.position = 0
        self.end = end
        self.repeat = repeat     
        self.timeline = {n+1: [] for n in range(end)}
    
    def add_event(self, pos, event):
        self.timeline[pos].append(event)
    
    def remove_event(self, pos):
        self.timeline[pos].clear()
    
    def reset_position(self):
        self.position = 0
    
    def ticks(self):
        if self.position < self.end:
            self.position += 1
            for event in self.timeline[self.position]:
                event()
            if self.repeat and self.position == self.end:
                self.reset_position()
```

In this lesson we'll be using this code again to program a new game. Start your program by making a new file and copy-pasting the code above into it. We'll just need to make few edits to the old code before using it in your new game!

### Modifying the Code

The `Ticker` class has a method called `register()` which adds objects to the frame. Sometimes you'll want to take objects out of the frame, not just add new ones, so this time we'll add a method called `remove()` to delete any object we specify!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-4/7-4_2_E.png"/>

#### ■ Managing In-Frame Objects with ID Numbers

You may remember from a previous lesson that Python objects always have an ID number assigned to them when you make them. You can use the native `id()` function to find out the ID number of any object you've created!

Try running the code you have copied into your editor, then use the terminal to make an instance of the `TickObj` class and check its ID!

<pre class="prettyprint">
&gt;&gt;&gt; obj = TickObj(10)
&gt;&gt;&gt; print(id(obj))
1065455472
</pre>

###### ★ ID numbers are set every time you make an object, so the ID number you get may not match the one in the example code.

Currently the objects in the frame are stored in the list property `objs`, but let's change this into a **dictionary using ID numbers as keys** instead. That will make us able to find objects by ID number and delete them. Just modify your code like this:

###### New Code (Line 7, Line 11, Line 15)

```python==1
import time

class Ticker:
    def __init__(self, fps):
        self.fps = fps
        self.ticks_time_ms = round(1000/fps)
        self.objs = {}  # Make the object into a dictionary
    
    def register(self, *objs):
        for obj in objs:
            self.objs[id(obj)] = obj  # Store objects with their ID numbers as their keys
    
    def ticks(self):
        time.sleep_ms(self.ticks_time_ms)
        for obj in self.objs.values():  # Use the dictionary method values() to retrieve just the stored objects
            obj.ticks()
```

Next let's define our new `remove()` method. This method needs to take multiple objects as arguments, then look up their ID numbers with the `id()` function and delete the items we specify from the `objs` property.

###### New Code (Lines 13-15)

```python==1
import time

class Ticker:
    def __init__(self, fps):
        self.fps = fps
        self.ticks_time_ms = round(1000/fps)
        self.objs = {}
    
    def register(self, *objs):
        for obj in objs:
            self.objs[id(obj)] = obj
    
    def remove(self, *objs):  # Remove the specified object from the frame
        for obj in objs:
            del self.objs[id(obj)]  # Delete specified items with a del statement
    
    def ticks(self):
        time.sleep_ms(self.ticks_time_ms)
        for obj in self.objs.values():
            obj.ticks()
```

Now we can delete objects from frames!

#### ■ Organizing Timeline Events for Objects by ID

Functions and methods also have their own ID numbers in Python. This means we can edit our code as shown below and use ID numbers to manage the events on an object's timeline!

###### New and Changed Code (Lines 27, Line 30, Lines 32-33, Line 41)

```python==22
class TickObj:
    def __init__(self, end, repeat=True):
        self.position = 0
        self.end = end
        self.repeat = repeat     
        self.timeline = {n+1: {} for n in range(end)}  # Changes into dictionary with format n+1: {}
    
    def add_event(self, pos, event):
        self.timeline[pos][id(event)] = event  # Store events using ID numbers as keys
    
    def remove_event(self, pos, event):  # Adds an event argument
        del self.timeline[pos][id(event)]  # Deletes specified items with a del statement
    
    def reset_position(self):
        self.position = 0
    
    def ticks(self):
        if self.position < self.end:
            self.position += 1
            for event in self.timeline[self.position].values():  # Uses the dictionary's values() method to retrieve only the stored events
                event()
            if self.repeat and self.position == self.end:
                self.reset_position()
```

Now we've finished updating our old program! In the next chapter we'll use this program to start making a racing game!

## Building the Racing Game

Follow the instructions linked below to construct the motorcycle controller for your racing game!

### Parts You'll Need

##### Parts
* Core Unit x 1
* Battery Box x 1
* Basic Cube (White) x 1
* Basic Cube (Gray) x 4
* Basic Cube (Black) x 1
* Basic Cube (Red) x 6
* Half A (Gray) x 2
* Half B (Gray) x 1
* Half C (White) x 7
* Half D (White) x 5
* Beam x 2
* Axle x 3

##### The Artec Block Shapes

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-4/7-4_3_E.png"/>

### Building Instructions

Download the instructions below to build the motorcyle you'll use in our racing game:

[Building a Motorcycle Controller](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-4/7-4_build_E.pdf)

## Programming a Racing Game

Now it's time to start designing the program for your racing game. Watch the video below to see how the game works and how to play it!

##### Gameplay Video

:::info
<iframe src="https://player.vimeo.com/video/482959860" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>
:::

##### How to Play the Game

1. Press the A button to start the game.
2. Once the game starts, enemies will appear as red and blue dots at the upper edge of the LED display, moving down one space at a time per set number of frames.
3. The player's racer is a green dot which they move to avoid enemies.
4. The racer moves in the direction you tilt the handlebars. (The Core Unit's Accelerometer is used to judge the handlebars' tilt.)
5. If the player can avoid all the enemies on screen for 30 seconds, they win the game!

### In-Game Objects

The following objects appear in this game. The classes for these objects can be made by inheriting from the `TickObj` class.

|Objects|What is it?|
|:---|:---|
|Racer|A green dot controlled by the player|
|Engine|A low engine noise that plays from the Buzzer at regular intervals|
|Enemy|Red and blue dots which appear randomly to chase the racer|

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-4/7-4_4_E.png"/>

We only need one racer object and one engine object in the frame at a time, but we want new enemy objects to be created and added to the frame at regular intervals. We also want to remove enemy objects from the frame when they reach the outer edge of the LED display so they can leave the game!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-4/7-4_5_E.png"/>

### Defining the Classes

All of the classes we're going to use to make objects that appear on the LED display are going to be subclasses of `TickObj`.

##### Class Table

|Class|What does it do?|
|:---|:---|
|`Racer`|Used to make the racer object|
|`Engine`|Used to make the engine object|
|`Enemy`|Used to make enemy objects|

Now let's go ahead and program all of these classes!

#### ■ The Racer Class

The `Racer` class inherits `TickObj`, and we'll give it the following new properties and methods as well.

##### New Properties in the `Racer` Class

|Property|What is it?|
|:---|:---|
|`accelerometer`|An instance of the `StuduinoBitAccelerometer` class, used to control the Core Unit's Accelerometer|
|`x`|X-coordinate on the LED display|
|`y`|Y-coordinate on the LED display|
|`color`|Color values for the LED display|

##### New and Overriden Methods in the `Racer` Class

|Property|What does it do?|
|:---|:---|
|`__init__()`|Sets the default values for the new properties</br>(Overrides the superclass method)|
|`move()`|Judges tilt based on Accelerometer values and changes x-coordinate|
|`reset()`|Resets properties to their default values|

First let's define the `__init__()` method to set the properties' default values. We're overriding a superclass method here, so remember that means this method will take the same arguments as the superclass!

###### New Code (Lines 46-53)

```python==46
class Racer(TickObj):
    from pystubit.board import accelerometer  # Imports an accelerometer object and adds it as a property
    
    def __init__(self, end, repeat=True):  # Takes the same arguments as the superclass method
        super().__init__(end, repeat)  # Calls the superclass method
        self.x = 2  # The starting position on the LED display is bottom center at (2, 4)
        self.y = 4
        self.color = (0, 31, 0)  # Racer appears as a green dot
```

Next let's define the `move()` method, which judges tilt based on the Accelerometer's values and moves the racer by changing its X-coordinate in response. The force of gravity makes the Accelerometer's values get larger when you tilt the Core Unit to the right and smaller when you tilt it to the left!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-4/7-4_6_E.png"/>

###### New Code (Lines 55-60)

```python==46
class Racer(TickObj):
    from pystubit.board import accelerometer
    
    def __init__(self, end, repeat=True):
        super().__init__(end, repeat)
        self.x = 2
        self.y = 4
        self.color = (0, 31, 0)
       
    def move(self):
        val = self.accelerometer.get_x()  # Finds the acceleration on the x-axis
        if val > 2 and self.x < 4:  # When tilted right ★ Except if at the right edge of the screen (x=4)
            self.x += 1  # Move right
        if val < -2 and self.x > 0:  # When tilted left ★ Except if at the left edge of the screen (x=0)
            self.x -= 1  # Move left
```

The last method we need to define is `reset()`, which sets the class's properties back to their default values. Set the racer's starting position to **(2,4)** and run the method `reset_position()` inherited from the superclass to reset the timeline to **0**!

###### New Code (Lines 62-65)

```python==46
class Racer(TickObj):
    from pystubit.board import accelerometer
    
    def __init__(self, end, repeat=True):
        super().__init__(end, repeat)
        self.x = 2
        self.y = 4
        self.color = (0, 31, 0)
       
    def move(self):
        val = self.accelerometer.get_x()
        if val > 2 and self.x < 4:
            self.x += 1 
        if val < -2 and self.x > 0:
            self.x -= 1   
        
    def reset(self):
        self.x = 2  # Returns racer to the starting position (2, 4)
        self.y = 4
        self.reset_position()  # Resets the timeline to 0
```

#### ■ The `Engine` Class

The `Engine` class inherits `TickObj`, and we'll give it the following new properties and methods. Unlike the `Racer` class, we won't be overriding `__init__()` in this one!

##### New Properties in the `Engine` Class

|Property|What is it?|
|:---|:---|
|`buzzer`|An instance of the `StuduinoBitBuzzer` class used to control the Core Unit's Buzzer|

##### New Methods in the `Engine` Class

|Property|What It Does|
|:---|:---|
|`on()`|Plays a low note (`"C3"`) from the Buzzer|
|`off()`|Turns off the Buzzer|
|`reset()`|Resets properties to their default values|

Now we'll define these new methods and properties like this:

###### New Code (Lines 67-77)

```python==67
class Engine(TickObj):
    from pystubit.board import buzzer  # Imports a buzzer object and adds it as a property
       
    def on(self):
        self.buzzer.on("C3")  # Be sure to add "self" here, since the buzzer object has been imported as a property

    def off(self):
        self.buzzer.off()

    def reset(self):
        self.reset_position()
```

#### ■ The `Enemy` Class

The `Enemy` class inherits `TickObj`, and we'll give it the following new properties and methods:

##### New Properties in the `Enemy` Class

|Property|What is it?|
|:---|:---|
|`x`|X-coordinate on the LED display (random)|
|`y`|Y-coordinate on the LED display|
|`color`|Color values for the LED display (random)|

##### New and Overriden Methods in the `Enemy` Class

|Property|What It Does|
|:---|:---|
|`__init__()`|Sets the default values for the new properties. </br>(Overrides the superclass method)|
|`move()`|Changes the y-coordinate by 1 at a time|

The color and x-coordinate at which enemies appear are choset a random, so we'll have to import the `random` module at the start of the program to let us use random numbers!

###### New Code (Line 2)
```python==1
import time
import random
```

We'll define the `Enemy` class as shown below. Since the racer's `color` property is set to green, we'll set the green level (G value) for the enemies to **0**, and use random numbers to pick their red (R value) and blue (B value) levels. In the code below, generating numbers with an increment of 1 code like `random.randint(1, 31)` wouldn't give us very many colors to choose from, so we've used `random.randint(1, 5) * 6` to make random numbers with an increment of 6 instead!

###### New Code (Lines 80-89)

```python==80
class Enemy(TickObj):
    def __init__(self, end, repeat=True):
        super().__init__(end, repeat)
        self.x = random.randint(0, 4)
        self.y = 0
        self.color = (random.randint(1, 5) * 6, 0, random.randint(1, 5) * 6)
        # randint(1, 5) generates a random number from 1-5, which is multiplied by 6 to increase the range of random numbers.
    def move(self):
        self.y += 1
```

### The Main Program

Now that we've defined our classes, we can move on to programming the main part of the game! We'll divide the main program into four functions.

##### Defining the Functions

|Function|What does it do?|
|:---|:---|
|main()|Contains the main processes of the game</br>(We'll define all of the functions listed below inside of this function!)|
|reset()|Resets the game to its initial state|
|clear_screen()|Turns off all lit LEDs for images shown in the display|
|start_game()|Contains processes which run while the game is in progress|

We'll put these functions together to make a complete program as shown in the flowchart below!

##### Game Outline

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-4/7-4_7_E.png"/>

We'll explain this in more detail later, but for now keep in mind that we'll count the number of frames we've moved on the timeline inside of the `start_game()` function. You'll need to use this count to set the enemy **spawn rate**, which is how often enemies appear!

You'll also need to use the `sound` module you made in Topic 7-1 to make the countdown to the start of the game and the Game Over sound. If you can't find the `sound` module on your Core Unit, download it using the link below before transferring it!

###### ✦ Right click the link and choose **Save file as...** to save the file.

[sound.py](https://www.artec-kk.co.jp//school/cl/textbooks/material_en/topic_7-1/sound.py)

We'll also have to set events on the timelines of every object which appears in the game as shown below:

##### Setting Up Timeline Events for Each Object

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-4/7-4_8_E.png"/>
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-4/7-4_9_E.png"/>
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-4/7-4_10_E.png"/>

Now let's go through and write the code for all of these!

#### ■ Setting the Framerate and Time Limit

Let's define the game's framerate and time limit as constants at the start of the program. For now we'll set the framerate to **60 FPS**, and the time limit to **30 seconds**.

###### New Code (Lines 4-5)

```python==1
import time
import random

FPS = 60  # The framerate
TIME_LIMIT = 30000  # The time limit (in milliseconds)

class Ticker:
```

#### ■ Making Objects and Variables
Start by importing the objects and modules from external files you need to use inside the `main()` function.

###### New Code (Lines 96-98)

```python==96
def main():
    from pystubit.board import display, button_a, Image
    import sound
```

Next let's make a racer object and an engine object, register their events, and add them to the frame. Let's also set up the image to be shown on the LED display as well as a list which keeps track of the enemy objects we'll have to generate again and again.

###### New Code (Lines 100-108)

```python==96
def main():
    from pystubit.board import display, button_a, Image
    import sound

    racer = Racer(4)  # The racer object
    racer.add_event(4, racer.move)
    engine = Engine(2)  # The engine object
    engine.add_event(1, engine.on)
    engine.add_event(2, engine.off)
    ticker = Ticker(FPS)  # Object which updates the frame
    ticker.register(racer, engine)
    screen = Image(5, 5)  # Blank image for the LED display
    enemies = []  # List for keeping track of enemy objects
```

#### ■ The `reset()` Function

The `reset()` function sets everything back to its initial state, and we define it as shown below! It runs the `reset()` methods for the `racer` and `engine` objects, while also removing all enemy objects from the frame and clearing the LED display.

###### New Code (Lines 110-117)

```python==110
    def reset():
        racer.reset()  # Resets racer to its initial state
        engine.reset()  # Resets engine to its initial state
        for enemy in enemies:  # Removes enemy objects from the frame
            ticker.remove(enemy)
        enemies.clear()  # Empties the list that records enemy objects
        clear_screen()  # Clears all images from the LED display
        display.clear()  # Clears everything from the LED display
```

#### ■ The `clear_screen()` Function

The `clear_screen()` turns off all LEDs for the image on the LED display. It does this by using a `for` statement and the `range()` function to go through every LEDs in the display starting at the upper left **(x=0, y=0)** to the bottom right **(x=4, y=4)** and turning them off!

###### New Code (Lines 119-122)

```python==119
    def clear_screen():
        for x in range(5):
            for y in range(5):
                screen.set_pixel(x, y, 0)
```

#### ■ The `start_game()` Function

Now it's time to write the `start_game()` function, which will be the core part of our game! This function begins by recording the game start time. Next, we'll define variables which count how many frames we've moved on the timeline as well as set the enemy spawn rate by determining how many frames should pass before a new enemy appears!

###### New Code (Lines 124-127)

```python==124
    def start_game():
        start_time = time.ticks_ms()  # Gets the game start time
        count = 0  # Variable that records how many frames have gone by on the timeline
        enemy_spawn_rate = 10  # The frequency at which new enemies appear
```



Next we'll set up a loop that repeats until the time limit is up and counts the number of frames that have passed!

###### New Code (Lines 129-131)

```python==124
    def start_game():
        start_time = time.ticks_ms()
        count = 0
        enemy_spawn_rate = 10

        while time.ticks_diff(time.ticks_ms(), start_time) < TIME_LIMIT:
            ticker.ticks()  # Continues to the next frame
            count += 1  # Counts the number of repeats
```

Now let's make a new enemy spawn when the number of frames we've counted is equal to `enemy_spawn_rate`. We'll then create an enemy object and register its events before adding it to the frame. Finally, we'll add the new enemy object to the `enemies` list!

###### New Code (Lines 132-137)

```python==124
    def start_game():
        start_time = time.ticks_ms()
        count = 0
        enemy_spawn_rate = 10

        while time.ticks_diff(time.ticks_ms(), start_time) < TIME_LIMIT:
            ticker.ticks()
            count += 1
            if count == enemy_spawn_rate:  # Count is equal to the enemy spawn rate
                new_enemy = Enemy(3)  # Makes a new enemy object
                new_enemy.add_event(3, new_enemy.move)  # Registers the enemy object's events
                ticker.register(new_enemy)  # Adds the enemy to the frame
                enemies.append(new_enemy)  # Adds the enemy to the list
                count = 0  # Resets the count
```

Next, let's write code which removes an enemy object once it has left the screen. We can judge that an enemy has left the screen if its `y` property (its y-coordinate on the LED display) is **greater than 4**!

###### New Code (Lines 139-145)

```python==129
        while time.ticks_diff(time.ticks_ms(), start_time) < TIME_LIMIT:
            ticker.ticks()
            count += 1
            if count == enemy_spawn_rate:
                new_enemy = Enemy(3)
                new_enemy.add_event(3, new_enemy.move)
                ticker.register(new_enemy)
                enemies.append(new_enemy)
                count = 0

            del_list = []  # List which stores enemy objects to be deleted
            for enemy in enemies:
                if enemy.y > 4:  # Enemy has left the LED display
                    ticker.remove(enemy)  # Removes the enemy from the frame
                    del_list.append(enemy)  # Adds to the list of enemies for deletion
            for enemy in del_list:  # Deletes enemies in list
                enemies.remove(enemy)  # Finds and deletes the specified enemy
```

Next we need some code to make the images which represent the racer and enemy objects appear on the LED display. First let's set up the images using the `StuduinoBitImage` class's `set_pixel_color` method.

###### New Code (Lines 147-151)

```python==139
            del_list = []
            for enemy in enemies:
                if enemy.y > 4:
                    ticker.remove(enemy)
                    del_list.append(enemy)
            for enemy in del_list:
                enemies.remove(enemy)

            clear_screen()  # Clears previous images from the display
            screen.set_pixel_color(racer.x, racer.y, racer.color)  # Sets up the racer image
            for enemy in enemies:  # Sets up the enemy images
                screen.set_pixel_color(enemy.x, enemy.y, enemy.color)
            display.show(screen, delay=0)  # Displays the completed images
```

Unlike last lesson's ping-pong game, not every object in this game gets a unique image as a unique instance of the StuduinoBitImage class. This is because using the `+` operator to combine images takes time to process, which means the game would lag behind the framerate we set. This game needs to change the images on the LED display faster than the last one did, so we're writing it like this instead!

Finally, this function needs to detect when the racer crashes into an enemy. Let's define a flag called `is_overlapped` and set its value to `True` if the racer's X- and Y- coordinate matches an enemy's. If the value of `is_overlapped` is `True`, we'll set the value of the variable `is_cleared` to `False` to show that the player has lost the game! We'll then use a `break` statement to end line 129's `while` loop without waiting for the time limit to run out.

###### New Code (Lines 153-160)

```python==147
            clear_screen()
            screen.set_pixel_color(racer.x, racer.y, racer.color)
            for enemy in enemies:
                screen.set_pixel_color(enemy.x, enemy.y, enemy.color)
            display.show(screen, delay=0)

            is_overlapped = False  # Flag marking whether the racer has hit an enemy
            for enemy in enemies:  # Checks all of the enemies to see if the racer is touching them
                if racer.x == enemy.x and racer.y == enemy.y:  # If the X- and Y- coordinate both match, the enemy and the racer have collided
                    is_overlapped = True
                    break  # Ends the loop if the racer has hit an enemy
            if is_overlapped:  # Loses the game if the racer hits an enemy
                is_cleared = False
                break  # Ends line 129's `while` loop without waiting for time to run out
```

If the `while` loop ends by reaching the time limit instead of the `break` statement, that means the player has won the game! Let's add an `else` statement to the `while` loop that sets the value of `is_cleared` to `True` to make that happen.

###### New Code (Lines 162-163)

```python==153
            is_overlapped = False
            for enemy in enemies:
                if racer.x == enemy.x and racer.y == enemy.y:
                    is_overlapped = True
                    break
            if is_overlapped:
                is_clear = False
                break

        else:  # Runs this part if the while loop ends normally. Make sure to indent the code correctly!
            is_cleared = True
```

This marks the end of one round of the game! Let's play a sound and show an image on the LED display to let the player know if they've won or lost.

###### New Code (Lines 165-172)

```python==162
        else:
            is_cleared = True

        engine.off()  # The Buzzer may still be playing the engine sound, so turn it off here
        time.sleep_ms(1000)  # Waits before displaying the game results
        if is_cleared:
            display.show(Image.HEART, color=(0, 31, 0))  # Shows a green heart on the display if the player has won
            sound.success()  # Plays the win sound from the sound module
        else:
            display.show(Image.SKULL, color=(31, 0, 0))  # Shows a red skull on the display if the player has lost
            sound.failed()  # Play the lose sound from the sound module
```

#### ■ Pressing A to Start the Game

The game starts when the player presses **button A**, so let's make the **letter A** appear on screen to let them know when to press the button! After they've pressed the button, run a countdown and then the `start_game()` function to start the game. When the game is over, pause for a moment then reset everything and make an A appear on the LED display again!

###### New Code (Lines 174-181)

```python==174
    display.show("A", delay=0, color=(31, 31, 31))
    while True:
        if button_a.is_pressed():
            sound.countdown()
            start_game()
            time.sleep_ms(2000)  # Pauses before starting the game again
            reset()
            display.show("A", delay=0, color=(31, 31, 31))
```

Now you've finished the main processes of the game. All you need to do to start playing your racing game is to call the function outside of itself!

###### New Code (Line 184)

```python==174
    display.show("A", delay=0, color=(31, 31, 31))
    while True:
        if button_a.is_pressed():
            sound.countdown()
            start_game()
            time.sleep_ms(2000)
            reset()
            display.show("A", delay=0, color=(31, 31, 31))


main()  # Calls the main() function
```

#### ■ Testing the Game

You might be tired out from writing a nearly 200 line program, but now you're done! You can transfer your program and see how it works. If your game isn't working right, compare the program you've written with the example code below to check for mistakes!

##### Example Code 4-3-1

```python==1
import time
import random

FPS = 60
TIME_LIMIT = 30000

class Ticker:
    def __init__(self, fps):
        self.fps = fps
        self.ticks_time_ms = round(1000/fps)
        self.objs = {}

    def register(self, *objs):
        for obj in objs:
            self.objs[id(obj)] = obj

    def remove(self, *objs):
        for obj in objs:
            del self.objs[id(obj)]

    def ticks(self):
        time.sleep_ms(self.ticks_time_ms)
        for obj in self.objs.values():
            obj.ticks()

class TickObj:
    def __init__(self, end, repeat=True):
        self.position = 0
        self.end = end
        self.repeat = repeat
        self.timeline = {n+1: {} for n in range(end)}

    def add_event(self, pos, event):
        self.timeline[pos][id(event)] = event

    def remove_event(self, pos, event):
        del self.timeline[pos][id(event)]

    def reset_position(self):
        self.position = 0

    def ticks(self):
        if self.position < self.end:
            self.position += 1
            for event in self.timeline[self.position].values():
                event()
            if self.repeat and self.position == self.end:
                self.reset_position()

class Racer(TickObj):
    from pystubit.board import accelerometer

    def __init__(self, end, repeat=True):
        super().__init__(end, repeat)
        self.x = 2
        self.y = 4
        self.color = (0, 31, 0)

    def move(self):
        val = self.accelerometer.get_x()
        if val > 2 and self.x < 4:
            self.x += 1
        if val < -2 and self.x > 0:
            self.x -= 1

    def reset(self):
        self.x = 2
        self.y = 4
        self.reset_position()

class Engine(TickObj):
    from pystubit.board import buzzer

    def __init__(self, end, repeat=True):
        super().__init__(end, repeat)

    def on(self):
        self.buzzer.on("C3")

    def off(self):
        self.buzzer.off()

    def reset(self):
        self.reset_position()

class Enemy(TickObj):
    def __init__(self, end, repeat=True):
        super().__init__(end, repeat)
        self.x = random.randint(0, 4)
        self.y = 0
        self.color = (random.randint(1, 5) * 6, 0, random.randint(1, 5) * 6)

    def move(self):
        self.y += 1

def main():
    from pystubit.board import display, button_a, Image
    import sound

    racer = Racer(4)
    racer.add_event(4, racer.move)
    engine = Engine(2)
    engine.add_event(1, engine.on)
    engine.add_event(2, engine.off)
    ticker = Ticker(FPS)
    ticker.register(racer, engine)
    screen = Image(5, 5)
    enemies = []

    def reset():
        racer.reset()
        engine.reset()
        for enemy in enemies:
            ticker.remove(enemy)
        enemies.clear()
        clear_screen()
        display.clear()

    def clear_screen():
        for x in range(5):
            for y in range(5):
                screen.set_pixel(x, y, 0)

    def start_game():
        start_time = time.ticks_ms()
        count = 0
        enemy_spawn_rate = 10

        while time.ticks_diff(time.ticks_ms(), start_time) < TIME_LIMIT:
            ticker.ticks()
            count += 1
            if count == enemy_spawn_rate:
                new_enemy = Enemy(3)
                new_enemy.add_event(3, new_enemy.move)
                ticker.register(new_enemy)
                enemies.append(new_enemy)
                count = 0

            del_list = []
            for enemy in enemies:
                if enemy.y > 4:
                    ticker.remove(enemy)
                    del_list.append(enemy)
            for enemy in del_list:
                enemies.remove(enemy)

            clear_screen()
            screen.set_pixel_color(racer.x, racer.y, racer.color)
            for enemy in enemies:
                screen.set_pixel_color(enemy.x, enemy.y, enemy.color)
            display.show(screen, delay=0)

            is_overlapped = False
            for enemy in enemies:
                if racer.x == enemy.x and racer.y == enemy.y:
                    is_overlapped = True
                    break
            if is_overlapped:
                is_cleared = False
                break

        else:
            is_cleared = True

        engine.off()
        time.sleep_ms(1000)
        if is_cleared:
            display.show(Image.HEART, color=(0, 31, 0))
            sound.success()
        else:
            display.show(Image.SKULL, color=(31, 0, 0))
            sound.failed()

    display.show("A", delay=0, color=(31, 31, 31))
    while True:
        if button_a.is_pressed():
            sound.countdown()
            start_game()
            time.sleep_ms(2000)
            reset()
            display.show("A", delay=0, color=(31, 31, 31))


main()
```

## Challenge: Adjusting the Difficulty

The difficult of the racing game we've made in this lesson is mainly based on these factors:

|Line Number|Factor|Effect on Difficulty|
|:---|:---|:---|
|2|Framerate (`FPS`)|The game moves faster the higher the framerate is, which makes the game harder|
|3|Time Limit (`TIME_LIMIT`)|The longer the time limit, the more difficult it will be to beat the game|
|100-101|Timeline Setting for `racer` Object|Giving the `move()` method a shorter cycle makes the player move the racer faster|
|127|Enemy Spawn Rate (`enemy_spawn_rate`)|The lower this value is, the more frequently enemies will appear|
|133-134|Timeline Setting for `new_enemy` Objects|Giving the `move()` method a shorter cycle lets enemies catch up with the racer faster|

Adjusting any of these five factors lets you change the game's difficulty level. Try out different values in your program to find a level that's most fun for you!

## Conclusion

### A Quick Review

In this lesson you used the techniques you learned in Topic 7-3 to write a more complex game program. You've been programming games throughout Topic 7, so by now you've probably realized that game programs can get very complicated, even if the game itself is simple. We're done with the game programming part of the course for now, but we encourage you to try designing and programming games of your own!

### Coming Up Next Time!

In the next topic you'll be learning about robot arms. Use multiple motors and sensors together to build robot arms that organize and transport items like real factory robots! 