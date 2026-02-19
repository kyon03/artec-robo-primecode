---
tags: Python_English, Topic 9
---

Python Robotics Course Lesson 34

Topic 9-2
Parallel Processing Using Multithreading
===

Use multithreading to make a football penalty kick game!

## In This Lesson...

In the last lesson, we used timers and General Purpose Input/Outputs to learn about **interrupts**. An interrupt switches between multiple processes running on a computer at the same time, and it’s a very popular way to do **parallel processing**, also known as **multitasking**!

But did you know that Python offers you several different ways to do parallel processing? In this lesson we’re going to introduce one of those ways: **multithreading**!



:::info

Lesson Introduction

■ Lesson 34 Contents
1. What is Multithreading? 
2. Making a Penalty Kick Game 
3. Challenge: Making a Program without Multithreading 



In this lesson we’ll continue to learn about parallel processing. Here we’ll learn about threads, which is how a computer decides which instructions to run, and multithreading, which lets you run multiple processes at the same time!

In the beginning of the lesson, we’ll use a real-life example to explain what makes multithreading so special. Next, we’ll use some simple example code to show you how you can use modules to implement multithreading.

In the last half of the lesson, we’ll use multithreading and take on the practical challenge of making a penalty kick game, just like football!



Our penalty kick game will control Servomotors, sensors, the LED display, and the buzzer all at the same time. Multithreading lets us use incredibly simple code to program complex controls like these!

Last, we’ll take on the challenge of rewriting our multithreading program to make a program which does the same thing without using multithreading. This challenge will give you an even clearer look at what multithreading does!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!

:::

## What is Multithreading?

While Python offers many ways for you to use parallel processing, not all of these are available in MicroPython. As we explained in the last lesson, this is because MicroPython is a lightweight version of Python made especially for microcontrollers. Of the more limited features supported in MicroPython, interrupts and the `_thread` module (which alllows for multithreading) are among the most popular. Here we’re going to use the `thread` module as we learn how to write code for parallel processing!

### So What is Multithreading?
In computer jargon, the word **thread** refers to the smallest unit used to run a program. This explanation might be a little hard to grasp, so let’s try looking at multithreading by comparing it to making a tasty dish!

When we cook, we often find ourselves doing multiple things at the same time. Most people who make miso soup in Japan, for example, would follow the steps below to prepare it:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-2/9-2_1_E.png"/>

Steps like boiling water or slicing tofu would be individual units of the job, and on a computer these would be units of a process called **threads**! You’ll find that as you move along threads preparing your miso, the work would be split into two parts. Moving along multiple threads as you work is known as **multitasking**. Work which involves moving along only one thread, on the other hand, is known as a **single threaded** process!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-2/9-2_2_E.png"/>

### A Multithreaded Program using the `_thread` Module

Let’s say that we have two processes: one that plays the buzzer when you press A and another which makes letters scroll across the LED display when you press B. If we used the event polling explained in the last lesson, the code we write to do this would look like this:

###### ★ Polling involves periodically monitoring whether a certain event (like pressing a button) occurs and running a corresponding process when the event happens!


##### Example Code 2-2-1
```python=1
from pystubit.board import display, buzzer
from pystubit.board import button_a, button_b

def sound():  # This function plays the buzzer
    buzzer.on("C4", duration=1000)

def scroll():  # Function which scrolls the string Hi! Across the LED display
    display.scroll("Hi!", delay=50)
    
while True:  # Loop which monitors the state of each button and runs the corresponding function if they\’re pressed (polling)
    if button_a.is_pressed():
        sound()
    if button_b.is_pressed():
        scroll()
```

Run this program and you’ll see that pressing a button will run the corresponding process, **but as long as that process is running, nothing will happen if you press the button for the other process**! By using the `_thread` module and rewriting our code to use multithreading, we can make a program which allows us to **press the other button to run its process while the other process is running**!

Now let’s use the `_thread` module for ourselves as we rewrite the code. Let’s start by importing the `_thread` module at the beginning of our program.

###### New Code (Line 1)
```python=1
import _thread  # Imports the module
from pystubit.board import display, buzzer
from pystubit.board import button_a, button_b
```

Next, let’s move each of the `while` statement loops which monitor the state of our buttons inside of the `sound()` and `scroll()` functions.

###### New and Changed Code (Lines 6-8, Lines 11-13)
```python=1
import _thread
from pystubit.board import display, buzzer
from pystubit.board import button_a, button_b

def sound():
    while True:  # Loop which monitors the A button
        if button_a.is_pressed():
            buzzer.on("C4", duration=1000)

def scroll():
    while True:  # Loop which monitors the B button
        if button_b.is_pressed():
            display.scroll("Hi!", delay=50)
```

We can use the `_thread` module’s `start_new_thread()` function when we want to start a process for a new thread. The `start_new_thread()` function takes the following three arguments:

```
_thread.start_new_thread(function, args [,kwargs])
```

The first argument, `function`, takes the name of the function you want to process on the new thread. The second argument is `args`, and it takes a tuple containing the positional argument you want to pass to the function in the first argument. `kwargs` is our third argument, and it takes a dictionary containing the keyword argument you wish to pass to the function in the first argument. You’ll always need to set the first and second arguments in this function, but the third argument is optional!

##### Using the `start_new_thread()` Function
```python=1
import _thread

def func(arg1, arg2, kwarg1, kwarg2):
    print("positional argument:", arg1, arg2)
    print("keyword argument:", kwarg1, kwarg2)

_thread.start_new_thread(func, ("A","B"), {"kwarg1":"C", "kwarg2":"D"})
```
##### Program Results
<pre class="pretty print">
&gt;&gt;&gt; positional argument: A B
keyword argument: C D
</pre>

Now let’s use the `start_new_thread()` function to start two new threads which will process both the `sound()` and `scroll()` functions!

##### Example Code 2-2-2
###### New Code (Line 15, Line 16)

```python=1
import _thread
from pystubit.board import display, buzzer
from pystubit.board import button_a, button_b

def sound():
    while True:
        if button_a.is_pressed():
            buzzer.on("C4", duration=1000)

def scroll():
    while True:
        if button_b.is_pressed():
            display.scroll("Hi!", delay=50)
    
_thread.start_new_thread(sound, ())  # Sets an empty tuple here since the sound() function won\’t take any arguments
_thread.start_new_thread(scroll, ())  # Sets an empty tuple here for the same reason
```

Next, let’s try running this program. Press the B button to make the message scroll across the display, then try pressing A while it’s scrolling. Unlike Example Code 2-2-1, you should hear the buzzer play a sound! As you can see, we can use multithreading to process two different threads, which is exactly what makes multithreading so great!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-2/9-2_3_E.png"/>

### Notes on Making Multithreaded Programs

If you only have one process in your program but the code for it is pretty complex, you can use multithreading to split up the parts of your program and make your code for it much, much cleaner! This makes multithreading incredibly convenient, but there are a few things you’ll want to keep in mind to prevent fatal bugs from popping up in your program. In this section, we’ll go over two especially important things you’ll want to keep in mind!

#### ■ Threads Share a Global Scope

When you start multiple threads, any variables or functions on the global scope are shared with them. The example code below, for instance, has two functions which record how many times the A and B buttons have been pressed, and these functions are processed in different threads!

##### Recording How Many Times A and B Have Been Pressed

```python=1
import _thread
from pystubit.board import button_a, button_b

count = 0  # Global variable on the global scope

def event_button_a():  # Monitors the A button and records how many times it\’s been pressed
    global count
    
    while True:
        if button_a.was_pressed():
            count += 1
            print(count)


def event_button_b():  # Monitors the B button and records how many times it\’s been pressed
    global count
    
    while True:
        if button_b.was_pressed():
            count += 1
            print(count)


_thread.start_new_thread(event_button_a, ())
_thread.start_new_thread(event_button_b, ())
```

The global variable`count` is referenced by both our `event_button_a()` and `event_button_b()` functions. If we wanted to use this program to get a total of how many times both buttons have been pressed, it would work exactly as we expect it to. However, if we tried to find a separate count for each button, we’d run into some unexpected results!

This is a very bare bones example, but it shows what can happen when you use multithreading with complex processes. In this case, a variable we don’t want to change gets rewritten, which has a negative effect on the other thread. This is why you should try as much as possible to make your variables local to each thread. If you find yourself needing to overwrite a global variable in a thread, be extra careful that it won’t have any bad effects on other threads!

#### ■ Allocating a Limited Calculation Resource

The calculation your computer needs to do for a multithreaded program is much heavier compared to a single threaded program! Since multiple threads share a single calculation resource (this is the CPU), the computer splits up its this resource into precise segments of time which are given, or **allocated** to each thread.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-2/9-2_4_E.png"/>

This allocation process also takes up resources, which means that having too many threads will cause the entire program to slow down!

Now let’s try this for real by making a program which adds every number from **1** to **1,000**. We’ll make a single threaded and multithreaded version and compare how long it takes to calculate each one!

##### Single Threaded vs. Multithreaded: Adding 1-1000
```python=1
import _thread
import time

def addition():  # Adds every number from 1 to 1000
    start_time = time.ticks_ms()  # Records the start time for the thread\’s process
    _sum = 0  # The sum (Since sum is a reserved word in Python, add a _ at the beginning to prevent a conflict) 
    for num in range(1, 1000):
        _sum += num
    print(time.ticks_diff(time.ticks_ms(), start_time))  # Shows the amount of time taken to calculate
    
def make_threads(num):  # Makes the specified number of threads
    for _ in range(num):
        _thread.start_new_thread(addition, ())  # Performs addition for each thread
```

This program will let us see that there’s a very big difference in the processing time when creating a single thread versus creating 10 threads.

##### Creating a Single Thread
<pre class="prettyprint">
&gt;&gt;&gt; make_threads(1)
&gt;&gt;&gt; 4
</pre>

##### Creating 10 Threads

<pre class="prettyprint">
&gt;&gt;&gt; make_threads(10)
&gt;&gt;&gt; 3100
3100
3100
3100
3100
3100
3090
3080
3100
3130
</pre>

###### ★ Your times may be different than the ones shown here. The most important thing is to see how slow the process becomes!

Just as we explained above, a large amount of the processing time is used to allocate calculation resources to each thread. This means that the processing time takes considerably longer than you’d expect given the number of threads!

## Making a Penalty Kick Game

Now let’s use the multithreading feature we’ve just learned about to make a penalty kick game, just like in football. Follow along with the instruction manual below to build it!

### Parts You'll Need

##### Parts
* Core Unit x 1
* Robot Expansion Unit x 1
* Battery Box x 1
* Servomotor x 1
* IR Photoreflector x 1
* Sensor Connecting Cable (3-wire, 15cm) x 1
* Basic Cube (White) x 5
* Basic Cube (Gray) x 4
* Basic Cube (Black) x 1
* Basic Cube (Red) x 4
* Basic Cube (Green) x 1
* Half A (Gray) x 6
* Half B (Red) x 1
* Half C (White) x 18
* Beam x 4
* Gear (L) x 1
* Rack ×1

##### The Artec Block Shapes

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-2/9-2_5_E.png"/>

### Building the Penalty Kick Game

Follow the link below to find instructions for building the game:

[Building a Penalty Kick Game](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-2/9-2_build_E.pdf)

## Programming the Penalty Kick Game

Before we start programming our penalty kick game, let’s take a look at how to play it.

### Playing the Game

This game uses a white block for the ball. The player will have to “kick” the ball with their finger as they try to score a goal within the time limit. The goalkeeper is represented by a green block. It moves back and forth at random to defend the goal, and the player will have the kick the ball at the right time to score a goal! Now let’s watch a video to see how the game is actually played!

##### Penalty Kick Gameplay Video

<iframe src="https://player.vimeo.com/video/482974946" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

### Features of the Game

The parts of your penalty kick game will play the roles shown in the table below:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-2/9-2_6_E.png"/>

|Part|Role|What does it do?|
|:---|:---|:---|
|Servomotor + Gear|Goalkeeper|Moves randomly back and forth to defend the goal |
|LED Display|Electronic Signboard|Shows a message on repeat |
|IR Photoreflector|Referee Sensor|Judges whether a goal has been scored |
|Buzzer|Sound System|Plays a melody on repeat |

While we do need to control each of these parts in parallel, trying to figure out the right timing and write code which switches between parts would be a pretty big headache! This is where multithreading comes in. Let’s put the features of our four parts into separate functions and create four threads which we’ll use to run them simultaneously!

### Making the Program

We'll start by writing the code for each part in order.

#### ■ Moving the Goalkeeper at Random

The gear connected to the Servomotor, when combined with the rack, will turn the Servomotor’s rotation into linear movement. This mechanism is known as a **rack and pinion**. Turning the Servomotor in a range from **60** to **120** degrees will let our green goalkeeper block defend the whole goal.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-2/9-2_7_E.png"/>

However, it would be pretty easy to score a goal if the goalkeeper just repeated the same movements. By picking a random angle in the 60-120 degree range and having it change periodically (every 200 milliseconds in this example), we can make the game a real challenge! Depending on which random numbers are chosen, picking angles based on one-degree increments (say 117 or 72 degrees) may leave our goalkeeper stuck in one place for a long time. This is why we’ll choose angles in 10-degree increments.

Follow along below as we put this process into our `goalkeeper()` function!

```python=1
import time
import random  # Imports random module in order to generate random numbers
from pyatcrobo2.parts import Servomotor

# Randomly moves the goalkeeper (the green block) 
def goalkeeper(): 
    servo = Servomotor("P13")

    while True:
        angle = random.randint(6, 12) * 10  # Moves randomly between 60-120 degrees in increments of 10 degrees
        servo.set_angle(angle)  # Turns the Servomotor to move the goalkeeper
        time.sleep_ms(200)  # Sets an interval of 200 milliseconds before moving again
```

If you want to check how this program works so far, run it and call the `goalkeeper()` function from the terminal. Once you’re done checking, press the Reset button on your Core Unit to stop the goalkeeper!

#### Judging Whether a Goal Has Been Scored

When the ball (that’s our white block) passes in front of the IR Photoreflector, infrared light will be reflected back into the sensor. This means that the IR Photoreflector’s value will go up. By monitoring this change, we can tell whether the ball has made it into the goal!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-2/9-2_8_E.png"/>

We’ll also need to make the goalkeeper stop once a goal has been scored. To do this, we’ll make a global variable and share it with the other threads.

Now let’s take the process above and put it into a function called `judgment()`!

New and Changed Code (Line 3, Line 5, Line 11, Lines 17-27)

```python=1
import time
import random
from pyatcrobo2.parts import Servomotor, IRPhotoReflector  # Imports the IR Photoreflector control class

is_goal = False  # Variable which shares whether a goal has been scored (True when goal is scored)


def goalkeeper():  
    servo = Servomotor("P13")

    while not is_goal:  # Stops action when goal is scored. This means that it only continues when the value is False.
        angle = random.randint(6, 12) * 10
        servo.set_angle(angle)
        time.sleep_ms(200)

# Determines whether the ball has entered the goal
def judgment():
    global is_goal  # Declares variable as global in order to change it inside of the function
    
    irp = IRPhotoReflector("P0")
    threshold = irp.get_value() + 200  # Sets a threshold based on the first value found when nothing is in front of the sensor
    
    while not is_goal:  # Repeats action until goal is made
        time.sleep_ms(1)  # Gets IR Photoreflector value every millisecond
        val = irp.get_value()
        if val > threshold:  # Determines the ball has passed if value goes over the threshold
            is_goal = True
```

We’ve made `is_goal` as a global variable. This means that if we change its value to `True`, it will change the condition of the `while` statement on line 11 to stop the goalkeeper!

On line 21, we use the first IR Photoreflector value our program gets to set a threshold which will judge whether the ball has passed in front of the sensor(★). We could also find our threshold by setting the ball in front of the sensor and looking up the IR Photoreflector’s value in advance, but it’s important to remember that there are methods like this which allow you to set a threshold automatically!

###### ★ There will be small, constant changes in the IR Photoreflector's values due to the vibration caused by the Servomotor's movements. Rather than using the first value taken from the IR Photoreflector, we can adjust it by adding a large number like 200 to this value to detect any **distinct changes** not caused by vibration. This will let the game judge whether a goal has been scored!

We’ll also want to pay extra attention to line 24. If we have it get the values of the IR Photoreflector immediately, this thread may end up using all of the calculation resources by itself! This is why we’ve added a short wait time here.

Now let’s try making a thread for each of these two functions and running them!

###### New and Changed Code (Line 3, 31, and 32)

```python=1
import time
import random
import _thread  # Imports a new multithread module
from pyatcrobo2.parts import Servomotor, IRPhotoReflector

is_goal = False


def goalkeeper():  
    servo = Servomotor("P13")

    while not is_goal:
        angle = random.randint(6, 12) * 10
        servo.set_angle(angle)
        time.sleep_ms(200)


def judgment():
    global is_goal
    
    irp = IRPhotoReflector("P0")
    threshold = irp.get_value() + 200
    
    while not is_goal:
        time.sleep_ms(1)
        val = irp.get_value()
        if val > threshold:
            is_goal = True

# Creates and runs threads
_thread.start_new_thread(goalkeeper, ())
_thread.start_new_thread(judgment, ())
```

Now let’s try running the program and see if the goalkeeper stops moving when we score a goal. At this point we have two of the four features our game needs. Now we’re going to add features which make our game much more exciting!

#### Playing a Melody on Repeat

Here we’re going to make a `play_melody()` function which uses the buzzer to play a simple melody on repeat. We’ll put our favorite song into a tuple and use a loop to play one note of the melody at a time!

###### New and Changed Code (Line 5, Lines 31-58, Line 63)
```python=1
import time
import random
import _thread
from pyatcrobo2.parts import Servomotor, IRPhotoReflector
from pystubit.board import buzzer  # Imports buzzer object in order to use the buzzer
```
```python=18
def judgment():
    global is_goal
    
    irp = IRPhotoReflector("P0")
    threshold = irp.get_value() + 200
    
    while not is_goal:
        time.sleep_ms(1)
        val = irp.get_value()
        if val > threshold:
            is_goal = True

# Plays melody on repeat   
def play_melody():
    # Defines melody played by the buzzer. Example shown below.
    melody = (
        ("A5" , 600),  # Make a tuple out of each note and note length
        ("C6" , 900),
        ("A5" , 300),
        ("C6" , 300),
        ("A5" , 300),
        ("C6" , 300),
        ("A5" , 300),
        ("F5" , 1200),
        ("A5" , 300),
        ("A5" , 300),
        ("A5" , 300),
        ("G5" , 1200),
        ("G5" , 300),
        ("G5" , 300),
        ("G5" , 300),
        ("A5" , 1200),
        ("F5" , 600),
    )
    
    # Plays each note in order
    while not is_goal:  # Breaks loop when goal is scored
        for sound in melody:  # Gets and plays 1 note at a time
            buzzer.on(sound[0], duration=sound[1])
            if is_goal:  # Breaks the loop in the for statement when a goal is scored
                break
                

_thread.start_new_thread(goalkeeper, ())
_thread.start_new_thread(judgment, ())
_thread.start_new_thread(play_melody, ())  # Starts and runs a new thread
```

Using a loop to play one note at a time lets us stop the melody when a goal is scored, even if the melody is in the middle of playing! The place where this actually happens is the `break` statement on line 58, which ends the loop.

#### ■ Repeating a Message on the Electronic Signboard

To continue, let’s make a `signboard()` function to scroll a text string across the LED display, which is our game’s electronic signboard! Text strings can use sequence data like tuples or lists, and we can use a `for` statement to pull one character at a time from that data.

###### New and Changed Code (Line 5, Lines 60-68, Line 74)
```python=1
import time
import random
import _thread
from pyatcrobo2.parts import Servomotor, IRPhotoReflector
from pystubit.board import buzzer, display  # Imports additional object in order to use the LED display
```
```python=53
    while not is_goal:
        for sound in melody:
            buzzer.on(sound[0], duration=sound[1])
            if is_goal:
                break

# Scrolls a message across the signboard on a loop
def signboard():
    message = "PKGAME"  # Display shows the message here. This can be anything.

    # Loop which shows each letter in the message in order
    while not is_goal:
        for word in message:  # Gets each letter of the message in order
            display.show(word)
            if is_goal:  # Breaks the loop in the for statement when a goal is scored
                break


_thread.start_new_thread(goalkeeper, ())
_thread.start_new_thread(judgment, ())
_thread.start_new_thread(play_melody, ())
_thread.start_new_thread(signboard, ())  # Starts and runs a new thread
```

And now we’ve made all four of the features for our game. Last, we’ll finish our game by adding processes which count the amount of elapsed time, show the result of the game on the signboard, and start the game when we press the A button!

#### ■ Counting Elapsed Time

We’ll set a temporary time limit of 30 seconds, or 30,000 milliseconds, here. Let’s define this time limit as a constant at the beginning of the program. You can adjust this time limit later on in order to change the difficulty of the game!

###### New Code (Line 7)

```python=1
import time
import random
import _thread
from pyatcrobo2.parts import Servomotor, IRPhotoReflector
from pystubit.board import buzzer, display

TIME_LIMIT = 30000  # Sets time limit to 30 seconds (30,000 milliseconds)
```

We’ll put all of the processes which start our game into a single function. This function includes the process which makes threads for the four main features of our game! This function will also reset our game and count the amount of time that has elapsed. We’ll need to make the game stop once the time limit has been passed, just like it does when the player scores a goal. Let’s put this information into a global variable which will be shared with every thread.

Now let’s take all of these processes and put them into a function called `start_game()`!

###### New and Changed Code (Line 10, Lines 70-88)

```python=1
import time
import random
import _thread
from pyatcrobo2.parts import Servomotor, IRPhotoReflector
from pystubit.board import buzzer, display

TIME_LIMIT = 30000

is_goal = False
over_time_limit = False  # Global variable which judges whether the time limit has been passed
```
```python=60
def signboard():
    message = "PKGAME"

    while not is_goal:  
        for word in message: 
            display.show(word)
            if is_goal:
                break

# Function which contains all of the processes which start the game
def start_game():
    global is_goal, over_time_limit
    
    # Resets the game
    is_goal = False
    over_time_limit = False
       
    # Move the code which starts threads for each function and runs them here
    _thread.start_new_thread(goalkeeper, ())
    _thread.start_new_thread(judgment, ())
    _thread.start_new_thread(play_melody, ())
    _thread.start_new_thread(signboard, ())
    
    start_time = time.ticks_ms()  # Records the game start time
    # Loop which monitors whether a goal has been scored or the time limit has been passed
    while not is_goal and not over_time_limit:
        # Compares the amount of time which has elapsed since the start of the game with the time limit
        if time.ticks_diff(time.ticks_ms(), start_time) > TIME_LIMIT:
            over_time_limit = True  # Use a global variable to tell other threads that the time limit is passed
```

Since we’ve made a variable which can share the information that the time limit is up, let’s change the code so it uses this information to tell each part of the game to stop!

###### Changed Code (Line 16, Line 28, Line 56, Line 59, Line 68, Line 69)
```python=13
def goalkeeper():
    servo = Servomotor("P13")

    while not is_goal and not over_time_limit:  # Continues only when both values are False
        angle = random.randint(6, 12) * 10
        servo.set_angle(angle)
        time.sleep_ms(200)


def judgment():
    global is_goal
    
    irp = IRPhotoReflector("P0")
    threshold = irp.get_value() + 200
    
    while not is_goal and not over_time_limit:  # Continues only when both values are False
        time.sleep_ms(1)
        val = irp.get_value()
        if val > threshold:
            is_goal = True

 
def play_melody():
    melody = (
        ("A5" , 600),
        ("C6" , 900),
        ("A5" , 300),
        ("C6" , 300),
        ("A5" , 300),
        ("C6" , 300),
        ("A5" , 300),
        ("F5" , 1200),
        ("A5" , 300),
        ("A5" , 300),
        ("A5" , 300),
        ("G5" , 1200),
        ("G5" , 300),
        ("G5" , 300),
        ("G5" , 300),
        ("A5" , 1200),
        ("F5" , 600),
    )
    
    while not is_goal and not over_time_limit:  # Continues only when both values are False
        for sound in melody:
            buzzer.on(sound[0], duration=sound[1])
            if is_goal or over_time_limit:  # Adds an or condition as the loop needs to stop when either condition is met
                break


def signboard():
    message = "PKGAME"

    while not is_goal and not over_time_limit:  # Continues only when both values are False
        for word in message:
            display.show(word)
            if is_goal or over_time_limit:  # Adds an or condition as the loop needs to stop when either condition is met
                break
```

#### ■ Showing the Result of the Game

Next, let’s add a process which shows us the result of the game. If the player scores a goal, they’ll see a happy face and hear a high note. If they don’t score, they’ll see a sad face and hear a low buzz!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-2/9-2_9_E.png"/>

The two images we need are included in the `StuduinoBitImage` (`Image`) class, so let’s import it!

###### New and Changed Code (Line 5, Lines 94-103)

```python=1
import time
import random
import _thread
from pyatcrobo2.parts import Servomotor, IRPhotoReflector
from pystubit.board import buzzer, display, Image  # Imports the Image class
```
```python=88
    start_time = time.ticks_ms()
    while not is_goal and not over_time_limit:
        if time.ticks_diff(time.ticks_ms(), start_time) > TIME_LIMIT:
            over_time_limit = True

# Shows the result of the game
def show_result():
    if is_goal:  # Player scores a goal and wins
        display.show(Image.HAPPY, color=(0, 31, 0))  # Makes image green
        buzzer.on("C7", duration=200)  # The win sound
        buzzer.on("A6", duration=600)
    else: # The round is over and the player loses
        display.show(Image.SAD, color=(0, 0, 31))  # Makes the image blue
        buzzer.on("C4", duration=800)  # The lose sound
    time.sleep_ms(1000)  # Waits for one second before clearing the image
    display.clear()
```

Now that we’ve defined the function which shows our images, let’s call it at the end of the `start_game()` function!

###### New Code (Line 93, Line 94)
```python=74
def start_game():
    global is_goal, over_time_limit
    
    is_goal = False
    over_time_limit = False
    
    for num in range(3, -1, -1):
        display.show(str(num))
    
    _thread.start_new_thread(goalkeeper, ())
    _thread.start_new_thread(judgment, ())
    _thread.start_new_thread(play_melody, ())
    _thread.start_new_thread(signboard, ())
    
    start_time = time.ticks_ms()
    while not is_goal and not over_time_limit:
        if time.ticks_diff(time.ticks_ms(), start_time) > TIME_LIMIT:
            over_time_limit = True

    time.sleep_ms(1000)  # Waits for one second after the game ends before showing the result
    show_result()
```


#### ■ Pressing A to Start the Game

Once we’ve added a part to run the `start_game()` function when the player presses the A button, our program is finished. We’ll also add some code at the end to add a countdown to the start of the game!

###### New Code (Line 5, Lines 108-110)

```python=1
import time
import random
import _thread
from pyatcrobo2.parts import Servomotor, IRPhotoReflector
from pystubit.board import buzzer, display, Image, button_a  # Imports a button A object
```
```python=96
def show_result():
    if is_goal:
        display.show(Image.HAPPY, color=(0, 31, 0))
        buzzer.on("C7", duration=200)
        buzzer.on("A6", duration=600)
    else:
        display.show(Image.SAD, color=(0, 0, 31))
        buzzer.on("C4", duration=800)
    time.sleep_ms(1000)
    display.clear()
    
# Starts game when you press the A button
while True:
    if button_a.is_pressed():
        # Counts down from 3 to 0
        for num in range(3, -1, -1):
            display.show(str(num))
        start_game()
```

Now try running this program and see how it plays! You have a few options if you want to adjust the difficulty of your game. These include changing the time limit, adjusting how quickly the Servomotor moves, or using two blocks to make the goalkeeper instead of just one.

If you’re having trouble with bugs in your program, try fixing it by comparing it with the example code below!

##### Example Code 4-3-1
```python=1
import time
import random
import _thread
from pyatcrobo2.parts import Servomotor, IRPhotoReflector
from pystubit.board import buzzer, display, Image, button_a

TIME_LIMIT = 30000

is_goal = False
over_time_limit = False


def goalkeeper():
    servo = Servomotor("P13")

    while not is_goal and not over_time_limit:
        angle = random.randint(6, 12) * 10
        servo.set_angle(angle)
        time.sleep_ms(200)


def judgment():
    global is_goal
    
    irp = IRPhotoReflector("P0")
    threshold = irp.get_value() + 200
    
    while not is_goal and not over_time_limit:
        time.sleep_ms(1)
        val = irp.get_value()
        if val > threshold:
            is_goal = True


def play_melody():
    melody = (
        ("A5" , 600),
        ("C6" , 900),
        ("A5" , 300),
        ("C6" , 300),
        ("A5" , 300),
        ("C6" , 300),
        ("A5" , 300),
        ("F5" , 1200),
        ("A5" , 300),
        ("A5" , 300),
        ("A5" , 300),
        ("G5" , 1200),
        ("G5" , 300),
        ("G5" , 300),
        ("G5" , 300),
        ("A5" , 1200),
        ("F5" , 600),
    )
    
    while not is_goal and not over_time_limit:
        for sound in melody:
            buzzer.on(sound[0], duration=sound[1])
            if is_goal or over_time_limit:
                break


def signboard():
    message = "PKGAME"

    while not is_goal and not over_time_limit:
        for word in message:
            display.show(word)
            if is_goal or over_time_limit:
                break


def start_game():
    global is_goal, over_time_limit
    
    is_goal = False
    over_time_limit = False
        
    _thread.start_new_thread(goalkeeper, ())
    _thread.start_new_thread(judgment, ())
    _thread.start_new_thread(play_melody, ())
    _thread.start_new_thread(signboard, ())
    
    start_time = time.ticks_ms()
    while not is_goal and not over_time_limit:
        if time.ticks_diff(time.ticks_ms(), start_time) > TIME_LIMIT:
            over_time_limit = True

    time.sleep_ms(1000)
    show_result()


def show_result():
    if is_goal:
        display.show(Image.HAPPY, color=(0, 31, 0))
        buzzer.on("C7", duration=200)
        buzzer.on("A6", duration=600)
    else:
        display.show(Image.SAD, color=(0, 0, 31))
        buzzer.on("C4", duration=800)
    time.sleep_ms(1000)
    display.clear()
    
    
while True:
    if button_a.is_pressed():
        for num in range(3, -1, -1):
            display.show(str(num))
        start_game()
```

## Challenge: Making a Program without Multithreading

In this challenge, we’re going to take part of your penalty kick game’s multithreaded program and rewrite it to see what it would look like without multithreading!

The parts we’re going to rewrite are the `goalkeeper()` and `judgment()` functions. We’ll turn these parts into comments before putting our new code inside of the `start_game()` function.

```python=13
"""  # Make this a comment
def goalkeeper():
    servo = Servomotor("P13")

    while not is_goal and not over_time_limit:
        angle = random.randint(6, 12) * 10
        servo.set_angle(angle)
        time.sleep_ms(200)


def judgment():
    global is_goal
    
    irp = IRPhotoReflector("P0")
    threshold = irp.get_value() + 200
    
    while not is_goal and not over_time_limit:
        time.sleep_ms(1)
        val = irp.get_value()
        if val > threshold:
            is_goal = True
"""
```
```python=73
def start_game():
    global is_goal, over_time_limit
    
    is_goal = False
    over_time_limit = False
        
    # _thread.start_new_thread(goalkeeper, ())  # Make this a comment
    # _thread.start_new_thread(judgment, ())  # Make this a comment
    _thread.start_new_thread(play_melody, ())
    _thread.start_new_thread(signboard, ())
    
    start_time = time.ticks_ms()
    while not is_goal and not over_time_limit:
        if time.ticks_diff(time.ticks_ms(), start_time) > TIME_LIMIT:
            over_time_limit = True

    time.sleep_ms(1000)
    show_result()
```

### Changing the Program

Since we won’t need to make threads to run these functions, we’re going to put the code which moves the goalkeeper at random as well as the code which judges whether a goal has been scored inside of the main loop of the `start_game()` function (that’s the **while** statement on line 85).

```python=84
    start_time = time.ticks_ms()
    while not is_goal and not over_time_limit:
        if time.ticks_diff(time.ticks_ms(), start_time) > TIME_LIMIT:
            over_time_limit = True
```

Let’s start by making the code which judges whether a goal has been scored. For the most part, this will just be moving the code inside of the `judgment()` function. The only thing we’ll need to change is making our program get the value of the IR Photoreflector every 10 milliseconds instead of every single millisecond. Getting the value every millisecond won’t cause any errors, but it will cause the Servomotor to slow down just a bit!

###### New and Changed Code (Lines 76-77, Lines 89-92)
```python=73
def start_game():
    global is_goal, over_time_limit
    
    irp = IRPhotoReflector("P0")  # Creates an IR Photoreflector object
    threshold = irp.get_value() + 200  # Sets a threshold
    
    is_goal = False
    over_time_limit = False
        
    # _thread.start_new_thread(goalkeeper, ())
    # _thread.start_new_thread(judgment, ())
    _thread.start_new_thread(play_melody, ())
    _thread.start_new_thread(signboard, ())
    
    start_time = time.ticks_ms()
    while not is_goal and not over_time_limit:
        time.sleep_ms(10)  # Changes check from once every ms to once every 10 ms        
        val = irp.get_value()
        if val > threshold:
            is_goal = True
       
        if time.ticks_diff(time.ticks_ms(), start_time) > TIME_LIMIT:
            over_time_limit = True

    time.sleep_ms(1000)
    show_result()
```

Next, let’s move the code which moves our goalkeeper randomly. We’ll need to get a bit creative here. This function makes the Servomotor move every 200 milliseconds. Using the `goalkeeper()` function without making any changes would cause a delay in getting the value of the IR Photoreflector’s values, making both functions run every 210 milliseconds!

##### Moving the Code As-Is
```python=87
    start_time = time.ticks_ms()
    while not is_goal and not over_time_limit:
        time.sleep_ms(10)  # This delay also affects the movement of the Servomotor
        val = irp.get_value()
        if val > threshold:
            is_goal = True

        angle = random.randint(6, 12) * 10  # The code is added here without any changes
        servo.set_angle(angle)
        time.sleep_ms(200)  # This delay also affects retrieval of IR Photoreflector values
       
        if time.ticks_diff(time.ticks_ms(), start_time) > TIME_LIMIT:
            over_time_limit = True
```

This is why we’ll keep our delay in `time.sleep_ms(10)` on line 89. We'll use this in place of our 200-millisecond delay by making a new variable which counts the number of times the 10-millisecond delay loops. Now let’s rewrite the program like the code shown below!

###### New and Changed Code (Line 76, Line 88, Lines 98-101)
```python=73
def start_game():
    global is_goal, over_time_limit
    
    servo = Servomotor("P13")  # Creates a Servomotor object
    irp = IRPhotoReflector("P0")
    threshold = irp.get_value() + 200
    
    is_goal = False
    over_time_limit = False
        
    # _thread.start_new_thread(goalkeeper, ())
    # _thread.start_new_thread(judgment, ())
    _thread.start_new_thread(play_melody, ())
    _thread.start_new_thread(signboard, ())
    
    count = 0  # Variable which counts the number of loops
    start_time = time.ticks_ms()
    while not is_goal and not over_time_limit:
        count += 1  # Adds to the loop count
        
        time.sleep_ms(10)        
        val = irp.get_value()
        if val > threshold:
            is_goal = True
        
        if count == 20:  # Counting 20 loops means that 200 milliseconds have passed
            angle = random.randint(6, 12) * 10
            servo.set_angle(angle)
            count = 0  # Resets the loop and counts to 20 again
        
        if time.ticks_diff(time.ticks_ms(), start_time) > TIME_LIMIT:
            over_time_limit = True

    time.sleep_ms(1000)
    show_result()
```

Now try running this program and see if your game works the same way! The entire code for this program should look like this:


##### Example Code 5-1-1

```python=1
import time
import random
import _thread
from pyatcrobo2.parts import Servomotor, IRPhotoReflector
from pystubit.board import buzzer, display, Image, button_a

TIME_LIMIT = 30000

is_goal = False
over_time_limit = False

"""
def goalkeeper():
    servo = Servomotor("P13")

    while not is_goal and not over_time_limit:
        angle = random.randint(6, 12) * 10
        servo.set_angle(angle)
        time.sleep_ms(200)


def judgment():
    global is_goal
    
    irp = IRPhotoReflector("P0")
    threshold = irp.get_value() + 200
    
    while not is_goal and not over_time_limit:
        time.sleep_ms(1)
        val = irp.get_value()
        if val > threshold:
            is_goal = True
"""

def play_melody():
    melody = (
        ("A5" , 600),
        ("C6" , 900),
        ("A5" , 300),
        ("C6" , 300),
        ("A5" , 300),
        ("C6" , 300),
        ("A5" , 300),
        ("F5" , 1200),
        ("A5" , 300),
        ("A5" , 300),
        ("A5" , 300),
        ("G5" , 1200),
        ("G5" , 300),
        ("G5" , 300),
        ("G5" , 300),
        ("A5" , 1200),
        ("F5" , 600),
    )
    
    while not is_goal and not over_time_limit:
        for sound in melody:
            buzzer.on(sound[0], duration=sound[1])
            if is_goal or over_time_limit:
                break


def signboard():
    message = "PKGAME"

    while not is_goal and not over_time_limit:
        for word in message:
            display.show(word)
            if is_goal or over_time_limit:
                break


def start_game():
    global is_goal, over_time_limit
    
    servo = Servomotor("P13")
    irp = IRPhotoReflector("P0")
    threshold = irp.get_value() + 200
    
    is_goal = False
    over_time_limit = False
        
    # _thread.start_new_thread(goalkeeper, ())
    # _thread.start_new_thread(judgment, ())
    _thread.start_new_thread(play_melody, ())
    _thread.start_new_thread(signboard, ())
    
    count = 0
    start_time = time.ticks_ms()
    while not is_goal and not over_time_limit:
        count += 1
        
        time.sleep_ms(10)        
        val = irp.get_value()
        if val > threshold:
            is_goal = True
        
        if count == 20:
            angle = random.randint(6, 12) * 10
            servo.set_angle(angle)
            count = 0
        
        if time.ticks_diff(time.ticks_ms(), start_time) > TIME_LIMIT:
            over_time_limit = True

    time.sleep_ms(1000)
    show_result()


def show_result():
    if is_goal:
        display.show(Image.HAPPY, color=(0, 31, 0))
        buzzer.on("C7", duration=200)
        buzzer.on("A6", duration=600)
    else:
        display.show(Image.SAD, color=(0, 0, 31))
        buzzer.on("C4", duration=800)
    time.sleep_ms(1000)
    display.clear()
    
    
while True:
    if button_a.is_pressed():
        for num in range(3, -1, -1):
            display.show(str(num))
        start_game()
```

While we didn’t make any major changes, having to make a `count` variable to make sure our delays were timed correctly means that we ended up with some pretty unconventional code. Now think about how you would feel if someone asked you to rewrite your `play_melody()` and `signboard()` functions without using multithreading. You don’t even have to guess that it would be a ton of work! However, by doing this challenge you can see what makes multithreading super convenient, and how using it well lets you make simpler, more concise code!

## Conclusion

### A Quick Review
In this lesson we learned about a new feature called **multithreading**. Multithreading allows you to run multiple processes in parallel as well as turn the complex code needed for a single process into code that’s much more simple!

On the other hand, multithreading can take up a lot of calculation resources and, if you have multiple threads, you can end up with unexpected problems in your program without a clear idea of where or why they happened. This is why you should always use flowcharts or other visuals to plan out your programs in advance rather than using multithreading in the spur of the moment!

### Coming Up Next Time!

In the next lesson, we’ll learn a new way to send and receive data and use it to coordinate threads in a multithreaded program, allowing them to work together on a single job!