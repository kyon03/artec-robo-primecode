---
tags: Python_English, Topic 3
---

Python Robotics Course Lesson 10

Topic 3-2
Motion Sensors
===
Learn how to use the Core Unit's Accelerometer and Gyroscope!

## In This Lesson...
Learn how to use two of the Core Unit's built-in sensors: the Accelerometer and the Gyroscope!

<iframe src="https://player.vimeo.com/video/484346697" width="640" height="360" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

:::info

Lesson Introduction

■ Lesson 10 Contents

1. Using an Accelerometer
2. Making a Digital Spirit Level
3. Using a Gyroscope
4. Making an Electric Glowstick
5. Challenge: More Glowstick Colors



In this lesson you'll learn how to use Accelerometers and Gyroscopes, two kinds of sensors that come built into your Core Unit.

We'll start by discussing how Accelerometers work in detail. An Accelerometer is a kind of sensor that detects how fast an object is speeding up. Use this sensor and the way it interacts with the force of gravity to build a digital spirit level that detects whether the surface you set it on is horizontal relative to the ground!

This spirit level moves the LED around the display when you tilt it!

Next you'll learn how to use a Gyroscope. A Gyroscope is a kind of sensor that detects how fast an object is rotating. Use a Gyroscope to make an electric glowstick that lights up when you wave it! You can make it turn different colors when you wave it in different directions!

At the end of the lesson, you can use what you've learned to make your electric glowstick turn more colors!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!
:::

## Using an Accelerometer

An Accelerometer is a kind of sensor that detects **acceleration**, but what does that mean? Let's say you're measuring the speed of a car. You'd describe the car's speed using a unit of measure like meters per second (**m/s**) or kilometers per hour (**km/h**), right? These units describe speed based on how far the car moves in a set amount of time (like one second or one hour).


The way these units are written actually describes an equation for calculating speed, like this:

**Speed (m/s) ＝ Distance (meters) ÷  Time (seconds)**

##### Example Speed Calculation: If a car drives 300 m in 20 seconds, how fast is it moving?
Car's speed ＝ 300(m) / 20(s) = 15(m/s)

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_1_E.png"/>

Units of acceleration are written like this: **m/s^2^ (meters per second per second)**.
######  The equation x^2^ is an exponent, and it means "multiply x by x." The number 2 tells you how many copies of x to multiply together, so for example, x^4^ would mean "multiply x by x by x by x." The exponent 2^4^ then means 2 x 2 x 2 x 2 = 16.

These units measure how much an object's speed (m/s) has changed in one second, as calculated with this equation:

**Acceleration (m/s^2^) ＝ Change in speed (m/s) ÷ Time it took to change (s)**

##### Example Acceleration Calculation: If a car speeds up from 10 m/s to 20 m/s in 5 seconds, how fast is it accelerating?

Car's acceleration ＝ ( 20(m/s) - 10(m/s) ) ÷ 5(s) = 2(m/s<sup>2</sup>)

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_2_E.png"/>

Looking at the units of measurement in this equation lets you break the equation down further, like this:

m/s ÷ s = m ÷ s ÷ s = m ÷ ( s × s )　＝　m/s<sup>2</sup>

With this new equation, you can see why acceleration is measured in units of **m/s^2^**!

### Measuring Acceleration with an Accelerometer
Let's write a new program to practice using an Accelerometer to measure acceleration.

#### ■ Writing the Program
The predefined class that lets us control the Accelerometer is called **StuduinoBitAccelerometer**. Let's import this class from the file `pystubit.sensor`, then make an instance from it called `accelerometer`.

```python=1
from pystubit.sensor import StuduinoBitAccelerometer

accelerometer = StuduinoBitAccelerometer()
```

To collect data from the Accelerometer, you need to use the method `get_values()`.

```
StuduinoBitAccelerometer.get_values()
-Retrieves values from the Accelerometer.
```

Add the following lines of code to run the **get_values()** method once every second, then display its results in the terminal using the `print()` function.


##### Example Code 2-1-1
###### New Code (Line 2, Lines 5-8)
```python=1
from pystubit.sensor import StuduinoBitAccelerometer
import time

accelerometer = StuduinoBitAccelerometer()
while True:
    values = accelerometer.get_values()
    print(values)
    time.sleep_ms(200)
```

#### ■ Testing Your Measurements
Remove the Core Unit's case and set it on a flat surface with the LED display facing up, as shown below. Then run Example Code 2-1-1 and see what results show up in your terminal!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_3_E.JPG"/>

##### Example Terminal Results

<pre class="prettyprint">
(-0.05, -0.22, -10.16)
(-0.03, -0.36, -10.03)
(-0.11, -0.33, -10.11)
(-0.09, -0.38, -10.09)
(-0.02, -0.31, -10.14)
(-0.02, -0.24, -10.05)
(-0.07, -0.33, -9.97)
</pre>

The data `get_values()` finds will appear as tuples, made up of these three pieces of information:

```
(X-Axis Acceleration, Y-Axis Acceleration, Z-Axis Acceleration)
```

The Accelerometer detects acceleration along three different axes (X, Y, and Z) and displays the acceleration for each one as a separate value in the tuple, in units of m/s^2^.

##### Accelerometers and Gravity

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_4_E.png"/>

Acceleration values can be positive or negative depending on whether the motion the Accelerometer detects is moving toward the positive (+) or negative (-) end of a particular axis.

#### ■ Accelerometers and Gravity

We've explained that acceleration is a measure of how fast something is changing speeds, but you may have noticed that even when your Core Unit is sitting still on a table, it detects some acceleration on the Z-axis. This is actually caused by the gravitational pull of the Earth! Your Accelerometer just isn't built to tell the difference between this gravitational pull and other acceleration.

When you drop an object from high up, the object will speed up as it falls, moving faster and faster. Things fall this way because of the Earth's gravitational pull, and the acceleration that happens when something falls this way is called **gravitational acceleration**. The gravitational acceleration of an object falling toward the ground is always roughly 9.8m/s<sup>2</sup> (moving 9.8 m/s faster every second), no matter the size and shape of the falling object!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_5_E.png"/>

Try picking up your Core Unit as shown and then dropping it into your other hand, so it falls (moving along the positive Z axis), and see what values the Accelerometer gives you then.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_6_E.png"/>

##### Example Terminal Results
<pre class="prettyprint">
(-2.28, -0.84, -9.95)
(-2.01, -0.55, -11.02)
(-2.38, -0.12, -10.23)
(-4.06, -1.43, -0.61)
</pre>

Line 4 of these results shows the values when the Core Unit was falling. The Core Unit is actually accelerating according to gravitational acceleration (approx. 9.8 m/s<sup>2</sup>) in the positive direction on the Z-axis, but because the force of gravity made its default acceleration negative (around -10 m/s<sup>2</sup>), those two measurements cancel each other out, making the acceleration you detect close to 0.

These results show us that the Core Unit's built-in Accelerometer can't measure gravitational acceleration. The reason for this is because of how the Accelerometer is constructed, and the full explanation for it is a bit too complicated for us to explain here. If you're interested in learning more about it, try looking up "how accelerometers work" in an online search engine. You should be able to find a number of webpages with helpful information! 

#### ■ The Acceleration of a Shaken Sensor
Shaking your Core Unit makes its speed change a lot! Try shaking the Core Unit in each axis direction and see what values the Accelerometer detects.

* Shake in the positive direction on the X-axis

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_7_E.png"/>

##### Values Right After Shaking
<pre class="prettyprint">
(16.32, -0.36, -9.04)
</pre>

* Shake in the negative direction on the X-axis

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_8_E.png"/>

##### Values Right After Shaking
<pre class="prettyprint">
(-13.35, -1.8, -7.09)
</pre>

* Shake in the positive direction on the Y-axis

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_9_E.png"/>

##### Values Right After Shaking
<pre class="prettyprint">
(1.37, 15.97, -7.96)
</pre>

* Shake in the negative direction on the Y-axis

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_10_E.png"/>

##### Values Right After Shaking
<pre class="prettyprint">
(-0.44, -16.88, -9.87)
</pre>

* Shake in the positive direction on the Z-axis

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_11_E.png"/>

##### Values Right After Shaking
<pre class="prettyprint">
(-2.14, 0.7, 17.71)
</pre>

* Shake in the negative direction on the Z-axis

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_12_E.png"/>

##### Values Right After Shaking
<pre class="prettyprint">
(19.61, -7.78, -19.61)
</pre>

Using its default settings, the Accelerometer measures acceleration from -19.6m/s^2^ to 9.6m/s<sup>2</sup>, so any acceleration that goes past either end of that range will just show up as -19.6 or 19.6.

### Tilt and Accelerometer Values
The Accelerometer can't separately measure acceleration caused by the pull of gravity, but you CAN use the way gravity affects the Accelerometer's values when it's not moving to detect if the Core Unit is tilted in any particular direction! Pick up the Core Unit in your hand and try tilting it in the directions shown below (holding it still in each direction) and see what values the Accelerometer detects!

* Tilted Left

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_13_E.png"/>

##### Example Values
<pre class="prettyprint">
(-7.43, -0.38, -6.3)
</pre>

*  Tilted Right

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_14_E.png"/>

##### Example Values
<pre class="prettyprint">
(6.35, -0.11, -7.04)
</pre>

* Tilted Away from You

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_15_E.png"/>

##### Example Values
<pre class="prettyprint">
(-0.59, -7.62, -6.78)
</pre>

* Tilted Toward You

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_16_E.png"/>

##### Example Values
<pre class="prettyprint">
(-2.24, 5.92, -8.11)
</pre>

Tilting the unit to the right or left makes the Accelerometer's X-axis value change a lot, while tilting it toward or away from you makes the Y-axis value change a lot. That means you can use the Accelerometer's X-axis values to tell if the Core Unit is tilted right or left, and the Y-axis values to tell if it's tilted toward or away from you. The Z-axis's values don't change much no matter which way the unit tilts, so you can ignore the Z-axis in the tilt detection methods we're going to start using now.

##### How to Detect Tilt (Examples)
-If the X-axis value is **less than -5** ➡ Unit is tilting **left**
-If the X-axis value is **less than 5** ➡ Unit is tilting **right**
-If the Y-axis value is **less than 5** ➡ Unit is tilting **away from you**
-If the Y-axis value is **less than -5** ➡ Unit is tilting **toward you**

### Making a Digital Spirit Level
A spirit level is a tool you can set on a surface to tell if that surface is tilted vertically or horizontally. The simplest spirit levels are sealed glass tubes, filled with liquid with an air bubble in it. The glass tube then has two measuring lines drawn on it, and if the air bubble is between the two lines, that means the surface it's resting on is horizontal.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_17_E.png"/>

We can make the Accelerometer and the LED display into a digital spirit level by programming different LEDs in the display to light up depending on how the Core Unit is tilted!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_18_E.png"/>

#### ■ Writing the Program
Let's take Example Code 2-1-1 and modify it to make a digital spirit level program! First let's make the program look up the Accelerometer's values every 100 milliseconds and save the value of the X-axis in a variable called **x**.

###### Changed Code (Lines 7-8)

```python=1
from pystubit.sensor import StuduinoBitAccelerometer
import time

accelerometer = StuduinoBitAccelerometer()
while True:
    values = accelerometer.get_values()
    x = values [0] #[0] retrieves the X value from the tuple (X-Value, Y-Value, Z-Value)
    time.sleep_ms(100)
```

Next we need to import the StuduinoBitImage class (under its shortened name Image) and the StuduinoBitDisplay class instance called display from `pystubit.board` so we can control the LED `display` in this program. After importing those, we need to make an `image` instance that turns on all the LEDs in the display.

###### New Code (Line 2, Lines 10-13)

```python=1
from pystubit.sensor import StuduinoBitAccelerometer
from pystubit.board import display
import time

accelerometer = StuduinoBitAccelerometer()
while True:
    values = accelerometer.get_values()
    x = values[0]
    
    if x < -5:
        display.set_pixel(0, 2, (31, 0, 0))
    elif x > 5:
        display.set_pixel(4, 2, (31, 0, 0))
        
    time.sleep_ms(100)
```

Finally, make it so the LED in the middle of the display lights up if the unit isn't tilting either way. Use the `clear()` method to reset the display between measurements, too. Now try running this program and see how it works!

##### Example Code 2-2-1
###### New Code (Lines 14-15, Line 18)
```python=1
from pystubit.sensor import StuduinoBitAccelerometer
from pystubit.board import display
import time

ac = StuduinoBitAccelerometer()
while True:
    values = ac.get_values()
    x = values[0]
    
    if x < -5:
        display.set_pixel(0, 2, (15, 0, 0))
    elif x > 5:
        display.set_pixel(4, 2, (15, 0, 0))
    else:
        display.set_pixel(2, 2, (15, 0, 0))
        
    time.sleep_ms(100)
    display.clear()   
```

## Using a Gyroscope
Gyroscopes are sensors that can detect the speed of rotating objects. They're best known for their role in navigation systems for boats and aircraft! There are a number of different ways to measure rotational speed, for example by how many full rotations an object makes in a set amount of time, or by how many degrees (as in a plane angle) an object turns in a set amount of time.

##### Here are a couple devices whose performance is measured in rotational speed...

*  Blenders
The blades can make 12,000 full rotations in 1 minute.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_19_E.png"/>

* Servomotors
The axle can rotate 180 degrees in 1 second.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_20_E.png"/>

The Gyroscope we'll be using measures rotational speed in degrees. This kind of rotational speed is called **angular velocity**!

While other kinds of speed are measured by distance in meters or kilometers traveled per unit of time elapsed, angular velocity is measured by distance traveled in degrees (°) per unit of time elapsed.


**Angular velocity = Size of rotated angle ÷ Elapsed time**

That means the units of measurement for angular velocity are **degrees per second (deg/s)**.

### Measuring Angular Velocity with a Gyroscope
Let's write a new program to practice using a Gyroscope to measure angular velocity.

#### ■ Writing the Program
The predefined class that lets us control the Gyroscope is called **StuduinoBitGyro**. Let's import this class from the file `pystubit.sensor`, then make an instance from it called `gyro`.

```python=1
from pystubit.sensor import StuduinoBitGyro

gyro = StuduinoBitGyro()
```

To collect data from the Gyroscope, you need to use the method `get_values()`.

```
StuduinoBitGyro.get_values()
・・・Retrieves angular velocity values for each of the Gyroscope's three axes as an (X-Value, Y-Value, Z-Value) tuple.
```

Add the following lines of code to run the **get_values()** method once every 0.2 seconds (200 milliseconds), then display its results in the terminal using **print()**.

##### Example Code 3-1-1
###### New Code (Line 2, Lines 5-8)
```python=1
from pystubit.sensor import StuduinoBitGyro
import time

gyro = StuduinoBitGyro()
while True:
    values = gyro.get_values()
    print(values)
    time.sleep_ms(200)
```

The picture below shows the positions of the Gyroscope's three axes, and which directions show positive or negative movement. The X, Y, and Z axes of a Gyroscope mark centers of rotation. Positive rotation around any axis means clockwise rotation with that axis as its center. Counter-clockwise rotation around an axis is negative rotation for that axis.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_21_E.png"/>

#### ■ Gyroscope Values and Rotational Direction
Transfer the program written above and run it, then try rotating your Core Unit in different directions around each of its axes and see what values show up in your terminal.

* Rotating Clockwise Around the X-Axis
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_22_E.png"/>

##### Values Right After Rotating
```
(250.12, 12.83, -14.31)
```

* Rotating Counter-Clockwise Around the X-Axis
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_23_E.png"/>

##### Values Right After Rotating
```
(-250.13, 17.16, -3.01)
```

* Rotating Clockwise Around the Y-Axis
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_24_E.png"/>

##### Values Right After Rotating
```
(-20.9, 250.12, -12.97)
```

* Rotating Counter-Clockwise Around the Y-Axis
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_25_E.png"/>

##### Values Right After Rotating
```
(30.21, -250.13, -38.01)
```

* Rotating Clockwise Around the Z-Axis
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_26_E.png"/>

##### Values Right After Rotating
```
(-5.07, -4.91, 250.12)
```

* Rotating Counter-Clockwise Around the Z-Axis
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_27_E.png"/>

##### Values Right After Rotating
```
(1.17, -4.29, -250.13)
```

Using its default settings, the Gyroscope measures angular velocity from -250 deg/s to 250 deg/s, so any angular velocity that goes past either end of that range will just show up as -250 deg/s or 250 deg/s.

### Making a Multi-Color Electric Glowstick
Have you ever seen people at a live concert waving lights in the air along with the rhythm of the song the band is playing? Audiences around the world participate in shows this way by waving lighters, their phones, or even special concert glowsticks!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_28_E.png"/>

Let's make our own electric glowstick by using the Gyroscope to make the LED display turn different colors when you wave the Core Unit in different directions! In the photos below, the display lights up blue when you wave the unit to the left, green when you wave it to the right, and white when you're not waving at all.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material/theme_3-2/3-2_29.png"/>

#### ■ Writing the Program
We'll use the angular velocity on the Z-axis to detect which way the Core Unit is being waved. To program this, let's take Example Code 3-1-1 and modify it so that it checks the Z-axis's angular velocity every 100 milliseconds and saves its value to a variable called **z**.

###### Changed Code (Lines 7-8)
```python=1
from pystubit.sensor import StuduinoBitGyro
import time

gyro = StuduinoBitGyro()
while True:
    values = gyro.get_values()
    z = values[2]
    time.sleep_ms(100)
```

Next we need to import the **StuduinoBitImage** class (under its shortened name `Image`) and the **StuduinoBitDisplay** class instance called `display` from `pystubit.board` so we can control the LED display in this program. After importing those, we need to make an image instance that turns on all the LEDs in the display.


###### New Code (Line 2, Line 6)

```python=1
from pystubit.sensor import StuduinoBitGyro
from pystubit.board import display, Image
import time

gyro = StuduinoBitGyro()
image = Image('11111:11111:11111:11111:11111:') 

while True:
    values = gyro.get_values()
    z = values[2]
    time.sleep_ms(100)
```

Now let's make the LEDs light up blue when the Core Unit is waved to the left, green when waved to the right, and white when not waved. Use conditions like `z < -200` and `z > 200` to make the program branch into  different processes depending on the angular velocity it detects around the Z-axis. Then use the `show()` method from the **StuduinoBitDisplay** class to make the LED display light up in different colors for each direction!

##### Example Code 3-2-1
###### New Code (Lines 10-15)
```python=1
from pystubit.sensor import StuduinoBitGyro
from pystubit.board import display, Image
import time

gyro = StuduinoBitGyro()
image = Image('11111:11111:11111:11111:11111:') 
while True:
    values = gyro.get_values()
    z = values[2]
    if z < -200:
        display.show(image, delay=100, color=(0, 0, 31)) #Wave left: Blue
    elif z > 200:
        display.show(image, delay=100, color=(0, 31, 0)) #Wave toward you: Red
    else:
        display.show(image, delay=100, color=(31, 31, 31)) #No wave: White
        
    time.sleep_ms(100) 
```

You can use the method `show()` to set the color for an image on the LED display by including a statement in the format "parameter name = value" (like `color=(0, 0, 31)` in this code) in the method's parameters when you run it. Parameters that you set up in this format are called **keyword parameters**! We'll explain them in detail in Topic 3-4, but for now just copy the one in the example code as-is.

Now your program is finished! Try running it to see how it works!

## Challenge: More Glowstick Colors

Try modifying Example Code 3-2-1 to make your electric glowstick change colors when you wave the Core Unit toward and away from you, too! Make it light up yellow when you wave it away and red when you wave it toward you.

###### ★ Hint: Use the angular velocity around the X-axis to judge whether the Core Unit is being waved toward or away from you!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-2/3-2_30_E.png"/>

### Making the Program
We'll use the angular velocity on the X-axis to detect which way the Core Unit is being waved. Add two **elif** statements to the program using the conditions `x <-200` (to judge if the Core Unit is waved toward you) and `x > 200` (to judge if the Core Unit is waved away from you), and add code branches to make the LED display turn red or yellow for each condition.

##### Example Code 4-1-1
###### New and Changed Code (Line 9, Lines 15-18)
```python=1
from pystubit.sensor import StuduinoBitGyro
from pystubit.board import display, Image
import time

gyro = StuduinoBitGyro()
image = Image('11111:11111:11111:11111:11111:') 
while True:
    values = gyro.get_values()
    x = values[0]
    z = values[2]
    if z < -200:
        display.show(image, delay=100, color=(0, 0, 31))  # Wave left: Blue
    elif z > 200:
        display.show(image, delay=100, color=(0, 31, 0))  # Wave right: Green
    elif x < -200:
        display.show(image, delay=100, color=(31, 0, 0))  # Wave toward you: Red
    elif x > 200:
        display.show(image, delay=100, color=(31, 31, 0))  # Wave away from you: Yellow
    else:
        display.show(image, delay=100, color=(31, 31, 31))  # No wave: White
        
    time.sleep_ms(100)  
```

## Conclusion
### A Quick Review
In this lesson you learned how to use Accelerometers and Gyroscopes, two kinds of sensors that come built into your Core Unit. It might be difficult at first to understand how acceleration, angular velocity, and the directions of the X, Y, and Z axes work, but it's very important to learn all of those concepts if you want to know how to use Accelerometers and Gyroscopes. If you feel like your understanding of these concepts isn't clear yet, go over this lesson again until you've got it all down!

### Coming Up Next Time!
In the next lesson you'll learn about special calculations you can make using methods from the StuduinoBitImage class, and more advanced things you can do with the LED display! 