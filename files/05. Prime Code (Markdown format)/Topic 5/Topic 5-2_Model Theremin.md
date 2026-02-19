---
tags: Python_English, Topic 5
---

Python Robotics Course Lesson 18

Topic 5-2
Model Theremin
===

Learn about functions with flexible numbers of arguments!

## In This Lesson...

In this lesson, you'll learn how to make functions with flexible numbers of arguments, and how to use **continue**, **break**, and **else** statements with your **for** and **while** statements!

After that you can use a Light Sensor and an Ultrasonic Sensor to build a digital instrument you can play without touching it, like a theremin!

:::info
Lesson Introduction

■ Lesson 18 Contents
1. Variable-Length Arguments
2. Using continue, break, and else Statements in Loops
3. Making a Model Theremin
4. Challenge: A Wider Range of Notes



In this lesson, we'll use a Light Sensor and an Ultrasonic Sensor to build a digital instrument you can play without touching it!

We'll start off by introducing some new Python syntax elements.

In previous lessons we've talked about three different kinds of arguments methods and functions can have: positional arguments, keyword arguments, and default arguments. By default, the number of arguments a function takes is set when you make it, but you can also make functions with optional arguments so the total number they need is flexible!

Next we'll introduce three new syntax elements you can use with while and for loops: continue, break, and else statements. Learning to use this new syntax will help you write more advanced program loops!

After that you'll use a Light Sensor and an Ultrasonic Sensor to build a theremin-like digital instrument you can play without touching it. This instrument determies what note to play based on the Light Sensor's values, and turns the Buzzer on and off based on the Ultrasonic Sensor's values.

Finally you can use what you've learned to expand the range of notes your instrument can play!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!

:::

## New Python Syntax
Let's start off by introducing some new Python code you'll be using for the first time in this lesson.

### Making Optional/Variable-Length Arguments

So far we've been writing functions with a predetermined number of arguments to take, using a format like this:

```
def function(a, b, c):
    .
    .
    .
```

Sometimes, however, it's more convenient for the number of arguments a function takes to be flexible. The `print()` function is a great example of this - you can set however many arguments you want for it by separating them with commas (`,`), and it can display all of them!


```
print(1)        # Displays `1`
print(1, 2, 3)  # Displays `1`, `2`, and `3`.
```

Let's see how we can make our own functions with flexible numbers of arguments like this.

#### Variable-Length Positional Arguments

If you write an asterisk (`*`) before the name of an argument when you're defining a function, that argument becomes a variable-length positional argument.

```
def function(*args):
    .
    .
    .
```

###### ★ `args` is short for **arguments**.

Arguments defined like this **store their values as tuples**. The example code below uses this feature to make a function that adds all the arguments you give it together and tells you their sum!

##### Example Code 2-1-1

```python==1
def sum_nums(*nums):
    print(nums) # The `nums` argument stores all the arguments you set in a tuple

    result = 0

    for n in nums:
        result += n

    print(result)

sum_nums(1, 2, 3)       # Displays `6`
sum_nums(1, 2, 3, 4, 5) # Displays `15`
```

#### Variable-Length Keyword Arguments

Apart from positional arguments, you also know how to use keyword arguments (the kind you set using their names). If you write two asterisks before the name of an argument when you're defining a function, that argument becomes a variable-length keyword argument.

```
def function(**kwargs):
    .
    .
    .
```

###### ★ `kwargs` is short for **keyword-arguments**.


Arguments defined like this **store their values as dictionaries**. The example code below uses this feature to make digital Japanese vocabulary flashcards!

##### Example Code 2-1-2

```python==1
import time
from pystubit.board import display

def show_phrases(**phrases):

    for phrase, japanese in phrases.items():  # The `items()` method retrieves both keys and values in order.
        print(japanese)  # Displays the Japanese vocab words
        time.sleep(1)
        display.scroll(phrase)  # Shows the English word on the LED display
        time.sleep(1)

show_phrases(apple="ringo", cat="neko", fish="sakana") # Set the English words as keyword argument names, and assign the Japanese words as strings.
```

Since the arguments are stored as a dictionary, they won't necessarily be stored in the order you write them in. Running this example program will display the vocab words in the order fish ⇒ cat ⇒ apple.

#### ■ Setting Variable- and Invariable-Length Arguments

When you make a function with both positional and keyword arguments, you always have to list the positional arguments first. If you also have variable-length arguments, you need to keep the argument types in the order shown below. Making function that puts the argument types in the wrong order will cause an error in Python!

```
def function(Positional Arguments, Variable-Length Positional Arguments, Keyword Arguments, Variable-Length Keyword Arguments):
    .
    .
    .
```

Try running the example code below and see if you get the intended results. Then try changing the order of the arguments to see if you get an error!

##### Example Code 2-1-3

```python==1
def sample(arg1, arg2, *args, kwarg1, kwarg2, **kwargs):
    print(arg1, arg2)  # `arg1` stores 1, `arg2` stores 2
    print(args)  # `args` stores the tuple (3, 4)
    print(kwarg1, kwarg2)  # `kwarg1` stores "a", `kwargs2` stores **b".
    print(kwargs)  # `kwargs` stores the dictionary ‘{"kwarg3":"c", "kwargs4":"d"}'


sample(1, 2, 3, 4, kwarg1="a", kwarg2="b", kwarg3="c", kwarg4="d")
```

##### Intended Program Results

<pre class="prettyprint">
1 2
(3, 4)
a b
{'kwarg4': 'd', 'kwarg3': 'c'}
</pre>

##### Program Results with Arguments Out of Order

<pre class="prettyprint">
>OK Traceback (most recent call last):
  File "&ltstdin&gt", line 4, in sample
SyntaxError: invalid syntax
</pre>


### Using continue, break, and else Statements

Next let's learn about **continue**, **break**, and **else** statements, which you can combine with the while and for statements you're already using to create more advanced program loops!

#### ■ continue Statements

Running a continue statement inside a loop makes your program skip the remaining code inside the loop, going right to the start of the next iteration of that loop.

##### Example: Display all the odds numbers between 1 and 30 with print()

```python==1
for n in range(1, 31):
    if n % 2 == 0:
        continue # If the number is even, `continue` makes the program skip ahead to run the next number through the loop

    print(n)
```

#### ■ break Statements

Running a break statement inside a loop makes that loop stop running and stop looping immediately.

##### Example: Play the Buzzer Until Button A is Pressed

```python==1
from pystubit.board import buzzer, button_a

buzzer.on('C4')

while True:
    if button_a.is_pressed():
        break # `break` stops the loop if Button A is pressed

buzzer.off()
```

#### ■ else Statements

If you add an else statement after a for or while statement, you can add some more code to run when the loop finishes (as long as it isn't cut off early by something like a break statement). Compared to continue and break statements there are relatively few times you'll want to use else statements, but they can be used to do things like letting you know **if a loop didn't work right** that make your programs easier to understand!

##### Example: Finding Numbers in a List

```python==1
from pystubit.board import display

def findNumber(n, xs):  # Set the number to search for in `n` and the list to search in 'xs'
    for x in xs:
        if x == n:
            display.scroll('{} is found'.format(n))
            break # If `n` is in the list, end the loop
    else:
        # Tells you that `n` is not in the list, since the break statement didn't run
        display.scroll('{} is not found!!!'.format(n))


findNumber(3, [0, 1, 2, 4, 5])
```


## Building Your Theremin
Theremins (named after their inventor, Leon Theremin), are an unusual kind of musical instrument you can play without touching the instrument itself. They're often used to make sound effects for sci-fi and horror movies because of the strange and unique sounds they make!

A theremin has two antennae, and you can make it play notes by moving your hands around them to adjust their magnetic field! When you play the theremin, your right hand controls the pitch of the note you play and your right hand controls the volume.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-2/5-2_1_E.png"/>



Now let's take a Light Sensor and an Ultrasonic Sensor use them to build our own **digital theremin** you can play without touching, just like a real one! Get a copy of the builder's guide for this lesson and follow the steps inside to build a model theremin!

### Parts You'll Need

##### Parts
* Core Unit x 1
* Robot Expansion Unit x 1
* Battery Box x 1
* Ultrasonic Sensor x 1
* Sensor Connecting Cable (3-wire, 15 cm) x 1
* Basic Cube (Red) x 4
* Basic Cube (Black) x 2
* Basic Cube (Gray) x 1
* Basic Cube (Red) x 4
* Triangle (Red) x 2
* Half A (Gray) x 1
* Half B (Red) x 1
* Half C (White) x 2
* Half D (White) x 1
* Beam x 1

##### The Artec Block Shapes
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-2/5-2_2_E.png"/>


### Building Instructions
Building instructions for the model theremin:

[Model Theremin Builder's Guide](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-2/5-2_build_E.pdf)


## Programming Your Model Theremin
You'll be able to play your model theremin by holding your right hand in front of the Light Sensor and your left hand in front of the Ultrasonic Sensor. The theremin will set the pitch of its sound based on the Light Sensor's value, and play notes whenever you hold your hand within a certain distance in front of the Ultrasonic Sensor. 

##### Completed Example Video

<iframe src="https://player.vimeo.com/video/482873252" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

We can set up a single program that lets you play an instrument like this by making **a function that takes variable-length keyword arguments**. By setting up the correspondence between in Light Sensor values and notes in the argument, we can even adjust the range of notes the theremin can play without changing any code inside the function!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-2/5-2_3_E.png"/>


### Program Outline

Your model theremin needs to have the following features to work.

##### Your Theremin Needs to...

1. Pick a note to play based on the Light Sensor's value and show that note on the LED display
2. Switch the Buzzer on and off based on the Ultrasonic Sensor's value

Let's put all these features together to make a complete program as shown in the flowchart below. Then you can put the whole program into a single function called `play_theremin`.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-2/5-2_4_E.png"/>

Now let's go and program all these features, one at a time!

### Picking Notes

Let's start by trying to come up with a program that plays just three notes: C4, D4, and E4. First we'll need to determine which hand positions correspond to which notes!

###### ★ In the example below, the note's pitch gets higher the less light the sensor detects (so, the closer the hand is placed to the sensor).

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-2/5-2_5_E.png"/>

#### ■ Light Sensor Values and Notes

Before writing your program, check the value of your Light Sensor with your hand held at the positions shown above. You can use the program below to find the Light Sensor's current value and display it in the terminal.

##### Example Code 4-2-1

```python==1
from pystubit.board import lightsensor
import time

while True:
    value = lightsensor.get_value()
    print(value)
    time.sleep_ms(500)
```

Now check the Light Sensor's value with your hand at the positions that mark the lines between different notes, as shown below.

##### Hand Positions and Light Sensor Values

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-2/5-2_6_E.png"/>

Once you've found your values, you can use them with the table beloe to determine the range of Light Sensor values that play each note. Make sure to write down the ranges you pick!

|Note|Sensor Value Range|
|--|--|
|C4|0 ≦ val < Value with hand at position ①|
|D4|Value with hand at position ① ≦ value ＜	Value with hand at position ②|
|E4|Value with hand at position ② ≦ value ＜ Value with hand at position ③|

For our example code, we'll be using these ranges for the notes:

|Note|Sensor Value Range|
|--|--|
|C4|0 ≦ value ＜ 1000|
|D4|1000 ≦ value ＜ 2000|
|E4|2000 ≦ value ＜ 3000|

#### ■ Connecting Sensor Values and Notes with Function Arguments

Start by making a new program, then create a function called `play_theremin()` with the note and Light Sensor value relationships you've picked as variable-length keyword arguments.

Write the arguments as shown below so they'll be stored as a dictionary with the notes as keys and Light Sensor value ranges (in tuples) as the stored items.

<pre class="prettyprint">
play_theremin(C4=(0, 1000), D4=(1000, 2000), E4=(0, 3000))
</pre>

You'll need to define the function with two asterisks before the argument's name to make it a variable-length argument, as in `play_theremin(**kwargs)`.

To check whether the arguments you've written are being stored correctly as a dictionary, use a for-in statement and the `items()` method for dictionaries to retrieve the notes (variable `p`) and Light Sensor value ranges (variable `_range`) in order and display them in the terminal.

```python==1
def play_theremin(**kwargs):
    for p, _range in kwargs.items():
        print('pitch:', p)
        print('range', _range)

play_theremin(C4=(0, 1000), D4=(1000, 2000), E4=(2000, 3000))
```
###### ★ The name `_range` starts with an underscore so that it doesn't overlap with the name of the built-in `range()` function.

Running this program should give you the results shown below. Remember, the order of items in a dictionary won't necessarily be the same as the order you added them in! However, this shouldn't cause any problems in the program we're writing this time.

#### Program Results
<pre class="prettyprint">
pitch: D4
range (1000, 2000)
pitch: E4
range (2000, 3000)
pitch: C4
range (0, 1000)
</pre>

#### Displaying Notes
Now let's make the current note selected by the Light Sensor's value appear on the LED display.

First import `lightsensor` and `display` objects, along with the `time` module, at the start of the program.

###### New Code (Lines 1-2)
```python==1
from pystubit.board import lightsensor, display
import time

def play_theremin(**kwargs):
    for p, _range in kwargs.items():
        print('pitch:', p)
        print('range', _range)

play_theremin(C4=(0, 1000), D4=(1000, 2000), E4=(2000, 3000))
```

Next set up an endless loop using a while statement and make the loop check the Light Sensor's values. Then it should take the value it finds and compare it to all of the ranges set in the function parameters (through the variable `_range`) to see which it belongs to. When it finds the range the note belongs to, it should record that range's note (from variable `p`) in the variable called `pitch`.

###### New and Changed Code (Lines 5-9)
```python==1
from pystubit.board import lightsensor, display
import time

def play_theremin(**kwargs):
    while True:
        value = lightsensor.get_value()
        for p, _range in kwargs.items():
            if value >= _range[0] and value < _range[1]:
                pitch = p
                
play_theremin(C4=(0, 1000), D4=(1000, 2000), E4=(2000, 3000))
```

###### ★ The `_range` variable is a tuple, so you can get the low end of its range with `_range[0]` and the upper end of its range with `_range[1]`.
###### ★ We don't need to display the values anymore, so we've removed the `print()` function from the program.

With this code as it's written now, the program won't be able to define the `pitch` variable if the Light Sensor's value doesn't belong to any of the ranges you have recorded. This would cause errors in the rest of the program! To fix this problem, let's add a break statement and an else statement to this code. The break statement will end the loop if the value isn't found in any of the ranges, and if that happens the else statement will run and set the `pitch` variable's value to `None`.

###### ★ None is a special object that represents something with no value.

###### New Code (Lines 10-12)
```python==1
from pystubit.board import lightsensor, display
import time

def play_theremin(**kwargs):
    while True:
        value = lightsensor.get_value()
        for p, _range in kwargs.items():
            if value >= _range[0] and value < _range[1]:
                pitch = p
                break
        else:
            pitch = None

play_theremin(C4=(0, 1000), D4=(1000, 2000), E4=(2000, 3000))
```

Next let's add code to make letters corresponding the note currently stored in the `pitch` variable appear on the LED display. Running this code while the value of `pitch` is `None` will cause an error, so let's split the program here to make it turn off the LED display in that case instead.

The LED display can't show more than one letter at a time, so we'll use just the first letter from note names like `C4` and `D4`. Since strings are a kind sequence data just like tuples and lists, `pitch[0]` will get you just the first letter of the string stored in the variable.

Using the `show()` method with the LED display set to its maximum brightness will interfere with the Light Sensor's readings, so use it with the argument `color=(5, 0, 0)` to keep the LED's brightness low.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-2/5-2_7_E.png"/>

##### Example Code 4-3-1

###### New Code (Lines 14-17)

```python==1
from pystubit.board import lightsensor, display
import time

def play_theremin(**kwargs):
    while True:
        value = lightsensor.get_value()
        for p, _range in kwargs.items():
            if value >= _range[0] and value < _range[1]:
                pitch = p
                break
        else:
            pitch = None

        if pitch:    # Becomes False if the value is None.
            display.show(pitch[0], delay=0, color=(5, 0, 0))
        else:
            display.clear()

play_theremin(C4=(0, 1000), D4=(1000, 2000), E4=(2000, 3000))
```

Now try running the program to see how it works!

### Turning the Buzzer ON/OFF with the Ultrasonic Sensor

Let's make the Buzzer play the note selected by Example Code 4-3-1 when the Ultrasonic Sensor's value goes below a certain threshold. Start by holding your left hand in front of the sensor in a position you'll find comfortable for playing your theremin, and measure how far from the sensor it is. Then you can use that distance to set a threshold your computer can use to judge when your hand is close enough to the sensor for it to turn on the Buzzer.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-2/5-2_8_E.png"/>

#### Finding Ultrasonic Sensor Values
First create a new program, separate from Example Code 4-3-1. Click the New button in the top menu bar to start a new tab, then copy the code below into the new tab where you can use it make the Ultrasonic Sensor's values appear in the terminal.

##### Example Code 4-4-1
```python==1
from pyatcrobo2 import UltrasonicSensor

us = UltrasonicSensor('P0')
while True:
    val = us.get_distance()
    print(val)
```

Use the results you find when you run this program to pick a distance to use as the threshold you move your hand across to turn the Buzzer on/off. We'll be using a threshold distance of 8cm in our example code.

#### ■ Playing Notes When Your Hand is Close to the Sensor
First we need to add code that allows our main program to use the Ultrasonic Sensor connected to port P0 on the Robot Expansion Unit. Import the UltrasonicSensor class, then make an instance from it and save it in a variable called `us`.

Next, use method `get_distance()` to retrieve the Ultrasonic Sensor's current value. If no note is selected, there's no need to check the Ultrasonic Sensor, so let's place this code where it will only run if the `pitch` variable has a string saved as its value. Since the Ultrasonic Sensor uses reflected sound to measure distance, it can't get new measurements in time intervals shorter than the time it takes for its ultrasonic waves to bounce back to it. We can account for this in the code by using the function `sleep_ms()` from the `time` module make the program wait for 20 milliseconds between measurements.

###### New Code (Line 2, Line 5, Lines 20-21)
```python==1
from pystubit.board import lightsensor, display
from pyatcrobo2.parts import UltrasonicSensor
import time

us = us = UltrasonicSensor('P0')

def play_theremin(**kwargs):
    while True:
        value = lightsensor.get_value()
        for p, _range in kwargs.items():
            if value >= _range[0] and value < _range[1]:
                pitch = p
                break
        else:
            pitch = None
            
        if pitch:
            display.show(pitch[0], delay=0, color=(5, 0, 0))
            
            distance = us.get_distance()
            time.sleep_ms(20)
        else:
            display.clear()

play_theremin(C4=(0, 1000), D4=(1000, 2000), E4=(2000, 3000))
```

Now let's use the sensor values we've retrieved to control the Buzzer. Import a `buzzer` object and run its `on()` method to play the note set in pitch if the value from the Ultrasonic Sensor is less than the threshold value (meaning your hand is closer to the sensor). If the value is greater than the threshold value, run the `off()` method instead.

If the Ultrasonic Sensor's value is less than its threshold but the Light Sensor's value isn't in the range for any of the notes, there won't be a note selected, so the program should run the `off()` method to stop the Buzzer then, too.

##### Example Code 4-4-2
###### New Code (Line 1, Lines 23-26, Line 29)
```python==1
from pystubit.board import buzzer, display, lightsensor
from pyatcrobo2.parts import UltrasonicSensor
import time

us = UltrasonicSensor('P0')

def play_theremin(**kwargs):
    while True:
        value = lightsensor.get_value()
        for p, _range in kwargs.items():
            if value > _range[0] and value <= _range[1]:
                pitch = p
                break
        else:
            pitch = None

        if pitch:
            display.show(pitch[0], delay=0, color=(5, 0, 0))
            
            distance = us.get_distance()
            time.sleep_ms(20)

            if distance < 8:
                buzzer.on(pitch)
            else:
                buzzer.off()
        else:
            display.clear()
            buzzer.off()

play_theremin(C4=(0, 1000), D4=(1000, 2000), E4=(2000, 3000))
```

Now your program is finished! Now try running the program and see if you can make the Buzzer play these three notes! Once you have this program working, you can try modifying it by making the arguments in the `play_theremin()` function accept a wider range of notes!

## Challenge: A Wider Range of Notes
Your challenge this time is to upgrade your model theremin so when your hold your hand farther away from the Ultrasonic Sensor it plays the notes set up the program arguments an octave higher!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-2/5-2_9_E.png"/>

##### Hints

Add new conditions to the branching code on lines 23-26 of Example Code 4-4-2.

### Example Program
Start by using Example Code 4-4-1 to pick thresholds marking both the outer and inner boundaries of a new position you can use to make your theremin play its notes in a higher octave.

In our example program we're using the distance 12-24 centimeters away from the sensor for this position.

Next use your thresholds to add a new elif statement that branches the program based on the Ultrasonic Sensor's value.

```python==25
elif distance >= 16 and distance < 24:
```

This branch will make strings representing notes one octave higher than the current selected note. The note names are strings made up of letters and numbers, like `'C4'`. To raise the octave of a note, you just need to raise the number in its name by one! That means that by using these string like sequence data, we can get to the location of the numbers in them with `pitch[1]` and use the function `int()` to change them into numeric data and add 1. Then we can put the result of that calculation through the `str()` function to turn it back into a string and add it to `pitch[0]` to make a new string representing the one octave higher note. Finally, save this string in the variable `new_pitch` and use the `on()` method to play the note!

```python==25
elif distance >= 16 and distance < 24:
    new_pitch = pitch[0] + str(int(pitch[1])+1)
    buzzer.on(new_pitch)
```

Now your program is finished! The completed program should look like this:

##### Example Code 5-1-1
###### New Code (Lines 25-27)
```python==1
from pystubit.board import buzzer, display, lightsensor
from pyatcrobo2.parts import UltrasonicSensor
import time

us = UltrasonicSensor('P0')

def play_theremin(**kwargs):
    while True:
        value = lightsensor.get_value()
        for p, _range in kwargs.items():
            if value > _range[0] and value <= _range[1]:
                pitch = p
                break
        else:
            pitch = None

        if pitch:
            display.show(pitch[0], delay=0, color=(5, 0, 0))
            
            distance = us.get_distance()
            time.sleep_ms(20)

            if distance < 8:
                buzzer.on(pitch)
            elif distance >= 16 and distance < 24:
                new_pitch = pitch[0] + str(int(pitch[1])+1)
                buzzer.on(new_pitch)
            else:
                buzzer.off()
        else:
            display.clear()
            buzzer.off()

play_theremin(C4=(0, 1000), D4=(1000, 2000), E4=(2000, 3000))
```

## Conclusion
### A Quick Review
In this lesson we learned...

* How to define functions with variable-length arguments
* You can define a variable-length positional argument by putting one asterisk (`*`) in front of its name. Arguments defined this way will store the data you give them as a tuple!
* You can define a variable-length keyword argument by putting two asterisks in front of its name. Arguments defined this way will store the data you give them as items in a dictionary with argument names as keys!
* Putting a **continue statement** inside a for or while loop lets you skip the rest of the loop's code to start the next iteration of that loop.
* Putting a **break statement** inside a for or while loop lets you immediately stop the loop.
* If you add an **else statement** after a for or while statement, you can add further code that runs only if the loop isn't cut off by a break statement.

All of this is important new syntax, so make sure you understand it before you move on to the next lesson!

### Coming Up Next Time!
In the next lesson, you'll learn about getting input (reading) from files and sending output (writing) to files.