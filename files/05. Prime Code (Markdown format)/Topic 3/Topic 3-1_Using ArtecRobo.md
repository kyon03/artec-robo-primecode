---
tags: Python_English, Topic 3
---

Python Robotics Course Lesson 9

Topic 3-1
Using ArtecRobo
===
Learn about the parts you need to build robots, like motors and sensors!

## In This Lesson...
Use the Robot Expansion Unit from your robotics kit to learn how to use DC Motors, Servomotors, and three different kinds of sensor!


:::info
Lesson Introduction

■ Lesson 9 Contents

1. Introducing ArtecRobo
2. Using the Robot Expansion Unit
3. Using DC Motors
4. Using Servomotors
5. Using IR Photoreflectors
6. Using Ultrasonic Sensors
7. Using Color Sensors



In this lesson you'll learn how to use several different electronic robot parts.

First we'll give you a list of the ArtecRobo parts you'll be using in this course,

and then talk about how to use the Robot Expansion Unit that connects to your Core Unit. You'll be using this Unit whenever you use ArtecRobo's block-connectable motors and sensors.

Next we'll explain how each of the robot parts we mentioned works and how to program it. Learn about the classes that let you control these parts and what kind of methods they have!

You'll be using the parts you learn about in this lesson in a lot more lessons from now on. Come back to this lesson later on if you feel like you need to review how any of them work!

Now let's start the lesson!
:::

### Before You Start

Before you start this lesson, follow these steps to update your Studuino:bit Core Unit's **firmware**. (★ In its default state, the Core Unit can't use the Ultrasonic Sensor or the Color Sensor which appear in this lesson. Updating the firmware will make these parts usable.)

###### ★ Firmware ...The programs that come installed in the Core Unit.

1. Connect your Core Unit to the PC using a USB cable.
2. Start up the **Studuino_bit** software.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_add1_E.png"/>
3. Select **Robots modes** on the screen that appears.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_add2_E.png"/>
4. Open the **Help** menu from the Menu bar, then select **Firmware update**.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_add3_E.png"/>
5. The update should take about one minute. Click **OK**.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_add4_E.png"/>
6. When the update is finished, click **OK** and then press the Core Unit's Reset button.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_add5_E.png"/>

## Introducing ArtecRobo
###  What is ArtecRobo?
**ArtecRobo** is a set of robotics and programming study materials, including **ArtecBlocks** (building blocks you can connect horizontally, vertically, and diagonally however you want) and electronic robot parts you can connect with ArtecBlocks to build robots!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_1_E.png"/>

You can build all kinds of robots and models with these parts, including robot cars, robotic arms, game cabinets, and more!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_2_E.png"/>

The Core Unit, which you've been programming so far, acts as ArtecRobo's main computer. When you connect the Core Unit to the Robot Expansion Unit, you can use it to control connectable motors and sensors.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_3_E.png"/>

###### ★ Remove the Core Unit's cover and insert its metal edge connector terminal into the Robot Expansion Unit.

### ArtecRobo Parts for This Lesson
There are lots of different electronic parts available for ArtecRobo. In this course, we'll be using just these two kinds of motors and three kinds of sensors:

* DC Motors
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_4_E.png"/>
* Servomotors
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_5_E.png"/>
* IR Photoreflectors
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_6_E.png"/>
* Ultrasonic Sensors
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_7_E.png"/>
* Color Sensors
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_8_E.png"/>


We'll also be using these ArtecRobo cables to plug sensors into the Robot Expansion Unit:

* Sensor Connecting Cable (3-wire, 15 cm)
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_9_E.png"/>
* Sensor Connecting Cable (3-wire, 30 cm)
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_10_E.png"/>
* Sensor Connecting Cable (4-wire, 30 cm)
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_11_E.png"/>


You need to use 3-wire cables with IR Photoreflectors and Ultrasonic Sensors, and 4-wire cables with Color Sensors.

Now let's start writing Python programs that use these parts! Look through your box for one of each part and get them out for this lesson.

## Using the Robot Expansion Unit
###  Matching Parts and Ports
The outer edges of the Robot Expansion Unit contain a number of different ports. Each port has its name written next to it. Look at the table below to see which parts you can plug into which ports!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_12_E.png"/>

|Ports|Compatible Parts|
|:---|:---|
|P13, P14, P15, P16|Servomotors|
|M1, M2|DC Motors|
|P0, P1, P2|IR Photoreflectors, Ultrasonic Sensors|
|I2C|Color Sensors|

### 2 Powering the Robot Expansion Unit
You'll need to use batteries in order to power your Robot Expansion Unit. The Battery Box takes three AA/LR6 batteries.

###### ★ When you insert the batteries, make sure their + and - ends are facing the right way!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_13_E.png"/>

The Battery Box's cable plugs into the BATT port on the bottom of the Robot Expansion Unit.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_14_E.png"/>

To turn on the Robot Expansion Unit, **hold down** the button on its side labeled ON/OFF. The Core Unit's Power light will turn on when the power is on. To turn it off, hold down the button again.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_15_E.png"/>

If you're using only the Core Unit, you can plug the Battery Box into the BATT port on top of the Core Unit instead.

###### ★ The Core Unit doesn't have an on/off button. It will turn on as soon as you connect it to the Battery Box!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_16_E.png"/>

The Battery Box also has studs on it which you can use to connect it to the Robot Expansion Unit, making it easy to move around!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_17_E.png"/>

## Using DC Motors
### How DC Motors Work
DC Motors are often used to make the wheels on robot cars turn. The computer can control how fast these motors spin and what direction they spin in. The internal mechanisms of a DC Motor create rotating force (called torque) and use several connected gears to increase that torque to up to 200 times its original power!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_18_E
.jpg"/>

### Connecting DC Motors
DC Motors can be connected to either port M1 or port M2. Let's plug one into M1 now!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_19_E.png"/>

### Programming DC Motors
Let's practice by writing a program that makes a DC Motor spin clockwise for 5 seconds.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_20_E.png"/>

###### ★ If you place the DC Motor upside down, its direction will be reversed, so it spins counter-clockwise instead of clockwise, or vice versa.

#### ■ Importing the DCMotor Class
You can control DC Motors using methods from the **DCMotor** class, which is defined in the file `pyatcrobo2.parts`. Let's use a **from-import** statement to import the **DCMotor** class, then make an instance from it.

```python=1
from pyatcrobo2.parts import DCMotor

dcm = DCMotor('M1')
```

When you make an instance of the **DCMotor** class like this, you need to specify the name of the port your motor is connected to as a string in the instance's parameters. If your DC Motor were plugged into port M2 instead, you would have to write **'M2'** in this code instead of **'M1'**.

#### ■ Setting a DC Motor's Direction and Speed
To make a DC Motor spin, you need to set both the direction and how fast you want it to go. Let's start with the speed, which you can set with the method `power()`. Set a number from the range **0-255** in this method's parameter to pick a speed!

```
DCMotor.power(power)
・・・Sets a DC Motor's speed.
```

###### ★ This method is actually controlling not the motor's speed, but the strength of the electric current being sent to the motor. That's why it's called power!

Now to set the motor's direction, you can use the methods `cw()` and `ccw()`. `cw` is short for **clockwise**, and `ccw` is short for **counter-clockwise**, so naturally using **cw()** makes the motor spin clockwise and **ccw()** makes it spin counter-clockwise. These methods don't have any parameters to set!

```
DCMotor.cw()
- Makes a DC Motor spin clockwise.
```

```
DCMotor.ccw()
- Makes a DC Motor spin counter-clockwise.
```

This means if you want to make a DC Motor spin clockwise at speed 100, you can program it like this:

###### New Code (Lines 4-5)

```python=1
from pyatcrobo2.parts import DCMotor

dcm = DCMotor('M1')
dcm.power(100)
dcm.cw()
```

#### ■ Stopping a DC Motor
There are two different methods you can use to make a DC Motor stop: **brake()** and **stop()**. Using **brake()** puts pressure on the motor, making it stop immediately, while **stop()** lets the motor come to a stop gradually on its own. These methods don't have any parameters to set!

```
DCMotor.brake()
- Puts pressure on a DC Motor to make it stop immediately.
```

```
DCMotor.stop()
- Makes a DC Motor come to a gradual stop (without putting pressure on it).
```

To make the motor run for five seconds, we'll use the `sleep_ms()` method from the `time` module to put a gap of five seconds (5000 milliseconds) between starting and stopping the motor. "Milliseconds" is abbreviated as **ms**, which is where the name of the `sleep_ms()` method comes from. One millisecond is one 1/1000th of a second, which means 5000 ms is 5 seconds.

##### Example Code 4-3-1
###### New Code (Line 2, Lines 7-8)

```python=1
from pyatcrobo2.parts import DCMotor
import time

dcm = DCMotor('M1')
dcm.power(100)
dcm.cw()
time.sleep_ms(5000)
dcm.brake()
```

Now your program is finished! Try running it to see how it works!

### Changing Speed and Direction
Let's try editing Example Code 4-3-1 above to make the DC Motor **spin counter-clockwise for ten seconds** instead. Replace the **cw()** method with **ccw()** and change **time.sleep_ms()**'s parameter to **10000** ms, like this:

##### Example Code 4-4-1
```python=1
from pyatcrobo2.parts import DCMotor
import time

dcm = DCMotor('M1')
dcm.power(100)
dcm.ccw()
time.sleep_ms(10000)
dcm.brake()
```

## Using Servomotors
### How Servomotors Work
Servomotors are motors that rotate to specific positions based on the angle you tell them to turn to. Inside a Servomotor is a mechanism called a rotary encoder, which measures rotation. ArtecRobo Servomotors can turn to any angle from 0 to 180, moving precisely degree by degree to the position you program for them.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_21_E.png"/>

###### ★ Servomotors have axles on both sides, but only one side is connected to the motor. The side that's harder to turn by hand is the side connected to the internal motor!

### Connecting Servomotors
Servomotors can be connected to ports P13-P16. Let's plug one into P13 now!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_22_E.png"/>


### Programming Servomotors
Let's see how we can make a program that rotates a Servomotor from 90 degrees to 180 degrees. First start a new program!

#### ■ Importing the Servomotor Class
You can control Servomotors using methods from the **Servomotor** class, which is defined in the file `pyatcrobo2.parts`. Let's use a **from-import** statement to import the **Servomotor** class, then make an instance from it. Just like with the **DCMotor** class, when you make an instance of the **Servomotor** class, you need to specify the name of the port your motor is connected to as a string in the instance's parameters.

```python=1
from pyatcrobo2.parts import Servomotor

servo = Servomotor('P13')
```

#### ■ Setting Servomotor Angles
To set what position you want a Servomotor to rotate to, you need to use the method `set_angle()`. You can set an angle in degrees (°) in this method's parameter.

```
Servomotor.set_angle(degree)
- Rotates one Servomotor to the specified angle.
```

Your Servomotor will start moving as soon as you run the `set_angle()` method. The computer won't wait for the Servomotor to finish turning to the angle you specified before it gives the next order in this program, so if you want to make the motor turn to several different angles, you'll need to insert pauses between the **set_angle()** methods in your program. In the example below we've done this using the code `time.sleep_ms(1000)` to insert one second pauses.

###### New Code (Line 2, Lines 5-7)

##### Example Code 5-3-1

```python=1
from pyatcrobo2.parts import Servomotor
import time

servo = Servomotor('P13')
servo.set_angle(0)
time.sleep_ms(1000)
servo.set_angle(180)
```

### 5. From 0°to 180° and Back
If you add a **for** statement using the built-in function `range()` to Example Code 5-3-1, you can program a Servomotor to swing back-and-forth between 0° and 180° five times, like a pendulum!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_23_E.gif"/>

In addition to putting the program into a **for** loop, you'll need to add another **time.sleep_ms(1000)** at the end of the code to make sure the Servomotor pauses between rotations.

##### Example Code 5-4-1
```python=1
from pyatcrobo2.parts import Servomotor
import time

servo = Servomotor('P13')
for i in range(5):
    servo.set_angle(0)
    time.sleep_ms(1000)
    servo.set_angle(180)
    time.sleep_ms(1000)   
```

## Using IR Photoreflectors
### How IR Photoreflectors Work
works by detecting reflected infrared (IR) light from objects in front of them. An IR Photoreflector contains an emitter, which sends out infrared light, and a detector, which picks up the light that bounces off of nearby objects. Then the sensor measures how strong the reflected light it receives back is.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_24_E.png"/>

### Connecting IR Photoreflectors
IR Photoreflectors can be connected to ports P0-P2. Let's use a Sensor Connecting Cable (3-wire, 15 cm) to plug one into P0 now!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_25_E.png"/>

### Programming IR Photoreflectors
Let's make a program that looks up the values your IR Photoreflector is detecting and see how these sensors work! First we'll need to start a new program!

#### ■ Importing the IRPhotoReflector Class
You can control IR Photoreflectors using methods from the **IRPhotoReflector** class, which is defined in the file `pyatcrobo2.parts`. Let's use an **import** statement to import the **IRPhotoReflector** class, then make an instance from it. When you make an instance of this class, you need to specify the name of the port your sensor is connected to as a string in the instance's parameters.

```python=1
from pyatcrobo2.parts import IRPhotoReflector

ir = IRPhotoReflector('P0')
```

#### ■ Looking Up Sensor Values
Just like the light levels detected by the Core Unit's Light Sensor, the strength of the infrared light your IR Photoreflector picks up will appear as a number on your computer. To find this number, use the method `get_value()`. This method doesn't have any parameters to set!

```
IRPhotoReflector.get_value()
- Finds the current value (0-4095) of an IR Photoreflector.
```

Add the code below to your program, then run it to see how it works!

##### Example Code 6-3-1
###### New Code (Line 2, Lines 5-8)

```python=1
from pyatcrobo2.parts import IRPhotoReflector
import time

ir = IRPhotoReflector('P0')
while True:
    value = ir.get_value()
    print(value)
    time.sleep_ms(500)
```

Make the program wait a little after each time it uses **print()** to display the sensor value in the terminal. If you don't, the program will create too much work for the software to do at once. This can crash the software, so be careful!

Now let's see what numbers appear in the terminal both when there's nothing in front of the IR Photoreflector, and when you cover it with your hand.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_26_E.png"/>

##### Example Display Values

<pre class="prettyprint">
59
58
54
54
1106
1384
1378
1374
</pre>

As you can see, moving your hand close to the IR Photoreflector makes the value get bigger! The IR Photoreflector's values get higher the stronger the infrared light reflected back to it is. This means that the closer an object is in front of the sensor, the higher its values get.

However, pointing an IR Photoreflector into direct sunlight will also make its values get bigger because sunlight contains infrared rays as well! Make sure your IR Photoreflector isn't in direct sunlight when you're using it or this may cause problems.

## Using Ultrasonic Sensors
### How Ultrasonic Sensors Work
Ultrasonic Sensors are a kind of sensor that uses ultrasonic (high-frequency sound) waves to detect how far away objects in front of them are. An Ultrasonic Sensor sends out an ultrasonic wave and measures the length of time (**T**) between sending that wave and the reflection of that wave coming back to the sensor. Then it takes that time and the speed of the wave (**V**) and uses the equation below to calculate how far away (**L**) the object the wave bounced off of is.

**L = T×V÷2**

###### ★ T measures how long it takes an ultrasonic wave to travel to an object and back to the sensor again. Multiplying T x V finds the distance the wave traveled going both ways, which is why the equation divides by 2 to find the distance.
###### ★ Ultrasonic waves move through the air at a speed of around 340 m/s.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_27_E.png"/>

### Connecting Ultrasonic Sensors
Ultrasonic Sensors can be connected to either port P0 or port P1. Let's use a Sensor Connecting Cable (3-wire, 30 cm) to plug one into P1 now!

###### ★ Ultrasonic Sensors plugged into port P2 will not work!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_28_E.png"/>

### Programming Ultrasonic Sensors
Let's practice using Ultrasonic Sensors by making a program that detects how far away an object in front of the sensor is when you press the A button. First start a new program!

#### ■ Importing the UltrasonicSensor Class
You can control Ultrasonic Sensors using methods from the **UltrasonicSensor** class, which is defined in the file `pyatcrobo2.parts`. Let's use a **from-import** statement to import the **UltrasonicSensor** class, then make an instance from it.

```python=1
from pyatcrobo2.parts import UltrasonicSensor

us = UltrasonicSensor('P1')
```

#### ■ Looking Up Distances
To find the distance of an object in front of an Ultrasonic Sensor, use the method `get_distance()`. This method's return value will be a measurement in centimeters (cm), and it doesn't have any parameters for you to set. ArtecRobo Ultrasonic Sensors can detect objects up to roughly 300 cm (3 meters) away!

```
UltrasonicSensor.get_distance()
- Finds the distance between an Ultrasonic Sensor and an object in front of it in centimeters.
```

Add the code below to your program, then run it to see how it works. Try placing your hand at different distances in front of the sensor and see if the computer finds the right distance!

##### Example Code 7-3-1
###### New Code (Line 2, Lines 5-8)

```python=1
from pyatcrobo2.parts import UltrasonicSensor
import time
 
us = UltrasonicSensor('P1')
while True:
    distance = us.get_distance()
    print(distance)
    time.sleep_ms(1000)
```

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_29_E.png"/>

##### Example Display Values

<pre class="prettyprint">
1.96
7.77
14.1
</pre>

## Using Color Sensors
### How Color Sensors Work
Color sensors detect the composition and brightness of colors by emitting light from three colored LEDs and analyzing the levels of red, green, and blue (RGB) light that reflect back to the sensor. Color Sensors contain emitters and detectors, much like IR Photoreflectors, but Color Sensors have three emitters: one red, one blue, and one green. All of them are LEDs.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_30_E.png"/>

Each of these three LEDs lights up very quickly to send out colored light, and then the detector measures the strength of the light that reflects back and the levels of red, green, and blue in that light.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_31_E.png"/>

### Connecting Color Sensors
Color Sensors can be connected to the I2C port. Let's use a Sensor Connecting Cable (4-wire, 30 cm) to plug one in now!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_32_E.png"/>

###### ★ I2C stands for Inter-Integrated Circuit, which is a communication system that lets microcontrollers and devices quickly send data to each other. Color Sensors use I2C because they need to send three separate data values (for red, green, and blue light levels) at once.

### Programming Color Sensors
Let's practice using Color Sensors by making a program that detects the color of an object in front of the sensor when you press the A button. First start a new program!

#### ■ Importing the ColorSensor Class
You can control Color Sensors using methods from the **ColorSensor** class, which is defined in the file `pyatcrobo2.parts` just like the others. Let's use a **from-import** statement to import the **ColorSensor** class, then make an instance from it. When you make an instance of this class, you need to specify the sensor's port using the string `'I2C'` in the instance's parameters.

```python=1
from pyatcrobo2.parts import ColorSensor

cs = ColorSensor('I2C')
```

#### ■ Looking Up Color Values
To find the brightness and composition of color using a Color Sensor, use the method `get_values()`. This method doesn't have any parameters to set! The return value for this method is a tuple containing values for the levels of each of the three primary colors (RGB) and for the color's overall brightness. That's four values total (which is why it's called `get_values`, not get_value).

```
ColorSensor.get_values()
- Detects the brightness and RGB composition of the color of an object in front of a Color Sensor. The return value will be in (R,G,B, Brightness) format.
```

Now add the code below to your program so it collects data from the Color Sensor whenever you press the A button.

##### Example Code 8-3-1
###### New Code (Line 2, Line 3, Lines 6-10)

```python=1
from pyatcrobo2.parts import ColorSensor
from pystubit.board import button_a
import time

cs = ColorSensor('I2C')
while True:
    if button_a.is_pressed():
        values = cs.get_values()
        print(values)
        time.sleep_ms(1000)
```

Now get four colored blocks (red, green, blue, and yellow) out of your kit. Transfer your program to the Core Unit, then try placing each colored block in front of the Color Sensor and press the A button to find the block's color values!

* Example Display Values (Red Block)
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_33_E.png"/>
<pre class="prettyprint">
[127, 50, 45, 38]
</pre>

* Example Display Values (Green Block)
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_34_E.png"/>
<pre class="prettyprint">
[57, 96, 57, 24]
</pre>

* Example Display Values (Blue Block)
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_35_E.png"/>
<pre class="prettyprint">
[59, 41, 79, 22]
</pre>

* Example Display Values (Yellow Block)
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_36_E.png"/>
<pre class="prettyprint">
[59, 42, 17, 44]
</pre>

You can see the R value is biggest with the red block, G is biggest with the green block, and B is biggest with the blue block. Yellow light contains both red and green, so with the yellow block, the R and G values should be close to each other.

##### The Primary Colors of Light

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-1/3-1_37_E.png"/>


## Conclusion

### A Quick Review
In this lesson, you learned how the following ArtecRobo parts work and how to write code that uses them...

* DC Motors
* Servomotors
* IR Photoreflectors
* Ultrasonic Sensors
* Color Sensors

In future lessons we'll be using these parts along with the Core Unit's built-in parts (the LED display, Buzzer, buttons, etc.)  in all sorts of programming challenges. If you're working on a project later on and don't remember how to use a particular part, you can come back to this lesson to look it up!

###  Coming Up Next Time!
In the next lesson, you'll learn how to use two of the Core Unit's built-in sensors: the **Accelerometer** and the **Gyroscope**.