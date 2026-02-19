---
tags: Python_English, Topic 4
---

Python Robotics Course Lesson 14

Topic 4-2
A Sound-Sensing Anti-Collision System
===

Program a driving support system that helps cars avoid accidents using an Ultrasonic Sensor!

## In This Lesson...
Learn how to use an Ultrasonic Sensor and program a robot car that automatically detects objects in front of it!

:::info
Lesson Introduction
■ Lesson 14 Contents

1. Inserting Text with the format Method
2. The Quirks of Ultrasonic Sensors
3. Programming a Driving Support System
4. Challenge: An Anti-Fall Robocar



In this lesson, you'll use an Ultrasonic Sensor to program a driving support system that avoids collisions by braking a car when it detects obstacles in its path.

First we'll explain how to use the method format() to edit how data from sensors gets displayed in your program.

Next we'll test out the Ultrasonic Sensor you'll be using in your driving support system to learn its quirks and how to use it right.

After that, you can build a robot car and program it to stop automatically if it detects an object it might run into! The robocar will drive forward while you hold down a button, but its program makes it stop anyway if it senses a potential collision!

At the end of the lesson, you can use the Ultrasonic Sensor to try building a robocar that automatically stops itself from driving off of tables!

Now let's start the lesson!
:::

## New Python Syntax
Let's start off by introducing some new Python code you'll be using for the first time in this lesson.

As you get further and further into this course, the programs you're writing will get longer and longer! When you're starting out as a programmer, the longer the program you're writing gets the more likely you are to make mistakes. When an error occurs in your code, you need to find the mistake, or **bug** that caused it and fix it!

A lot of people check their code for bugs by dividing their program into several steps and then checking the data output of each step separately. This method is especially useful if you need to find bugs in a complex program all by yourself!

When you're checking data output, you'll want that data to be formatted in a way that makes it easy to tell what that data is and does, instead of just looking at a bunch of numbers and strings. Let's have a look at how you can do that formatting!


### Embedding Data in Strings with `format()`

In Topic 1-4 we briefly talked about a method for string objects called `format()`, which can make new strings by replacing any text wrapped in curly brackets `{}` inside a string with new text specified in `format()`'s parameters. Write the code below into your terminal and run it to review how this works!

<pre class="prettyprint">
&gt;&gt;&gt; '{}, {}, {}'.format(1, 2, 3)
'1, 2, 3'
</pre>

The area enclosed by a pair of curly brackets `{}` like this is called a **replacement field**. Usually you'll write a **field name** inside the brackets as well. If you don't include a field name (like in the code above), the **format()** method's parameters will be inserted into the string's replacement fields in the order they appear in. If you do use field names, you can specify particular replacement fields to put text in as keyword parameters, like this:

<pre class="prettyprint">
&gt;&gt;&gt; '{a}, {b}, {c}'.format(a=1, c=3, b=2)
'1, 2, 3'
</pre>

You can also use a colon `:` after a field name to specify formatting for the text you insert. For example, writing **:f** after the field name makes the inserted number appear as **fixed-point data** (a floating-point number with a specific number of digits after the decimal point displayed).

<pre class="prettyprint">
&gt;&gt;&gt; '{a:f}, {b:f}, {c:f}'.format(a=1,c=3,b=2)
'1.000000, 2.000000, 3.000000'
</pre>

Writing `.n` before `f` here sets the number of places after the decimal point in the fixed point number to `n`.

<pre class="prettyprint">
&gt;&gt;&gt; '{a:.3f}, {b:.2f}, {c:.1f}'.format(a=1, c=3, b=2)
'1.000, 2.00, 3.0'
</pre>

Here are a few other formatting tricks you can do with the `format()` method...

* Show Symbols for Numbers

|Code|Meaning|
|:---|:---|
|+|Show symbols in front of both positive and negative numbers.|
|-|Show symbols only in front of negative numbers.|
|One space|Put a single space in front of positive numbers and a  - symbol in front of negative numbers.|

<pre class="prettyprint">
&gt;&gt;&gt; '{a:+}, {b:-}, {c: }'.format(a=1, c=3, b=2)
'+1, 2,  3'
</pre>

* Express Numbers as Percentages

|Code|Meaning|
|:---|:---|
|%|Multiply a number by 100 and display it as fixed-point data (like f) with a % sign attached.|

<pre class="prettyprint">
&gt;&gt;&gt; '{a:0.0%}, {b:0.1%}, {c:0.2%}'.format(a=1, c=3, b=2)
'100%, 200.0%, 300.00%'
</pre>

* Set Field Size
Writing numbers after a colon `:` lets you set the width of the resulting field.

<pre class="prettyprint">
&gt;&gt;&gt; '{:15}'.format('python')
'python         '
</pre>

* Specify Positions Inside a Field
You can specify text alignment (centered, right-aligned, left-aligned) for the resulting field with these symbols.

|Code|Meaning|
|:---|:---|
|<|Align to the left. Left-alignment is the default for most objects.|
|>|Align to the right.|
|^|Center.|

<pre class="prettyprint">
&gt;&gt;&gt; '{:&lt16}'.format('python')
'python          '
&gt;&gt;&gt; '{:^16}'.format('python')
'     python     '
&gt;&gt;&gt; '{:&gt16}'.format('python')
'          python'
</pre>

These are just a few kinds of formatting you can do in Python. If you want to learn more, check out the official Python guide!

[Format String Syntax](https://docs.python.org/3/library/string.html#formatstrings)


## Building a Robocar
Get a copy of the building instructions for this lesson and follow the steps inside to build a robot car (robocar for short)!

### Parts You'll Need
* Core Unit x 1
* Robot Expansion Unit x 1
* Battery Box x 1
* DC Motor x 1
* Ultrasonic Sensor x 1
* Sensor Connecting Cable (3-wire, 15 cm) x 1
* Basic Cube (Gray) x 5
* Basic Cube (Red) x 1
* Half A (Gray) x 1
* Half B (Gray) x 2
* Half C (White) x 5
* Half D (White) x 1
* Beam x 1
* Disk x 1
* Gear (L) x 2
* Tire x 2

##### The Artec Block Shapes
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-2/4-2_1_E.png"/>

### Building Instructions

[Robocar Building Instructions](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-2/4-2_build_E.pdf)

## Programming a Driving Support System
As you may know from TV commercials, many modern cars come equipped with computerized systems that support the driver by automatically braking or adjusting direction when they detect traffic lines on the highway. Completely computer controlled self-driving cars are also getting closer to becoming a reality as prototypes are being tested around the world.

Advances in sensing technology, especially radar and cameras, are what's making all of this possible. Millimeter wave radar, for example, can use the reflective properties of radio waves to detect the presence of people and cars as well as how far away they are. Even more technologies are now being developed to sense and identify pedestrians, vehicles, signals, traffic lines, and more from camera footage.  All of these new developments bring us closer to making real self-driving cars.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-2/4-2_2_E.png"/>

In this lesson we'll be building a driving support system with the following features:

1. Plays an alarm sound with the Buzzer if it detects an obstacle within a certain distance from the car
2. Uses the brakes to stop automatically if the car is in danger of hitting something

We can accomplish both of these things using an **Ultrasonic Sensor**. We explained the basic functions of Ultrasonic Sensors in Topic 3-1, but let's talk about more of the details now.

### Detecting Distance with Ultrasonic Sensors
Let's try using the Ultrasonic Sensor to judge distances in various environments!

#### ■ Writing the Measurement Program

Make a program that uses the method `get_distance()` to get a distance measurement from the Ultrasonic Sensor once every second, then try running it on your Core Unit!

##### Example Code 4-1-1
```python=1
from pyatcrobo2.parts import UltrasonicSensor
import time

us = UltrasonicSensor('P0')

while True:
    print('{}cm'.format(us.get_distance()))
    time.sleep_ms(1000)
```

The Ultrasonic Sensor can't detect any waves for 10 milliseconds after it takes a measurement. If you try to make it detect new distances too quickly, you'll start getting incorrect measurements like -0.03, so make sure to always leave at least 15 milliseconds between Ultrasonic Sensor measurements in your code!

#### ■ How Far Away is the Ceiling/Floor?
Your Ultrasonic Sensor can measure distances from 3 to 250 centimeters. Try pointing your sensor at surfaces in the room (the ceiling,  walls, floor, table, etc.) and find the maximum distance it can detect that surface from.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-2/4-2_3_E.png"/>

###### ★ Make sure the USB cable connecting your Core Unit to your computer stays plugged in.
###### ★ The USB cable isn't very long, so you may want to measure maximum distance from the ceiling and minimum distance from a tabletop.

Use the side of your kit's storage box to test whether your sensor can get valid measurements when it's very close (less than 3 cm) to a surface.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-2/4-2_4_E.png"/>


#### ■ Testing Measurement Range

Your Ultrasonic Sensor can detect objects within 15° to the right or left of points directly in front of it. Set up a stack of blocks and test this out!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-2/4-2_5_E.png"/>

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-2/4-2_6_E.png"/>


#### ■ What if There's More Than One Object?

If more than one object is within the sensor's range, it measures the distance of the nearest object. Use two stacks of blocks to test this out!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-2/4-2_7_E.png"/>


#### ■ Detecting Smaller Objects

It's easy for the Ultrasonic Sensor to detect objects with large flat surfaces like walls and boxes, but it has a hard time detecting smaller, narrower objects like pens. Try setting a pen or other small, thin object in front of your sensor to test this out!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-2/4-2_8_E.png"/>

###### ★ Lower the pen into the sensor's range from above so it doesn't detect your hand instead!

Ultrasonic Sensors also have trouble detecting soft, sound-absorbent objects like sponges and cloth.

#### ■ Angles of Objects vs. Angles of Reflection

If you have an object with a flat surface set in front of an Ultrasonic Sensor, but its positioning is slanted, the sound waves the sensor emits will bounce off in a different direction and the sensor won't be able to pick them up to make a measurement. Try setting your storage box at a 45° angle in front of the sensor and see if you get incorrect distance measurements!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-2/4-2_9_E.png"/>


If you have another object set in front of the box, you'll get a measurement based on half of the distance between that object and the sensor.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-2/4-2_10_E.png"/>


#### ■ What We've Learned About Ultrasonic Sensors

Don't forget these factors when you're using an Ultrasonic Sensor!

* Ultrasonic Sensors can measure distances from 3 cm to 250 cm.
* The sensory range of an Ultrasonic Sensor is 15° to the right and left of points in front of it.
* If more than one object is within range, the Ultrasonic Sensor finds the distance of the nearest object.
* Ultrasonic Sonics can't detect objects with small surfaces or sound-absorbent objects.
* If an object is diagonally positioned (at a 45° angle) in front of the Ultrasonic Sensor, the sound waves will bounce off in a different direction, preventing the sensor from taking measurements.


### Programming the Alarm Sound
Usually when driving support systems detect your car is too close to the car in front of it or in danger of hitting something, they'll play an alarm before the automatic brakes turn on. You can make your robocar do this by programming the Buzzer to play when the Ultrasonic Sensor detects an object within 10 cm in front of the car!

#### ■ Press A to Drive
Let's program the A button on the Core Unit to act like an accelerator, making the car drive forward when you press it and slowly come to a stop when you let it go. Write the code like this:

##### Example Code 4-2-1
```python=1
from pystubit.board import button_a
from pyatcrobo2.parts import DCMotor

dcm = DCMotor('M1')
dcm.power(100)

while True:
    if button_a.is_pressed():
        dcm.cw()
    else:  
        dcm.stop() #Makes the car stop slowly without braking.
```

#### ■ Sounding the Alarm

When the Ultrasonic Sensor detects an object within 10 cm of the car, make the Buzzer play a short note three times as an alarm! Let's set up the Buzzer's alarm program as a function so we can call it later in the program. Modify Example Code 4-2-1 as shown below!

###### New and Changed Code (Line 1, Lines 5-8)
```python=1
from pystubit.board import button_a, buzzer
from pyatcrobo2.parts import DCMotor
import time

def alarm(): 
    for i in range(3):
        buzzer.on('C7', duration=50)　# Use a high-pitched note so it's easy to hear!
        time.sleep_ms(50)

dcm = DCMotor('M1')
dcm.power(100)

while True:
    if button_a.is_pressed():
        dcm.cw()
    else:  
        dcm.stop()
```

Next you need to add code that gets measurements from the Ultrasonic Sensor and runs the **alarm** function if it gets a measurement under 10 cm. Remember that the Ultrasonic Sensor produces a measurement of -0.03 when it fails to measure a distance, so we need to take that into account when writing the **if** statements that make the program branch here.

##### Example Code 4-2-2
###### New and Changed Code (Line 2, Line 10, Lines 16-18)
```python=1
from pystubit.board import button_a, buzzer
from pyatcrobo2.parts import DCMotor, UltrasonicSensor
import time

def alarm():
    for i in range(3):
        buzzer.on('C7', duration=50)
        time.sleep_ms(50)

us = UltrasonicSensor('P0')
dcm = DCMotor('M1')
dcm.power(100)

while True:
    if button_a.is_pressed():
        distance = us.get_distance()
        if distance > 0 and distance < 10: # The condition distance > 0 is added to exclude measurements of -0.03
            alarm()
        dcm.cw()
    else:  
        dcm.stop()
```

Now try running this program and see how it works!


### Programming Automatic Braking
Now let's make the robocar turn on the brakes to stop automatically if it detects an object within 5 cm ahead of it, even if you're holding the A button! Modify Example Code 4-2-2 as shown below!

##### Example Code 4-3-1
###### New and Changed Code (Lines 19-22)
```python=1
from pystubit.board import button_a, buzzer
from pyatcrobo2.parts import DCMotor, UltrasonicSensor
import time

def alarm():
    for i in range(3):
        buzzer.on('C7',duration=50)
        time.sleep_ms(50)

us = UltrasonicSensor('P0')
dcm = DCMotor('M1')
dcm.power(100)

while True:
    if button_a.is_pressed():
        distance = us.get_distance()
        if distance > 0 and distance < 10:
            alarm()
        if distance < 5:
            dcm.brake()
        else:
            dcm.cw()
    else:  
        dcm.stop()
```

Now your program is finished! Try running it to see how it works!

## Challenge: An Anti-Fall Robocar
Reattach your Ultrasonic Sensor to your robocar so it faces down, like this:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-2/4-2_11_E.png"/>


Try making a program based on Example Code 4-3-1 that makes the car stop driving if it's about to fall off the table!
★ For this program, make the car drive forward automatically instead of when you're pressing the A button.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-2/4-2_12_E.png"/>


### Example Program
If the Ultrasonic Sensor is attached to your robocar as shown above, it will be about 7 cm away from the table you set it on. The example program below uses this measurement, so it makes the car keep driving as long as the distance the sensor detects is less than 8 cm and stops it if the distance gets bigger.  However, not every sensor will get the same measurements, so please use Example Code 4-1-1 to measure the distance of the table from your own sensor and use that measurement to write the conditions in your program.

##### Example Code 5-1-1
```python=1
from pyatcrobo2.parts import DCMotor, UltrasonicSensor

us = UltrasonicSensor('P0')
dcm = DCMotor('M1')
dcm.power(100)

while True:
    distance = us.get_distance()
    if distance > 0 and distance < 8    # Pick a condition based on your own measurements!
        dcm.cw()
    else:
        dcm.brake()
```

## Conclusion
### A Quick Review
In this lesson we learned...

* About string formatting
* The quirks of Ultrasonic Sensors
* How to control a robot car with an Ultrasonic Sensor

Ultrasonic Sensors can be essential for building robots, so make sure you understand what they can and can't do!

### Coming Up Next Time!
In the next lesson, we'll build an electric guitar using an IR Photoreflector and a Color Sensor!