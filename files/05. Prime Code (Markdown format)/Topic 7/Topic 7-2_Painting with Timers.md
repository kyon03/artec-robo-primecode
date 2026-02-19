---
tags: Python_English, Topic 7
---

Python Robotics Course Lesson 26

Topic 7-2
Painting with Timers
===

Use an Accelerometer to make a game!

## In This Lesson...
In this lesson, we’ll continue where we left off in the last lesson as we continue to program slightly complex games. We’ll also be learning about a new concept called **list comprehension**, which is a convenient way to keep your programs short!



:::info

Lesson Introduction

■ Lesson 26 Contents
1. Lists, Dictionaries, and List Comprehension
2. Making a Painting Game
3. Challenge: Adding a Difficulty Selection Feature


In this lesson, we’ll be making a game which uses our Core Unit’s LED display and Accelerometer.

The new Python syntax we’ll study in the first half of the lesson is **list comprehension** of lists and dictionaries. Let’s say that we had a series of numbers like **1, 2, 3, 4, 5***. List comprehension allows you to generate ordered data before storing it inside of a list or a dictionary! We’ll be creating several example programs to learn what’s so special about list comprehension as well as how to code it.

In the second half of the lesson, we’ll turn your LED display into a canvas and make a **painting game** where you use your brush to paint the canvas within the time limit!



In this game, you’ll use your red dot brush to paint by tilting your Core Unit. If you can brush every LED to change its color within the time limit, you win!

In the challenge at the end of the lesson, we’ll add a feature which lets the player change the game’s difficulty before starting the game.

Now let's start the lesson!

:::

## New Python Syntax

Lists contain regularly-ordered values like **1, 2, 3, 4, 5...** or *2, 4, 6, 8, 10...**. Writing programs using **list comprehension** is a convenient way to generate and then store values like these! Now let’s make a simple example program to learn about it!

### List Comprehension
The simplest way to code list comprehension is to combine a formula, `for` statement, and sequence data.

```
[ **variable_1 formula** for **variable_1** in **range()** **sequence data like a function, list, or tuple** ]
```

Let’s say that we wanted to generate a list which contains the numbers **1** to **9**. We’d code it like this:

##### Example Code 2-1-1
```python==1
_list = [num for num in range(1, 10)]
print(_list)
```

Run this code and you’ll see that our list contains every number from **1** to **9**!

#### Program Results
<pre class="prettyprint">
[1, 2, 3, 4, 5, 6, 7, 8, 9]
</pre>

You can put all sorts of operations inside of a formula. A good example would be multiplying each number by three by changing the `num` in Example Code 2-1-1 to `num * 3`:

##### Example Code 2-1-2
```python==1
_list = [num * 3 for num in range(1, 10)]
print(_list)
```

##### Program Results
<pre class="prettyprint">
[3, 6, 9, 12, 15, 18, 21, 24, 27]
</pre>

What’s more, you can also use list comprehension to combine multiple `for` statements and even `if` statements! Let’s take a look at some example code which uses these.

#### ■ List Comprehension with Multiple `for` Statements

Combining multiple `for` statements allows you to store values based on multiple pieces of sequence data in order! As an example, let’s try writing code to combine two `for` statements:

```
[ **variable_1 + variable_2** for **variable_1** in **sequence_data** for **variable_2** in **sequence_data** ]
```

Now let’s try combining two `for` statements to store the numerical sequence **11, 12, 21, 22, 31, 32** in a list!

##### Example Code 2-2-1
```python==1
_list = [i * 10 + j for i in range(1, 4) for j in range(1, 3)]
print(_list)
```
##### Program Results
<pre class="prettyprint">
[11, 12, 21, 22, 31, 32]
</pre>

If we were to write Example Code 2-2-1 without using list comprehension it would look like this:

```python==1
_list = []

for i in range(1,4):
    for j in range(1,3):
        _list.append(i * 10 + j)

print(_list)
```
 This program makes a nested loop with our `i` and `j` variables. `i * 10` calculates the **10s** place and `j` calculates the **1s** place before adding each number to the list.

#### ■ List Comprehension with `if` Statements

Following a `for` with an `if` statement allows you to use a condition to evaluate the values pulled from the sequence data. If the data matches the condition, the result gets stored in the list!

```
[ **variable_1** formula for **variable_1** in **sequence_data** if **condition** ]
```

For example, if we wanted to take any value greater than **5** out of a list, we would code it like this:

##### Example Code 2-3-1
```python==1
mylist = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
_list = [num for num in mylist if num > 5]
print(_list)
```

##### Program Results
<pre class="prettyprint">
[6, 7, 8, 9, 10]
</pre>

If we were to write Example Code 2-3-1 without using list comprehension it would look like this:

```python==1
mylist = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
_list = []

for num in mylist:
    if num > 5:
        _list.append(num) 

print(_list)
```

Code you write using list comprehension is not only simpler and easier to read, it also takes a lot less time to process!

### Dictionary Comprehension

You can use list comprehension with dictionaries, too (but not with tuples, unfortunately)! The basic way to write this is shown below. You can use the elements of your sequence data to set the keys and definitions. Keep in mind that until Python 3.6, the dictionaries you make using dictionary comprehension won’t be in perfect order!

```
{ **key formula**:**value formula** for **variable** in **sequence data **}
```

Say that we wanted to assign every item in the list below a key which starts with **2000-**. We would write it like this:

##### Example Code 2-4-1
```python==1
fruits = ["banana", "apple", "strawberry", "pineapple"]
_dict = { "2000-"+str(num):fruits[num] for num in range(len(fruits)) }
print(_dict)
```

##### Program Results
<pre class="prettyprint">
{'2000-2': 'strawberry', '2000-3': 'pineapple', '2000-0': 'banana', '2000-1': 'apple'}
</pre>

And just like with lists, you can combine multiple `for` statements and use `if` statements!

The example code below, for instance, combines two `for` statements to make a dictionary. The keys of this dictionary are multiplication tables of **1 or 2** by **4 or 5**, and the definitions are the result of the operation!

##### Example Code 2-4-2

```python==1
_dict = {str(i)+"*"+str(j): i*j for i in range(1, 3) for j in range(4, 6)}
print(_dict)
```
##### Program Results
<pre class="prettyprint">
{'2*5': 10, '2*4': 8, '1*4': 4, '1*5': 5}
</pre>

This next example code adds an `if` statement to pull every word which has **5 or more letters** from the list. It then makes a dictionary with the word as the key and the length of word as the definition!

##### Example Code 2-4-3
```python==1
mywords = ["bike", "apple", "milk", "sugar", "violin"]
_dict = {word: len(word) for word in mywords if len(word) >= 5}
print(_dict)
```
##### Program Results
<pre class="prettyprint">
{'apple': 5, 'violin': 6, 'sugar': 5}
</pre>

List comprehension might take you some time to get used to, but you’ll really be programming in Python once you get the hang of it. Try thinking of how you might be able to use list comprehension to replace the lists and dictionaries in the programs you’re going to make from now on!

## Making a Painting Game

Now we’re going to make a game which uses our Core Unit’s LED display and onboard Accelerometer. Start by watching the video below to see how you’ll play the game.

##### Gameplay Video

:::info
<iframe src="https://player.vimeo.com/video/482886168" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>
:::

## Playing the Game

In this game, we’ll be treating our 5 x 5 LED display as a canvas. The player will use a red LED on the canvas as a brush, moving it around to paint the other spaces. You’ll have to tilt the Core Unit to move the brush. If you can manage to paint the entire canvas within the 10-second time limit, you win the game!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-2/7-2_1_E.png"/>


### Programming the Game

This game will need to have the following features:

##### Your Game Needs to...

* Move the brush (that’s your red LED) when you tilt your Core Unit
* Turn any LEDs you paint green
* Decide if you’ve painted all of the LEDs on the canvas
* Measure the amount of time that has passed

We’ll put these into a function called `game()`, which will work according to the flowchart below:

##### Program Outline

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-2/7-2_2_E.png"/>

Now let’s make each of these features in order!

#### ■ Importing Modules and Objects

Let’s start by importing the objects and modules we'll be using in our program. We’re going to use the A button to start our game when we press it. We’ll also be using the `sound` module which we saved to the Core Unit in the last lesson!

```python==1
from pystubit.board import button_a, display, accelerometer
import sound
import time
```

If you can’t find the `sound` module on your Core Unit, download it using the link below before transferring it!

###### ✦ Right click the link and choose **Save file as...** to save the file.

[sound.py](https://www.artec-kk.co.jp//school/cl/textbooks/material_en/topic_7-1/sound.py)

#### ■ Tilting to Paint / Painting LEDs Green

We’ll place our brush at the center of the LED display when the game starts. This current position will help us get the coordinates of the brush after it moves, so we’ll make two variables called `brush_x` and `brush_y` to record our coordinates.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-2/7-2_3_E.png"/>

We’ll put the main processes of the game into our `game()` function. Let’s declare the function. At the beginning, we’ll put its initial coordinates (`x=2`, `y=2`) into `brush_x` and `brush_y` and make its LED red!

###### New Code (Lines 5-8)
```python==1
from pystubit.board import button_a, display, accelerometer
import sound
import time

def game():
    brush_x = 2
    brush_y = 2
    display.set_pixel(brush_x, brush_y, (31, 0, 0))
```

Next, we’ll make the brush move up, down, left, and right when we tilt our Core Unit.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-2/7-2_4_E.png"/>

Once we have four directions, we can combine them to make the brush move diagonally, too!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-2/7-2_5_E.png"/>

We can determine a tilt to the **left or right** by looking up the Accelerometer’s **X-axis** values, and a tilt **forward or backward** by looking up its **Y-axis** values!

##### Accelerometer Axes and Directions
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-2/7-2_6_E.png"/>

In order to check our Accelerometer’s values, make a new program before copying and pasting the code below into it.

##### Example Code 3-2-1

```python==1
from pystubit.board import accelerometer
import time

while True:
    print(accelerometer.get_values())
    time.sleep_ms(500)
```

Now let’s think of a condition which will let your Core Unit know when it’s tilted left, right, forward, and backward. We’re going to take conditions like the ones we have here and put them into our program.

|Direction|Condition|
|:---|:---|
|Tilted left|X-axis value is less than -2|
|Tilted right|X-axis value is greater than 2|
|Tilted forward|Y-axis value is less than -2|
|Tilted backward|Y-axis value is greater than 2|

Now let’s go back to our main program to program these conditions. We’ll combine them with a condition which freezes the coordinates when the brush reaches the edge of the LED display (this means our **X-axis** and **Y-axis** values are **0** or **4**)!

###### New Code (Lines 10-19)

```python==1
from pystubit.board import button_a, display, accelerometer
import sound
import time

def game():
    dot_x = 2
    dot_y = 2
    display.set_pixel(dot_x, dot_y, (31, 0, 0))
	
    while True:
        vals = accelerometer.get_values()
        if vals[0] < -2 and not brush_x == 0:  # Tilted left
            brush_x -= 1
        elif vals[0] > 2 and not brush_x == 4:  # Tilted right
            brush_x += 1
        if vals[1] < -2 and not brush_y == 0:  # Tilted forward
            brush_y -= 1
        elif vals[1] > 2 and not brush_y == 4:  # Tilted backward
            brush_y += 1
```

Take note that line 16 uses an `if` statement rather than an `elif` statement. Using an `elif` statement here would mean our brush couldn’t move diagonally because it couldn’t move along the X- and Y-axis at the same time!

Now let’s make any LED red before it moves. We only need to change an LED’s color if our brush moves. We can do this by having our program record the brush’s coordinate and checking to see if it changes!

###### New Code (Lines 11-12, Lines 22-24)

```python==5
def game():
    brush_x = 2
    brush_y = 2
    display.set_pixel(brush_x, brush_y, (31, 0, 0))
    
    while True:
        old_brush_x = brush_x  # Records X coordinate before moving
        old_brush_y = brush_y  # Records Y coordinate before moving
        vals = accelerometer.get_values()
        if vals[0] < -2 and not brush_x == 0:
            brush_x -= 1
        elif vals[0] > 2 and not brush_x == 4:
            brush_x += 1
        if vals[1] < -2 and not brush_y == 0:
            brush_y -= 1
        elif vals[1] > 2 and not brush_y == 4:
            brush_y += 1
        if brush_x != old_brush_x or brush_y != old_brush_y:
            display.set_pixel(brush_x, brush_y, (31, 0, 0))
```

Now let’s make the LED we just painted turn green!

###### New Code (Line 24)

```python==22
        if brush_x != old_brush_x or brush_y != old_brush_y:
            display.set_pixel(brush_x, brush_y, (31, 0, 0))
            display.set_pixel(old_brush_x, old_brush_y, (0, 31, 0))
```

Last, we’ll set the speed at which our brush moves. This speed is determined by how long we make the brush wait until it can move again. We’ll set it to **100 milliseconds** for now. Feel free to adjust it once you’ve played the game later in the lesson!

###### New Code (Line 25)
###### ★ Make sure everything is indented correctly!

```python==22
        if brush_x != old_brush_x or brush_y != old_brush_y:
            display.set_pixel(brush_x, brush_y, (31, 0, 0))
            display.set_pixel(old_brush_x, old_brush_y, (0, 31, 0))
        time.sleep_ms(100)
```

Now let’s run our program to see how it works so far. Click the **Run** button at the top of the screen. Now call the `game()` function in the terminal!

<pre class="prettyprint">
&gt;&gt;&gt; game()
</pre>

#### ■ Checking if the Canvas is Painted

The player wins the game if they’ve painted every LED green before the time limit is up. Now let’s write the process which judges whether they’ve done this!

We’ll do this by using a Boolean `True` or `False` value to manage each individual LED. Once every single LED’s value is `True`, the game knows that you’ve painted the whole canvas!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-2/7-2_7_E.png"/>

We’ll make a `canvas` dictionary at the start of our `game()` function. This dictionary uses a tuple with the X and Y value of each LED as a key and a Boolean value of `True` or `False` as the definition. Our game should start by looking like the picture on the left. Let’s use list comprehension to write the code which does this!

###### New Code (Lines 9-10)

```python==5
def game():
    brush_x = 2
    brush_y = 2
    display.set_pixel(brush_x, brush_y, (31, 0, 0))
    canvas = {(x, y): False for x in range(5) for y in range(5)}  # List comprehension
    canvas[(brush_x, brush_y)] = True  # Places brush in the center of the screen when the game starts
```

This is an example of how you can use tuples as the keys in a dictionary. We also used list comprehension to make our code much, much shorter!

Next, we’ll add the code which turns the value of an LED to `True` when you paint it with your brush. Since our key is a tuple with the X and Y value of the LED, we can put this tuple inside of `[]` when we call it!

###### New Code (Line 27)

```python==12
    while True:
        old_brush_x = brush_x
        old_brush_y = brush_y
        vals = accelerometer.get_values()
        if vals[0] < -2 and not brush_x == 0:
            brush_x -= 1
        elif vals[0] > 2 and not brush_x == 4:
            brush_x += 1
        if vals[1] < -2 and not brush_y == 0:
            brush_y -= 1
        elif vals[1] > 2 and not brush_y == 4:
            brush_y += 1
        if brush_x != old_brush_x or brush_y != old_brush_y:
            display.set_pixel(brush_x, brush_y, (31, 0, 0))
            display.set_pixel(old_brush_x, old_brush_y, (0, 31, 0))
            canvas[(brush_x, brush_y)] = True
        time.sleep_ms(100)
```

Once the brush has stopped moving, we’ll check whether or not the canvas is fully painted. We’ll check each entry in `canvas`. If even one of those is `False`, the canvas hasn’t been painted yet! If the canvas is painted, we’ll end the game and end the `while` statement outside of this code.

###### New Code (Lines 30-33)

```python==12
    while True:
        old_brush_x = brush_x
        old_brush_y = brush_y
        vals = accelerometer.get_values()
        if vals[0] < -2 and not brush_x == 0:
            brush_x -= 1
        elif vals[0] > 2 and not brush_x == 4:
            brush_x += 1
        if vals[1] < -2 and not brush_y == 0:
            brush_y -= 1
        elif vals[1] > 2 and not brush_y == 4:
            brush_y += 1
        if brush_x != old_brush_x or brush_y != old_brush_y:
            display.set_pixel(brush_x, brush_y, (31, 0, 0))
            display.set_pixel(old_brush_x, old_brush_y, (0, 31, 0))
            canvas[(brush_x, brush_y)] = True
        time.sleep_ms(100)

        for val in canvas.values():
            if val is False:
                break  # Ends for statement on line 30
        else:  # Run only when break statement on line 32 includes for statement on line 30
            break  # Ends while statement on line 12
```

In the code we just added, the `for` statement on line 30 checks our `canvas` dictionary line-by-line for a `False` value. The first time it finds one, it uses the `break` statement on line 32 to end the `for` statement! The `else` statement on line 33 won’t run in this case, which means that the `while` statement on line 12 begins another loop. However, if the `break` statement on line 32 doesn’t finish, the `else` statement on line 33 will run. This means that the `break` statement on line 34 will end the `while` statement on line 12.

Last, before we end line 12’s `while` statement we need to add code which turns the entire canvas green by changing the color of the brush’s LED to green!

###### New Code (Line 34)

```python=30
        for val in canvas.values():
            if val is False:
                break
        else: 
            display.set_pixel(brush_x, brush_y, (0, 31, 0))
            break
```

Now let’s see how our program works so far! Since we’ll be testing this multiple times, let’s run `display.clear()` before our `game()` function to reset the LED display.

<pre class="prettyprint">
&gt;&gt;&gt; display.clear()
&gt;&gt;&gt; game()
</pre>

#### ■ Measuring Elapsed Time

Here we’re going to add a feature which measures the amount of time that has passed and checks whether you’ve cleared the game within the time limit.

We’ll start by making our time limit as a constant. To begin, we’ll set this limit as **10000 milliseconds** (10 seconds) to give you plenty of time to complete the game!

###### New Code (Line 5)

```python=1
from pystubit.board import button_a, display, accelerometer
import sound
import time

TIME_LIMIT = 10000
```

Next, we’ll put the `time` module’s `ticks_ms()` function at the beginning of our `game()` function. This will get the current time, which we’ll store in a variable called `start_time`. After that, let’s change the condition in the `while` statement to check if our **elapsed time (`time.ticks_diff(time.ticks_ms(), start_time)`)** is less than our **`TIME_LIMIT`**.

###### New and Changed Code (Lines 14-15)

```python=7
def game():
    brush_x = 2
    brush_y = 2
    display.set_pixel(brush_x, brush_y, (31, 0, 0))
    canvas = {(x, y): False for x in range(5) for y in range(5)}
    canvas[(brush_x, brush_y)] = True

    start_time = time.ticks_ms()
    while time.ticks_diff(time.ticks_ms(), start_time) < TIME_LIMIT:
        old_brush_x = brush_x
        old_brush_y = brush_y
```

Now the `break` statement will force the `while` statement to end if you’ve painted the canvas. The `while` statement will also end naturally once the time limit has been passed! We can use this to tell whether you’ve won or lost the game by adding the following code:

###### New Code (Lines 39-45)

```python=14
    start_time = time.ticks_ms()
    while time.ticks_diff(time.ticks_ms(), start_time) < TIME_LIMIT:
        old_brush_x = brush_x
        old_brush_y = brush_y
        vals = accelerometer.get_values()
        if vals[0] < -2 and not brush_x == 0:
            brush_x -= 1
        elif vals[0] > 2 and not brush_x == 4:
            brush_x += 1
        if vals[1] < -2 and not brush_y == 0:
            brush_y -= 1
        elif vals[1] > 2 and not brush_y == 4:
            brush_y += 1
        if brush_x != old_brush_x or brush_y != old_brush_y:
            display.set_pixel(brush_x, brush_y, (31, 0, 0))
            display.set_pixel(old_brush_x, old_brush_y, (0, 31, 0))
            canvas[(brush_x, brush_y)] = True
        time.sleep_ms(100)

        for val in canvas.values():
            if val is False:
                break
        else:
            display.set_pixel(brush_x, brush_y, (0, 31, 0))
            break
    else:  # Lose (the while statement on line 14 ends normally without the break statement on line 38)
        sound.failed()  # Lose sound (uses function from the sound module)
        display.clear()
        return  # This line ends the game() function when you lose
    # Win (if the return statement on line 42 doesn’t end the game function)
    sound.failed()  # Win sound (uses function from the sound module)
    display.clear()
```

The `else` statement on line 39 only runs if line 38’s `break` statement doesn’t force the `while` statement on line 15 to end. The `return` statement on line 42 will then end the `game()` function. We usually use `return` statements to return the value of a standard function, but you can see here that we can use one without a return value, too! Running the `return` statement means that the code inside of the function which follows it won’t run. This means that everything after line 43 only runs if you win!

And now we’ve finished making the main part of our game! Since we’ve made it this far, let’s add code which lets us start the game by pressing the A button instead of running the `game()` function in the terminal.

###### New Code (Lines 47-54)

```python=47
def main():
    while True:
        if button_a.is_pressed():
            sound.countdown()
            game()


main()
```

Check below to see what the finished program should look like. Test your own program and check it against the example code if it’s not working correctly!

##### Example Code 3-2-2
```python==1
from pystubit.board import button_a, display, accelerometer
import sound
import time

TIME_LIMIT = 10000

def game():
    brush_x = 2
    brush_y = 2
    display.set_pixel(brush_x, brush_y, (31, 0, 0))
    canvas = {(x, y): False for x in range(5) for y in range(5)}
    canvas[(brush_x, brush_y)] = True
    
    start_time = time.ticks_ms()
    while time.ticks_diff(time.ticks_ms(), start_time) < TIME_LIMIT:
        old_brush_x = brush_x
        old_brush_y = brush_y
        vals = accelerometer.get_values()
        if vals[0] < -2 and not brush_x == 0:
            brush_x -= 1
        elif vals[0] > 2 and not brush_x == 4:
            brush_x += 1
        if vals[1] < -2 and not brush_y == 0:
            brush_y -= 1
        elif vals[1] > 2 and not brush_y == 4:
            brush_y += 1
        if brush_x != old_brush_x or brush_y != old_brush_y:
            display.set_pixel(brush_x, brush_y, (31, 0, 0))
            display.set_pixel(old_brush_x, old_brush_y, (0, 31, 0))
            canvas[(brush_x, brush_y)] = True
        time.sleep_ms(100)

        for val in canvas.values():
            if val is False:
                break
        else:
            display.set_pixel(brush_x, brush_y, (0, 31, 0))
            break
    else:
        sound.failed()
        display.clear()
        return

    sound.conguratulation()
    display.clear()
    
def main():
    while True:
        if button_a.is_pressed():
            sound.countdown()
            game()


main()
```

## Challenge: Changing the Difficulty of the Game

Many of the games you buy give the player an option to change the difficulty of the game, and there are plenty of ways that we can use to change the difficulty of the painting game we made here! The simplest way would be to **change the time limit**. This means that we can adjust the difficulty by changing the value of our `TIME_LIMIT` constant.

```python=1
from pystubit.board import button_a, display, accelerometer
import sound
import time

TIME_LIMIT = 10000  # The shorter the time limit, the harder the game is
```

If we change the time limit ahead of time, however, our player won’t be able to select a difficulty. This is why we’re going to modify Example Code 3-2-2 in this challenge to allow the player to choose from an `Easy`, `Normal`, or `Hard` mode before the game starts! We’re also going to change the colors of the LED for each difficulty so we can tell which mode the player has selected.

##### Difficulty and Time Limit

|Mode (Difficulty)|Time Limit|
|:---|:---|
|`Easy`|10000 milliseconds (10 seconds)|
|`Normal`|8000 milliseconds (8 seconds)|
|`Hard`|6000 milliseconds (6 seconds)|

##### Mode and LED Color

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-2/7-2_8_E.png"/>

We’ll have to use the B button to choose a mode. Let’s make `Easy` the default mode and put our difficulty selection process inside of our `main()` function.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-2/7-2_9_E.gif"/>

### Example Program

Now we’ll add code to do the following two things to Example Code 3-2-2:

* Use the B button to select the mode
* Change the time limit and LED color for each mode

Now let’s code each one in order!

#### ■ Selecting the Mode with the B Button

We’ll start the program by adding our three modes as well as defining constants for their time limits and colors (that’s the color of the paintbrush)!

|Constant|Data Structure|Values|
|:---|:---|:---|
|`MODE`|Tuple|`（"Easy", "Normal", "Hard"）`|
|`TIME_LIMIT`|Dictionary|Time limits for each mode:</br>`{"Easy":10000, "Normal":8000, "Hard":6000}`|
|`PAINT_COLOR`|Dictionary|LED colors for each mode:</br>`{"Easy":(0, 31, 0), "Normal":(0, 0, 31), "Hard":(31, 0, 31)}`|

Next, we’ll create a variable called `active_mode` to store the mode the player currently has selected. And don’t forget to import and object for button B!

###### New and Changed Code (Line 1, Lines 5-8)

```python=1
from pystubit.board import button_a, button_b, display, accelerometer
import sound
import time

MODE = ("Easy", "Normal", "Hard")
TIME_LIMIT = {"Easy": 10000, "Normal": 8000, "Hard": 6000}
PAINT_COLOR = {"Easy": (0, 31, 0), "Normal": (0, 0, 31), "Hard": (31, 0, 31)}
active_mode = MODE[0]  # Default mode is "Easy"
```

Next, let’s add the process which allows us to use the B button to switch between modes to our `main()` function. To do this, we’ll use the tuple’s `index()` method, which looks like the index of an element in the tuple. This method will let us find the index of `MODE`, which holds the active mode, before requesting the next index. But be careful! We’ll have to go back to **0** once we reach the last index. We’ll also have to use the `display` object’s `scroll()` method to let the player know when the mode has changed.

##### New Code (Lines 42-43, Lines 49-53)

```python=41
def main():
    global active_mode
    display.scroll(active_mode, delay=20, color=PAINT_COLOR[active_mode])
    
    while True:
        if button_a.is_pressed():
            sound.countdown()
            game()
        if button_b.is_pressed():
            index = MODE.index(active_mode)
            index = index + 1 if index < len(MODE) - 1 else 0
            active_mode = MODE[index]
            display.scroll(active_mode, delay=20, color=PAINT_COLOR[active_mode])
```

`active_mode` is a global variable. If we want to change its value inside of our `main()` function we’ll have to declare it as **global** on line 42! In order to let the player know which mode is the default one, let’s show it on the LED display immediately after we call the `main()` function.

## Setting Mode Time Limit and Color

Now that we can select modes, let’s change our program to change the time limit and color for the mode which we select! We’ll have to change `TIME_LIMIT` in Example Code 3-2-2 to `TIME_LIMIT[active_mode]` and `(0, 31, 0)` to `PAINT_COLOR[active_mode]`. Now your program is finished!

##### Example Code 4-1-1
###### New and Changed Code (Line 18, Lines 32, Line 40)

```python=1
from pystubit.board import button_a, button_b, display, accelerometer
import sound
import time

MODE = ("Easy", "Normal", "Hard")
TIME_LIMIT = {"Easy": 10000, "Normal": 8000, "Hard": 6000}
PAINT_COLOR = {"Easy": (0, 31, 0), "Normal": (0, 0, 31), "Hard": (31, 0, 31)}
active_mode = MODE[0]

def game():
    brush_x = 2
    brush_y = 2
    display.set_pixel(brush_x, brush_y, (31, 0, 0))
    canvas = {(x, y): False for x in range(5) for y in range(5)}
    canvas[(brush_x, brush_y)] = True
    
    start_time = time.ticks_ms()
    while time.ticks_diff(time.ticks_ms(), start_time) < TIME_LIMIT[active_mode]:
        old_brush_x = brush_x
        old_brush_y = brush_y
        vals = accelerometer.get_values()
        if vals[0] < -2 and not brush_x == 0:
            brush_x -= 1
        elif vals[0] > 2 and not brush_x == 4:
            brush_x += 1
        if vals[1] < -2 and not brush_y == 0:
            brush_y -= 1
        elif vals[1] > 2 and not brush_y == 4:
            brush_y += 1
        if brush_x != old_brush_x or brush_y != old_brush_y:
            display.set_pixel(brush_x, brush_y, (31, 0, 0))
            display.set_pixel(old_brush_x, old_brush_y, PAINT_COLOR[active_mode])
            canvas[(brush_x, brush_y)] = True
        time.sleep_ms(100)

        for val in canvas.values():
            if val is False:
                break
        else:
            display.set_pixel(brush_x, brush_y, PAINT_COLOR[active_mode])
            break
    else:
        sound.failed()
        display.clear()
        return
    sound.conguratulation()
    display.clear()
    
def main():
    global active_mode
    display.scroll(active_mode, delay=20, color=PAINT_COLOR[active_mode])
    
    while True:
        if button_a.is_pressed():
            sound.countdown()
            game()
        if button_b.is_pressed():
            index = MODE.index(active_mode)
            index = index + 1 if index < len(MODE) - 1 else 0
            active_mode = MODE[index]
            display.scroll(active_mode, delay=20, color=PAINT_COLOR[active_mode])


main()
```

## Conclusion

### A Quick Review

In this lesson we learned how to use **list comprehension** to make lists and dictionaries. List comprehension allows you to keep your programs tidy, which is what Python is all about!

We also continued on from our last lesson as we took on the challenge of making our painting game. While this game is pretty complex, the extra difficulty modes we added in the challenge let the player enjoy the game for even longer.

Try thinking of ways to increase the difficulty of your games, whether it’s the ones we’ll make after this or in games which you’ve already made!

### Coming Up Next Time!

In the next lesson we’re going to make a game with an even more complex structure! There we’ll start learning how to **control multiple objects** as we apply the same type of thinking used to handle frames in films and video games!
