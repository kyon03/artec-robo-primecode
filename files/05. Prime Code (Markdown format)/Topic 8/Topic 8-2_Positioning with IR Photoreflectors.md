---
tags: Python_English, Topic 8
---

Python Robotics Course Lesson 30

Topic 8-2
Positioning with IR Photoreflectors
===

Use a sensor to detect spaces on the field!

<iframe src="https://player.vimeo.com/video/484350419" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

:::info

Lesson Introduction

■ Lesson 30 Contents
1. Adding a Position Detection Feature 
2. Challenge: Choosing the Most Efficient Route


In this lesson, we’re going to use an IR Photoreflector to add a **positioning system** to our Arm Robot from the last lesson. This will let your Arm Robot detect spaces on the field and rotate to any location you set.

In the first half of the lesson, we’ll program our new IR Photoreflector to detect black spaces on the field and add a method which rotates the body to an exact location! 



While we set specific rotation times for the body in the last lesson, this time we'll set a location first before rotating!



In the challenge in the second half of the lesson, we’ll improve our program using an algorithm which decides which way to rotate the body for the most efficient delivery route!

Now let's start the lesson!

:::

## In This Lesson...
In this lesson, we’re going to add an IR Photoreflector to our Arm Robot and get it to detect and move to exact spaces on the field.



## Adding a Photoreflector
We’ll start by adding an IR Photoreflector to the front of our Arm Robot’s body. This IR Photoreflector will detect the Arm Robot’s position by reading black spaces on the field! 

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_1_E.png"/>

### Parts You'll Need
##### Parts
* Lesson 29’s Arm Robot x 1
* IR Photoreflector x 1
* Sensor Connecting Cable (3-wire, 15cm) x 1
* Half C (White) x 6
* Half D (White) x 2

##### The Artec Block Shapes

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_2_E.png"/>

### Adding an IR Photoreflector
Follow the link below to find instructions for adding an IR Photoreflector:

[Adding an IR Photoreflector](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_build_E.pdf)
## Programming the Body to Rotate to a Set Position
The field we’ll be using has six positions, each with a different color. Starting at the top, we’ll move down clockwise and assign each position a number.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_3_E.png"/>

In the previous lesson, we tuned the **rotation time** of the DC Motor’s body to make our Arm Robot transport blocks to a set location. Even if you set the DC Motor’s speed inside of a program, however, the strength of the batteries will affect how fast it actually moves. This means that when you use the method above your robot’s rotations will be slightly off as more time passes!

Take a moment to remember how we used an IR Photoreflector to detect a black line and white paper. If we can get our Arm Robot to detect the black spaces on the field as it rotates, we can get it to rotate to an exact location without having to take the speed of the motor into account!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_4_E.png"/>

### Setting a Black Space Threshold Value
Let’s start by setting **thresholds** which allow us to detect the black spaces on the paper. In order to get the most accurate measurement, let’s use a `threshold_low` value closer to the black space and a `threshold_high` value closer to the white space!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_5_E.png"/>

#### ■ Checking Your IR Photoreflector Values

We’ll begin by running the code below and checking our IR Photoreflector’s values when it’s over the black versus the white space!

#### ■ Updating the IR Photoreflector Value One Per Second in the Terminal

<pre class="prettyprint linenums:1 lang-py">
from pyatcrobo2.parts import IRPhotoReflector
import time

irp = IRPhotoReflector("P0")

while True:
    val = irp.get_value()
    print(val)
    time.sleep_ms(1000)
</pre>

#### ■ Picking a Threshold

Now let’s use our results to pick the thresholds. To give a rough estimate, we'll set our thresholds 300 away from each value we found. This should let us easily detect the black line!

###### ★ Every IR Photoreflector is different! You may find that you have to make different settings for yours! If you find that the program we make later isn’t working correctly, try adjusting your thresholds!

##### Example Thresholds

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_6_E.png"/>

#### ■ Adding Thresholds as a Property of the ArmRobot Class

Let’s go ahead and add our threshold values as a property of the `ArmRobot` class we made in the previous lesson. If you deleted this program already, go ahead and copy the code below into your program and change the thresholds to your own!

###### New Code (Line 6, Line 7)

<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor
    import myservo
    import time
    
    threshold_high = 900  # White space threshold
    threshold_low = 650  # Black space threshold

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
</pre>
### Adding a Method which Moves to a Numbered Position
Now we’re going to add a method called `move_to()`. This method will make our Arm Robot’s body **rotate to a set position just by specifying its number on the field**!

#### ■ Adding an IR Photoreflector Instance

We’ll start by adding a new argument to our `__init()__` method which will let us set the port of the IR Photoreflector. Next, we’ll define an `irp` property before creating and storing an instance of the `IRPhotoreflector` class inside of it!

###### New Code (Line 2, Line 9, Line 13)

<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor, IRPhotoReflector  # Import IRPhotoReflector class here
    import myservo
    import time

    threshold_high = 900
    threshold_low = 650

    def __init__(self, *, pin_arm, pin_hand, pin_body, pin_irp):  # Adds pin_irp argument
        self.arm = self.myservo.MyServomotor(pin_arm)
        self.claw = self.myservo.MyServomotor(pin_claw)
        self.body = self.DCMotor(pin_body)
        self.irp = self.IRPhotoReflector(pin_irp)  # Creates an instance of the IRPhotoReflector class
</pre>

#### ■ Setting the Initial Position

Our IR Photoreflector will only need to detect the black spaces on the field. Our Arm Robot, however, can’t tell which space is which number when you first set it down! This means we’ll have to give our starting space the number **1** and make sure that our IR Photoreflector is placed directly over it.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_7_E.png"/>

Now let’s store this initial position to our Arm Robot. To do this, we’ll define a `position` property and give it a value of **1**.

###### New Code (Line 14)

<pre class="prettyprint linenums:9 lang-py">
    def __init__(self, *, pin_arm, pin_hand, pin_body, pin_irp):
        self.arm = self.myservo.MyServomotor(pin_arm)
        self.claw = self.myservo.MyServomotor(pin_claw)
        self.body = self.DCMotor(pin_body)
        self.irp = self.IRPhotoReflector(pin_irp)
        self.position = 1  # Stores initial position
</pre>

#### ■ Defining the `moveto()` Method

The `move_to()` method takes the number of the space in its `num` argument. The difference between the destination number in `num` and the current position number in `position` decides **how many spaces** and **which direction to move** before rotating the body.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_8_E.png"/>

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_9_E.png"/>

First, let’s use `diff` to find the difference between these two numbers. Next, we’ll set the `direction` to `"cw"`, or **clockwise** if the difference is positive. If the difference is negative, we’ll set it to `"ccw"`, or **counterclockwise**!

###### New Code (Lines 39-41)
<pre class="prettyprint linenums:36 lang-py">
    def stop(self):
        self.body.brake()
    
    def move_to(self, num):
        diff = num - self.position  # Finds difference between destination and current position number
        direction = "cw" if diff > 0 else "ccw"  # Turns clockwise for positive difference and counterclockwise for negative difference
</pre>



Next, we’ll use the direction we found to rotate the body. Since we won’t need to specify a time, we can leave out the `duration` argument!

###### New Code (Line 42)

<pre class="prettyprint linenums:39 lang-py">
    def move_to(self, num):
        diff = num - self.position
        direction = "cw" if diff > 0 else "ccw"
        self.rotate(direction=direction)  # You can leave out the speed argument too!
</pre>

So, if we’re not setting a duration, how do we decide how much our Arm Robot should rotate? This is where the IR Photoreflector comes in!

Think of how the IR Photoreflector's values change as it moves from one black space to the next one. Take a look at the picture below, which uses our two threshold values to show this change.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_10_E.png"/>

This means that our Arm Robot can wait until it’s **over the white space** and the sensor value is **greater than `threshold_high`**, then wait until it’s **over the black space** and the sensor value is **less than `threshold_low`** to tell that it’s moved spaces!

While Python doesn’t have a native **wait until** statement, we can combine a `while` statement with a `not` operator to do the same thing.

<pre>
while not sensor value > threshold_high:  # Wait until over white space
    pass
while not sensor value < threshold_low:  # Wait until over black space
    pass
</pre>

Our program will repeat the `for` statement for the number of times in `diff` before stopping the rotation to complete the movement. There is one thing we have to keep in mind: if the difference is negative, we’ll have to use the native `abs` function to convert this number to an absolute value!

###### New Code (Lines 43-48)

<pre class="prettyprint linenums:39 lang-py">
    def move_to(self, num):
        diff = num - self.position
        direction = "cw" if diff > 0 else "ccw"
        self.rotate(direction=direction)
        for _ in range(abs(diff)):  # Use abs() function here to convert difference to absolute value
            while not self.irp.get_value() > self.threshold_high:  # Wait until over white space
                pass
            while not self.irp.get_value() < self.threshold_low:  # Wait until over black space
                pass
        self.stop()  # Stop rotating body
</pre>

Last, we’ll store the number of the destination to the `position` property. This new number will become our base position when want to move to the next space!

###### New Code (Line 49)

<pre class="prettyprint linenums:39 lang-py">
    def move_to(self, num):
        diff = num - self.position
        direction = "cw" if diff > 0 else "ccw"
        self.rotate(direction=direction)
        for _ in range(abs(diff)):
            while not self.irp.get_value() > self.threshold_high:
                pass
            while not self.irp.get_value() < self.threshold_low:
                pass
        self.stop()
        self.position = num  # Record destination number
</pre>
### Testing the Program
A fully defined `ArmRobot` class would look like the code shown below:

##### Example Code 3-3-1

<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor, IRPhotoReflector
    import myservo
    import time

    threshold_high = 900
    threshold_low = 650

    def __init__(self, *, pin_arm, pin_hand, pin_body, pin_irp):
        self.arm = self.myservo.MyServomotor(pin_arm)
        self.claw = self.myservo.MyServomotor(pin_claw)
        self.body = self.DCMotor(pin_body)
        self.irp = self.IRPhotoReflector(pin_irp)
        self.position = 1

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
    
    def move_to(self, num):
        diff = num - self.position
        direction = "cw" if diff > 0 else "ccw"
        self.rotate(direction=direction)
        for _ in range(abs(diff)):
            while not self.irp.get_value() > self.threshold_high:
                pass
            while not self.irp.get_value() < self.threshold_low:
                pass
        self.stop()
        self.position = num
</pre>

Now we’re going to take our class and see how our Arm Robot works. This program picks up three blocks and moves them to a set location in order!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_11_E.png"/>

##### Test Program Example

<pre class="prettyprint linenums:52 lang-py">
armrobot = ArmRobot(pin_arm="P13", pin_claw="P14", pin_body="M1", pin_irp="P0")

armrobot.pick()  # Carries white block to black circle 4
armrobot.move_to(4)
armrobot.place()

armrobot.move_to(2)  # Carries red block to green circle 3
armrobot.pick()
armrobot.move_to(3)
armrobot.place()

armrobot.move_to(6)  # Carries yellow block to blue circle 5
armrobot.pick()
armrobot.move_to(5)
armrobot.place()

armrobot.move_to(1)  # Returns to starting circle 1
</pre>
## Challenge: Choosing the Most Efficient Route
The `move_to()` method we added to the `ArmRobot` class in the last section may not always choose the most efficient delivery route for your blocks!

For example, the Arm Robot has to make a big clockwise rotation when moving from space 1 to space 6 even though rotating counterclockwise would be much, much faster!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_12_E.png"/>

That’s why in this challenge we’ll improve our program by making our Arm Robot rotate the shortest possible distance to deliver a block!
### Visualizing the Program
Changing this program is incredibly easy once you understand the theory behind it. To put it in concrete terms, if your Arm Robot is set to rotate in one direction, you can **add or subtract 6** to make it rotate in the opposite direction! In order to do this, we’ll use the following formulas to **convert the difference** for **positive** and **negative** differences between numbers.

* **Positive Difference**

<pre>
Converted difference = Number difference - 6 
</pre>

* **Negative Difference**

<pre>
Converted difference = Number difference + 6
</pre>

Now let’s use an actual example to check whether our conversion formulas actually work:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_13_E.png"/>

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-2/8-2_14_E.png"/>

You’ll see that each conversion works without an issue! Let’s use the conversion formulas we just checked to let our Arm Robot know that it’s more efficient to turn in the opposite direction if the **absolute value of the difference is greater than 3**!
### Example Program Changes
Writing code for our conversion formulas would look like the code shown here. If you’re having trouble getting your code to work correctly, use this code as a reference to fix it!

##### Example Code 4-2-1
###### New Code (Lines 41-42)

<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor, IRPhotoReflector
    import myservo
    import time

    threshold_high = 900
    threshold_low = 650

    def __init__(self, *, pin_arm, pin_claw, pin_body, pin_irp):
        self.arm = self.myservo.MyServomotor(pin_arm)
        self.claw = self.myservo.MyServomotor(pin_claw)
        self.body = self.DCMotor(pin_body)
        self.irp = self.IRPhotoReflector(pin_irp)
        self.position = 1

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
    
    def move_to(self, num):
        diff = num - self.position
        if abs(diff) > 3:  # Turning in opposite direction is more efficient if absolute difference is > 3
            diff = diff - 6 if diff > 0 else diff + 6  # Split formula for positive vs. negative difference
        direction = "cw" if diff > 0 else "ccw"  # No changes to code after this line
        self.rotate(direction=direction)
        for _ in range(abs(diff)):
            while not self.irp.get_value() > self.threshold_high:
                pass
            while not self.irp.get_value() < self.threshold_low:
                pass
        self.stop()
        self.position = num   

    
armrobot = ArmRobot(pin_arm="P13", pin_claw="P14", pin_body="M1", pin_irp="P0")

armrobot.pick()
armrobot.move_to(4)
armrobot.place()

armrobot.move_to(2)
armrobot.pick()
armrobot.move_to(3)
armrobot.place()

armrobot.move_to(6)
armrobot.pick()
armrobot.move_to(5)
armrobot.place()

armrobot.move_to(1)
</pre>
## Conclusion

### A Quick Review
In this lesson we added a positioning feature to our Arm Robot from the previous lesson. Sensor-based positioning is absolutely essential if you want a robot to perform precise movements! The mechanism behind the system in this lesson is pretty simple, but real robots get their precision control by running complex internal calculations on data pulled from a multitude of sensors. Understanding this requires some serious knowledge in math and physics, so be sure to pay extra attention to those subjects in school!
### Coming Up Next Time!
In the next lesson, we’ll add a Color Sensor to our Arm Robot to make a color-coded delivery system!
