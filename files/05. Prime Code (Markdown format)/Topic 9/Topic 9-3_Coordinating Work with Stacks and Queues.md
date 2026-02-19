---
tags: Python_English, Topic 9
---

Python Robotics Course Lesson 35

Topic 9-3
Coordinating Work with Stacks and Queues
===

Learn how to send and receive data between multiple tasks to move through the process for a whole job!

## In This Lesson...
Parallel processing techniques like interrupts and multithreading allow us to put together programs which transmit data between multiple tasks in order to work through a single job. You can use global variables in the global scope in order to send and receive data between tasks, using them as a kind of temporary storage space for data. In this lesson we’re going to use **stacks** and **queues**, two data structures we can use as storage spaces. We’ll use these to coordinate multiple tasks and make a robot which can transport and sort blocks!



:::info

Lesson Introduction

■ Lesson 35 Contents
1. Data Structures: Stacks and Queues
2. Making a Sorting Robot 


In programs which use techniques like interrupts and multithreading to do parallel processing, you can split a series of processes between multiple tasks to complete a single job in a much more efficient way. When you do this, the results of a process completed by one task are passed on to the next task.

That’s why in this lesson we’ll be learning about two new data structures called stacks and queues. Using these lets tasks share information with one another! In the first half of the lesson, we’ll look at example code to learn the difference between stacks and queues.

In the second half of the lesson, we’ll take what we’ve learned to make a robot which does its job in multiple stages to sort the blocks it receives by color!

The stages of our sorting robot’s job include detecting the blocks it receives, transporting them, and then recognizing their color before sorting them to the left or right. Doing this single job will require the robot to pass the block data it receives in one stage on to the next.

You won’t find a challenge in this lesson. If you finish early, feel free to take another look at what you’ve learned in Topic 9 so far!

Now let's start the lesson!

:::

###### In computer jargon, a **task** is the smallest unit of work in a job the computer will process!



## Transmitting Data Between Tasks

Let’s start by taking a look at the process behind a real-life example of transmitting data between tasks in order to complete a single job.

Completing orders in a fast food restaurant is a great example of multitasking! The customer places their order, and then the store employees work together to get the order to them, with each employee in charge of a different task.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_1_E.png"/>

In the flowchart below, you can see our three tasks: **order**, **prepare**, and **package** as well as the data and objects passed between each task. But there’s something we have to pay extra attention to as we move through the flow to complete tasks. That something is making sure that a piece of data or object is **sent and received at the right time**!

To give an example: if the cook is in the middle of making an order, they won’t be able to receive and prepare any new orders that come in. The clerk taking orders will find themselves unable to take payments and orders from new customers until the cook takes the current one, making for a lot of lost time.

The answer to this problem is simple: we just have to make a place where we can temporarily store orders! Storing orders on the order board lets the clerk move on to the next task, and once the cook finishes making the current order, the only thing they have to do to get the next order is take it from the board. This system is used not just in fast food, but in restaurants everywhere!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_2_E.png"/>

A computer does essentially the same thing when it sends and receives data to coordinate between tasks and work through a process. Next, we’ll be explaining **stacks** and **queues**. These are two representative data structures you can use to send and receive data!

### Data Structures: Stacks and Queues

Stacks and queues are very simple data structures, and both of them can be thought of as lists with limited entry and exit points for data.

#### ■ The Structure of Stacks

Stacks are a data structure with a single point which serves as an entrance and exit for data. This means that when you take data out of a stack, you’re taking out the last piece of data that was put inside of it!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_3_E.png"/>

This characteristic of a stack means that you’ll sometimes hear it called LIFO, or **L**ast **I**n **F**irst **O**ut.

In real life, a stack would be like retrieving products stored in cardboard boxes inside of a warehouse!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_4_E.png"/>


#### ■ The Structure of Queues

Unlike stacks, queues have separate entry and exit points for data. This means that the first piece of data you take out of a queue is the first piece of data you put into it!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_5_E.png"/>

This means that you’ll sometimes hear a queue called a FIFO, or **F**irst **I**n **F**irst **O**ut.

Queues would come in very handy in the fast food restaurant from earlier. If you tried using a stack to pass an order down the line, the customer who ordered first would never get their food!

### Programming with Stacks and Queues

While you’ll find stacks and queues inside of the `collections` module in Python, they aren’t supported by MicroPython’s version of `collections`. This means that we’ll have to use lists to create structures which act like a stack and a queue. Now let’s take a look at some example code as we learn how to use them!

#### ■ Using Stacks

We can mimic a stack by using `list`. If we want to put data into the stack, we’ll use the `append()` method, and if we want to take it out we’ll use the `pop()` method!

* The `append()` method adds a piece of data to the end of a list.
* The `pop()` method deletes and retrieves an item at the specified position of a list. Running this method without an argument will make it target the last item on the list!

Now let’s make the following two methods which will let us send and receive data!

|Function|What does it do?|
|:---|:---|
|`send_data()`|Sends data to the stack once per second. Stops after every number from 1 to 9 has been sent in order. |
|`receive_data()`|Receives and shows data from the stack once every two seconds. It checks the data inside of the stack and stops after the stack has been empty three times in a row. |

We’ll write the code for each function as shown below:

```python=1
import time  # Imports time module in order to measure time

stack = []  # List for the stack

def send_data():  # Function which sends data
    global stack

    num = 0  # Adds one number from 1-9 to the stack every second
    while num < 9:
        time.sleep_ms(1000)
        num += 1
        stack.append(num)  # Appends data to the end of the list


def receive_data():  # Function which receives data
    global stack
    
    count = 0  # Variable which counts number of times stack has been empty
    while count < 3:  # Repeats until the stack is empty 3 times in a row
        time.sleep_ms(2000)
        if len(stack):  # Checks whether the stack is empty
            data = stack.pop()  # Takes data from the end of the list
            print(data)  # Shows retrieved data
            count = 0  # Resets count if data is successfully retrieved from stack
        else:
            count += 1  # Adds to count if stack was empty
    print("Finished.")  # Ends process
```

`Stack.append()` on line 12 adds data to the end of the stack, and `stack.pop` on line 22 removes data from the end. Since we’ve created a fixed, shared entry and exit point for data, we’ve made a structure which acts exactly like a stack would!

Next, we’ll try using multithreading to run our two functions. Let’s add the code below:

##### Example Code 2-3-1
###### New Code (Line 2, Lines 31-32)
```python=1
import time
import _thread  # Imports module to use multithreading

stack = []

def send_data():
    global stack

    num = 0
    while num < 9:
        time.sleep_ms(1000)
        num += 1
        stack.append(num)


def receive_data():
    global stack
    
    count = 0
    while count < 3:
        time.sleep_ms(2000)
        if len(stack):
            data = stack.pop()
            print(data)
            count = 0
        else:
            count += 1
    print("Finished.")
    
# Starts threads for each function before running them
_thread.start_new_thread(send_data, ())
_thread.start_new_thread(receive_data, ())
```

Now let’s try running our finished program! Your results should look like this:

##### Program Results

<pre class="prettyprint">
&gt;&gt;&gt; 1
3
5
7
9
8
6
4
2
Finished.
</pre>

Since the last data added to a stack is removed first, the numbers received from `send_data()` won’t necessarily be in order!

#### ■ Using Queues

Just like with our stack, we can use `list`’s `append()` and `pop()` methods to make a structure which acts like a queue! However, since we want to take our data from the top of queue, we’ll set a value of **0** in the `pop()` method’s argument.

Now let’s rewrite a portion of the example code we used for our stack to make it into a queue, then run it and see how it affects our results!

##### Example Code 2-3-2
###### Changed Code (Line 4, Line 7, Line 13, Line 17, Lines 22-23)
```python=1
import time
import _thread

queue = []  # Creates a list for the queue

def send_data():
    global queue  # Change to queue

    num = 0
    while num < 9:
        time.sleep_ms(1000)
        num += 1
        queue.append(num)  # Adds data to the end of the queue


def receive_data():
    global queue  # Change to queue
    
    count = 0
    while count < 3:
        time.sleep_ms(2000)
        if len(que):  # Retrieves data only when queue isn\’t empty
            data = queue.pop(0)  # Takes data from the top for queues
            print(data)
            count = 0
        else:
            count += 1
    print("Finished.")
    

_thread.start_new_thread(send_data, ())
_thread.start_new_thread(receive_data, ())
```

##### Program Results

<pre class="prettyprint">
&gt;&gt;&gt; 1
2
3
4
5
6
7
8
9
Finished.
</pre>

Since the first data stored in a queue is the first data to be taken out, the numbers sent by the `send_data()` function appear in order!

## Building a Sorting Robot

Now we’re going to take what we’ve learned and use it to make a robot. This robot will be able to take the blocks it transports and sort them by color. Follow along with the instruction manual below to build it!

### Parts You'll Need

##### Parts
* Core Unit x 1
* Robot Expansion Unit x 1
* Battery Box x 1
* DC Motor x 1
* Servomotor x 2
* IR Photoreflector x 1
* Color Sensor x 1
* Sensor Connecting Cable (3-wire, 30 cm) x 1
* Sensor Connecting Cable (4-wire, 30 cm) x 1
* Basic Cube (White) x 5
* Basic Cube (Gray) x 5
* Basic Cube (Black) x 6
* Basic Cube (Red) x 4
* Triangle (Gray) x 4
* Triangle (Red) x 2
* Half A (Gray) x 6
* Half B (Gray) x 2
* Half B (Black) x 2
* Half C (White) x 20
* Half D (White) x 8
* Beam x 7
* Disk x 2
* Gear (S) x 1
* Gear (L) x 1
* Rack ×1
* Tire x 1

##### The Artec Block Shapes

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_6_E.png"/>

### Building the Sorting Robot

Follow the link below to find instructions for building your sorting robot:

[Building a Sorting Robot](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_build_E.pdf)

## Programming the Sorting Robot

The job our robot has to do is split into three stages, and these stages jobs are what allows it to sort blocks one at a time!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_7_E.png"/>

Watch the video below to see how the sorting robot works!

##### Completed Example Video
<iframe src="https://player.vimeo.com/video/482979185" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

### Making the Program

We’ll make the functions listed in the table below for each of our three stages, then make a thread and run each one.

|Function|What does it do?|
|:---|:---|
|`stage1_detect`|Detects arriving blocks|
|`stage2_transport`|Transports blocks|
|`stage3_separate`|Sorts blocks by color|


In order to coordinate multiple stages to complete the job, we’ll use queues to send and receive information about the blocks. We’ll make two queues called `queue_1to2` and `queue_2to3` to place between our stages.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_8_E.png"/>

Now let’s write our code in order!

#### ■ Detecting a Block

In the first stage, our sorting robot will use its IR Photoreflector, monitoring the sensor’s value to detect when a block has come down the line.

The picture below shows how the IR Photoreflector’s values will change when a block passes in front of it. This means that it can tell a block has passed by when the **sensor’s value rises above and then falls below the threshold**!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_9_E.png"/>

We’ll need to make a variable which acts as a flag. We’ll raise the flag when the value rises above the threshold and lower it once it falls below the threshold. Once the flag is lowered, the block will be assigned a number starting from one. This block data will be saved to the queue and passed on to the next stage!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_10_E.png"/>

The code we would write to coordinate these processes would look like this.

```python=1
import time
from pyatcrobo2.parts import IRPhotoReflector  # Imports IR Photoreflector class

queue_1to2 = []  # Queue which passes block data from work1 to work2

# Function which detects block passage
def stage1_detect():
    irq = IRPhotoReflector("P0")  # Creates an IR Photoreflector object
    threshold = irq.get_value() + 100  # Uses the value to set a threshold automatically immediately after starting
    is_passing = False  # Flag which shows whether or not the block is passing
    num = 0  # Assigns a number to blocks starting with 1
    
    while True:  # Gets IR Photoreflector\’s value every 10 milliseconds
        time.sleep_ms(10)
        val = irq.get_value()

        # If block isn\’t passing, judges that a new block has appeared and raises flag
        if val > threshold and not is_passing:
            is_passing = True
        elif val < threshold and is_passing:  # If the value is lower than the threshold and a block is passing
            is_passing = False  # Judges that the block has passed and raises flag
            num += 1  # Assigns a number
            queue_1to2.append(num)  # Saves number to queue as data for that block
```

The `threshold = irq.get_value() + 100` on line 9 uses the IR Photoreflector’s value when nothing is front of it to set a threshold automatically, just like we did with our penalty kick game in the last lesson. Of course, you can feel free to set a threshold by comparing your IR Photoreflector’s values when there’s a block in front of it and when there’s no block in front of it!

#### ■ Transporting a Block

In the second stage, the two Servomotors will turn in order to transport the blocks to the third stage and save the data in the queue. 

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_11_E.png"/>

Since this stage works closely in tandem with the third stage, it will have to wait until the third stage finishes its job to transport the block!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_12_E.png"/>

Due to this, we’ll make the sorting robot wait until the queue is empty before lowering the lift to its original position to transport the next block.

Now let’s write the code to perform these actions!

At the beginning of the program, we’ll import the class we need to control the Servomotors as well as make the queue which shares data with the third stage.

###### New Code (Line 4, Line 7)

```python=1
import time
import _thread
from pyatcrobo2.parts import IRPhotoReflector
from pyatcrobo2.parts import Servomotor  # Imports a Servomotor object

queue_1to2 = []
queue_2to3 = []  # Queue which passes task from work2 to work3
```

We’ll put the code which controls the two Servomotors in order into the `stage2_transport()` function. Immediately after the program starts, the Servomotors will turn to their initial position and the `while` statement loop will check the queue shared between stages one and two (`queue_1to2`) every 100 milliseconds. It will take any block data present in this queue and pass the block to the third stage. Once the block is in the third stage, the data will be passed to queue `queue_2to3` and the program will wait until the third stage sorts the block. When the block is sorted, the lift will lower itself to its original position.

###### New Code (Lines 28-53)
```python=28
def stage2_transport():
    servo13 = Servomotor("P13")  # Creates a Servomotor object
    servo14 = Servomotor("P14")

    # Moves to starting position（1）
    servo13.set_angle(180)
    servo14.set_angle(75)

    # Checks the queue every 100 milliseconds
    while True:
        time.sleep_ms(100)
        if len(queue_1to2):  # Takes task present in the queue and delivers the block
            num = queue_1to2.pop(0)  # Receives block data from the queue
            time.sleep_ms(500)  # Time between block detection and arrival           
            servo14.set_angle(145)　# Receives block（2）
            time.sleep_ms(1000)  # Waits 1 second for the Servomotor to finish rotating
            servo14.set_angle(75)  # Moves the block to the lift（3）
            time.sleep_ms(1000)
            servo13.set_angle(5)  # Raises the lift to deliver block to workspace 3（4）
            time.sleep_ms(1000)
            queue_2to3.append(num)  # Adds block data to the queue
            while len(queue_2to3):  # Next job waits until the task is complete (the queue is empty)
                time.sleep_ms(100)  # Checks every 100 milliseconds to keep from taking up too many resources
                pass
            servo13.set_angle(180)  # Lowers the lift
            time.sleep_ms(1000)
```

#### ■ Sorting a Block
If block data is present in `queue_2to3` in this stage, the Color Sensor will detect the color of the block and the DC Motor will rotate to sort the block to the left or right. Red or green blocks will be sorted to the right, and yellow or blue blocks will be sorted to the left.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_13_E.png"/>

Once the block is sorted, the number and color of that block will appear in the terminal!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-3/9-3_14_E.png"/>

The code we would write for this process would look like the example code below.

We’ll start the program by adding the Color Sensor and DC Motor classes to the classes we need to import!

###### New Code (Lines 3-4)
```python=1
import time
import _thread
from pyatcrobo2.parts import IRPhotoReflector, ColorSensor  # Imports the Color Sensor class
from pyatcrobo2.parts import Servomotor, DCMotor  # Imports the DC Motor class
```

We’ll put the processes which detect the color of and sort our blocks into the `stage3_sort()` function. The program will check `queue_2to3` every 100 milliseconds. If there’s data present, the Color Sensor will detect the color of the block and the DC Motor will rotate in different directions depending on the color!

###### New Code (Lines 54-80)
```python=54
def stage3_sort():  
    cs = ColorSensor("I2C")
    dcm = DCMotor("M1")
    dcm.power(255)
    
    # Checks the queue every 100 milliseconds
    while True:
        time.sleep_ms(100)
        if len(queue_2to3):
            color = cs.get_colorcode()  # Looks up color
            if color == ColorSensor.COLOR_RED:  # If block is red
                color_name = "red"  # Saves color name to variable in order to show it in the terminal
                dcm.cw()
            elif color == ColorSensor.COLOR_GREEN:  # If block is green
                color_name = "green"
                dcm.cw()
            elif color == ColorSensor.COLOR_YELLOW:  # If block is yellow
                color_name = "yellow"
                dcm.ccw()
            else:
                color_name = "blue"  # If block is blue
                dcm.ccw()
            time.sleep_ms(2000)  # Rotates the DC Motor for two
            dcm.stop()
            #  Shows result once the job is done
            num = queue_2to3.pop(0)  # Deletes data from the queue once the job is done
            print("{}:{}".format(num, color_name))  # Shows color number and name
```

Once the block is done being sorted in this stage, the program will pull data from `queue_2to3` on line 79. This allows the `stage2_transport()` function in stage two to lower the lift once the block is sorted!

#### ■ Making Threads and Running Each Function

Since we’ve made functions for each of our three stages, let’s create threads for each one and run them. Add the code you see below and try running it for yourself!

##### Example Code 4-1-1
New Code (Line 2, Lines 81-83)

```python=1
import time
import _thread  # Imports multithreading module
from pyatcrobo2.parts import IRPhotoReflector, ColorSensor 
from pyatcrobo2.parts import Servomotor, DCMotor

queue_1to2 = [] 
queue_2to3 = []


def stage1_detect():
    irq = IRPhotoReflector("P0")
    threshold = irq.get_value() + 100
    is_passing = False
    num = 0
    
    while True:
        time.sleep_ms(10)
        val = irq.get_value()

        if val > threshold and not is_passing:
            is_passing = True
            num += 1
            queue_1to2.append(num)
        elif val < threshold and is_passing:
            is_passing = False


def stage2_transport():
    servo13 = Servomotor("P13")
    servo14 = Servomotor("P14")

    servo13.set_angle(180)
    servo14.set_angle(75)

    while True:
        time.sleep_ms(100)
        if len(queue_1to2):
            num = queue_1to2.pop(0)
            time.sleep_ms(500)           
            servo14.set_angle(145)
            time.sleep_ms(1000)
            servo14.set_angle(75)
            time.sleep_ms(1000)
            servo13.set_angle(5)
            time.sleep_ms(1000)
            queue_2to3.append(num)
            while len(queue_2to3):
                time.sleep_ms(100)
                pass
            servo13.set_angle(180)
            time.sleep_ms(1000)


def stage3_sort():  
    cs = ColorSensor("I2C")
    dcm = DCMotor("M1")
    dcm.power(255)
    
    while True:
        time.sleep_ms(100)
        if len(queue_2to3):
            color = cs.get_colorcode()
            if color == ColorSensor.COLOR_RED:
                color_name = "red"
                dcm.cw()
            elif color == ColorSensor.COLOR_GREEN:
                color_name = "green"
                dcm.cw()
            elif color == ColorSensor.COLOR_YELLOW:
                color_name = "yellow"
                dcm.ccw()
            else:
                color_name = "blue"
                dcm.ccw()
            time.sleep_ms(2000)
            dcm.stop()
            num = queue_2to3.pop(0)
            print("{}:{}".format(num, color_name))
            
# Makes threads for each function and runs them
_thread.start_new_thread(stage1_detect, ())
_thread.start_new_thread(stage2_transport, ())
_thread.start_new_thread(work3_separate, ())
```


## Conclusion
### A Quick Review
In this lesson we learned about two new types of data structures: stacks and queues. These are both relatively simple structures, which makes them pretty easy to wrap your head around! The most important thing to remember is that stacks are **last in, first out** structures, while queues are **first in, first out** structures.

In this lesson we used multitasking with stacks and queues to run multiple processes at the same time on a single computer. The internet and other networks, however, link multiple computers together. Tasks are distributed across these computers and they communicate with each other to process enormous amounts of data!

When we reach Topic 11, you’ll be using queues again and we’ll introduce how to set up a wireless connection on your Core Unit and pull data from the internet in order to control your robot. Topic 11 is quite a long way off, so come back to this lesson to review if you find yourself forgetting!

### Coming Up Next Time!

In the next lesson, we’ll introduce ways to precisely track the passage of time on your microcontroller, and secure the data shared between threads in a multithreaded program!