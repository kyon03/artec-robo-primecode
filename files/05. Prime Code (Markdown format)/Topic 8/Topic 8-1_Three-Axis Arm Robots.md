---
tags: Python_English, Topic 8
---

Python Robotics Course Lesson 29

Topic 8-1
Three-Axis Arm Robots
===

Use motors to make an Arm Robot which moves on multiple axes!

## In This Lesson...
In Topic 8, we’re going to take a look at the mechanisms behind arm robots used in factories before making and programming simple models of our own!

In this lesson, we’re going to combine multiple motors to make a **three-axis Arm Robot**. In order to control it, we’ll need to define unique modules and classes and program a simple conveyance system.

<iframe src="https://player.vimeo.com/video/484349648" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

:::info

Lesson Introduction

■ Lesson 30 Contents
1. What Are Arm Robots?
2. Asynchronous vs. Synchronous Servomotor Control
3. Controlling a Three-Axis Arm Robot
4. Challenge: Conveying Multiple Blocks


In Topic 8, we’re going to take on the challenge of making arm robots. Arm robots are used in factories for everything from assembling and transporting goods to painting and welding! We’ll begin in this lesson by using three motors to build an arm robot of our own, then defining the classes containing the features we’ll need to control it. In the next lessons we’ll expand on its functionality by adding sensors!

To get things started, we’ll learn what arm robots are used for as well as the different types of arm robots. Arm robots play a big role in factory automation and come in many shapes and sizes depending on their job! The first section introduces some of the arm robots factories typically use.

Next, we’ll take a look at the Servomotors we’ll use for the joints of our Arm Robot and learn the difference between controlling them separately or making them move in sync! We’ll also have to define a unique Servomotor class with expanded functionality for the Arm Robot we’re going to make.

In the second half of the lesson we’ll tackle building an Arm Robot of our own. We’ll use three motors for its joints and program it to grab and carry what we need it to!

We can get our arm robot to pick up and carry the blocks on the field by programming the motors to move in order.



In the challenge at the end of the lesson, we’ll program our robot to pick up and set down multiple blocks exactly where we tell it to!

Now let's start the lesson!

:::



## What Are Arm Robots?
Arm robots are made for industrial work, and they have multiple joints they use to do this, just like our hands and arms. The joints are made of moving parts like motors, called **actuators**, and they can move them in sync to do all sorts of jobs!
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_1_E.png"/>
### What Do Arm Robots Do?
Arm robots can be put to a variety of uses, from transporting products to assembling components on a production line. They’re even used to automate processes like painting and welding!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_2_E.png"/>
### The Types of Arm Robots
An arm robot’s type depends on how many joints it has as well as where those joints are placed! The arm robots in the table below are some of the most common ones you’ll see:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_3_E.png"/>

## Asynchronous vs. Synchronous Control
The arm robot we’re going to make in this lesson moves multiple joints at once to lift and carry objects. When you’re working with multiple Servomotors, you have the choice of moving them **asynchronously** (separately), or **synchronously** (at the same time)! 
### What Makes Them Different?
Take a look at the video below and pay close attention to how the motors move with each different control method.


<iframe src="https://player.vimeo.com/video/485732748" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

With asynchronous control, motors with smaller differences between their starting and ending angles will complete their rotations more quickly. Synchronous control, however, ignores the difference between each motor’s starting and ending angles, making sure every motor finishes rotating at the same time!

This happens because the motors are controlled in the following ways with each method:

* **Asynchronous Control**
Each motor is controlled separately.
* **Synchronous Control**
The speed of each motor is adjusted to ensure they all finish rotating at the same time.

In this lesson we’re going to control the joints of our Arm Robot **synchronously**.
### Programming Synchronous Control
We’ll have to set the speed of each motor individually to ensure that each one starts and stops rotating at the same time.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_4_E.png"/>

In order to decide these speeds, we need to look at each motor’s **starting**, or **current angle**, rather than the angle at which it finishes.

There’s only one problem: the `Servomotor` class doesn’t have a property or method that tells us a motor’s current angle! This means that we’ll have to make a new, unique class which inherits the `Servomotor` class. We’ll give this class a new property which can record angles as well as a method which retrieves the current angle.

#### ■ Defining a Unique Servomotor Class

Let’s define this class with the name `MyServomotor`. We’ll add a new property and method as well as override the methods from the superclass shown below!

##### New Properties and Methods

|Property or Method|What is it?|
|:---|:---|
|`angle` Property|Variable which records the angle of the motor|
|`get_angle()` Method|Gets the current angle of the motor|

##### Methods to Override

|Method|What does it do?|
|:---|:---|
|`__init__()` Method|Sets the initial value of the `angle` property to 90 degrees and rotates to that angle|
|`set_angle()` Method|Records the end angle to the `angle` property|

Let’s open up a new file for our `MyServomotor` class and follow along with the code below as we write it!

<pre class="prettyprint linenums:1 lang-py">
from pyatcrobo2.parts import Servomotor

class MyServomotor(Servomotor):  # Inherits the Servomotor
    def __init__(self, pin):
        super().__init__(pin)
        self.angle = 90  # Sets the initial angle to 90 degrees
        super().set_angle(self.angle)  # Uses the superclass method to rotate to 90 degrees
    
    def set_angle(self, degree):
        super().set_angle(degree) # Uses the superclass method to rotate to the specified angle
        self.angle = degree  # Stores angle in the angle property
    
    def get_angle(self):
        return self.angle  # Returns the angle currently stored in the property
</pre>

#### ■ Defining Synchronous Control Functions

We’ve already made a class which can get a Servomotor’s starting angle, and we’ll use that class to make a function called `syncRotateServos()` which can move multiple Servomotors in sync!

###### ★ **Sync** is short for **synchronous**!

In our `syncRotateServos()` function we’ll set the **rotation time** as a positional argument and a **tuple containing an instance of the `MyServomotor` class and the motor’s ending angle** as a variable-length positional argument!


###### New Code (Lines 16-17)
<pre class="prettyprint linenums:1 lang-py">
from pyatcrobo2.parts import Servomotor

class MyServomotor(Servomotor):
    def __init__(self, pin):
        super().__init__(pin)
        self.angle = 90
        super().set_angle(self.angle)
    
    def set_angle(self, degree):
        super().set_angle(degree)
        self.angle = degree
    
    def get_angle(self):
        return self.angle

def syncRotateServos(duration, *args):
    pass
</pre>

We’ll use the function like this:

<pre class="prettyprint">
# Defined as an instance of the MyServomotor class
myservo_1 = MyServomotor("P13")
myservo_2 = MyServomotor("P14")

# Makes two Servomotors take 1000 milliseconds to rotate to the specified angles
syncRotateServos(1000, (myservo_1, 180), (myservo_2, 135))
</pre>

Now let’s get back to defining our function. We’ll start the function by setting a rotation speed for each Servomotor. We can use the following formula to find the speed.

<pre>
Rotation speed = (Ending angle / Starting angle) / Rotation time
</pre>

We’ll put the speeds we find into a dictionary which uses the ID value of the `MyServomotor` instance as a key!

###### New Code (Lines 17-22)

<pre class="prettyprint linenums:1 lang-py">
from pyatcrobo2.parts import Servomotor

class MyServomotor(Servomotor):
    def __init__(self, pin):
        super().__init__(pin)
        self.angle = 90
        super().set_angle(self.angle)
    
    def set_angle(self, degree):
        super().set_angle(degree)
        self.angle = degree
    
    def get_angle(self):
        return self.angle
        
def syncRotateServos(duration, *args):   
    speeds = {}  # Makes a dictionary which stores the speed of each Servomotor
    for arg in args:  # Retrieves data from the variable-length argument in order
        myservo = arg[0]  # MyServomotor class instance
        degree = arg[1]  # Ending angle
        diff = degree - myservo.get_angle()  # Finds difference between the ending and starting angles
        speeds[id(myservo)] = diff/duration  # Finds speeds and stores them in the dictionary.
</pre>

Since we found the speed of each servomotor in degrees rotated per millisecond, we’ll make each motor rotate a small amount every millisecond. You'll need to use the `time` module to set this interval, so let’s import it at the beginning of the program. And now we’ve finished our function!

##### Example Code 3-2-1
###### New Code (Line 1, Lines 25-30)

<pre class="prettyprint linenums:1 lang-py">
import time  # Don’t forget this!
from pyatcrobo2.parts import Servomotor

class MyServomotor(Servomotor):
    def __init__(self, pin):
        super().__init__(pin)
        self.angle = 90
        super().set_angle(self.angle)
    
    def set_angle(self, degree):
        super().set_angle(degree)
        self.angle = degree
    
    def get_angle(self):
        return self.angle
        
def syncRotateServos(duration, *args):   
    speeds = {}
    for arg in args:
        myservo = arg[0]
        degree = arg[1]
        diff = degree - myservo.get_angle()
        speeds[id(myservo)] = diff/duration
    
    for _ in range(duration):  # Runs every millisecond
        for arg in args:
            myservo = arg[0]
            degree = myservo.get_angle() + speeds[id(myservo)]  # Gets degrees rotated per millisecond
            myservo.set_angle(degree)
        time.sleep_ms(1)  # Waits 1 millisecond before rotating again
</pre>

#### ■ Making the Program into a Module

Let’s go ahead and make Example Code 3-2-1 into a module. Save it to your computer with the name `myservo(.py)` before copying it to your Core Unit.

#### ■ Synchronous Control in Action

It’s time to test how our new module works! Plug your two Servomotors into `P13` and `P14` on your Robot Expansion Unit and use a beam to keep them in place as shown below.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_5_E.png"/>

Now let’s make a new program and see how we can use it to make two Servomotors rotate in sync!

##### Example Code 3-2-2
<pre class="prettyprint linenums:1 lang-py">
import myservo

myservo_p13 = myservo.MyServomotor("P13")
myservo_p14 = myservo.MyServomotor("P14")

for _ in range(5):
    myservo.syncRotateServos(1000, (myservo_p13, 0), (myservo_p14, 0))
    myservo.syncRotateServos(1000, (myservo_p13, 45), (myservo_p14, 180))
</pre>
## Building a Three-Axis Arm Robot
Now we’re going to continue our learning by using an arm robot with three joints. Let’s follow along with the instruction manual below as we build it!
### Parts You'll Need
##### Parts
* Core Unit x 1
* Robot Expansion Unit x 1
* Battery Box x 1
* DC Motor x 1
* Servomotor x 2
* Basic Cube (White) x 2
* Basic Cube (Gray) x 2
* Triangle (Gray) x 1
* Half A (Gray) x 4
* Half B (Gray) x 2
* Half B (Black) x 2
* Half B (Red) x 2
* Half C (White) x 11
* Half D (White) x 8
* Beam x 7
* Gear (L) x 2
* Tire x 2
* Axle x 4

##### The Artec Block Shapes

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_6_E.png"/>
### Building the Arm Robot
Follow the link below to find instructions for building your Arm Robot:

[Building an Arm Robot](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_build_E.pdf)
### Programming the Arm Robot
The Arm Robot we’re going to build has three joints, one each for its **arm**, **claw**, and **body**.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_7_E.png"/>

The pictures below show the axis of each of these joints:

* Arm (Servomotor P13)
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_8_E.png"/>
* Claw (Servomotor P14)
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_9_E.png"/>
* Body (DC Motor M1)
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_10_E.png"/>

In real life, you’ll usually find that arm robots can move on four or more axes. However, even though our Arm Robot only has three axes, it can still do basic things like **grab**, **release**, **raise**, **lower**, and **carry** objects!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_11_E.png"/>
### Defining the Arm Robot Classes
Let’s take the `myservo(.py)` module we made in the last section and use it to define a `ArmRobot` class which controls our Arm Robot! This class will need to have the following properties and methods:

##### `ArmRobot` Class Properties

|Property|What does it do?|
|:---|:---|:---|
|`arm`|Stores object used to control the Servomotor for the arm joint|
|`claw`|Stores object used to control the Servomotor for the claw joint|
|`body`|Stores object used to control the DC Motor for the body joint|

##### `ArmRobot` Class Methods

|Method|What is it?|
|:---|:---|:---|
|`__init__()`|Constructor which sets the initial values of each property|
|`pick()`|Controls the arm and claw to grab and raise an object|
|`place()`|Controls the arm and claw to set down an object|
|`rotate()`|Turns the body|
|`stop()`|Stops the body as it’s turning|

Now let’s write our code in order!

#### ■ Defining Properties and the __init__() Method

We’ll start by declaring our `ArmRobot` class and the `__init()__` method which will get our properties ready. This class takes the ports which our Servomotors and DC Motor are connected to as keyword arguments before making class instances for them and storing them to variables.

<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor
    import myservo  # Imports the module we made in the last section

    def __init__(self, *, pin_arm, pin_claw, pin_body):  # Takes port as a keyword argument
        self.arm = self.myservo.MyServomotor(pin_arm)  # Arm Servomotor
        self.claw = self.myservo.MyServomotor(pin_claw)  # Claw Servomotor
        self.body = self.DCMotor(pin_body)  # Body DC Motor
</pre>

#### ■ Defining a `pick()` Method to Lift Objects

In this lesson we’re going to have our Arm Robot pick up blocks. The Arm Robot has to do the following in order to pick up a block:

1. Lower its arm as it opens its claw
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_12_E.png"/>
2. Close its claw to grab the block
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_13_E.png"/>
3. Raise its arm to lift the block
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_14_E.png"/>

The code we would write to perform this series of movements would look like the code below. We can use our `syncRotateServos()` function to adjust the speed of any number of Servomotors, even if we’re just using one!
###### ★ We’ll adjust our Servomotors’ angles and rotation times later as we test out how the Robot arm works.

###### New Code (Lines 10-13)

<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor
    import myservo

    def __init__(self, *, pin_arm, pin_claw, pin_body):
        self.arm = self.myservo.MyServomotor(pin_arm)
        self.claw = self.myservo.MyServomotor(pin_claw)
        self.body = self.DCMotor(pin_body)
		
    def pick(self):
        self.myservo.syncRotateServos(600, (self.arm, 165), (self.claw, 135))  # 1. Lowers arm as it opens its claw
        self.myservo.syncRotateServos(600, (self.claw, 60))  # 2. Closes claw to grab the block
        self.myservo.syncRotateServos(600, (self.arm, 90))  # 3. Raises arm to lift block
</pre>

Now let’s add the following code, which will allow us to test our Arm Robot. Try running the program and see if it makes your robot grab and pick up a block! If your Arm Robot is having trouble, try adjusting the angle of the claw’s Servomotor on line 12.

###### New Code (Lines 16-17)

<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor
    import myservo

    def __init__(self, *, pin_arm, pin_claw, pin_body):
        self.arm = self.myservo.MyServomotor(pin_arm)
        self.claw = self.myservo.MyServomotor(pin_claw)
        self.body = self.DCMotor(pin_body)
		
    def pick(self):
        self.myservo.syncRotateServos(600, (self.arm, 165), (self.claw, 135))
        self.myservo.syncRotateServos(600, (self.claw, 60))  # Try a smaller angle if your robot can’t pick up the block
        self.myservo.syncRotateServos(600, (self.arm, 90))	

# Test code
armrobot = ArmRobot(pin_arm="P13", pin_claw="P14", pin_body="M1")
armrobot.pick()
</pre>

#### ■ Defining a `place()` Method to Set Down Objects

The Arm Robot has to do the following in order to set a block down:

1. Lower its arm to set down the block
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_15_E.png"/>
2. Open its claw to release the block
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_16_E.png"/>
3. Raise its arm as it closes its claw
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_17_E.png"/>

The code we would write to perform this series of movements would look like the code below. Just like with our `pick()` method, add the code below to test your Arm Robot and see if it sets the block down!

###### New Code (Lines 16-19, Line 24)
<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor
    import myservo
    import time

    def __init__(self, *, pin_arm, pin_claw, pin_body):
        self.arm = self.myservo.MyServomotor(pin_arm)
        self.claw = self.myservo.MyServomotor(pin_claw)
        self.body = self.DCMotor(pin_body)
		
    def pick(self):
        self.myservo.syncRotateServos(600, (self.arm, 165), (self.claw, 135))
        self.myservo.syncRotateServos(600, (self.claw, 60))
        self.myservo.syncRotateServos(600, (self.arm, 90))

    def place(self):
        self.myservo.syncRotateServos(600, (self.arm, 165))  # 1. Lowers arm to set down the block
        self.myservo.syncRotateServos(600, (self.claw, 135))  # 2. Opens claw to release the block
        self.myservo.syncRotateServos(600, (self.arm, 90), (self.claw, 90))  # 3. Raises arm and closes claw

# Test code
armrobot = ArmRobot(pin_arm="P13", pin_claw="P14", pin_body="M1")
armrobot.pick()
armrobot.place()  # Sets the block down
</pre>

#### ■ Defining `rotate()` and `stop()` Methods to Rotate the Body

The Arm Robot’s body uses a DC Motor for its joint. Unlike a Servomotor, we’ll have to control the motor’s **time**, **direction**, and **speed**.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_18_E.png"/>

This means that we’re going to make our function take three arguments: `duration`, `direction`, and `speed`.

We'll also give the following default values for these arguments so we can leave them out later in the program!

<pre>
def rotate(self, duration=-1, speed=50, direction="cw"):
</pre>

We’re giving the `duration` argument a default value of **-1** so we can control the motor without having to give it a specific time. This means that we can use it as a method which will make the motor rotate until we tell it to stop!

Let’s name the method which sends the stop command `stop()`.

New Code (Line 4, Lines 21-29, Lines 31-32)
<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor
    import myservo
    import time  # Use to control the rotation time

    def __init__(self, *, pin_arm, pin_claw, pin_body):
        self.arm = self.myservo.MyServomotor(pin_arm)
        self.claw = self.myservo.MyServomotor(pin_claw)
        self.body = self.DCMotor(pin_body)
		
    def pick(self):
        self.myservo.syncRotateServos(600, (self.arm, 165), (self.claw, 135))
        self.myservo.syncRotateServos(600, (self.claw, 60))
        self.myservo.syncRotateServos(600, (self.arm, 90))

    def place(self):
        self.myservo.syncRotateServos(600, (self.arm, 165))
        self.myservo.syncRotateServos(600, (self.claw, 135))
        self.myservo.syncRotateServos(600, (self.arm, 90), (self.claw, 90))
		
    def rotate(self, duration=-1, speed=50, direction="cw"):
        self.body.power(speed)  # DCMotor class method
        if direction == "cw":
            self.body.cw()  # DCMotor class method
        else:
            self.body.ccw()  # DCMotor class method
        if duration != -1:  # Runs for this time before stopping ONLY if a specific value is set
            self.time.sleep_ms(duration)  # Be sure to import the time module on line 4 of this program
            self.body.brake()  # DCMotor class method
			
    def stop(self):
        self.body.brake()  # DCMotor class method


# Test code
armrobot = ArmRobot(pin_arm="P13", pin_claw="P14", pin_body="M1")
armrobot.pick()
armrobot.place()
</pre>

Last, we’ll add a single line of code which will let us test our program. Your program is working correctly if your Arm Robot turns to set down the block it’s holding!

###### New Code (Line 38)

<pre class="prettyprint linenums:35 lang-py">
# Test code
armrobot = ArmRobot(pin_arm="P13", pin_claw="P14", pin_body="M1") 
armrobot.pick()
armrobot.rotate(1000, direction="cw")  # Set any time and direction you’d like
armrobot.place()
</pre>

#### ■ Defining the `ArmRobot` Class

A fully defined `ArmRobot` class would look like the code shown below. We’ll be continuing to use this class in the lessons after this one!

##### Example Code 5-1-1

<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor
    import myservo  # Unique Servomotor sychronous control module
    import time

    def __init__(self, *, pin_arm, pin_claw, pin_body):  # Constructor
        self.arm = self.myservo.MyServomotor(pin_arm)  # Arm Servomotor
        self.claw = self.myservo.MyServomotor(pin_claw)  # Claw Servomotor
        self.body = self.DCMotor(pin_body)  # Body DC Motor
		
    def pick(self):  # Method used to grab and lift block
        self.myservo.syncRotateServos(600, (self.arm, 165), (self.claw, 135))
        self.myservo.syncRotateServos(600, (self.claw, 60))  # Be sure to adjust the angle used to grab the block
        self.myservo.syncRotateServos(600, (self.arm, 90))

    def place(self):  # Method used to set down block
        self.myservo.syncRotateServos(600, (self.arm, 165))
        self.myservo.syncRotateServos(600, (self.claw, 135))
        self.myservo.syncRotateServos(600, (self.arm, 90), (self.claw, 90))
		
    def rotate(self, duration=-1, speed=50, direction="cw"):  # Method used to turn body
        self.body.power(speed)
        if direction == "cw":
            self.body.cw()
        else:
            self.body.ccw()
        if duration != -1:  # Waits before stopping the motor ONLY if a time is specified
            self.time.sleep_ms(duration) 
            self.body.brake()
			
    def stop(self):  # Method used to stop the body while turning
        self.body.brake()
</pre>
## Challenge: Conveying Multiple Blocks
In this challenge we’re going to take the `ArmRobot` class we made in the previous section and use it to make our Arm Robot deliver three blocks to set locations!

##### Challenge Outline

* We’ll need to take a black, green, and yellow block and set them down as shown below. Our Arm Robot will then pick up each block and deliver it to the circle of the same color (you can deliver the blocks in any order you like)!
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_19_E.png"/>

* Place your Arm Robot on the two gray circles in the middle as shown here:
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_20_E.png"/>

The front of the Arm Robot should face the small gray circle with the white square:
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_21_E.png"/>
### Getting the Field Ready
We’ll use the field below for this challenge.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_22_E.png"/>


Follow the link below to download your own copy of the course and print it out on A3 paper.

[Arm Robot Field (A3)](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_field_A3.pdf)

If you can’t print A3 size paper, download the field below and print it out on two pieces of A4 paper. Cut off the dotted parts before taping them together!

[Arm Robot Field (A4)](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_field_A4.pdf)
### Example Program
If we wanted our Arm Robot to move the **black**, **green**, and **yellow** block in order, we would have to make it do the following:

### Arm Robot Movements

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-1/8-1_23_E.png"/>

1. Use `pick()` to grab and lift the black block
2. Use `rotate()` to turn the body clockwise to the black circle
3. Use `place()` to set down the black block
4. Use `rotate()` to turn the body counterclockwise to the red circle
5. Use `pick()` to grab and lift the green block
6. Use `rotate()` to turn the body clockwise to the green circle
7. Use `place()` to set down the green block
8. Use `rotate()` to turn the body clockwise to the blue circle
9. Use `pick()` to grab and lift the yellow block
10. Use `rotate()` to turn the body clockwise to the yellow circle
11. Use `place()` to set down the yellow block

With these steps in mind, we’ll have to figure out **how long the body should rotate** for each step, but the code would look something like this:

##### Example Code 6-3-1
###### ★ Lines 36-47 contain the code for our challenge!

<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor
    import myservo
    import time

    def __init__(self, *, pin_arm, pin_claw, pin_body):
        self.arm = self.myservo.MyServomotor(pin_arm)
        self.claw = self.myservo.MyServomotor(pin_claw)
        self.body = self.DCMotor(pin_body)

    def pick(self):
        self.myservo.syncRotateServos(600, (self.arm, 165), (self.claw, 135))
        self.myservo.syncRotateServos(600, (self.claw, 60))
        self.myservo.syncRotateServos(600, (self.arm, 90))

    def place(self):
        self.myservo.syncRotateServos(600, (self.arm, 165))
        self.myservo.syncRotateServos(600, (self.claw, 135))
        self.myservo.syncRotateServos(600, (self.arm, 90), (self.claw, 90))

    def rotate(self, duration=-1, speed=50, direction="cw"):
        self.body.power(speed)
        if direction == "cw":
            self.body.cw()
        else:
            self.body.ccw()
        if duration != -1:
            self.time.sleep_ms(duration) 
            self.body.brake()

    def stop(self):
        self.body.brake()
        

# Add the lines below
armrobot = ArmRobot(pin_arm="P13", pin_claw="P14", pin_body="M1") 
armrobot.pick()  # 1. Grabs and lifts black block
armrobot.rotate(3400, direction="cw") # 2. Turns body to face black circle
armrobot.place()  # 3.Sets down black block
armrobot.rotate(2550, direction="ccw")  # 4. Turns body to face red circle
armrobot.pick()  # 5. Grabs and lifts green block
armrobot.rotate(1700, direction="cw")  # 6. Turns body to face green circle
armrobot.place()  # 7.Sets down green block
armrobot.rotate(1700, direction="cw")  # 8. Turns body to face blue circle
armrobot.pick()  # 9. Grabs and lifts yellow block
armrobot.rotate(1700, direction="cw")  # 10. Turns body to face yellow circle
armrobot.place()  # 11. Sets down yellow block
</pre>
## Conclusion

### A Quick Review
In this lesson, we learned about **asynchronous** and **synchronous** Servomotor control. We then used three motors to make an Arm Robot. In order to control our robot, we defined a unique class and used it to make a simple conveyance system!

Both the Arm Robot you made here and the Robocars we’ve built in previous lessons are standard robots with a wide range of uses. They’re a great tool for learning robotics engineering, which means you can find them in quite a few trade schools and universities!

We weren’t able to touch on everything in this lesson, but we’d highly encourage you to research the latest advances in robotics on the internet or at the library if you might want to pursue it as a future career.
### Coming Up Next Time!
In the next lesson, we’re going to add an IR Photoreflector to our Arm Robot and get it to detect and move to precise positions on the field!
