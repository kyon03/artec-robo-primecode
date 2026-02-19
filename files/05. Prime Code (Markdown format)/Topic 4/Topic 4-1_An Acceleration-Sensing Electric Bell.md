---
tags: Python_English, Topic 4
---

Python Robotics Course Lesson 13

Topic 4-1
An Acceleration-Sensing Electric Bell
===

Detect shaking with an Accelerometer to make your Buzzer play sound when shaken!

## In This Lesson...
Use the Core Unit's built-in Accelerometer and Buzzer to make it into an electric bell!
:::info
Lesson Introduction

■ Lesson 13 Contents

1. Conditional Expressions
2. Variables with Unchanging Values
3. Making an Electric Bell
4. Storing Programs on the Core Unit
5. Challenge: Play a New Song



In this lesson you'll use the Core Unit's built-in Accelerometer and Buzzer to make it into an electric bell!

We'll start off by introducing some new Python syntax elements.

The first element we'll talk about is conditional expressions. Conditional expressions let you assign a variable different values based on specific conditions using just one line of code!

The second element we'll talk about is constants. Constants are variables that have the same value throughout a program,and we'll talk about how to write constants in Python!

Once you've learned the new syntax for this lesson, you can use it to make an electric bell! Use the Accelerometer to program a bell that plays a pre-prepared song when you shake it!

Finally, try transferring your finished program to the Core Unit so you can run it even when the Core Unit isn't connected to your computer!

At the end of the lesson, you can use what you've learned to make your electric bell play a new song!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!
:::

## New Python Syntax
Let's start off by introducing some new Python syntax you'll be using for the first time in this lesson.

### Conditional Expressions

There's a type of code in Python called a **conditional expression** which you can use to change the value of a variable based on the value of a conditional statement. Conditional expressions are also called  "ternary operators"  in other programming languages.

For example, let's try making a program that takes the first item from a list and assigns its value to a new variable. Every item in a list is numbered (starting with 0) with an **index** number. Using index numbers to retrieve list items lets you write a program like the one below.

##### Example Code 2-1-1 

```python=1
a = ['A', 'B', 'C', 'D', 'E']

b = a[0] # Displays 'A' in the terminalAssigns the value of the 0th item on list a to variable b.

print(b) # Displays 'A' in the terminal
```

**If list `a` has no items stored in it**, trying to retrieve an item from it like this will cause an error.

##### Example Code 2-1-2

```python=1
a = [] # Makes an empty list

b = a[0] # Displays "IndexError: list index out of range" in the terminal

print(b) # Doesn't run because of the error in the previous line
```

How can we make a program like this assign a different value to variable `b` instead of causing an error? One thing you can do is use the built-in function `len()` to check how many items are stored in the list, so the program can judge if the list is empty. Then you can use an **if** statement to make the program branch based on that judgment.

##### Example Code 2-1-3

```python=1
a = [] 

if len(a) != 0: # Retrieves the first item from the list IF the list isn't empty.
    b = a[0] 
else:
    b = 0　# Assigns the value 0 to variable b if the list is empty.

print(b)
```

Try running the code above to see how it runs both when list `a` is empty and when it has several items in it.

This method works fine, but if you use a Python **conditional expression** instead, you can accomplish the same thing in just one line of code!

#### ■ Writing Conditional Expressions

You can write a conditional expression like this:

```
Variable Name = (Value if the condition is True) if Conditional Statement else (Value if the condition is False)
```

If we rewrite Example Code 2-1-3 using a conditional expression, it will look like this:

##### Example Code 2-1-4

```python=1
a = []
b = a[0] if len(a) != 0 else 0

print(b)
```

Now try running this program. It should work both when list `a` is empty and when it has items in it!


### Variables with Unchanging Values

Sometimes when you're writing a long program, you may want to use the same value in several different parts of your code. Let's look at this program for a simple maze game you can play on the Core Unit's LED display as an example.

##### A Maze Game
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_1_E.png"/>

In the image above, the red LEDs mark the walls of the maze. Let's make a few mazes like this, with the walls of every maze set on lines 0 and 4 of the y axis. How can we write code to make these mazes more efficiently?

One method would be to first make a separate image of just these two outer walls. A program using this method might look like this:

##### Example Code 2-2-1

```python=1
from pystubit.board import Image

outer_walls = Image('11111:00000:00000:00000:11111:')

inner_walls_1 = Image('00000:00010:01000:01010:00000:')
maze_1 = outer_walls + inner_walls_1

inner_walls_2 = Image('00000:01010:01000:00010:00000:')
maze_2 = outer_walls + inner_walls_2
```


In this code, the outer walls are made as a pre-prepared image, which you can combine with images of the inner walls to make various mazes.

The problem with this is that if someone other than the original programmer were to look at this code, they might not realize that they don't need to change the value of the  variable `outer_walls` to make a new maze. Since we want `outer_walls` to always have the same value, we can rewrite this code to make that intent clear!

In Python, it's customary to **write the names of variables with values that don't change throughout the program in all capital letters**. Let's edit Example Code 2-2-1 so it follows this rule, like this:

##### Example Code 2-2-2
###### Changed Code (Line 3, Line 6, Line 9)
```python=1
from pystubit.board import Image

OUTER_WALLS = Image('11111:00000:00000:00000:11111:') 

inner_walls_1 = Image('00000:00010:01000:01010:00000:')
maze_1 = OUTER_WALLS + inner_walls_1

inner_walls_2 = Image('00000:01010:01000:00010:00000:')
maze_2 = OUTER_WALLS + inner_walls_2
```

Variables with unchanging values written in all capital letters like this are called **constants**. If you're looking at a program with a variable written in capitals like this, try to understand why that variable was made a constant instead of changing its value around!

## ３．Building a Bell
Get a copy of the building instructions for this lesson and follow the steps inside to build an electric bell!

### Parts You'll Need
* Core Unit x 1
* Battery Box x 1
* Basic Cube (White) x 1
* Triangle (Red) x 2
* Half B (Gray) x 1
* Half B (Red) x 2
* Half C (White) x 6
* Half D (White) x 2
* Beam x 2

##### The Artec Block Shapes
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_2_E.png"/>

### Electric Bell Building Instructions
Go to this link for building instructions:

[Electric Bell Building Instructions](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_build_E.pdf)

## Programming Your Electric Bell

Your goal in this project is to build an electric bell that does the following:

1. Makes sound when shaken.
2. Makes different sounds when shaken again, so that it plays a song when shaken repeatedly.
3. Changes the tempo of the song when you press the buttons.

Let's go through and program each of these features until we have a complete electric bell program!

### Making Sound When Shaken

Let's start by programming the bell to make a sound when you shake it.

##### Example Video
<iframe src="https://player.vimeo.com/video/482868515" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

#### ■ Accelerometer Values When Shaken
You can use the Accelerometer's Z-axis values to judge when your bell is being shaken!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_3_E.png"/>

By default, the Accelerometer detects acceleration in the range from -19.6 to 19.6 m/s^2^, but you can change that range using the method `set_fs()`.

```
StuduinoBitAccelerometer.set_fs(value)
- Sets the sensor's measurement range to '2g', '4g', '8g', or '16g'.
　　　The default setting is '2g'.
```

Each of these settings corresponds to a different range of measurements, as follows:

* '2 g' = -19.6-19.6 m/s<sup>2</sup>
* '4 g' = -39.2-39.2 m/s<sup>2</sup>
* '8 g' = -78.4-78.4 m/s<sup>2</sup>
* '16 g' = -156.8-156.8 m/s<sup>2</sup>

###### ★ 1g of acceleration is equal to the acceleration of gravity, 9.8 m/s^2^. These units of n times gravitational acceleration are used to set ranges of measurement.

The acceleration of the shaken bell will exceed the Accelerometer's default **2g** range, so let's use the `set_fs()` method with its parameter set to **8g** to change the range to -78.4 - 78.4m/s^2^.

Now, remember how when we used the method `get_values()` in Topic 3-2 it gave us the acceleration of the X-, Y-, and Z-axes together as `(x, y, z)`-formatted tuples? In this program we only need the acceleration value from the Z-axis, so let's use the method `get_z()` instead.

```
StuduinoBitAccelerometer.get_x()
- Finds acceleration along the X-axis.
```

```
StuduinoBitAccelerometer.get_y()
- Finds acceleration along the Y-axis.
```

```
StuduinoBitAccelerometer.get_z()
 - Finds acceleration along the Z-axis.
```

Now we can use these methods to write a program that detects when the Accelerometer's Z-axis value changes, like this:

##### Example Code 4-1-1
```python=1
from pystubit.board import accelerometer
import time

accelerometer.set_fs('8g')

while True:
    accel_z = accelerometer.get_z()
    print(accel_z)
    time.sleep_ms(200)
```

Shake your bell up and down a few times while this program is running. If you don't see much change in the sensor values, try shaking a little harder!

###### ★ Keep the USB cable plugged in while you test this!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_4_E.png"/>


##### Example Terminal Results

<pre class="prettyprint">
1.89
-11.1
-53.22
-24.23
23.74
18.68
8.07
</pre>

The larger values you get will be over 20 (m/s^2^), and the smaller values will be under -40 (m/s^2^). You can use these values as a baseline to judge when the bell is being shaken!

#### ■ Telling When the Bell is Shaken

The motion of a shaking bell is made up of alternating raising and lowering motions. How can we use the way the Accelerometer's values change when these two motions happen so our program can tell when the electric bell has been **shaken once**?

Let's think about how the raising and lowering motions affect the bell's acceleration. When you raise the bell, acceleration increases in the - direction on the Z-axis. Once the bell is moving at the full speed of your shake, the acceleration slows down, which makes acceleration toward the + end of the Z-axis increase.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_5_E.png"/>

When you start lowering the bell, the acceleration toward the + end of the Z-axis will increase, but once you have it moving at its full speed again, the acceleration will slow again, increasing acceleration toward the - end of the Z-axis instead.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_6_E.png"/>

If we combine these two motions into a single shake and plot out the Accelerometer's Z-values during the shake on a graph, it will look like this:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_7_E.png"/>

From this graph, we can see that the bell's acceleration goes over 20m/s^2 when it's raised, and under -40m/s^2 when it's lowered. Using this information, we can check the bell's current position with the following code.

<pre class="prettyprint">
if accel_z &gt; 20:
    The bell has been raised
elif accel_z &lt; -40:
    The bell has been lowered
</pre>

However, this code alone can't tell us when the bell has been shaken once. What we need to do is set some kind of marker for when the bell's motion switches from raising to lowering, so the program can see that marker and know the bell has been shaken once. We call markers like these **flags**.

Boolean values (**True** or **False**) and numbers are usually used as flags. For this program, let's make a variable called **flag_accel** and set its value to **1** when we detect the bell raising and **0** when we detect it lowering. Then we can use it to detect a single shake with the following code.

<pre class="prettyprint">
if accel_z &gt; 20 and flag_accel == 0:
    flag_accel = 1
    # Sets flag_accel to 1 when raising detected, setting a marker.
elif accel_z &lt; -40 and flag_accel == 1:
    flag_accel = 0
    # Judges the bell has been shaken when lowering is detected.
    # Next set flag_accel back to 0 and wait until raising is detected again.
</pre>

This program judges that the bell has been shaken when `flag_accel`'s value changes from 1 to 0.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_8_E.png"/>

Now that we have this shake-detection program in place, we can make the Buzzer play when you shake the bell. Try running the code below yourself and see how it works!

##### Example Code 4-1-2

```python=1
from pystubit.board import buzzer,accelerometer

accelerometer.set_fs('8g')
flag_accel = 0 # Sets initial value to 0 at the start of the program.

while True:
    accel_z = accelerometer.get_z()
    if accel_z > 20 and flag_accel == 0:
        flag_accel = 1
    elif accel_z < -40 and flag_accel == 1:
        flag_accel = 0
        buzzer.on('C4', duration=600) # Plays the note Do (C4) for 600 ms to test
```

### Playing a Song When Shaken
Let's program the bell to play 8 bars of Twinkle Twinkle Little Star.

##### Twinkle Twinkle Little Star Notes
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_9_E.png"/>

##### Example Video
<iframe src="https://player.vimeo.com/video/482868889" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

#### ■ Storing Notes in a Tuple

In musical notation, notes are drawn differently to indicate how long they last. Using a quarter note as the basic note length, here's what some other common note types look like in notation.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_10_E.png"/>

The musical score above also has a note with a dot next to it in the seventh bar, which is called a **dotted eighth note**. Dotted notes last 1.5 times as long as a regular note of the same type.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_11_E.png"/>

Now, with our quarter notes being 600 ms long, we can set the length of all the other note types as well using the code below. Later on we'll be using the **StuduinoBitBuzzer** class's `on()` method to play notes, and that method's parameters can only be set using integers. That's why we have to use a function `int()` or `round()` to turn any floating-point values we get from the calculations in this part into integers. In the example code we've used `int()` to cut off any numbers after the decimal point.

###### New Code (Lines 3-6)
```python=1
from pystubit.board import buzzer,accelerometer

QUARTER_NOTE = 600    # Quarter Note
HALF_NOTE = QUARTER_NOTE*2    # Half Note
DOTTED_EIGHTH_NOTE = int(QUARTER_NOTE/2*1.5)    # Dotted Eighth Note
SIXTEENTH_NOTE = int(QUARTER_NOTE/4)    # Sixteenth Note

accelerometer.set_fs('8g')
flag_accel = 0
```

Now we can use these constants to make nested tuples that store the song's notes, like this:
```
( 
  (Pitch of Note 1, Length of Note 1),  
  (Pitch of Note 2, Length of Note 2),
  .
  .
  .
)
```

The nested tuples each contain data that describes one note in the song, and the tuple they're nested inside keeps them all together as one song. Writing out our eight-bar song this way looks like this:

###### New Code (Lines 8-24)
```python=1
from pystubit.board import buzzer,accelerometer

QUARTER_NOTE = 600
HALF_NOTE = QUARTER_NOTE*2
DOTTED_EIGHTH_NOTE = int(QUARTER_NOTE/2*1.5)
SIXTEENTH_NOTE = int(QUARTER_NOTE/4)

melody = (
    ('C4', QUARTER_NOTE),
    ('C4', QUARTER_NOTE),
    ('G4', QUARTER_NOTE),
    ('G4', QUARTER_NOTE),
    ('A4', QUARTER_NOTE),
    ('A4', QUARTER_NOTE),
    ('G4', QUARTER_NOTE),
    ('G4', QUARTER_NOTE),
    ('F4', QUARTER_NOTE),
    ('F4', QUARTER_NOTE),
    ('E4', QUARTER_NOTE),
    ('E4', QUARTER_NOTE),
    ('D4', QUARTER_NOTE),
    ('D4', DOTTED_EIGHTH_NOTE),
    ('E4', SIXTEENTH_NOTE),
    ('C4', HALF_NOTE))
    
accelerometer.set_fs('8g')
flag_accel = 0
```


#### ■ Playing Notes in Order
Let's add some code that counts the number of times you shake the bell, so we can use it to retrieve the data for each note of the song in order from the tuple we've stored them in. We can use the shake count to pick which index numbers of items in the tuple to retrieve. Once you add the code below, your program will be finished!

##### Example Code 4-2-1
###### New and Changed Code (Line 28, Lines 36-38)
```python=1
from pystubit.board import buzzer,accelerometer

QUARTER_NOTE = 600
HALF_NOTE = QUARTER_NOTE*2
DOTTED_EIGHTH_NOTE = int(QUARTER_NOTE/2*1.5)
SIXTEENTH_NOTE = int(QUARTER_NOTE/4)

melody = (
    ('C4', QUARTER_NOTE),
    ('C4', QUARTER_NOTE),
    ('G4', QUARTER_NOTE),
    ('G4', QUARTER_NOTE),
    ('A4', QUARTER_NOTE),
    ('A4', QUARTER_NOTE),
    ('G4', QUARTER_NOTE),
    ('G4', QUARTER_NOTE),
    ('F4', QUARTER_NOTE),
    ('F4', QUARTER_NOTE),
    ('E4', QUARTER_NOTE),
    ('E4', QUARTER_NOTE),
    ('D4', QUARTER_NOTE),
    ('D4', DOTTED_EIGHTH_NOTE),
    ('E4', SIXTEENTH_NOTE),
    ('C4', HALF_NOTE))

accelerometer.set_fs('8g')
flag_accel = 0
count = 0

while True:
    accel_z = accelerometer.get_z()
    if accel_z > 20 and flag_accel == 0:
        flag_accel = 1
    elif accel_z < -40 and flag_accel == 1:
        flag_accel = 0
        count = count+1 if count < len(melody) else 1
        note = melody[count-1]
        buzzer.on(note[0], duration=note[1])
```

The code `count = 0` on line 28 sets the initial shake count to 0 at the start of the program. Let's take a close look at the code we added and changed on lines 36-38 as well.

```python=36
        count = count+1 if count < len(melody) else 1
        note = melody[count-1]
        buzzer.on(note[0], duration=note[1])
```

On line 36, `count = count+1 if count < len(melody) else 1` uses a conditional expression to record the number of times the bell has been shaken in the variable `count`. Writing this part of the code with an **if-else** statement instead would look like this:

```python=36
        if count < len(melody):
            count = count+1
        else:
            count = 1
```

The `count` variable's value is used to pick index numbers from the tuple `melody`, so we've set a limit to stop its value from exceeding `melody`'s length. It also resets the shake count when `count`'s value reaches the last index number in `melody` and starts counting again from 1.

The code `note = melody[count-1]` on line 37 uses the value of `count` to pick an index number from `melody`, and attaches that value to the variable `note`. The reason we use `melody[count-1]` is because the shake count will start at 1, but tuple index numbers start at 0!

Line 38, `buzzer.on(note[0], duration=note[1])`, plays a note. It's written this way because the 0th item in the nested note tuples stores the note's pitch while the 1st item stores their length.


### Changing Tempo with Buttons

Now let's add another feature to the bell that lets you change the tempo of the bell's song by pressing the A and B buttons on the Core Unit and shows the current tempo in the LED display.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_12_E.png"/>

##### Example Video
<iframe src="https://player.vimeo.com/video/482871156" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

#### ■ Detecting Button Presses

The **StuduinoBitButton** class has a couple methods you can use to find out when the Core Unit's buttons have been pressed, including `is_pressed()` and `was_pressed()`.

```
StuduinoBitButton.was_pressed()
- Returns True if the button has been pressed in the past, and False if it hasn't.
　　　Resets all past data when called.
```

If we run a program like the one below using the `is_pressed()` method, it will show us the value of the variable `num` increasing over and over as long as button A is being pressed.

###### ★ If you want to test this code for yourself, make a new program file to copy it into and run it there!

```python=1
from pystubit.board import button_a

num = 0
while True:
    if button_a.is_pressed():
        num += 1
        print(num)
```

If we use the method `was_pressed()` instead, we'll only get the value of `num` once, even if you hold down button A for a while when you press it. Then if you let go of button A and press it again, it will increase `num`'s value by one and show us that new value.

```python=1
from pystubit.board import button_a

num = 0
while True:
    if button_a.was_pressed():
        num += 1
        print(num)
```

This is why you should use the `was_pressed` method if you're writing a program where you want to make a number go up or down by one when you press a button, like this one.

#### ■ Changing Tempo When A and B are Pressed
Let's modify Example Code 4-2-1 to add a variable called `tempo` that decreases by 1 when you press button A and increases by 1 when you press button B. We'll also need to add some calculations to make the length of the notes the Buzzer plays change when the value of `tempo` changes. Adding these two features makes the program look like this:

##### Example Code 4-3-1
###### New and Changed Code (Line 1, Lines 29-30, Line 40, Lines 42-47)

```python=1
from pystubit.board import display, buzzer, accelerometer, button_a, button_b

QUARTER_NOTE = 600
HALF_NOTE = QUARTER_NOTE*2
DOTTED_EIGHTH_NOTE = int(QUARTER_NOTE/2*1.5)
SIXTEENTH_NOTE = int(QUARTER_NOTE/4)

melody = (
    ('C4', QUARTER_NOTE),
    ('C4', QUARTER_NOTE),
    ('G4', QUARTER_NOTE),
    ('G4', QUARTER_NOTE),
    ('A4', QUARTER_NOTE),
    ('A4', QUARTER_NOTE),
    ('G4', QUARTER_NOTE),
    ('G4', QUARTER_NOTE),
    ('F4', QUARTER_NOTE),
    ('F4', QUARTER_NOTE),
    ('E4', QUARTER_NOTE),
    ('E4', QUARTER_NOTE),
    ('D4', QUARTER_NOTE),
    ('D4', DOTTED_EIGHTH_NOTE),
    ('E4', SIXTEENTH_NOTE),
    ('C4', HALF_NOTE))

accelerometer.set_fs('8g')
flag_accel = 0
count = 0
tempo = 1 # Initial value is 1x original tempo
display.show(str(tempo),delay=0,clear=False) # Show initial tempo on the display

while True:
    accel_z = accelerometer.get_z()
    if accel_z > 20 and flag_accel == 0:
        flag_accel = 1
    elif accel_z < -40 and flag_accel == 1:
        flag_accel = 0
        count = count+1 if count < len(melody) else 1
        note = melody[count-1]
        buzzer.on(note[0], duration=int(note[1]/tempo)) # Dividing here multiplies the speed by tempo

    if button_b.was_pressed():
        tempo = tempo + 1 if tempo < 5 else 5 # Maximum is 5x original speed
        display.show(str(tempo), delay=0, clear=False) # Show new tempo on the display
    elif button_a.was_pressed():
        tempo = tempo - 1 if tempo > 1 else 1 # Minimum is 1x original speed
        display.show(str(tempo), delay=0, clear=False) # Show new tempo on the display
```
The `duration` parameter on line 40 can only be set using integers, so we used the built-in function `int()` to make sure the calculation results are always an integer.

We also used the **StuduinoBitDisplay** class's `show()` method on lines 30, 44, and 47. This method's first parameter can't take numeric data, so we used the `str()` function to make the numbers we want to display into strings.

## Storing Programs on the Core Unit
Apart from running your completed programs with Mu's `Run` button, you can also transfer and save programs to your Core Unit and run them from there! If you have a program stored on the Core Unit, you'll be able to run it even when the Core Unit isn't connected to your PC via USB cable. Follow these steps to transfer a program to your Core Unit and run it from there:

### Saving Programs on the Core Unit

1. First you'll have to save your completed program as a program file. Click the **Save** button in the upper menu bar, then name your file **main** and save it. The file you save with automatically have the file extension **.py**, indicating it's a Python program file.

###### ★ If you've already saved your current program, click **New** to make a new program file and copy-paste your code into it. Then save your new file.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_13_E.png"/>

Click the **Files** button in the menu bar.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_14_E.png"/>

If the **Files** button is grayed out, click **REPL** and close the terminal window. That should make the **Files** button work again. You need to do this because you can't use Mu's REPL functions and transfer program files from it at the same time.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_15_E.png"/>

3. Find the file called **main.py** in the **Files on your computer** window that appears in the lower right, then drag and drop it into the **Files on your device** window to overwrite the main.py file that's already there.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_16_E.png"/>

4. Click the **Files** button in the menu bar to close the Files window. Unplug the USB cable from your Core Unit and check to make sure it has a Battery Box plugged in, then press the Core Unit's reset button. Wait a few seconds, and then the program you just transferred will run!

#### ■ Why Does Saving a Program as main.py Make It Run?
When the Core Unit turns on or after its reset button is pressed, it automatically runs the programs it has stored on it with certain special names. Any other programs it has stored won't run until they get a specific command to start.

The Core Unit first runs the program called **boot.py**, and then runs the program called **main.py. boot.py** contains the code the Core Unit needs to run so it can start up and start working, so generally you shouldn't ever overwrite this file!

Instead you can write any code you want to run automatically after start-up in the program that runs next, `main.py.` You can't directly edit the `main.py` file on the Core Unit, so to change it you need to follow the steps we just explained to overwrite it with a new program.

## Challenge: Play a New Song
Try modifying Example Code 4-3-1 so your electric bell plays the song below instead of Twinkle Twinkle Little Star!

##### My Grandfather's Clock
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-1/4-1_17_E.png"/>

###### ★ Hint: An eighth note is half as long as a quarter note, and the **dotted half note** at the end of this song is 1.5 times as long as a half note, making it three times as long as a quarter note. Make constants to define these new note lengths and store the information for up to the 5th bar of the song above in a tuple.

###  Example Program
Define constants to describe quarter notes, dotted half notes, and eighth notes, and store data for each note of the song in the tuple `melody` to complete this program!

##### Example Code 6-1-1
###### New and Changed Code (Lines 2-4, Lines 7-28)
```python=1
from pystubit.board import display,buzzer,accelerometer,button_a,button_b

QUARTER_NOTE = 600
DOTTED_HALF_NOTE = QUARTER_NOTE*2*1.5
EIGHTH_NOTE = int(QUARTER_NOTE/2)

melody = (
    ('G4', EIGHTH_NOTE),
    ('G4', EIGHTH_NOTE),
    ('C5', QUARTER_NOTE),
    ('B4', EIGHTH_NOTE),
    ('C5', EIGHTH_NOTE),
    ('D5', QUARTER_NOTE),
    ('C5', EIGHTH_NOTE),
    ('D5', EIGHTH_NOTE),
    ('E5', QUARTER_NOTE),
    ('F5', EIGHTH_NOTE),
    ('E5', EIGHTH_NOTE),
    ('A4', QUARTER_NOTE),
    ('D5', EIGHTH_NOTE),
    ('D5', EIGHTH_NOTE),
    ('C5', QUARTER_NOTE),
    ('C5', EIGHTH_NOTE),
    ('C5', EIGHTH_NOTE),
    ('B4', QUARTER_NOTE),
    ('A4', EIGHTH_NOTE),
    ('B4', EIGHTH_NOTE),
    ('C5', DOTTED_HALF_NOTE))

accelerometer.set_fs('8g')
flag_accel = 0
count = 0
tempo = 1
display.show(str(tempo),delay=0,clear=False)

while True:
    accel_z = accelerometer.get_z()
    if accel_z > 20 and flag_accel == 0:
        flag_accel = 1
    elif accel_z < -40 and flag_accel == 1:
        flag_accel = 0
        count = count+1 if count < len(melody) else 1
        note = melody[count-1]
        buzzer.on(note[0],duration=int(note[1]/tempo)) 

    if button_b.was_pressed():
        tempo = tempo + 1 if tempo < 5 else 5
        display.show(str(tempo),delay=0,clear=False)
    elif button_a.was_pressed():
        tempo = tempo - 1 if tempo > 1 else 1 
        display.show(str(tempo),delay=0,clear=False)
```

## Conclusion
### A Quick Review
In this lesson you learned about two new elements of Python syntax: **conditional expressions** and **constants**.

* Conditional Expressions
You can use this format to make a variable's value change based on a particular condition with just one line of code:
```
Variable Name = (Value if the  condition is True) if Conditional Statement else (Value if the condition is False)
```

* Constants
Variables with their names written in all capital letters have fixed values throughout a program that aren't supposed to be changed.

You also used these elements and the Accelerometer to make an electric bell that plays songs when you shake it!

Finally, you learned how to store and run programs on the Core Unit. You'll be doing that a lot in later lessons so make sure you understand how to do it!

### Coming Up Next Time!
In the next lesson you'll use an Ultrasonic Sensor to program a driving support system that helps cars avoid accidents!