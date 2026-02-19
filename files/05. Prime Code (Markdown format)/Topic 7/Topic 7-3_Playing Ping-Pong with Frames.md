---
tags: Python_English, Topic 7
---

Python Robotics Course Lesson 27

Topic 7-3
Playing Ping-Pong with Frames
===

Learn how to control multiple objects at the same time!

## In This Lesson...

Whether in films or video games, the pictures are made of still images, or **frames**, which update at regular intervals of time to simulate smooth movement! In this lesson, we're going to take a look at how we can update frames over an interval of time as we introduce techniques to control multiple objects on a shared timeline. In the second half of the lesson, we'll take our new knowledge and apply it to make a game of ping-pong on our LED display!

:::info

Lesson Introduction

■ Lesson 27 Contents
1. Controlling Timing for Multiple In-Game Objects
2. Making Ping-Pong
3. Challenge: Changing Difficulty with New Actions


In this lesson, we're going to learn new control methods which will let us develop complex game programs more efficiently!

At the start of the lesson, we'll introduce timing control methods. These will let us move individual objects on the same screen as well as make them talk! These concepts might be a little difficult to grasp, but when you use them they make the developing the complex programs used in games much, much easier.

In the second half of the lesson, we'll take what we learned in the first half and use it to make a game of ping-pong!



In this competitive two-player game, each player has to keep an eye on the ball and press their button at the right time to hit the ball back to their opponent. This might sound simple, but we're going to have to do some tricky things with timing to move the ball and make the rackets appear and disappear!

In the challenge at the end of the lesson we'll make the game even more fun by giving the ball a fixed chance to change speed!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!

:::

## Controlling Objects on a Shared Timeline

While this isn't specific to Python, we're going to learn about a new concept which involves controlling multiple objects on a **timeline** which they all share!

### Controlling Multiple Objects in Games Simultaneously

In games today, there's nothing at all surprising about seeing multiple objects like characters and backgrounds moving around the screen at once. The truth is, however, that making games like these requires some pretty advanced techniques!

As an easy example, let's try thinking about the following:

Let's say that we have a map of a village, and we want to make two villagers named **Brian** and **Anne**. We want them to talk to each other, with Brian talking **once every 3 seconds** and Anne talking **once every 6 seconds**. How would we make a program like that?

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_1_E.png"/>

Here we'll make the following class which we can use to model our characters.

##### Character Model Class

```python==1
class Character():
    def __init__(self, name):  # Constructor
        self.name = name  # Character name
        
    def speak(self):  # Method which shows character name and message
        print("{}:Hi!".format(self.name))
```

One way might be to count **1 second** at a time, then run each character object's `speak()` method when the time reaches **3 times** and **6 times** that amount! The code we can write to do this is as follows:

##### Example Code 2-1-1

###### ★ We can do this by using a **%** operator to divide the amount of elapsed time and see if the remainder is 0!

```python==1
import time
 
class Character():
    def __init__(self, name):
        self.name = name
        
    def speak(self):
        print("{}:Hi!".format(self.name))
        
Brian = Character("Brian")
Anne = Character("Anne")
 
count = 0  # Counter which increases by 1 every second
while True:
    time.sleep_ms(1000) # One second passes
    count += 1          # Counts elapsed time
    if count % 3 == 0:  # Elapsed time counted by threes (once every 3 seconds)
        Brian.speak()
    if count % 6 == 0:  # Elapsed time counted by sixes (once every 6 seconds)
        Anne.speak()
```

The thing to keep in mind with this program is that it uses a timeline which **progresses 1 second at a time**, and we control the characters based on that timeline!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_2_E.png"/>


This means that if we wanted to add a new character, we would just have to make a few small changes. The program below adds a new character named **Thomas** who talks every **5 seconds**!

##### Example Code 2-1-2
###### New Code (Line 12, Lines 22-23)

```python==1
import time
 
class Character():
    def __init__(self, name):
        self.name = name
        
    def speak(self):
        print("{}:Hi!".format(self.name))
        
Brian = Character("Brian")
Anne = Character("Anne")
Thomas = Character("Thomas")  # Creates new character Thomas
 
count = 0
while True:
    time.sleep_ms(1000)
    count += 1
    if count % 3 == 0:
        Brian.speak()
    if count % 6 == 0:
        Anne.speak()
    if count % 5 == 0:　# Elapsed time counted by fives (once every 5 seconds)
        Thomas.speak()
```

This is a simple example, but now we see how using a shared timeline to control multiple objects allows us to manage the progression of an entire game!

And now we're going to move on to something a bit more advanced. While we used a timeline which counted seconds in Example Code 2-1-1, let's take a look at a way to use **framerates** to control our objects. **Framerate** is used to manage the picture in movies and video games!

### What's a Framerate?

Whether films you shoot on a camera or games on a screen, the picture you see is made of still images, or **frames**, which update at regular intervals of time to simulate smooth movement. The speed at which the images update is called the **framerate**!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_3_E.png"/>

A framerate is measured in **fps** or **frames per second**. This is the number of still images shown per second on the screen!

As an example, let's think about a video which shows one of our characters walking. The character looks like it's walking because the video switches regularly between four different still images!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_4_5fps_E.gif"/>

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_6_E.png"/>

We can change how fast our character walks by changing the framerate of the video.

*  5 fps (the image updates **5** times per second)
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_4_5fps_E.gif"/>

*  10 fps (the image updates **10** times per second)
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_7_10fps_E.gif"/>

*  20 fps (the image updates **20** times per second)
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_8_20fps_E.gif"/>

While our examples above make our character walk faster, you usually update the framerate when you want to make a movement smoother. Video cameras often give you the option of recording at 30 fps or 60 fps, and a video shot at 60 fps looks a lot more fluid! The more frames you have, however, the bigger the file size of the video.


### Controlling Multiple Objects with Frames

Let's try putting a scene from a video game inside of one frame. This frame will hold multiple objects, which are the two characters in the image shown below. While the frames update at the framerate we've set, our objects will perform actions that we've set as they proceed along with each frame.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_9_E.png"/>

Real video games put multiple objects into a frame and use a shared timeline to control them, just like this. Now let's try this method for ourselves to make a program which works exactly like Example Code 2-1-1!

#### ■ Making Program Objects

We need to make the following objects for our program:

* **Framerate Control Object**
We'll use this object to make sure the frames update according to the framerate we set.
* **Character Objects**
These objects will control our two characters.

Since our characters will perform actions in the frame, we'll also assign them their own unique timelines in addition to the timeline which moves from frame to frame!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_10_E.png"/>

These timelines will proceed at the same rate as our frame timeline. These unique timelines hold methods, which are our characters' actions, at specific positions. By doing this we can make the characters perform these actions once we reach that frame on the timeline!

We can also make the program go back to the beginning position of the timeline and repeat the action once it reaches the end.

Now let's define each of the classes we'll need to make our objects in order, then start working on our program!

#### ■ Defining the Framerate Control Class

We're going to define this class using the name `Ticker`. Our `Ticker` class will have the properties and methods you see below.

We call it a **ticker** because it ticks at a regular interval to move on to the next frame!

##### `Ticker` Class Properties
* The `interval_time` Property
The amount of time (in milliseconds) between frames, calculated using our framerate.
* The `objs` Property
List which stores objects we control in the frame.

##### `Ticker` Class Methods

* The `register()` Method
Adds objects to the `objs` property.
* The `ticks()` Method
Runs the methods which move through the unique timelines of objects stored in the `objs` property.

Now let's define each of these properties and methods in order.

We'll start by declaring the `Ticker` class and defining our constructor `__init__()` method. This constructor takes the framerate (that's our `fps` property) as an argument. The program takes that framerate and uses the formula `1000/fps` to calculate the interval between frames in milliseconds and stores it in our `interval` property. We also have to make our `objs` property as an empty list.

###### ★ Since we'll use our `interval` property as an argument in the `time` module's `sleep_ms()` method, we'll have to use the `round()` function to convert it into an integer first!

```python==1
class Ticker:   
    def __init__(self, fps):  # Constructor
        self.interval_time = round(1000/fps)   # Counts in milliseconds
        self.objs = []  # List which stores objects controlled in frames
```

Next, we'll define the `register()` method. We'll use this to add objects to the frame! Since this method can take multiple objects as an argument, we'll use the list's `append()` method to save them to the `objs()` property in order.

###### New Code (Lines 6-8)

```python==1
class Ticker:   
    def __init__(self, fps):
        self.interval_time = round(1000/fps)
        self.objs = []
    
    def register(self, *objs):  # Defined as a variable-length positional argument in order to take multiple objects as arguments
        for obj in objs:  # Take note that this objs is the objs in the argument
            self.objs.append(obj)  # This objs is the objs property of the instance
```

Last, we'll define the `ticks()` method. This is the method we'll use to run the `ticks()` method defined in the `TickObj` superclass. This superclass serves as the model we'll use the create objects for our frame. Now let's write the code which makes our program retrieve the objects stored in our `objs` property in order before running the `ticks()` method.

###### New Code (Line 1, Lines 12-15)

```python==1
import time
 
class Ticker:   
    def __init__(self, fps):
        self.interval_time = round(1000/fps)
        self.objs = []
    
    def register(self, *objs):
        for obj in objs:
            self.objs.append(obj)
    
    def ticks(self):
        time.sleep_ms(self.interval_time)  # Once this methods runs, the program pauses for a single frame before running the code below
        for obj in self.objs:
            obj.ticks()
```

Since we can assume that this method will run repeatedly due to the `while` statement's infinite loop, we'll use the `time` module's `sleep_ms()` method to make sure that it only runs at every interval set in the `interval_time` property.

#### ■ Defining a Class with Critical Features for Frame Objects

The objects we'll be controlling in the frame have a common set of critical properties and methods. We're going to put all of these into a superclass called `TickObj` before defining subclasses for each object to inherit it.

##### `TickObj` Class Properties
* The `position` Property
Current position on an object's unique timeline. Initial position is **0**.
* The `end` Property
The last position on the timeline.
* The `repeat` Property
Decides whether to repeat once the end of the timeline is reached. Its default setting is `True`.
* The `timeline` Property
Dictionary which holds the events at each position on the timeline.

##### `TickObj` Class Methods
* The `add_event()` Method
Registers an event at the specified position on the timeline to the `timeline` property.
* The `remove_event()` Method
Deletes an event at the specified position on the timeline from the `timeline` property.
* The `ticks()` Method
Moves to the next position on the timeline.
* The `reset_position()` Method
Resets the `position` property to **0**.

Now let's define each of these properties and methods in order.

We'll start by declaring the `TickObj` class and defining our constructor `__init__()` method.

###### New Code (Lines 18-23)

```python==18
class TickObj:
    def __init__(self, end, repeat=True):
        self.position = 0  # Stores 0 as default position
        self.end = end  # End point of timeline
        self.repeat = repeat  # Boolean value which shows whether the program should repeat from beginning
        self.timeline = {n+1: [] for n in range(end)}  # Dictionary which manages events on the timeline
```

###### ★ Writing code like line 23's will store a dictionary in the `timeline` property as shown below.

<pre class="prettyprint">
{1: [], 2: [], 3: [], ... , end value: []}
</pre>

Our constructor takes the values stored in our `end` and `repeat` properties as arguments. The constructor also sets the initial position in the `position` property to **0**. Inside of our `timeline` property we'll store a dictionary with every position from 1 to the end point on the timeline as keys and placeholders as the definitions. We're using placeholders so we can register multiple events at the same position!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_11_E.png"/>

Next, we'll define our `add_event()` and `remove_event()` methods.

The `add_event()` method takes the event's method and a position on the timeline as arguments, then stores them to the `timeline` property.

The `remove_event()` method takes the event you wish to delete's method and a position on the timeline as arguments, then deletes them from the `timeline` property.

###### New Code (Lines 25-26, Lines 28-29)

```python==18
class TickObj:
    def __init__(self, end, repeat=True):
        self.position = 0
        self.end = end
        self.repeat = repeat     
        self.timeline = {n+1: [] for n in range(end)}
    
    def add_event(self, pos, event):
        self.timeline[pos].append(event)  # Registers event at the specified position
    
    def remove_event(self, pos):
        self.timeline[pos].clear()  # Deletes all events at the specified position
```

Let's continue by defining our `reset_position()` method. This method sets the value of the `position` property to **0**, which is the beginning of the timeline.

###### New Code (Lines 31-32)

```python==18
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
        self.position = 0  # Returns to initial position
```

Last, we'll define the `ticks()` method. This method adds **1** to the value we first stored in the `position` property to move one position forward on the timeline. Any events registered at that position will then run in order. If the `repeat` property is set to `True` and the program has reached the end of the timeline, it will also run the `reset_position()` method to go back to the beginning of the timeline!

###### New Code (Lines 34-40)

```python==18
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
        if self.position < self.end:  # Stops moving if already at the end of the timeline
            self.position += 1  # Moves position by one at a time
            for event in self.timeline[self.position]:
                event()  # Since this stores method objects, you can add () to run it
            if self.repeat and self.position == self.end:
                self.reset_position()  # Returns to initial position if program is set to repeat AND has reached the end of the timeline
```

And with that, we've finished defining our `TickObj` class. Now let's start defining the character classes which will inherit this one!

#### ■ Defining a Character Model Class

Here we're going to define a `Character` class which will be the model for our character objects. This class will inherit the `TickObj` class which we defined above. Similar to Example Code 2-1-1, we'll add a new `name` property for our character's name and a `speak()` method to make them talk!

We'll start by overriding our constructor `__init()__` method and adding a name parameter. This will let us add the `name` property. We'll use the `super()` function inside of this class so we can call the superclass's `__init()__` method. Since our superclass doesn't have a `speak()` method, we can go ahead and use the same code from Example Code 2-1-1 here.

###### New Code (Lines 42-48)

```python==42
class Character(TickObj):
    def __init__(self, name, end, repeat=True):
        super().__init__(end, repeat=repeat)
        self.name = name
       
    def speak(self):
        print("{} speaks.".format(self.name))
```

#### ■ Making Objects and Running Methods

Last, we'll take the classes we've made so far and use them to create objects in order to finish our program! First, let's create an object from our `Ticker` class. For now, we'll set the **fps** to **1**, or **one frame per second** like we did in Example Code 2-1-1.

###### New Code (Line 50)

```python==50
ticker = Ticker(1)
```

Next, let's create objects for our two characters. After that, let's use the `add_event()` method to register our `speak()` methods as events at the positions shown in the picture below.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_12_E.png"/>

###### New Code (Lines 51-54)

###### ★ Remember to leave out the `()` when using a method as an argument!

```python==50
ticker = Ticker(1)
Brian = Character("Brian", 3)  # Ends at 3 on the timeline
Anne = Character("Anne", 6)  # Ends at 6 on the timeline
Brian.add_event(3, Brian.speak)  # Registers Brian\'s speak() method at position 3
Anne.add_event(6, Anne.speak)  # Registers Anne\'s speak() method at position 6
```

Finally, we'll take our `ticker` object on line 50 and use its `register()` method to add our two character objects. Now everything is ready to go! The only thing we need to do is run the `ticker` object's `ticks()` method inside of our `while` statement's infinite loop, which will let us control our characters as we move from frame to frame!

###### New Code (Lines 56-58)

```python==50
ticker = Ticker(1)
Brian = Character("Brian", 3)
Anne = Character("Anne", 6)
Brian.add_event(3, Brian.speak)
Anne.add_event(6, Anne.speak)
 
ticker.register(Brian, Anne)  # Adds character objects to frame
while True:
    ticker.ticks()  # Moves to next frame
```

Check below to see what the finished program should look like. Make sure that everything in your code is correct before trying it out for yourself. If your code works in the same way as Example Code 2-1-1, you've made it correctly!

##### Example Code 2-3-1

```python==1
import time
 
class Ticker:  # Class which manages frame progression
    def __init__(self, fps):
        self.fps = fps  # Sets framerate
        self.ticks_time_ms = round(1000/fps)  # Interval between frames (in milliseconds)
        self.objs = []  # List which stores objects in frames
    
    def register(self, *objs):  # Registers objects to a frame
        for obj in objs:
            self.objs.append(obj)
    
    def ticks(self):  # Moves to the next frame and moves the object in the frame along the timeline
        time.sleep_ms(self.ticks_time_ms)  # Waits for a bit of time after the previous frame is processed
        for obj in self.objs:  # Moves every object along the timeline
            obj.ticks()  # TickObj class\'s ticks() method
 
class TickObj:  # Superclass for objects controlled in the frame
    def __init__(self, end, repeat=True):
        self.position = 0  # Current timeline position unique to each object. Initial position is 0.
        self.end = end  # Position of end point on the timeline
        self.repeat = repeat  # Decides whether to repeat once end of the timeline is reached
        self.timeline = {n+1: [] for n in range(end)}  # Dictionary which manages events on the timeline
    
    def add_event(self, pos, event):  # Adds event to specified position on the timeline
        self.timeline[pos].append(event)
    
    def remove_event(self, pos):  # Deletes event from specified position on the timeline
        self.timeline[pos].clear()
    
    def reset_position(self):  # Returns to initial position of 0
        self.position = 0
    
    def ticks(self):  # Moves to next position on the timeline
        if self.position < self.end:  # Only runs when not at the end point
            self.position += 1  # Moves to next position
            for event in self.timeline[self.position]:
                event()  # Runs event registered to current position
            if self.repeat and self.position == self.end:
                self.reset_position()  # Returns to beginning if set to repeat AND has reached the end point
 
class Character(TickObj):  # TickObj subclass which models characters
    def __init__(self, name, end, repeat=True):  # Overrides superclass constructor
        super().__init__(end, repeat=repeat)  # Runs superclass constructor
        self.name = name  # Defines a new property
       
    def speak(self):
        print("{} speaks.".format(self.name))
 
ticker = Ticker(1)
Brian = Character("Brian", 3)  # Sets timeline with an end point at 3
Anne = Character("Anne", 6)  # Sets timeline with an end point at 6
Brian.add_event(3, Brian.speak)  # Registers speak method to position 3
Anne.add_event(6, Anne.speak)  # Registers speak method to position 6
 
ticker.register(Brian, Anne)  # Registers character objects
while True:
    ticker.ticks()
```

Compared to Example Code 2-1-1, the code we wrote in Example Code 2-3-1 is a lot more complicated! Right now, you may be wondering why we have to write such complex code for a program which does the exact same thing. What we can tell you is that, when it comes to writing games with a serious number of processes to handle, these techniques are a secret weapon. In the next section, we're going to take Example Code 2-3-1 and make a game of ping-pong to show you how powerful these techniques can be!

## Making Ping-Pong

In this section we're going to make a game of ping-pong! Start by watching the video below to see how you'll play the game.

##### Ping-Pong Gameplay Video

:::info
<iframe src="https://player.vimeo.com/video/482911975" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>
:::

## Playing the Game

This game is meant for two players. Each player will choose either button A or button B and try their best to hit their button at the right time to hit it back to their opponent! The racket appears when you press your button. If the ball and racket align, the ball flies back to your opponent. Whoever miss the ball first loses!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_13_E.png"/>

### Critical Game Objects and Classes

Next, let's take a look at the objects we'll need to make this game as well as the classes we'll need to make those objects!

##### Critical Game Objects

|Variable|What is it?|
|:---|:---|
|`ball`|The ball|
|`racket_a`|Player A's racket|
|`racket_b`|Player B's racket|
|`ticker`|`Ticker` class object we created in Example Code 2-3-1|

##### Critical Classes for Objects

|Class|What is it?|
|:---|:---|
|`Ball`|Used to create our `ball` object</br>(Inherits our `TickObj` class from Example Code 2-3-1)|
|`Racket`|Used to create our `racket_a` and `racket_b`racket objects</br></br>(Inherits our `TickObj` class from Example Code 2-3-1)|

We'll have our `Racket` class take `"A"` or `"B"` in its argument in order to create objects for both player A and B. 

### Defining the Two Classes

First, let's create our `Ball` and `Racket` classes using the `TickObj` class we made in Example Code 2-3-1.

We can prepare by copying and pasting every from Example Code 2-3-1 up to line 40 into a new program!

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

### Defining the `Ball` Class

We'll have to make the `Ball` class inherit our `TickObj` class, then add the following properties and methods.

##### New Properties

|Property|What is it?|
|:---|:---|
|`x`|X-coordinate value for the ball's position on the LED display|
|`y`|Y-coordinate value for the ball's position on the LED display|
|`image`|Image used to show the ball on the LED display</br>(this is an instance of the StuduinoBitImage class)|
|`color`|Tuple for the color of the ball|
|`direction`|Variable for the ball's current direction|
|`RIGHT`, `LEFT`|Constant for the ball's current direction (class-member variable)|

##### Methods to Add and Override

|Method|What does it do?|
|:---|:---|
|`__init__()`|Adds properties and registers events using `add_event()`</br>(overrides superclass method)|
|`move()`|Moves the ball horizontally on the LED display's X-axis based on the `direction` property|
|`change_direction()`|Makes the ball move in the opposite direction|
|`get_image()`|Returns the image of the ball|
|`reset()`|Resets each property to its initial value|

We'll have to make the timeline in the picture below for our `Ball` class. In order to get the ball to move back and forth repeatedly, we'll set the `repeat` property inherited from our `TickObj` class to `True`.

### The `Ball` Class Timeline

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_14_E.png"/>

Now let's write our code in order! We'll start by defining our `RIGHT` and `LEFT` properties. These are both constants (class-member variables) which determine the direction the ball travels in. We'll use these instead of strings like `"right"` or `"left"`!

###### New Code (Lines 42-44)
```python==42
class Ball(TickObj):
    RIGHT = 1
    LEFT = 2
```

Next, we'll override the `__init__()` method and set the initial values for each property.

Let's start by setting our `LEFT` or `RIGHT` variable at random in the `direction` property to choose the direction of the ball. We'll also need to make an image of the ball to show on our LED display. We can do this by importing a `random` module and `StuduinoBitImage` (`Image`) class at the beginning of our program. 

###### New Code (Lines 2-3)

```python==1
import time
import random
from pystubit.board import Image
```

Now let's define our `__init__()` method and give its properties the following values. We'll also take the time to register our `move()` method (which we'll define later) to position **6** on the timeline.

###### New Code (Lines 48-55)

```python==44
class Ball(TickObj):
    RIGHT = 1
    LEFT = 2
 
    def __init__(self):
        super().__init__(6)  # End point is at position six. Leaves out repeat argument as the default value is True.
        self.x = 2	# The game starts with the ball at the center of the LED display (2, 2)
        self.y = 2
        self.image = Image(5, 5)  # Creates a 5 x 5 blank image
        self.color = (31, 20, 0)  # Orange
        self.direction = random.choice((self.RIGHT, self.LEFT))  # Decides direction of first serve at random
        self.add_event(6, self.move)  # Registers move() method to position 6 on the timeline
```

Let's continue by defining our `move()` method, which changes the position of the ball. The value of `x` will **increase by 1 at a time for `RIGHT`** and **decrease by 1 at a time for `LEFT`**.

###### New Code (Lines 57-61)

```python==48
    def __init__(self):
        super().__init__(6)
        self.x = 2
        self.y = 2
        self.image = Image(5, 5)
        self.color = (31, 20, 0)
        self.direction = random.choice((self.RIGHT, self.LEFT))
        self.add_event(6, self.move)
 
    def move(self):
        if self.direction == self.RIGHT:
            self.x += 1  # Moves ball to the right across the LED display
        else:
            self.x -= 1  # Moves ball to the left across the LED display
```

Next, we'll define the `change_direction()` method which will change the direction of the ball! If the current direction is `RIGHT`, it changes to `LEFT`. If it's `LEFT`, it changes to `RIGHT`! Once the direction has changed, we'll run the `reset_position()` method to reset the timeline's position to **0**!

###### New Code (Lines 63-68)

```python==57
    def move(self):
        if self.direction == self.RIGHT:
            self.x += 1
        else:
            self.x -= 1
 
    def change_direction(self):  # This method changes the ball's direction
        if self.direction == self.RIGHT:
            self.direction = self.LEFT
        else:
            self.direction = self.RIGHT
        self.reset_position()       # This method is already defined in our TickObj superclass
```

Now let's define the `get_image()` method, which will get the images we'll show on the LED display. This method will reset the image in the `image` property before turning on the LED at the current position of the ball using the color in the `color` property. Its return value will be the image we used with this method!

###### New Code (Lines 70-75)

```python==63
    def change_direction(self):
        if self.direction == self.RIGHT:
            self.direction = self.LEFT
        else:
            self.direction = self.RIGHT
        self.reset_position()
 
    def get_image(self):
        for x in range(0, 5):  # Turns LEDs off for the row the ball is moving through
            self.image.set_pixel(x, self.y, 0)
        if self.x >= 0 and self.x <= 4:  # Turns LED on at ball's current position
            self.image.set_pixel_color(self.x, self.y, self.color)
        return self.image
```

Last we'll define our `reset()` method, which will return every property to its initial value. This is the method you'll call when you want to replay the game! Write the code as shown below.

###### New Code (Lines 77-80)

```python==70
    def get_image(self):
        for x in range(0, 5):
            self.image.set_pixel(x, self.y, 0)
        if self.x >= 0 and self.x <= 4:
            self.image.set_pixel_color(self.x, self.y, self.color)
        return self.image
 
    def reset(self):
        self.x = 2  # Returns ball to the center of the LED display
        self.direction = random.choice((self.RIGHT, self.LEFT))  # Chooses and sets a random direction for the ball again
        self.reset_position()  # Returns position on timeline to 0
```

#### Defining the `Racket` Class

Let's define our `Racket` class next. This one will define our `TickObj` class, too! We'll add the following properties and methods as we go along.

##### New Properties

|Property|What is it?|
|:---|:---|
|`image`|Image used to show the rackets on the LED display</br>(this is an instance of the `StuduinoBitImage` class)|
|`is_valid`|Boolean value which decides whether to show the racket on the display</br>(`True` when shown, `False` when hidden)|



##### Methods to Add and Override

|Method|What is it?|
|:---|:---|
|`__init__()`|Adds properties and registers events using `add_event()`</br>(overrides superclass method)|
|`swing()`|Restarts timeline progression to show racket|
|`valid()`|Allows racket to be shown|
|`invalid()`|Keeps racket from being shown|
|`get_image()`|Returns image in order to show racket|
|`reset()`|Resets each property to its initial value|

We'll have to make the timeline in the picture below for our `Racket` class. Let's set the `repeat` process to `False` to make sure that our rackets only appear once when the player pushes their button! When we run the `valid()` method, we'll also make the image stay on the display until the `invalid()` method runs. Additionally, we'll make sure that the racket stays hidden until the program reaches the end of the timeline, even when you press the button again!

### The `Racket` Class Timeline

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_15_E.png"/>

Now let's write our code in order! We'll start by defining our `__init__()` method. Since we'll be making two racket objects for players A and B using the `Racket` class, we'll have it take either `"A"` or `"B"` for the buttons as an argument.

We'll also give this an initial value of the timeline's end point instead of **0**. This way we can make it start when the timeline has stopped moving. If we set this initial value to 0, the racket would show as soon as the program starts!

###### New Code (Lines 82-92)

```python==82
class Racket(TickObj):
    def __init__(self, button):
        super().__init__(15, repeat=False)
        if button == "A":  # Creates a blue racket image for player A
            self.image = Image("10000:10000:10000:10000:10000:", color=(0, 31, 0)) 
        else:  # Creates a green racket image for player B
            self.image = Image("00001:00001:00001:00001:00001:", color=(0, 0, 31))
        self.is_valid = False  # Starts as hidden
        self.position = self.end  # Setting the initial value to the end point instead of 0 makes it start when the timeline has stopped moving
        self.add_event(1, self.valid)  # Registers event
        self.add_event(5, self.invalid)  # Registers event
```

Next, let's define our `swing()` method. This will allow us to show the ball by restarting the timeline! When the `position` and `end` property's values match, this method will use the `reset_position()` method to reset the timeline's position to **0**. The timeline will start moving again once it's no longer at the end point!

###### New Code (Lines 94-96)

```python==82
class Racket(TickObj):
    def __init__(self, button):
        super().__init__(15, repeat=False)
        if pos == "A":
            self.image = Image("10000:10000:10000:10000:10000:", color=(0, 31, 0))
        else:
            self.image = Image("00001:00001:00001:00001:00001:", color=(0, 0, 31))
        self.is_valid = False
        self.position = self.end
        self.add_event(1, self.valid)
        self.add_event(5, self.invalid)
 
    def swing(self):
        if self.position == self.end:  # Only shows racket when at the end point of the timeline
            self.reset_position()
```

To continue, we'll define our `valid()` and `invalid()` methods. These tell our program whether to show or to hide images! Let's write the code as shown below:

###### New Code (Lines 98-102)

```python==94
    def swing(self):
        if self.position == self.end:
            self.reset_position()

    def valid(self):
        self.is_valid = True

    def invalid(self):
        self.is_valid = False
```

Next let's define the `get_image()` method, which will return images for our rackets! Let's write the code as shown below:

###### New Code (Lines 104-105)

```python==101
    def invalid(self):
        self.is_valid = False

    def get_image(self):
        return self.image
```

Last we'll define our `reset()` method, which will return every property to its initial value.

###### New Code (Lines 107-109)

```python==104
    def get_image(self):
        return self.image

    def reset(self):
        self.is_valid = False
        self.position = self.end
```

### Making the Main Game Processes

Now we've defined all of the classes we'll need to make our objects. It's time to move on to write the main part of our game! We'll also make the following functions to make our program easier to follow.

|Function|What does it do?|
|:---|:---|
|`main()`|Contains the main processes of our game|
|`reset()`|Contains the processes which reset our game to its initial state|
|`start_game()`|Contains the processes which play the game one time|

We'll define our `reset()` and `start_game()` functions inside of our `main()` function. Once we've put all of these functions together, our program will work according to the flowchart below!

##### Program Outline

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_16_E.png"/>

We'll use the `sound` module from **Topic 7-1** for our countdown to the start of the game as well as our win and lose sounds. If you can't find the `sound` module on your Core Unit, download it using the link below before transferring it!

###### ✦ Right click the link and choose **Save file as...** to save the file.

[sound.py](https://www.artec-kk.co.jp//school/cl/textbooks/material_en/topic_7-1/sound.py)

Now let's make each function in order!

#### Defining the `main()` Function

Let's start by defining our `main()` function. Before that, let's import our `display`, `button_a`, and `button_b` objects as well as our `sound` module at the beginning of the program.

###### New and Changed Code (Lines 3-4)

```python==1
import time
import random
from pystubit.board import display, button_a, button_b, Image
import sound
```

Next we'll use our `FPS` constant to set the framerate of the game. We can change this value later, but for now let's set it to **60 frames per second**!

###### New Code (Line 6)

```python==1
import time
import random
from pystubit.board import button_a, button_b, display, Image
import sound

FPS = 60

class Ticker:   
```

Now let's define the `main()` function! We'll have to define four objects using the classes that we made. These objects are `ticker`, `racket_a`, `racket_b`, and `ball`. Let's use the `ticker` object's `register()` method to add the other three objects to the frame's timeline!

###### New Code (Lines 114-119)

```python==114
def main():
    ticker = Ticker(FPS)
    racket_a = Racket("A")
    racket_b = Racket("B")
    ball = Ball()
    ticker.register(racket_a, racket_b, ball)
```

We'll have to press both the A and B buttons to start the game. While the `while` statement's infinite loop is running, let's have the program check whether you've pressed both buttons. If you have, the program will run each of our functions in order!

★ We'll define our `start_game()` and `reset()` functions later!

###### New Code (Lines 121-125)

```python==114
def main():
    ticker = Ticker(FPS)
    racket_a = Racket("A")
    racket_b = Racket("B")
    ball = Ball()
    ticker.register(racket_a, racket_b, ball)
 
    while True:
        if button_a.is_pressed() and button_b.is_pressed():
            sound.countdown()  # Countdown
            start_game()  # Starts the game
            reset()  # Resets game once it's over (*We\'ll define this next)
```


#### Definite the `reset()` Function

Since we won't have to call our `reset()` function outside of the `main()` function, let's define it inside of `main()`. The `reset()` function will run the `reset()` method for our three `racket_a`, `racket_b`, and `ball` objects. Let's write the code as shown below:

###### New Code (Lines 126-130)

```python==114
def main():
    ticker = Ticker(FPS)
    racket_a = Racket("A")
    racket_b = Racket("B")
    ball = Ball()
    ticker.register(racket_a, racket_b, ball)
	
    def reset():
        racket_a.reset()
        racket_b.reset()
        ball.reset()
 
    while True:
        if button_a.is_pressed() and button_b.is_pressed():
            sound.countdown()
            start_game()
            reset()
```

#### Defining the `start_game()` Function

We'll also define our `start_game()` function inside of the `main()` function since we won't have to call it anywhere else! Let's start by writing the code which moves from one frame to the next.

###### New Code (Lines 126-128)

```python==114
def main():
    ticker = Ticker(FPS)
    racket_a = Racket("A")
    racket_b = Racket("B")
    ball = Ball()
    ticker.register(racket_a, racket_b, ball)
    
    def reset():
        racket_a.reset()
        racket_b.reset()
        ball.reset()
        
    def start_game():
        while True:
            ticker.ticks()  # Moves to next frame
 
    while True:
        if button_a.is_pressed() and button_b.is_pressed():
            sound.countdown()
            start_game()
            reset()
```

Next, let's run the `swing()` method when either player presses their button. This will make the racket appear only when the timeline has reached the end point and gone back to position **0**!

###### New Code (Lines 130-133)

```python==126
    def start_game():
        while True:
            ticker.ticks()
 
            if button_a.is_pressed():
                racket_a.swing()  # Sets value of position property to 0 and moves along the timeline
            if button_b.is_pressed():
                racket_b.swing()  # Sets value of position property to 0 and moves along the timeline
```

Next, let's make the images which will appear on the LED display. We'll start by getting the image of the ball from the `ball` object and storing it in a variable called `screen`.

###### New Code (Line 135)

```python==130
            if button_a.is_pressed():
                racket_a.swing()
            if button_b.is_pressed():
                racket_b.swing()
 
            screen = ball.get_image()	
```

Using the `+` operator we overrode in `StuduinoBitImage` allows us to merge the ball's image with the images we take from `racket_a` and `racket_b`! However, this image will only be merged if the `is_valid` property is set to `True`. If it is, the merged image will appear on the LED display!

###### New Code (Lines 136-140)

```python==130
            if button_a.is_pressed():
                racket_a.swing()
            if button_b.is_pressed():
                racket_b.swing()

            screen = ball.get_image()
            if racket_a.is_valid:  # Merges image only if display setting is valid
                screen += racket_a.get_image()
            if racket_b.is_valid:
                screen += racket_b.get_image()
            display.show(screen, delay=0)	# Shows merged image
```

If the racket and ball images align, we'll have to run the `ball` object's `change_direction()` method to change the direction of the ball. However, since we have to take the ball's current direction into account, we'll make the ball fly **left** when **player A hits it** and fly **right** if **player B hits it**!

New Code (Lines 138-139, Lines 142-143)

```python==135
            screen = ball.get_image()
            if racket_a.is_valid:
                screen += racket_a.get_image()
                if ball.x == 0 and ball.direction == Ball.LEFT:   # Aligns with player A\'s racket when x-coordinate of ball is 0
                    ball.change_direction()
            if racket_b.is_valid:
                screen += racket_b.get_image()
                if ball.x == 4 and ball.direction == Ball.RIGHT:  # Aligns with player B\'s racket when x-coordinate of ball is 4
                    ball.change_direction()
            display.show(screen, delay=0)
```

Last, we'll need to check if the ball has flown outside of the LED display! Player A wins if `x > 4`, which means the ball has flown out of the right side of the screen. If `x < 0`, the ball has flown out of the left side of the screen and Player B wins! Let's show the winner on the LED display. Since this game only has one round, let's use a `break` statement to end the infinite loop!

###### New Code (Lines 146-150)

```python==135
            screen = ball.get_image()
            if racket_a.is_valid:
                screen += racket_a.get_image()
                if ball.x == 0 and ball.direction == Ball.LEFT:
                    ball.change_direction()
            if racket_b.is_valid:
                screen += racket_b.get_image()
                if ball.x == 4 and ball.direction == Ball.RIGHT:
                    ball.change_direction()
            display.show(screen, delay=0)
            
            if ball.x < 0 or ball.x > 4:  # If the ball travels outside of the LED display
                sound.failed()  # Play fail sound from sound module
                winner = "A" if ball.x > 4 else "B"  # Conditional statement to simplify code
                display.scroll("{} wins!".format(winner), delay=30)
                break  # Ends while statement loop on line 127
```

Let's put the `main()` function outside of the rest of the program in order to start this chain of processes. Now your program is finished!

###### New Code (Line 159)

```python==152
    while True:
        if button_a.is_pressed() and button_b.is_pressed():
            sound.countdown()
            start_game()
            reset()


main()
```

It took us a while and a bit of work to get there, but our code would be a lot messier if it wasn't for the techniques we learned in section 2. As a final step, compare your code with the example code below and make sure that everything is correct before trying it out for yourself!

##### Example Code 3-4-1

```python==1
import time
import random
from pystubit.board import button_a, button_b, display, Image
import sound
 
FPS = 60  # Framerate. Runs at 60 frames per second.
 
class Ticker:   # Identical to Example Code 2-3-1
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
 
class TickObj:  # Identical to Example Code 2-3-1
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
 
class Ball(TickObj):  # Class for the ball object
    RIGHT = 1  # Constant indicating the direction of the ball
    LEFT = 2
 
    def __init__(self):
        super().__init__(6)  # Timeline end point is 6
        self.x = 2  # Ball's starting coordinates are (2, 2)
        self.y = 2
        self.image = Image(5, 5) # Blank image
        self.color = (31, 20, 0) # Orange
        self.direction = random.choice((self.RIGHT, self.LEFT))  # Chooses random direction for first serve
        self.add_event(6, self.move)  # Registers event to position 6 on the timeline
 
    def move(self):  # Changes X value to move in the current direction
        if self.direction == self.RIGHT:
            self.x += 1
        else:
            self.x -= 1
 
    def change_direction(self):  # Changes the current direction of the ball
        if self.direction == self.RIGHT:
            self.direction = self.LEFT
        else:
            self.direction = self.RIGHT
        self.reset_position()  # Resets timeline position after change
 
    def get_image(self):  # Returns image showing ball
        for x in range(0, 5):  # Clears image for a moment
            self.image.set_pixel(x, self.y, 0)
        if self.x >= 0 and self.x <= 4:  # Turns on LED at ball's current position
            self.image.set_pixel_color(self.x, self.y, self.color)
        return self.image
 
    def reset(self):  # Used to reset game when you decide to play again
        self.x = 2
        self.direction = random.choice((self.RIGHT, self.LEFT))
        self.reset_position()
 
class Racket(TickObj):  # Class for racket object
    def __init__(self, button):
        super().__init__(15, repeat=False)  # Timeline end point is 15. Will not repeat.
        if button == "A":  # Changes image depending on button used
            self.image = Image("10000:10000:10000:10000:10000:", color=(0, 31, 0))
        else:
            self.image = Image("00001:00001:00001:00001:00001:", color=(0, 0, 31))
        self.is_valid = False  # Starts with image display set to false
        self.position = self.end  # Initial position at end of timeline to prevent movement on timeline
        self.add_event(1, self.valid)  # Registers event to position 1 on timeline
        self.add_event(5, self.invalid)  # Registers event to position 5 on timeline
 
    def swing(self):  # Allows movement on timeline by setting position to 0
        if self.position == self.end:
            self.reset_position()
 
    def valid(self):  # Show images
        self.is_valid = True
 
    def invalid(self):  # Hide images
        self.is_valid = False
 
    def get_image(self):
        return self.image
 
    def reset(self):  # Used to reset game when you decide to play again
        self.is_valid = False
        self.position = self.end
 
def main():  # Main game processes
    ticker = Ticker(FPS)
    racket_a = Racket("A")
    racket_b = Racket("B")
    ball = Ball()
    ticker.register(racket_a, racket_b, ball)
    
    def reset():  # Resets settings before you play again
        racket_a.reset()
        racket_b.reset()
        ball.reset()
        
    def start_game():  # In-game processes
        while True:
            ticker.ticks()  # Move to next frame
 
            if button_a.is_pressed():  # Press button to swing racket
                racket_a.swing()
            if button_b.is_pressed():
                racket_b.swing()
 
            screen = ball.get_image()  # Creates image to be shown on LED display
            if racket_a.is_valid:  # Merge image only if display setting is valid
                screen += racket_a.get_image()
                if ball.x == 0 and ball.direction == Ball.LEFT:
                    ball.change_direction()  # Hits ball back if aligned with racket
            if racket_b.is_valid:  # Merge image only if display setting is valid
                screen += racket_b.get_image()
                if ball.x == 4 and ball.direction == Ball.RIGHT:
                    ball.change_direction()  # Hits ball back if aligned with racket
            display.show(screen, delay=0)  # Show generated image
            
            if ball.x < 0 or ball.x > 4:  # Judges result once ball passes outside of the LED display
                sound.failed()  # sound module's lose sound
                winner = "A" if ball.x > 4 else "B"  # Show winner on LED display
                display.scroll("{} wins!".format(winner), delay=30)
                break  # Ends infinite loop on line 127
 
    while True:  # Starts game when you press A and B buttons at the same time
        if button_a.is_pressed() and button_b.is_pressed():
            sound.countdown()
            start_game()
            reset()
 
 
main()
```

### Changing the Framerate

We set our game's framerate to **60 fps** in Example Code 3-4-1, but we can change this value to change the speed of our game! If we set it to **90 fps**, for example, the ball would move a bit faster. Setting it to **30 fps**, on the other hand, would slow the ball down!

* Setting `90fps` to Make the Ball Faster

```python==1
import time
import random
from pystubit.board import button_a, button_b, display, Image
import sound

FPS = 90  # 90fps = Moves at 11 ms per frame
```

* Setting `30fps` to Make the Ball Slower

```python==1
import time
import random
from pystubit.board import button_a, button_b, display, Image
import sound

FPS = 30  # 30fps = Moves at 33 ms per frame 
```

You might think that setting our game to `90fps` would make the ball three times faster than `30fps`, but this won't be the case. This is because it takes your Core Unit a bit of time to show images on its LED display! We'll tackle this this in the upcoming challenge by **making the ball move twice as fast without changing the framerate**!

## Challenge: Giving the Ball a Chance to Change Speed

While players can have fun with the game in Example Code 3-4-1 as it is, just hitting the ball back and forth at the same speed might start to get a little boring. That's why in this challenge we're going to improve the code and make our game a lot more exciting by giving the ball a **20% chance to move faster when you hit it back**!


##### Enhanced Ping-Pong Gameplay Video

:::info
<iframe src="https://player.vimeo.com/video/482916368" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>
:::

### Example Program

This challenge can be a bit tough, so let's keep the following in mind as we write the code for it:

#### ■ How do you make the ball move twice as fast?

This part is easy! We just have to register another `move()` method to a different position on the timeline for the ball's speed. Registering this method to position **3**, for example, would make the ball twice as fast!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_17_E.png"/>

If we wanted the ball to go back to its normal speed, we would just need to delete the event from position **3**!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-3/7-3_18_E.png"/>

Repeatedly adding and deleting the `move()` method at position **3** is what allows us to change the speed of our ball!

#### ■ How do you give the ball a 20% chance to speed up?

Let's try changing the program to give our ball a **20% chance to speed up when it changes direction**. 

We'll start by defining `SLOW` and `FAST` constants at the beginning of the `Ball` object. These constants show the ball's speed!

###### New Code (Lines 50-51)

```python==47
class Ball(TickObj):
    RIGHT = 1
    LEFT = 2
    SLOW = 3  # Ball moves slowly (normal speed)
    FAST = 4  # Ball moves quickly
```

We'll need to make a new `speed` property to add the **speed** data of our ball! Let's set the `__init__()` method's initial value to `SLOW`.

###### New Code (Line 60)

```python==53
    def __init__(self):
        super().__init__(6)
        self.x = 2
        self.y = 2
        self.image = Image(5, 5)
        self.color = (31, 20, 0)
        self.direction = random.choice((self.RIGHT, self.LEFT))
        self.speed = self.SLOW  # Starts as slow
        self.add_event(6, self.move)
```

Next, we'll define a `change_speed()` method to change the speed of the ball. This method will take the speed of the ball as an argument of either `SLOW` or `FAST`. If the argument doesn't match the ball's current speed, it will add or delete the `move()` method on the timeline!

###### New Code (Lines 70-76)

```python==63
    def change_direction(self):
        if self.direction == self.RIGHT:
            self.direction = self.LEFT
        else:
            self.direction = self.RIGHT
        self.reset_position()
    
    def change_speed(self, speed):
        if self.speed != speed:  # If current speed changes
            if speed == self.SLOW:  # Changes from fast to slow
                self.remove_event(3)  # Deletes event
            else:  # Changes from slow to fast
                self.add_event(3, self.move)  # Registers event
            self.speed = speed  # Stores new speed to property
```

We'll change the ball's speed inside of the `change_direction()` method. In order to give the ball a **20% chance** to change, we'll use the `random` module's `randint()` method. `randint()` picks a random number from **1** to **5**. We'll make our ball move **fast** if it picks **3** and **slow** if it picks **any other number**!

```python==63
    def change_direction(self):
        if self.direction == self.RIGHT:
            self.direction = self.LEFT
        else:
            self.direction = self.RIGHT
        self.reset_position()
        if random.randint(1, 5) == 3:  # Picks one number out of five, which gives a 20% chance
            self.change_speed(self.FAST)  # Sets speed to fast
        else:
            self.change_speed(self.SLOW)  # Sets speed to slow
```

And now we've finished changing our program! Take a look at the example code below to see what it should look like. Check the `Ball` class you defined against the example code if your program isn't working correctly!

##### Example Code 4-1-1

```python==1
import time
import random
from pystubit.board import button_a, button_b, display, Image
import sound
 
FPS = 60
 
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
 
class Ball(TickObj):  # Check definition for ball class which starts here
    RIGHT = 1
    LEFT = 2
    SLOW = 3
    FAST = 4
 
    def __init__(self):
        super().__init__(6)
        self.x = 2
        self.y = 2
        self.image = Image(5, 5)
        self.color = (31, 20, 0)
        self.direction = random.choice((self.RIGHT, self.LEFT))
        self.speed = self.SLOW
        self.add_event(6, self.move)
 
    def move(self):
        if self.direction == self.RIGHT:
            self.x += 1
        else:
            self.x -= 1
 
    def change_direction(self):
        if self.direction == self.RIGHT:
            self.direction = self.LEFT
        else:
            self.direction = self.RIGHT
        self.reset_position()
        if random.randint(1, 5) == 3:
            self.change_speed(self.FAST)
        else:
            self.change_speed(self.SLOW)
    
    def change_speed(self, speed):
        if self.speed != speed:
            if speed == self.SLOW:
                self.remove_event(3)
            else:
                self.add_event(3, self.move)
            self.speed = speed
 
    def get_image(self):
        for x in range(0, 5):
            self.image.set_pixel(x, self.y, 0)
        if self.x >= 0 and self.x <= 4:
            self.image.set_pixel_color(self.x, self.y, self.color)
        return self.image
 
    def reset(self):
        self.x = 2
        self.direction = random.choice((self.RIGHT, self.LEFT))
        self.change_speed(self.SLOW)
        self.reset_position()
 
class Racket(TickObj):
    def __init__(self, button):
        super().__init__(15, repeat=False)
        if button == "A":
            self.image = Image("10000:10000:10000:10000:10000:", color=(0, 31, 0))
        else:
            self.image = Image("00001:00001:00001:00001:00001:", color=(0, 0, 31))
        self.is_valid = False
        self.position = self.end
        self.add_event(1, self.valid)
        self.add_event(5, self.invalid)
 
    def swing(self):
        if self.position == self.end:
            self.reset_position()
 
    def valid(self):
        self.is_valid = True
 
    def invalid(self):
        self.is_valid = False
 
    def get_image(self):
        return self.image
 
    def reset(self):
        self.is_valid = False
        self.position = self.end
 
def main():
    ticker = Ticker(FPS)
    racket_a = Racket("A")
    racket_b = Racket("B")
    ball = Ball()
    ticker.register(racket_a, racket_b, ball)
    
    def reset():
        racket_a.reset()
        racket_b.reset()
        ball.reset()
        
    def start_game():
        while True:
            ticker.ticks()
 
            if button_a.is_pressed():
                racket_a.swing()
            if button_b.is_pressed():
                racket_b.swing()
 
            screen = ball.get_image()
            if racket_a.is_valid:
                screen += racket_a.get_image()
                if ball.x == 0 and ball.direction == Ball.LEFT:
                    ball.change_direction()
            if racket_b.is_valid:
                screen += racket_b.get_image()
                if ball.x == 4 and ball.direction == Ball.RIGHT:
                    ball.change_direction()
            display.show(screen, delay=0)
            
            if ball.x < 0 or ball.x > 4:
                sound.failed()
                winner = "A" if ball.x > 4 else "B"
                display.scroll("{} wins!".format(winner), delay=30)
                break
 
    while True:
        if button_a.is_pressed() and button_b.is_pressed():
            sound.countdown()
            start_game()
            reset()
 
 
main()
```

## Conclusion

### A Quick Review

In this lesson we took a look at the **frame handling** used in films and video games as we learned new techniques in game making. These techniques allowed us to use a shared timeline to control multiple objects simultaneously.

In the second half of the lesson we applied these techniques to make a game of ping-pong! If we think of the LED display as a video game screen, it takes some pretty advanced processes to draw multiple objects like your ball and rackets on the screen at the same time.

We'll be taking the techniques we learned here and using them in the next lesson, so don't be afraid to go back and review this lesson if there's anything you don't understand!

### Coming Up Next Time!

In the next lesson we'll be making a **racing game**. While the objects we've registered in games so far have been static, we'll be using more advanced techniques in our racing game to make new objects appear as well as delete any objects which we no longer need to draw!

