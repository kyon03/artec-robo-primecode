---
tags: Python_English, Topic 4
---

Python Robotics Course  Lesson 15

Topic 4-3
A Color-Sensing Electronic Guitar
===

Use a Color Sensor and an IR Photoreflector to build a musical instrument!

## In This Lesson...
Make an electronic guitar by controlling the Buzzer with two different sensors: an IR Photoreflector and a Color Sensor!

<iframe src="https://player.vimeo.com/video/484347769" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

:::info
Lesson Introduction

■ Lesson 15 Contents

1. Building an Electronic Guitar
2. Challenge: Tilting Guitar



In this lesson you'll make an electronic guitar by controlling the Buzzer with an IR Photoreflector and a Color Sensor!

Use an IR Photoreflector and a slider to pick the note you want to play, and make the Buzzer play that note when you wave a blue block in front of the Color Sensor, like strumming a guitar with a pick!

With practice, you can use your guitar to play songs!

At the end of the lesson, try using the Core Unit's Accelerometer to add a new feature letting you set how long your guitar plays notes by tilting the guitar up and down!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!
:::

## New Python Syntax
You won't need to use any new Python syntax for this project!

## Building a Guitar
Get a copy of the builder's guide for this lesson and follow the steps inside to build an electronic guitar!

### Parts You'll Need
* Core Unit x 1
* Robot Expansion Unit x 1
* Battery Box x 1
* IR Photoreflector x 1
* Color Sensor x 1
* Sensor Connecting Cable (3-wire, 15 cm) x 1
* Sensor Connecting Cable (4-wire, 30 cm) x 1
* Basic Cube (White) x 2
* Basic Cube (Black) x 2
* Basic Cube (Gray) x 4
* Basic Cube (Red) x 4
* Half C (White) x 4
* Half D (White) x 5
* Beam x 3

##### The Artec Block Shapes
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_1_E.png"/>

### Building Instructions
[Building a Guitar](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_build_E.pdf)


## Programming Your Electronic Guitar
Your goal in this project is to build an electronic guitar that does the following:

1. Plays different notes based on the position of the slider
2. Plays the note set by 1 when you "strum" the guitar with a colored block pick

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_2_E.png"/>

Let's go through and program each of these features until we have a complete electronic guitar program!

### Programming the Slider
Let's start by writing the part of the program that picks what note the Buzzer should play based on the position of the slider.

#### ■ Detecting the Slider's Position

We'll use an IR Photoreflector to detect the position of the slider. In Topic 1-3 you learned about how IR Photoreflectors detect the strength of reflected infrared light, but you can also use the values the sensor picks up from objects directly in front of it to judge just how far away those objects are!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_3_E.png"/>

###### ★ Unlike Ultrasonic Sensors, using the class methods for IR Photoreflectors won't directly give you a distance measurement in centimeters!

Try running the code below to see how setting the slider at different positions affects your IR Photoreflector's values. Be sure to write down the values you find, you'll need to use them later to write your program!
```python=1
from pyatcrobo2.parts import IRPhotoReflector
import time

ir = IRPhotoReflector('P0')

while True:
    print(ir.get_value())
    time.sleep_ms(1000)
```

Let's define the slider's positions by dividing the neck of your electronic guitar into seven segments, and then assign a note to each segment. Move your slider to the points marking the boundaries between all the segments and see what values your IR Photoreflector finds for each one.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_4_E.png"/>

##### Line Between Segment 1 and Segment 2 Value
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_5_E.png"/>

##### Line Between Segment 2 and Segment 3 Value
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_6_E.png"/>

##### Line Between Segment 3 and Segment 4 Value
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_7_E.png"/>

##### Line Between Segment 4 and Segment 5 Value
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_8_E.png"/>

##### Line Between Segment 5 and Segment 6 Value
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_9_E.png"/>

##### Line Between Segment 6 and Segment 7 Value
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_10_E.png"/>


#### ■ Pick Notes Based on Slider Position
With the values you found above, you can write conditions that assign different notes to each segment the slider can be in. Let's define a new function called `get_tone(value)` that takes the IR Photoreflector's value as its parameter and gives you a corresponding note as its return value.

###### ★ There are slight differences between every IR Photoreflector, so just using the same conditions with the same values as the example code may not work for your instrument!

###### New and Changed Code (Lines 6-21)
```python=1
from pyatcrobo2.parts import IRPhotoReflector
import time

ir = IRPhotoReflector('P0')

def get_tone(value):
    if value > 1230:  #Line Between Segment 1 and Segment 2 Value
      tone = 'B4'
    elif value > 770: #Line Between Segment 2 and Segment 3 Value
      tone = 'A4'
    elif value > 550: #Line Between Segment 3 and Segment 4 Value
      tone = 'G4'
    elif value > 415: #Line Between Segment 4 and Segment 5 Value
      tone = 'F4'
    elif value > 340: #Line Between Segment 5 and Segment 6 Value
      tone = 'E4'
    elif value > 130: #Line Between Segment 6 and Segment 7 Value
      tone = 'D4'
    else:
      tone = 'C4'
    return tone

while True:
    print(ir.get_value())
    time.sleep_ms(1000)
```

Next let's add code that looks up the IR Photoreflector's value and makes the name of the note retrieved from the `get_tone()` function appear on the LED display. The LED display can only show one letter at a time, so we'll use just the first letter of the note names.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_11_E.png"/>

##### Example Code 4-1-1
###### New and Changed Code (Lines 1-2, Lines 25-26)
```python=1
from pystubit.board import display
from pyatcrobo2.parts import IRPhotoReflector
import time

ir = IRPhotoReflector('P0')

def get_tone(value):
    if value > 1230:
      tone = 'B4'
    elif value > 770:
      tone = 'A4'
    elif value > 550:
      tone = 'G4'
    elif value > 415:
      tone = 'F4'
    elif value > 340:
      tone = 'E4'
    elif value > 130:
      tone = 'D4'
    else:
      tone = 'C4'
    return tone

while True:
    tone = get_tone(ir.get_value())
    display.show(tone[0], delay=0, clear=False)
    # You can retrieve items from a string by index number just like with a tuple or list
    # That means tone[0] gives you the first letter of a note name
```

Now try running this program and see if the letters that appear on the display match the position of the slider when you move it!


### Programming the Pick
Next let's add code that makes the Buzzer play a note when you wave a blue block in front of the Color Sensor like strumming a guitar with a pick.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_12_E.png"/>

#### ■ Getting Color Values

First let's see what the Color Sensor's value is when you place a blue block in front of it.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_13_E.png"/>

The light from the LED display can influence the Color Sensor's values, so you'll need to keep the LEDs' brightness as low as you can. Add the new code shown below before you run the program!

###### New and Changed Code (Line 2, Line 6, Lines 27-29)
```python=1
from pystubit.board import display
from pyatcrobo2.parts import IRPhotoReflector, ColorSensor
import time

ir = IRPhotoReflector('P0')
cs = ColorSensor('I2C')

def get_tone(value):
    if value > 1230:
      tone = 'B4'
    elif value > 770:
      tone = 'A4'
    elif value > 550:
      tone = 'G4'
    elif value > 415:
      tone = 'F4'
    elif value > 340:
      tone = 'E4'
    elif value > 130:
      tone = 'D4'
    else:
      tone = 'C4'
    return tone

while True:
    tone = get_tone(ir.get_value())
    display.show(tone[0], delay=0, clear=False, color=(5, 0, 0)) # The code color=(5, 0, 0) lowers the LEDs' brightness
    print(cs.get_values())
    time.sleep_ms(500)
```

In Topic 3-1 we explained how the Color Sensor's data values are displayed as tuples in the format (R,G,B, Brightness). Look at the data that appears in your terminal to figure out a base value you can use to mean "the blue block is in front of the sensor."

##### Values with Blue Block
<pre class="prettyprint">
[43, 32, 44, 31]
</pre>

##### Values with No Block
<pre class="prettyprint">
[31, 15, 10, 56]
</pre>

If your results look like the ones above, for example, you can pick a base value between 10 and 44 the program can use to judge that the blue block has passed in front of the sensor. Now use your base value to modify the program so your guitar plays a note (determined by the current position of the slider) when you wave the blue block in front of the Color Sensor.

###### Changed Code (Line 1, Lines 28-30)
```python=1
from pystubit.board import display, buzzer
from pyatcrobo2.parts import IRPhotoReflector, ColorSensor
import time

ir = IRPhotoReflector('P0')
cs = ColorSensor('I2C')

def get_tone(value):
    if value > 1230:
      tone = 'B4'
    elif value > 770:
      tone = 'A4'
    elif value > 550:
      tone = 'G4'
    elif value > 415:
      tone = 'F4'
    elif value > 340:
      tone = 'E4'
    elif value > 130:
      tone = 'D4'
    else:
      tone = 'C4'
    return tone

while True:
    tone = get_tone(ir.get_value())
    display.show(tone[0], delay=0, clear=False, color=(5, 0, 0))      
    cs_values = cs.get_values() 
    if cs_values[2] > 30:   # Use your base value to write a condition here.
        buzzer.on(tone, duration=600) 
```

Now your program can do both of the things we set out to make it do at the start! However, there's still one problem with this program. The **StuduinoBitDisplay** class's `show()` and `scroll()` methods are more involved processes than the other class methods or Python's built-in functions, so it'll be hard for the computer to run them over and over again quickly. That means if you write your program like this, there will be a delay while your guitar processes the sensor's values.

```python=1
from pystubit.board import display, lightsensor

while True:
    display.show('C', delay=0)
    value = lightsensor.get_value()
```

Let's try to deal with this problem by streamlining the program so the LED display only changes when the note you're playing is different from the last one.

Make a variable called `current_tone` to record the name of the note you currently have selected. Then the program can compare the value of `current_tone` with the value of the `tone` variable that stores the note selected by the IR Photoreflector's current value, and run the **StuduinoBitDisplay** class's `show()` only when the two are different.

##### Example Code 4-2-1
###### New and Changed Code (Line 26, Lines 30-32)
```python=1
from pystubit.board import display, buzzer
from pyatcrobo2.parts import IRPhotoReflector, ColorSensor
import time

ir = IRPhotoReflector('P0')
cs = ColorSensor('I2C')

def get_tone(value):
    if value > 1230:
        tone = 'B4'
    elif value > 770:
        tone = 'A4'
    elif value > 550:
        tone = 'G4'
    elif value > 415:
        tone = 'F4'
    elif value > 340:
        tone = 'E4'
    elif value > 130:
        tone = 'D4'
    else:
        tone = 'C4'
    return tone

    
current_tone = ''  #  Set the value with an empty string to start with.
while True:
    tone = get_tone(ir.get_value())
    if current_tone != tone:  # Compare whether the current note name and the note currently selected by the IR Photoreflector value match
        display.show(tone[0], delay=0, clear=False, color=(5, 0, 0))
        current_tone = tone  # If the displayed note changes, record the new current note
    cs_values = cs.get_values()
    if cs_values[2] > 30:
        buzzer.on(tone, duration=600)
```

Now your program is finished! Try running it to see how it works!

## Challenge: Tilting Guitar

In Topic 4-1 you made an electric bell change its song's tempo by pressing the Core Unit's buttons, but for this challenge we'll use the tilt of your guitar to change the length of the notes you're playing!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_14_E.png"/>

You can use the **Accelerometer's Y-axis** values to judge how the guitar is tilting, and set certain note lengths to go with certain ranges of Y-axis values. Use the **StuduinoBitAccelerometer** class's `get_y()` method to find the Accelerometer's Y-axis values.

* Y-Axis Acceleration < -8.5 ➡ 150 ms
* Y-Axis Accelereation >= -8.5 and  Y-Axis Acceleration < -4.9 ➡ 300 ms
* Y-Axis Acceleration >= 4.9 ➡ 600 ms

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-3/4-3_15_E.png"/>


### Example Program
We'll make a variable called **length** and use it to set how long `buzzer.on()` plays each note. Then we can make the **length** variable's value change based on Y-axis acceleration. Adding these two features to Example Code 4-2-1 lets you set note lengths by tilting your guitar!

###### New and Changed Code (Line 1, Lines 34-40, Line 44)
```python=1
from pystubit.board import display, buzzer, accelerometer
from pyatcrobo2.parts import IRPhotoReflector, ColorSensor
import time

ir = IRPhotoReflector('P0')
cs = ColorSensor('I2C')

def get_tone(value):
    if value > 1230:
      tone = 'B4'
    elif value > 770:
      tone = 'A4'
    elif value > 550:
      tone = 'G4'
    elif value > 415:
      tone = 'F4'
    elif value > 340:
      tone = 'E4'
    elif value > 130:
      tone = 'D4'
    else:
      tone = 'C4'
    return tone

    
current_tone = ''

while True:
    tone = get_tone(ir.get_value())
    if current_tone != tone:
        display.show(tone[0], delay=0, clear=False, color=(5, 0, 0))
        current_tone = tone
    
    accel_y = accelerometer.get_y()
    if accel_y < -8.5:
        length = 150    # Create length variable to record length of notes
    elif accel_y < -4.9:
        length = 300
    else:
        length = 600
        
    cs_values = cs.get_values()
    if cs_values[2] > 30:
        buzzer.on(tone, duration=length) # The length variable sets the time so you don't need to put numbers in directly
```

## A Quick Review
In this lesson we learned...

* How to use IR Photoreflector values to measure distance/position
* How to use a Color Sensor to tell when an object passes by

If you're using an IR Photoreflector to measure distance, keep in mind that they're bad at detected objects that don't reflect infrared well (for example if they're colored black) and the range of distances they can detect is only around 0.5 - 10 cm.

## Coming Up Next Time!
In the next lesson, learn about random number generation and use it to make a randomly-biting alligator game!