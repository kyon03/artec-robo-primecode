---
tags: Python_English, Topic 8
---

Python Robotics Course Lesson 31

Topic 8-3
A Color-Coded Conveyance System
===

Add a color-detection feature to your conveyance system!

<iframe src="https://player.vimeo.com/video/484358819" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

:::info

Lesson Introduction

■ Lesson 31 Contents
1. Programming a Color-Coded Conveyance System
2. Challenge: Moving Randomly-Placed Blocks to the Correctly Colored Location
 

In this lesson, we’ll take our modified Arm Robot from the last lesson and add a Color Sensor. This will allow our Arm Robot to detect the color of the blocks it transports!

In the first half of the lesson, we’ll use our new Color Sensor to add a method which detects the color of the block before moving it to a specific location. We’ll then use this method to make a program which moves blocks to a space of the same color!



The Arm Robot will detect the color of the block when it picks it up.

In the challenge in the second half of the lesson, we’ll set down four blocks in random locations on the field before programming our Arm Robot to deliver each block to the space of the same color using an algorithm we’ve created!

Now let's start the lesson!

:::

## In This Lesson...
In this lesson, we’ll take our modified Arm Robot from the last two lessons and add a Color Sensor. This will allow our Arm Robot to detect the color of the blocks it transports!

## Adding a Color Sensor
Let’s add a Color Sensor to our Arm Robot from Lesson 30. Follow the instructions below to do it!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_1_E.png"/>

### Parts You'll Need
##### Parts
* Lesson 30’s Positioning Arm Robot x 1
* Color Sensor x 1
* Sensor Connecting Cable (4-wire, 30cm) x 1
* Half A (Gray) x 1
* Half C (White) x 1

##### The Artec Block Shapes

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_2_E.png"/>

### Adding a Color Sensor
Follow the link below to find instructions for building your Arm Robot:

[Adding a Color Sensor](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_build_E.pdf)
## Programming a Color-Coded Conveyance System
Here we’re going to add a feature which uses the Color Sensor to read the color of the block the robot lifts and carry it to the circle of the same color. If the Arm Robot can’t detect the color, it will transport the block to the black circle!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_3_E.png"/>
### Expanding the `ArmRobot` Class
We’ll need to add new properties and methods to our `ArmRobo` class from lesson 30 in order to add this feature!

##### New `ArmRobot` Class Properties

|Property|What is it?|
|:---|:---|
|`cs`|`ColorSensor` class instance|
|`COLOR_POSITONS`|Dictionary which assigns each colored space a number|

##### New `ArmRobot` Class Methods

|Method|What does it do?|
|:---|:---|
|`recognize_color()`|Gets and returns the color of the block the Arm Robot has picked up|
|`move_to_color()`|Rotates the body to face the space of the specified color|

Now let’s define each of these properties and methods in order.

#### ■ Defining the `cs` Property

We’ll be adding the properties and methods in the above tables to our `ArmRobot` class. If you deleted this program already, go ahead and copy the code below into a new file!

##### The Expanded `ArmRobot` Class

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
        if abs(diff) > 3:
            diff = diff - 6 if diff > 0 else diff + 6
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

We’ll create an instance of the `Color Sensor` class and store it in the `cs` property here in order to use our new Color Sensor. Let's import the `ColorSensor` class, then use an `__init__` method with the Color Sensor’s port number in its argument to create an instance!

###### New Code (Line 2, Line 9, Line 14)

<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor, IRPhotoReflector, ColorSensor  # Adds the ColorSensor
    import myservo
    import time
   
    threshold_high = 900
    threshold_low = 650

    def __init__(self, *, pin_arm, pin_hand, pin_body, pin_irp, pin_cs):  # Takes the Color Sensor’s port as an argument
        self.arm = self.myservo.MyServomotor(pin_arm)
        self.claw = self.myservo.MyServomotor(pin_claw)
        self.body = self.DCMotor(pin_body)
        self.irp = self.IRPhotoReflector(pin_irp)
        self.cs = self.ColorSensor(pin_cs)  # Creates a ColorSensor class instance
        self.position = 1
</pre>

#### ■ Defining the `COLOR_POSITIONS` Property

In order to define the `move_to()` method in lesson 30, we assigned each circle a number from **1** to **6**.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_4_E.png"/>

Instead of using numbers here, we’re going to make a dictionary which assigns each color a number of its own! We’ll name this dictionary `COLOR_POSITIONS` and define it as shown below:

###### New Code (Lines 6-13)

<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor, IRPhotoReflector, ColorSensor
    import myservo
    import time

    COLOR_POSITIONS = {
        "grey": 1,
        "red": 2,
        "green": 3,
        "undef": 4,
        "blue": 5,
        "yellow": 6,
    }
    
    threshold_high = 900
    threshold_low = 650
</pre>

#### ■ Defining the `recognize_color()` Method

We’re going to use the `ColorSensor` class’s `recognize_color` method to let our Arm Robot detect colors. The return values of this method will be the `ColorSensor` class properties shown in the table below.

##### The `get_colorcode()` Method Return Values

|Color|Return Value (`ColorSensor` class property)|
|:---|:---|
|Red|COLOR_RED|
|Green|COLOR_GREEN|
|Blue|COLOR_BLUE|
|White|COLOR_WHITE|
|Yellow|COLOR_YELLOW|
|Orange|COLOR_ORANGE|
|Purple|COLOR_PURPLE|
|Unknown|COLOR_UNDEF|

The `recognize_color()` method will turn the property our `get_colorcode()` method retrieves into a text string before returning that string. For this challenge, we’ll have to make the text strings `”red”`, `”green”`, `”blue”`, and `”yellow”` for the blocks our Arm Robot will transport and have any unrecognized colors return the value `”undef”`.

###### New Code (Lines 63-75)

<pre class="prettyprint linenums:49 lang-py">
    def move_to(self, num):
        diff = num - self.position
        if abs(diff) > 3:
            diff = diff - 6 if diff > 0 else diff + 6
        direction = "cw" if diff > 0 else "ccw"
        self.rotate(direction=direction)
        for _ in range(abs(diff)):
            while not self.irp.get_value() > self.threshold_high:
                pass
            while not self.irp.get_value() < self.threshold_low:
                pass
        self.stop()
        self.position = num

    def recognize_color(self):  # This method detects the color
        color = self.cs.get_colorcode()  # Retrieves color data as a property from the ColorSensor class
        if color == self.cs.COLOR_RED:  # If color is red
            color_name = "red"
        elif color == self.cs.COLOR_GREEN:  # If color is green
            color_name = "green"
        elif color == self.cs.COLOR_BLUE:  # If color is blue
            color_name = "blue"
        elif color == self.cs.COLOR_YELLOW:  # If color is yellow
            color_name = "yellow"
        else:  #  If color isn\’t any of the above (COLOR_WHITE、COLOR_ORANGE、COLOR_PURPLE、COLOR_UNDEF)
            color_name = "undef"
        return color_name
</pre>

#### ■ Defining the `move_to_color()` Method

Defining the `move_to_color()` method is incredibly easy! We’ll have it take the name of the color as an argument and use the `COLOR_POSITIONS` property to convert it to a number before running the `move_to()` method.

###### New Code (Lines 77-79)

<pre class="prettyprint linenums:63 lang-py">
    def recognize_color(self):
        color = self.cs.get_colorcode()
        if color == self.cs.COLOR_RED:
            color_name = "red"
        elif color == self.cs.COLOR_GREEN:
            color_name = "green"
        elif color == self.cs.COLOR_BLUE:
            color_name = "blue"
        elif color == self.cs.COLOR_YELLOW:
            color_name = "yellow"
        else:
            color_name = "undef"
        return color_name

    |`move_to_color()`|Rotates the body to face the space of the specified color|
        num = self.COLOR_POSITIONS[color]  # Converts color name to number
        self.move_to(num)  # Moves to the specified number
</pre>
### Testing the Program
Now let’s see how our new and improved `ArmRobot` class. Before we use our new color sensing feature, let’s adjust the angles of our Servomotor which picks up the block.

#### ■ Calibrating Servomotor Angles

One thing to keep in mind when you use a Color Sensor to detect color is that your results can vary depending on the Color Sensor’s distance and position relative to the block! This means that means that we’ll have to lay each block on its side leave about a half a block’s worth of space between the block and the Color Sensor when the claw picks it up.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_5_E.png"/>

Let’s follow the code shown below and increase the angle when the arm lowers from 165 to 170 degrees. If your Arm Robot still can’t reach the block, try increasing the angle a bit!

###### Changed Code (Line 27)
<pre class="prettyprint linenums:26 lang-py">
    def pick(self):  # Lower the arm\’s position just a bit when it picks up the block
        self.myservo.syncRotateServos(600, (self.arm, 170), (self.hand, 135))  # 165 → 170
        self.myservo.syncRotateServos(600, (self.claw, 60))
        self.myservo.syncRotateServos(600, (self.arm, 90))
</pre>


## ■ Programming a Color-Coded Conveyance System

In order to test how our Arm Robot works, we’re going to make a program which lets us **press A to pick up the block at space 1** then **detect the block’s color and move it**! Take a look at the example code below as you program it!

##### Example Code 3-2-1
###### New Code (Lines 82-96)

<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor, IRPhotoReflector, ColorSensor
    import myservo
    import time

    COLOR_POSITIONS = {
        "grey": 1,
        "red": 2,
        "green": 3,
        "undef": 4,
        "blue": 5,
        "yellow": 6,
    }
    
    threshold_high = 900
    threshold_low = 650

    def __init__(self, *, pin_arm, pin_hand, pin_body, pin_irp, pin_cs):
        self.arm = self.myservo.MyServomotor(pin_arm)
        self.claw = self.myservo.MyServomotor(pin_claw)
        self.body = self.DCMotor(pin_body)
        self.irp = self.IRPhotoReflector(pin_irp)
        self.cs = self.ColorSensor(pin_cs)
        self.position = 1

    def pick(self):
        self.myservo.syncRotateServos(600, (self.arm, 170), (self.claw, 135))
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
        if abs(diff) > 3:
            diff = diff - 6 if diff > 0 else diff + 6
        direction = "cw" if diff > 0 else "ccw"
        self.rotate(direction=direction)
        for _ in range(abs(diff)):
            while not self.irp.get_value() > self.threshold_high:
                pass
            while not self.irp.get_value() < self.threshold_low:
                pass
        self.stop()
        self.position = num

    def recognize_color(self):
        color = self.cs.get_colorcode()
        if color == self.cs.COLOR_RED:
            color_name = "red"
        elif color == self.cs.COLOR_GREEN:
            color_name = "green"
        elif color == self.cs.COLOR_BLUE:
            color_name = "blue"
        elif color == self.cs.COLOR_YELLOW:
            color_name = "yellow"
        else:
            color_name = "undef"
        return color_name

    def move_to_color(self, color):
        num = self.COLOR_POSITIONS[color]
        self.move_to(num)

# Test Program
def main():
    from pystubit.board import button_a, display  # Imports button A and display objects
    # Creates an instance of the ArmRobot class
    armrobot = ArmRobot(pin_arm="P13", pin_claw="P14", pin_body="M1", pin_irp="P0", pin_cs="I2C")

    while True:
        if button_a.is_pressed():
            armrobot.pick()  # 1. Picks up the block
            color = armrobot.recognize_color()  # 2. Detects the color of the block
            display.scroll(color, delay=10)  # 3. Name of color scrolls across the display
            armrobot.move_to_color(color)  # 4. Turns to the space of the same color
            armrobot.place()  # 5. Sets down the block
            armrobot.move_to(1)# 6. Returns to starting position


main()
</pre>

#### ■ What if My Arm Robot Doesn’t Recognize a Color?

Try changing the position of the arm when it lowers to adjust the distance between the front of the Color Sensor and the block. **Blue is a hard color to recognize**, so if adjusting the position of the arm doesn’t work, try using the `recognize_color()` method to change any `”undef”` result to `”blue”`!

###### Changed Code (Line 75)
<pre class="prettyprint linenums:63 lang-py">
    def recognize_color(self):
        color = self.cs.get_colorcode()
        if color == self.cs.COLOR_RED:
            color_name = "red"
        elif color == self.cs.COLOR_GREEN:
            color_name = "green"
        elif color == self.cs.COLOR_BLUE:
            color_name = "blue"
        elif color == self.cs.COLOR_YELLOW:
            color_name = "yellow"
        else:
            # color_name = "undef"  # Changes undef to blue
            color_name = "blue"
        return color_name
</pre>

## Challenge: Moving Randomly-Placed Blocks to the Correctly Colored Location
In this challenge, you’ll set one **red**, **blue**, **green**, or **yellow** at random on **red space 2**, **green space 3**, **blue space 5**, and **yellow space 6**. The algorithm we’ll use to do this is pretty complex, so if you’re having trouble thinking of how to make yours follow along with the explanation below as we program.

###### ★ An **algorithm** is a set of rules that you make into a formula and use to solve a problem!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_6_E.png"/>

### Creating an Algorithm
#### ■ Making a Temporary Storage Area for Blocks

In order to change where our Arm Robot places a block, we’ll need to create a **storage area** where we can temporarily set a block down. We’ll use the black circle as a storage area which we can use to switch blocks.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_7_E.png"/>

#### ■ When Does the Arm Robot Pick Up a Block?

There a three situations in which our Arm Robot might pick up a block:

1. The color of the block matches the color of the current space.
2. The color of the space doesn’t match, but there is an open space of the same color.
3. Neither situation 1 or 2. 

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_8_E.png"/>

We’ll need to make our Arm Robot do the following in each of these three situations:

* **Situation 1**
Set the block back down as there’s no need to transport it.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_9_E.png"/>


* **Situation 2**
Move the block to the space of the same color.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_10_E.png"/>


* **Situation 3**
Set the block in the storage area.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_11_E.png"/>
 
**Situation 3 is where we need to be extra careful!** Both situations 1 and 2 end with the block in a space of the same color, which means that the block has been successfully transported. In situation 3, however, the colors won’t match. This means we have to move the block again later! Any blocks which we have to move later get put in the **transport queue**, which we’ll have to manage the data for.

Taking this into account, our program will have to manage the following three sets of data every time we transport a block:

* **Spaces with blocks which haven’t been transported**
This data tells our Arm Robot where there are still blocks which need to be transported.
* **Open spaces without blocks**
This data tells which spaces act as destinations for blocks.
* **Blocks in the transport queue and their locations**
This data manages which blocks are in the transport queue.

We’ll create the following three variables in order to manage these pieces of data:

|Variable|What does it store?|
|:---|:---|
|`incomplete`|A list of spaces with blocks that need to be transported|
|`open`|The color of open spaces|
|`queue`|A list of blocks in the transport queue and their locations|

#### ■ The Order of Each Process

Let’s finish by thinking about how we should organize the processes of our program.

The first thing to consider is that the goal of this challenge is to get every block to a space with the same color. Our transport process should loop until every block is transported. Once every block has been transported, this loop should stop!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_12_E.png"/>


Depending on whether there’s a block in the transport queue, the transport process will change the destination of the Arm Robot. If there is a block in the transport queue, your Arm Robot will turn to different spaces depending on whether the color of the block in the queue matches the color of the open space. If will also lift the block in the destination space regardless of the situation.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_13_E.png"/>


Next, the Arm Robot will do one thing if it already knows the color of the block (blocks in the transport queue have already been scanned once) and another thing if it doesn’t know the color.

If it doesn’t know the color of the block, the Arm Robot will use its Color Sensor to scan the color of the block and show the result on the LED display. There’s no need to transport the block if the color of the destination matches the color of the block the Arm Robot has just picked up, so we’ll have the robot set the block down and delete that space from the list of incomplete deliveries.

If the colors don’t match and the Arm Robot recognizes the block, it will carry the block to the open space.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_14_E.png"/>

The last step is for our Arm Robot to perform different actions depending on whether the color of the block and the color of the open space match.

If the colors match, we’ll delete that space from the list of incomplete deliveries. If the colors don’t match, we’ll add both the color of the block and the space to the transport queue list.

Since the original space of the block is now open, we’ll change the value of its variable.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_15_E.png"/>

Put the processes above together and your program will follow the steps in the flowchart below:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_16_E.png"/>

#### ■ Verifying the Algorithm with a Concrete Example

Now let’s use the following case as a concrete example and see if the algorithm we just made can make our Arm Robot get the job done!

##### Example Block Placements

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_17_E.png"/>

### Arm Robot Movements Using the Algorithm

1. Choose the item at the top of the incomplete deliveries list. Check that the block there is `”blue”` before rotating to open space `”undef”`. Since the color of the block doesn’t match the color of the space, we’ll add the data `(“blue”, “undef”)` to the transport queue list. Next, we’ll set the `”red”` circle as a new open space.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_18_E.png"/>

2. Check the item at the top of the transport queue list. The `”blue”` block doesn’t match the color of our open `”red”` space, so we’ll have our Arm Robot turn to the `”blue”` space which matches the color of the block and carry the `”yellow”` block at that space to the newly-opened `”undef”` space. Since the color of the block doesn’t match the color of the space, we’ll add the data `(“yellow”, “red”)` to the transport queue list. Next, we’ll set the `”blue”` circle as a new open space.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_19_E.png"/>

3. Check the item at the top of the transport queue list. The `”blue”` block matches the color of our newly-opened `”blue”` space, so we’ll have our Arm Robot carry the block from its current `”undef”` space to the newly-opened space. We’ll now delete this space from the incomplete deliveries list since the block has been delivered. Next, we’ll set the black `”undef”` circle as a new open space.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_20_E.png"/>

4. Check the item at the top of the transport queue list. The `”yellow”` block doesn’t match the color of our newly-opened `”undef”` space, so we’ll have our Arm Robot turn to the `”yellow”` space which matches the color of the block and carry the `”green”` block at that space to the newly-opened `”undef”` space. Since the color of the block doesn’t match the color of the space, we’ll add the data `(“green”, “undef”)` to the transport queue list. Next, we’ll set the `”yellow”` circle as a new open space.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_21_E.png"/>

5. Check the item at the top of the transport queue list. The `”yellow”` block matches the color of our newly-opened `”yellow”` space, so we’ll have our Arm Robot carry the block from its current `”red”` space to the newly-opened space. We’ll now delete this space from the incomplete deliveries list since the block has been delivered. Next, we’ll set the `”red”` circle as a new open space.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_22_E.png"/>

6. Check the item at the top of the transport queue list. The `”green”` block doesn’t match the color of our open `”red”` space, so we’ll have our Arm Robot turn to the `”green”` space which matches the color of the block and carry the `”red”` block at that space to the newly-opened `”undef”` space. Since the color of the block and the space match, we’ll delete this data from the incomplete deliveries list. Next, we’ll set the `green”` circle as a new open space.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_23_E.png"/>

7. Check the item at the top of the transport queue list. The `”green”` block matches the color of our newly-opened `”green”` space, so we’ll have our Arm Robot carry the block from its current `”undef”` space to the newly-opened space. We’ll now delete this space from the incomplete deliveries list since the block has been delivered. Next, we’ll set the black `”undef”` circle as a new open space.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_24_E.png"/>

8. The incomplete deliveries list is now empty, which means the job is done!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_25_E.png"/>

### Example Program
Since our test was a success, we’re now going to write a program which follows the algorithm we made!

#### ■ Defining Variables

We’ll need to define three variables at the beginning of the `main()` function in order to manage our data. Additionally, we’ll have the Arm Robot start its work sequence when we press the A button!

###### New and Changed Code (Lines 87-91)

<pre class="prettyprint linenums:82 lang-py">
def main():
    from pystubit.board import button_a, display

    armrobot = ArmRobot(pin_arm="P13", pin_claw="P14", pin_body="M1", pin_irp="P0", pin_cs="I2C")

    while True:
        if button_a.is_pressed():  # Arm Robot starts working when you press A
            incomplete = ["red", "green", "blue", "yellow"]  # Creates incomplete deliveries list
            open = "undef"  # Defines an open space
            queue = []  # Creates a list of blocks and spaces in the transport queue
</pre>

#### ■ Choosing a Destination

Next, let’s set how our Arm Robot chooses a destination. If the `queue` list for our transport queue isn’t empty, we’ll have it retrieve the data from that list and use the `target` variable to set its target destination. If the `queue` list is empty, we’ll use the item at the top of the incomplete deliveries list as the destination. If the Arm Robot recognizes the color of the block at the destination, it will store that data in the `b_color` variable. If it doesn’t recognize the color, it will store `None` to the variable.

###### ★ The list `queue` stores data as a tuple formatted `(block_color, space_color)`.

###### New Code (Lines 93-105)

<pre class="prettyprint linenums:82 lang-py">
def main():
    from pystubit.board import button_a, display

    armrobot = ArmRobot(pin_arm="P13", pin_claw="P14", pin_body="M1", pin_irp="P0", pin_cs="I2C")

    while True:
        if button_a.is_pressed():
            incomplete = ["red", "green”, "blue", "yellow"]
            open = "undef"
            queue = [] 

            while len(incomplete):  # Repeats until the incomplete deliveries list is empty
                if len(queue):  # Takes priority when there are items in the transport queue list
                    if queue[0][0] is open:  # When the color of the open space matches the color of the block in the queue
                        data = queue.pop(0)  # Deletes data from the shipping queue to complete the delivery
                        b_color = data[0]  # Stores color data of blocks in the shipping queue
                        target = data[1]  # Turns to space where block is located
                    else:  # If there\’s no open space which matches the color of the block
                        # Leaves the data in the transport queue as the delivery is still incomplete
                        target = queue[0][0]  # Turns to space which matches the color of the block in the queue
                        b_color = None  # Color of the block at the destination space is unrecognized
                else:  # If there\’s no data in the transport queue list
                    target = incomplete[0]  # Sets item at the top of the incomplete deliveries list as the destination
                    b_color = None  # Color of the block at the destination space is unrecognized
</pre>

#### Detecting the Color of the Block

Now let’s get our Arm Robot to rotate to the target destination and lift the block. If it doesn’t know the color of the block, the Arm Robot will scan the color of the block and scroll the name of the color across the LED display.

###### New Code (Lines 107-111)

<pre class="prettyprint linenums:93 lang-py">
            while len(incomplete):
                if len(queue):
                    if queue[0][0] is open:
                        data = queue.pop(0)
                        b_color = data[0]
                        target = data[1]
                    else:
                        target = queue[0][0]
                        b_color = None
                else:
                    target = incomplete[0]
                    b_color = None

                # Arm Robot rotates and lifts the block
                armrobot.move_to_color(target)
                armrobot.pick()
                if b_color is None:  # If the Arm Robot doesn\’t recognize the color of the block
                    b_color = armrobot.recognize_color()  # Detects the color
                    display.scroll(b_color, delay=10)  # Scrolls the name of the detected color across the display
</pre>

#### ■ Completing the Delivery if the Color of the Block and Destination Match

If the color of the block matches the color of the current space, the Arm Robot will set it back down. We’ll now delete this space from the incomplete deliveries list (`incomplete`) since the block has been delivered.

###### New Code (Lines 112-115)

<pre class="prettyprint linenums:106 lang-py">
                armrobot.move_to_color(target)
                armrobot.pick()
                if b_color is None:
                    b_color = armrobot.recognize_color()
                    display.scroll(b_color, delay=10)

                    if b_color == target:  # If the color of the block matches the color of the current space
                        armrobot.place()  # Sets the block back down
                        incomplete.remove(target)  # Deletes this data from the incomplete list since this block has been delivered
                        continue  # Moves on to next loop
</pre>

#### ■ Delivering Blocks to Open Spaces

If the program doesn’t move on to the next loop, the Arm Robot will deliver blocks to an `open` space. If the color of the block and the open space match, the delivery has been completed and the space will be deleted from the incomplete deliveries list. If the colors don’t match, we’ll add a tuple containing both the color of the block and the space to the transport queue list. Since `target`, which was the block’s original space, is now open we’ll store `open` in this variable.

###### New Code (Lines 118-119, Lines 122-126)

<pre class="prettyprint linenums:106 lang-py">
                armrobot.move_to_color(target)
                armrobot.pick()
                if b_color is None:
                    b_color = armrobot.recognize_color()
                    display.scroll(b_color, delay=10)

                    if b_color == target:
                        armrobot.place()
                        incomplete.remove(target)
                        continue

                # Delivers block to open space
                armrobot.move_to_color(open)
                armrobot.place()

                # If the color of the block and the open space match, completes delivery and deletes space from incomplete deliveries list
                if b_color is open:
                    incomplete.remove(open)
                else:  # If the colors don\’t match, adds a tuple containing both the color of the block and the space to the transport queue list
                    queue.append((b_color, open))
                open = target  # Changes the block\’s original space to an open space
</pre>

#### ■ Returning to the Starting Position

Since every block has been delivered, we’ll end line 93’s `while` statement loop. Once the loop as ended, let’s make our Arm Robot returning to its starting position at space **1**.

###### New Code (Line 127)

<pre class="prettyprint linenums:121 lang-py">
                if b_color is open:
                    incomplete.remove(open)
                else: 
                    queue.append((b_color, open))
                open = target

            armrobot.move_to(1)  # Returns to space 1 after delivering all blocks
</pre>
### Checking How the Program Works
Now let’s try running our finished program. Try placing your blocks in different spots and you’ll see that your Arm Robot can deliver them no matter where they are!

##### Example Code 4-3-1

<pre class="prettyprint linenums:82 lang-py">
def main():
    from pystubit.board import button_a, display

    armrobot = ArmRobot(pin_arm="P13", pin_claw="P14", pin_body="M1", pin_irp="P0", pin_cs="I2C")

    while True:
        if button_a.is_pressed():  # Arm Robot starts working when you press A
            incomplete = ["red", "green", "blue", "yellow"]  # Creates incomplete deliveries list
            open = "undef"  # Defines an open space
            queue = []  # Creates a list of blocks and spaces in the transport queue

            while len(incomplete):  # Repeats until the incomplete deliveries list is empty
                if len(queue):  # Takes priority when there are items in the transport queue list
                    if queue[0][0] is open:  # When the color of the open space matches the color of the block in the queue
                        data = queue.pop(0)  # Deletes data from the shipping queue to complete the delivery
                        b_color = data[0]  # Stores color data of blocks in the shipping queue
                        target = data[1]  # Turns to space where block is located
                    else:  # If there\’s no open space which matches the color of the block
                        # Leaves the data in the transport queue as the delivery is still incomplete
                        target = queue[0][0]  # Turns to space which matches the color of the block in the queue
                        b_color = None  # Color of the block at the destination space is unrecognized
                else:  # If there\’s no data in the transport queue list
                    target = incomplete[0]  # Sets item at the top of the incomplete deliveries list as the destination
                    b_color = None  # Color of the block at the destination space is unrecognized

                # Arm Robot rotates and lifts the block
                armrobot.move_to_color(target)
                armrobot.pick()
                if b_color is None:  # If the Arm Robot doesn\’t recognize the color of the block
                    b_color = armrobot.recognize_color()
                    display.scroll(b_color, delay=10)

                    # Sets the block back down if the color of the block matches the color of the current space
                    if b_color == target:
                        armrobot.place()
                        incomplete.remove(target)  # Deletes this data from the incomplete list since this block has been delivered
                        continue  # Moves on to next loop

                # Delivers block to the open space
                armrobot.move_to_color(open)
                armrobot.place()

                # If the color of the block and the open space match, completes delivery and deletes space from incomplete deliveries list
                if b_color is open:
                    incomplete.remove(open)
                else:  # If the colors don’t match, adds a tuple containing both the color of the block and the space to the transport queue list
                    queue.append((b_color, open))

                # Changes the original space of the block to a new open space
                open = target

            armrobot.move_to(1)  # Returns to starting location after delivering all blocks


main()  # Runs the main function
</pre>
## Conclusion

### A Quick Review
While the Arm Robot in this lesson uses a Color Sensor to detect the colors of the blocks it delivers, cutting-edge arm robots use camera-based image recognition to scan not just the color of an object, but its shape, orientation, and more!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-3/8-3_26_E.png"/>

They also use the data their sensors collect to evaluate how the arm robot should move and build optimal algorithms to control it. Be sure to take the Arm Robot we’ve made here and see if you can make control algorithms which allow it to complete a challenge that you’ve developed!
### Coming Up Next Time!
While we’ve written code in Python to control our Arm Robot so far, not everyone who operates an arm robot is familiar with programming languages. That’s why in the next lesson we’ll challenge ourselves to develop a system which lets anyone control our Arm Robot by inputting simple commands!
