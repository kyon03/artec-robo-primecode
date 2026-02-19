---
tags: Python_English, Topic 7
---

Python Robotics Course Lesson 25

Topic 7-1
Playing Ninja with the Command Line
===

Make games using modules!

## In This Lesson...
In Topic 7, we’re going to take the Python syntax we’ve learned so far and use it to program games with complex processes. In this lesson, we’re going to learn how to turn the common features we need to make a game into easily-managed modules and packages!

:::info

Lesson Introduction

■ Lesson 25 Contents
1. Modules and Packages
2. Making a Ninja Game
3. Challenge: Changing Difficulty with New Actions


In Topic 7 we’ll take what we’ve learned so far and use it to develop slightly more complex programs for games.

We’ll begin this lesson by reviewing what modules and packages are and what they do. When developing a complex program with a lot of moving parts like a game, using ready-made components like modules and packages is a much more effective way to program compared to writing a new program from scratch!

In the second half of the lesson, we’ll use pre-made ASCII art and sound modules to develop a ninja game where you take down opponents with *shuriken*!



This game uses Mu Editor’s terminal to show ASCII art, which is a kind of art made up of letters, numbers, and other symbols. The player will only have a split-second to tell friend from foe and wave their hand over the Core Unit’s Light Sensor to throw their *shuriken*. They’ll win points for defeating a bad ninja and lose points for attacking a friend! If they reach a certain score once time is up, they win!

Last, we’ll take on the challenge of adding a new action to our game to increase its difficulty.

Now let's start the lesson!

:::

## New Python Syntax

Let’s start by taking another look at what exactly packages and modules are.

### What’s a Module?

A **module** is a **set of related program components**. Putting a block of code which appears several times in a program into one file and giving it a name makes it into a module! Let’s use the table below to take another look at some of the Python, Core Unit, and ArtecRobo modules we’ve used so far.

##### Native Python Modules

|Module|What does it do?|
|:---|:---|
|time|Holds functions which get the current time and measure how much time has passed.|
|random|Holds a function for use when you want to add randomization to your program.|

##### Core Unit Modules

|Module|What does it do?|
|:---|:---|
|dsply|Holds classes which are used to control the LED display.|
|image|Holds classes which handle images shown on the LED display.|
|bzr|Holds classes which are used to control the Buzzer.|
|button|Holds classes which handle the Core Unit’s buttons.|
|board|Holds instances made from the classes for each part of your Core Unit.|

##### ArtecRobo Modules

|Module|What does it do?|
|:---|:---|
|bzr|Holds classes which are used to control ArtecRobo electronic parts.|

### What’s a Package?

A **package** is a **bundle of multiple modules**.

If you think of a package like the tool chest in a workshop, the modules would be the drawers in the chest, while those drawers would store tools like variables, functions, and classes. In other words, they form a nested structure which follows the order of package -> modules -> variables, functions, and classes!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-1/7-1_1_E.png"/>

You’ll find the Core Unit and ArtecRobo modules listed above inside of the following packages:

##### Packages Containing the Core Unit and ArtecRobo Modules

|Package|Main Modules|
|:---|:---|
|pystubit|dsply, image, bzr, button, board|
|pyatcrobo2|parts|

### Opening a Package

Let’s use Mu to find out what’s inside of our `pyatcrobo2` package!


Connect a USB cable to your Core Unit. Now click **Files**. In the window that opens, look for the `pyatcrobo2` folder and click it to show its content. You’ll be able to see that it contains several modules, including a `parts` module.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-1/7-1_2_E.png"/>

Whenever we want to treat a folder as a package like this, we need to include an `__init__.py` folder inside of it. This file lets Python know that this folder is a package!

###### ★ `__init__.py` can be empty!



### Importing Modules and Their Variables, Functions, and Classes

Let’s take a moment and review how to load modules as well as the variables, functions, and classes they contain.

If you’d like to use a variable, function, or class from a module, you have two choices: import and then call that module, or import that specific variable, function, or class directly from the module!

Let’s take a look at a program which uses the `StuduinoBitDisplay` class from the `pystubit` package’s `display` class as an example.

##### Importing and Calling a Module

```python==1
from pystubit import dsply

display = dsply.StuduinoBitDisplay()
display.scroll("Hello!")
```

##### Importing a Class from a Module

```python==1
from pystubit.dsply import StuduinoBitDisplay

display = StuduinoBitDisplay()
display.scroll("Hello!")
```

One of the best things about importing a specific variable, function, or class is that you can leave out the name of the module! However, items with identical names imported from multiple classes may clash in your program, which can cause errors. If you do want to use items with identical names in your program, make sure to import their module first before calling them!

### The Benefits of Modules and Packages

Instead of making a single file with hundreds or even thousands of lines of code, putting blocks of code into modules or packages lets you keep your programs clean and manageable! It also cuts down on a lot of work when you need to edit your program in addition to letting you easily add new features.

## Making a Ninja Game

Now we’re going to take a new module and use it to make a game! This game will use Mu’s REPL window as a screen. In this **ninja-themed** game, your allies and enemies will appear at random. If an enemy appears, you’ll have to throw *shuriken* at them to take them down! In order to throw a *shuriken*, just pass a hand over your Core Unit’s Light Sensor.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-1/7-1_3_E.png"/>

Take a look at the video below to see how to play the game.

##### Gameplay Video

:::info
<iframe src="https://player.vimeo.com/video/482885657" width="640" height="360" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>
:::

### Introducing the Characters

The **enemies** and **allies** in this game appear as **ASCII art** in the terminal, and the player has a split-second to tell which is which before attacking!

###### ★ ASCII art is a kind of art made using letters, numbers, and symbols on a computer! ASCII art can be anything from single-line emojis to elaborate pictures which take up a whole page!

##### Enemies
* Bad Ninja A

```
       X
  ∠￣＼∩
   |/ﾟUﾟLﾉ
　~~( ニ⊃
　  ( ヽ/
   ﾉ>ノ
```

* Bad Ninja B

```
  ∩
⊂ ＼
 /ヽ ）
⊂二 ,）~~
/|,∩,/|
U＼＿フ
```

##### Allies
* The Princess

```
　 ∧M∧
  (ﾟーﾟ*)
⊂(|０|)⊃
　 ﾉ~~~ヽ
  ﾉ~~~~~ヽ
   ∪ ∪
```

* King Meow

```
 　 ∧＿∧
　　(  ﾟ∀ﾟ)
　　/~~〉〈/つ
　ノ ノ｜ ｜､
 ~(＿_)__)ゝ~~~
~~~~~~~~~~~
```

 The display will also show the player’s ninja when it throws *shuriken*!

##### The Player Character

```
　　 ∧_∧ ∩
　　<＃･+･>彡　　-==✦
　 ／ 三三つﾆつ-=≡✦
　/　　/ミヽ -=✦ -==✦
／／~> >　∪　-=≡✦
＼) (＿)
```

### The Rules of the Game

The player scores points by throwing *shuriken* when an enemy appears and not doing anything when an ally appears. If they fail to attack an enemy or accidently attack an ally, they lose points!

|  |Bad Ninja A| Bad Ninja B|Princess|King Meow|
|:---|:---|:---|:---|:---|
|**Attack**|+1|+1|-1|-1|
|**Don’t attack**|-1|-1|+1|+1|

The game will start with a 30 second round. If the player can get 7 or more points by the end of the round, they win!

### Preparing the Game Modules

Some game development software comes with tools, or **assets** that you can use to make your game, like character sprites and music. Assets like these allow people to make games, even if they don’t know how to draw or compose music themselves! There are even websites which allow people to post their artwork and music for free, and plenty of games are developed using sites like these.

Here, we’re going to speed along our own game development process by using modules which include ASCII art and some simple sound effects.

#### ■ Terminal ASCII Art Module

Click the link below to download **`picture.py`** before transferring it to your Core Unit.

###### ✦ Right click the link and choose **Save file as...** to save the file.

[picture.py](https://www.artec-kk.co.jp//school/cl/textbooks/material_en/topic_7-1/picture.py)

Inside this module we’ll find constants which store the text strings of the ASCII art.

* **Numbers**
From left to right: `ZERO`, `ONE`, `TWO`, `THREE`, `FOUR`, `FIVE`, `SIX`, `SEVEN`, `EIGHT`, `NINE`

```
┏━━┓ ┏┓ ┏━━┓ ┏━━┓ ┏┓┏┓ ┏━━┓ ┏━━┓ ┏━━┓ ┏━━┓ ┏━━┓
┃┏┓┃ ┃┃ ┗━┓┃ ┗━┓┃ ┃┃┃┃ ┃┏━┛ ┃┏━┛ ┃┏┓┃ ┃┏┓┃ ┃┏┓┃
┃┃┃┃ ┃┃ ┏━┛┃ ┏━┛┃ ┃┗┛┃ ┃┗━┓ ┃┗━┓ ┗┛┃┃ ┃┗┛┃ ┃┗┛┃
┃┃┃┃ ┃┃ ┃┏━┛ ┗━┓┃ ┗━┓┃ ┗━┓┃ ┃┏┓┃   ┃┃ ┃┏┓┃ ┗━┓┃
┃┗┛┃ ┃┃ ┃┗━┓ ┏━┛┃   ┃┃ ┏━┛┃ ┃┗┛┃   ┃┃ ┃┗┛┃ ┏━┛┃
┗━━┛ ┗┛ ┗━━┛ ┗━━┛   ┗┛ ┗━━┛ ┗━━┛   ┗┛ ┗━━┛ ┗━━┛
```

* **Characters**
From left to right: `NINJA_A`, `NINJA_B`, `PRINCESS`, `KING`, `SHURIKEN`

```
       X     ∩　        ∧M∧  　   ∧＿∧             ∧_∧ ∩
  ∠￣＼∩   ⊂ ＼       (ﾟーﾟ*)     (  ﾟ∀ﾟ)     　 　<＃･+･>彡　　-==✦
   |/ﾟUﾟLﾉ   /ヽ ）   ⊂(|０|)⊃    /~~〉  〈/つ     ／ 三三つﾆつ-=≡✦
　~~( ニ⊃  ⊂二 ,）~~　 ﾉ~~~ヽ     ノ ノ｜ ｜､      /　　/ミヽ -=✦ -==✦
　  ( ヽ/  /|,∩,/|     ﾉ~~~~~ヽ   ~(＿_)__)ゝ~~~  ／／~> >　∪　-=≡✦
   ﾉ>ノ    U＼＿フ       ∪ ∪    ~~~~~~~~~~~     ＼) (＿)
```

* **Text**
From left to right: `OK`, `NG`

```

 *****  *  *    *   *  *****
 *   *  * *     **  *  *   
 *   *  **      * * *  *  ***
 *   *  * *     *  **  *   *
 *****  *  *    *   *  *****
```

* **Results Screen**
From top to bottom: `SUCCESS`, `FAILED`

```

　　　　 ∧＿∧         ／￣￣￣￣
　／＼（　・∀・）／ヽ ＜ You got {} points! Congratulations!
（ ☆　⊂　　　つ　☆ ） ＼＿＿＿＿
　＼/⊂、　 ノ　 ＼ノ
　　　　　し’
	 
  ∧＿∧　  ／￣￣￣￣
（  ･_･)　＜　{} points... Try again!
（ つ　と） ＼＿＿＿＿
 と＿）＿）
```


#### ■ SFX Module
This module contains the simple sound effects we’ll use in our game. Click the link below to download **`sound.py`** before transferring it to your Core Unit. We’ll be using this module in future lessons, too!

###### ✦ Right click the link and choose **Save file as...** to save the file.

[sound.py](https://www.artec-kk.co.jp//school/cl/textbooks/material_en/topic_7-1/sound.py)

This module contains the following functions.

|Function|What does it do?|
|:---|:---|
|`success()`|Plays a doorbell sound when you win the game.|
|`failed()`|Plays a buzzer noise when you lose.|
|`operation()`|Plays a short beep as the game progresses.|
|`incorrect()`|Plays a short buzz for an incorrect move.|
|`congratulation()`|Plays the do-re-mi scale.|
|`countdown()`|Plays a countdown sound as **3, 2, 1, 0** appears on the LED display.|

### Programming Command Line Ninja
Now we’re going to take these modules and use them to program our game! Your game will need to have the following features:

##### Your Game Needs to...
* Count down to the start of the round
* Measure the amount of time that has passed since the start of the round
* Pick a character at random to show in the terminal
* Detect whether you’ve passed your hand over the Light Sensor
* Determine if you win or lose points
* Determine whether you have a winning score once the time limit is up

The program will work according to the flowchart below. We’ll have to put the main processes of the game into the `start_game()` function and call this function from the terminal to run the game.

##### Program Outline

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-1/7-1_4_E.png"/>

Now let’s make each of these features in order!

#### ■ Importing Each Module

We’ll start by importing the objects and modules we'll be using in our program. In addition to `picture(.py)` and `sound(.py)`, we’ll need to add randomization to our game by importing the `random` module, and import the `time` module so our program can count how much time has passed!

```python=1
from pystubit.board import lightsensor, buzzer, Image, display
import picture  # Module we just saved to the Core Unit
import sound    # Module we just saved to the Core Unit
import random
import time
```

#### ■ The Countdown
In order to signal the start of the game, we want to show a countdown from **3** to **0** in the terminal. To keep this part of the program short, we’re going to store the ASCII art numbers in order inside of the tuple `numbers`.

###### New Code (Lines 7-12)

```python=1
from pystubit.board import lightsensor, buzzer, Image, display
import picture
import sound
import random
import time

numbers = (
    picture.THREE,
    picture.TWO,
    picture.ONE,
    picture.ZERO
)
```

Next, we’ll add the code to first declare our `start_game()` function, then pull each element from our `numbers` tuple in order and show them in the terminal. Besides showing the numbers in the terminal, we’ll need the Buzzer to play `”C6”` for 600 milliseconds, then wait 600 milliseconds before showing the next number.

###### New Code (Lines 14-18)

```python=1
from pystubit.board import lightsensor, buzzer, Image, display
import picture
import sound
import random
import time

numbers = (
    picture.THREE,
    picture.TWO,
    picture.ONE,
    picture.ZERO
)

def start_game():
    for number in numbers:
        print(number)
        buzzer.on("C6", duration=600)
        time.sleep_ms(600)
```

Now let’s call our `start_game()` function from the terminal. Before we test out how it works, we’ll need to adjust the size of our terminal window.

1. Move your mouse cursor to the bottom edge of the editor window. Notice how it changes shape? Now all you have to do is click and drag the cursor up or down to adjust the size of the window!
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-1/7-1_5_E.png"/>

2. Press the **Enter** key to fill the window with **>>>**, then adjust the window until it’s 7 lines tall.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-1/7-1_6_E.png"/>

Each piece of ASCII art is six lines tall. Since our `print()` function adds an extra line on the end, making the terminal window seven lines tall makes sure that only one piece of ASCII art is shown at a time!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-1/7-1_7_E.png"/>

#### ■ Counting Elapsed Time

Next, we’ll need our program to measure how much time has passed since the start of the round. Let’s start by defining our time limit of **30,000 milliseconds**, or **30 seconds**, in the constant `TIME_LIMIT`.

###### New Code (Line 7)
```python=1
from pystubit.board import lightsensor, buzzer, Image, display
import picture
import sound
import random
import time

TIME_LIMIT = 30000
```

Now let’s add code inside of our `start_game()` function to record the time the countdown ends and the round starts. We’ll then have to make our program repeat until the time limit is up. We’ll put the part of our program which shows the characters and calculates the score inside of this `while` statement.

###### New Code (Line 22-24)
```python=16
def start_game():
    for number in numbers:
        print(number)
        buzzer.on("C6", duration=600)
        time.sleep_ms(600)

    start_time = time.ticks_ms()  # Records the game start time
    while time.ticks_diff(time.ticks_ms(), start_time) < TIME_LIMIT:  # Calculates difference between game start time and current time and compares with the time limit
        pass # Write a pass statement since this is still empty
```

#### ■ Choosing and Showing a Random Character

We’ll need to use the `random` module’s `choice()` function in order to make a random character appear in the terminal. The `choice()` function takes a tuple as an argument and then picks one item from the tuple at random.

Here we’re going to take the characters who will appear in the terminal and put them together in a tuple called `characters`! In order to make a decision later in our program, we’ll give our enemies an `"enemy"` and our allies an `”ally”` attribute in our tuple.



###### New Code (Lines 14-19)
```python=1
from pystubit.board import lightsensor, buzzer, Image, display
import picture
import sound
import random
import time

TIME_LIMIT = 30000
numbers = (
    picture.THREE,
    picture.TWO,
    picture.ONE,
    picture.ZERO
)
characters = (
    (picture.NINJA_A, "enemy"),
    (picture.NINJA_B, "enemy"),
    (picture.PRINCESS, "ally"),
    (picture.KING, "ally")
)
```

Now let’s make our program show a random character. We’ll have our `operation()` function from the `sound` module play a sound effect every time a character appears.

###### New and Changed Code (Lines 29-31)

```python=21
def start_game():
    for number in numbers:
        print(number)
        buzzer.on("C6", duration=600)
        time.sleep_ms(600)

    start_time = time.ticks_ms()
    while time.ticks_diff(time.ticks_ms(), start_time) < TIME_LIMIT:
        character = random.choice(characters)
        sound.operation()
        print(character[0])  # Keep in mind that the first item in the tuple is an ASCII text string!
```

###### ★ Notice we’ve the deleted the `pass` statement under our `while` statement!

Let’s make each character appear for a random time from **0.5** to **1.5** seconds in **50-millisecond** increments.

###### New Code (Lines 30-31)

```python=21
def start_game():
    for number in numbers:
        print(number)
        buzzer.on("C6", duration=600)
        time.sleep_ms(600)
 
    start_time = time.ticks_ms()
    while time.ticks_diff(time.ticks_ms(), start_time) < LIMIT_TIME:
        character = random.choice(characters)
        wait_time = random.randint(10, 30) * 50  # Multiplies a random number from 10-30
        time.sleep_ms(wait_time)                 # to get a time of 500-1500 ms in 50 ms increments
        sound.operation()
        print(character[0])
```

In order to make our 50 millisecond increments, let’s take a random number from **10** to **30** and multiply it by 50 milliseconds. This is the same as choosing from a range of numbers starting with **500, 550...** and ending in **1,450, 1,500**!

#### ■ Detecting a Hand with the Light Sensor

The Light Sensor’s values become small when your hand passes over it. Knowing this, we can say that your hand has passed over the sensor once the value falls below the threshold we set.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_7-1/7-1_3_E.png"/>

In order to find our threshold, **open a new program** and run Example Code 3-4-1 below. Check your Light Sensor’s value when your hand is covering it. Now remove your hand and check again!

##### Example Code 3-4-1

```python=1
from pystubit.board import lightsensor
import time

while True:
    print(lightsensor.get_value())
    time.sleep_ms(500)
```

Let’s put the threshold we found in the `THRESHOLD` constant in our game program.

###### New Code (Line 8)

```python=1
from pystubit.board import lightsensor, buzzer, Image, display
import picture
import sound
import random
import time

TIME_LIMIT = 30000
THRESHOLD = 2000  # Put your own threshold here
```

Next, we’ll use our threshold as we add a process which checks whether a hand has passed over the Light Sensor.

When the player’s hand passes over the Light Sensor, the ninja will throw a *shuriken* to attack! We’ll need to make a variable called `is_attacked`, using it as a flag to check whether the player has attacked. We’ll give the `is_attacked` variable a starting value of `False`, and make it `True` when a hand has passed over the sensor.

In order to give the player only a split second to attack, we’ll make the program wait only **600 milliseconds** to detect the hand. The attack will be unsuccessful unless the player passes their hand over the Light Sensor!

###### New Code (Lines 36-39)

```python=22
def start_game():
    for number in numbers:
        print(number)
        buzzer.on("C6", duration=600)
        time.sleep_ms(600)

    start_time = time.ticks_ms()
    while time.ticks_diff(time.ticks_ms(), start_time) < TIME_LIMIT:
        character = random.choice(characters)
        wait_time = random.randint(10, 30) * 50
        time.sleep_ms(wait_time)
        sound.operation()
        print(character[0])

        is_attacked = False  
        input_start_time = time.ticks_ms()  # Point where the 600 millisecond count starts
        while time.ticks_diff(time.ticks_ms(), input_start_time) < 600:  # Only detects for 600 milliseconds
            pass # Write a pass statement since this is still empty
```

Inside of the `while` statement, we’ll compare the Light Sensor’s value with the threshold. If the player’s hand passes over the sensor, we’ll change `is_attacked` to `True` and show the `picture.SHURIKEN` throwing animation for 600 milliseconds!

###### New and Changed Code (Lines 39-43)

```python=36
        is_attacked = False
        input_start_time = time.ticks_ms()
        while time.ticks_diff(time.ticks_ms(), input_start_time) < 600:
            val = lightsensor.get_value()  # Gets value of Light Sensor
            if val < THRESHOLD:  # Compares with threshold
                is_attacked = True  # Judges shuriken is thrown and raises flag
                print(picture.SHURIKEN)  # Shows ASCII art for player's ninja
                time.sleep_ms(600)  # Shows in terminal for 600 ms
```

#### ■ Calculating the Score

The player will win or lose points depending on which character is on the screen when they attack! See the table below to find out which combinations win or lose points:

||Attack</br>`is_attacked == True`|No Attack</br>`is_attacked == False`|
|:---:|:---:|:---:|
|Enemy</br>`"enemy"`|+1|-1|
|Ally</br>`"ally"`|-1|+1|

Now let’s define a variable called `score` to record the total score! We’ll put it at the beginning of our `start_game()` function and give it a starting value of **0**.

###### New and Changed Code (Line 23)

```python=22
def start_game():
    score = 0
    
    for number in numbers:
        print(number)
        buzzer.on("C6", duration=600)
        time.sleep_ms(600)
```

Once we have a value for `is_attacked`, we’ll need to run the code which decides if you win or lose points. Let’s use the table above to make conditions for this. If the player gains points, we’ll need to show the `picture.OK` ASCII art and run the `sound` module’s `correct()` function. If they lose points, however, we need to show `picture.NG` and run the `incorrect()` function from the `sound` module!

###### New Code (Lines 47-64)

```python=38
        is_attacked = False
        input_start_time = time.ticks_ms()
        while time.ticks_diff(time.ticks_ms(), input_start_time) < 600:
            val = lightsensor.get_value()
            if val < THRESHOLD:
                is_attacked = True
                print(picture.SHURIKEN)
                time.sleep_ms(600)
        
        if is_attacked:  # If you attack
            if character[1] == "enemy":  # Item 2 in the tuple picks enemy or ally
                score += 1  # Gives 1 point for successful attack
                print(picture.OK)  # Shows OK ASCII art
                sound.correct()  # Success noise
            else:
                score -= 1  # Takes 1 point for unsuccessful attack
                print(picture.NG)  # Shows NG ASCII art
                sound.incorrect()  # Fail noise
        else:  # If you don\’t attack
            if character[1] == "enemy":
                score -= 1
                print(picture.NG)
                sound.incorrect()
            else:
                score += 1
                print(picture.OK)
                sound.correct()
```

Now let’s add code which waits **600 milliseconds** and prints **five empty lines** to clear the terminal before the loop repeats again!

###### New Code (Lines 66-67)

```python=56
        else:
            if character[1] == "enemy":
                score -= 1
                print(picture.NG)
                sound.incorrect()
            else:
                score += 1
                print(picture.OK)
                sound.correct()

        time.sleep_ms(600)
        print("\n\n\n\n\n")  # Clears terminal window
```

#### ■ Finding and Showing the Results

Last, we’ll need to add a process which decides if the player has a winning score of **7 points** or more before showing the results. We’ll start by defining that winning score in the constant `WIN_SCORE`.

###### New Code (Line 9)
```python=1
from pystubit.board import lightsensor, buzzer, Image, display
import picture
import sound
import random
import time

TIME_LIMIT = 30000
THRESHOLD = 2000
WIN_SCORE = 7
```

Once we’ve finished the loop in the `while` statement which checks whether time is up, let’s see if the player has won. Remember that we already have two ASCII pictures we can use to show the results: `picture.SUCCESS` and `picture.FAILED`!

* `picture.SUCCESS`
```
　　　　 ∧＿∧         ／￣￣￣￣
　／＼（　・∀・）／ヽ ＜ You got {} points! Congratulations!
（ ☆　⊂　　　つ　☆ ） ＼＿＿＿＿
　＼/⊂、　 ノ　 ＼ノ
　　　　　し’
```

* `picture.FAILED`
```
  ∧＿∧　  ／￣￣￣￣
（  ･_･)　＜　{} points... Try again!
（ つ　と） ＼＿＿＿＿
 と＿）＿）
```

You’ll notice that both of these pieces of ASCII art have a set of curly brackets (`{}`). You can use the `format()` method to switch out these brackets with a value or text string from somewhere else. This means that we can put the score in the `format()` method’s argument and show its return value in the terminal!

Depending on whether the player wins or loses, we’ll need to run different functions in our `sound` module to play the right sound!

###### New Code (Lines 69-74)
###### ★ Make sure everything is indented correctly!
```python=66
        time.sleep_ms(600)
        print("\n\n\n\n\n")

    if score >= WIN_SCORE:  # Game cleared
        print(picture.SUCCESS.format(score))  # Inserts score and shows win ASCII art
        sound.success()
    else:  # Game failed
        print(picture.FAILED.format(score))  # Inserts score and shows lose ASCII art
        sound.failed()
```

Check below to see what the finished program should look like. Make sure that everything in your code is correct before trying it out for yourself!

##### Example Code 3-4-2

```python==1
from pystubit.board import lightsensor, buzzer, Image, display
import picture
import sound
import random
import time

TIME_LIMIT = 30000
THRESHOLD = 2000
WIN_SCORE = 7
numbers = (
    picture.THREE,
    picture.TWO,
    picture.ONE,
    picture.ZERO
)
characters = (
    (picture.NINJA_A, "enemy"),
    (picture.NINJA_B, "enemy"),
    (picture.PRINCESS, "ally"),
    (picture.KING, "ally")
)

def start_game():
    score = 0
    
    for number in numbers:
        print(number)
        buzzer.on("C6", duration=600)
        time.sleep_ms(600)

    start_time = time.ticks_ms()
    while time.ticks_diff(time.ticks_ms(), start_time) < TIME_LIMIT:
        character = random.choice(characters)
        wait_time = random.randint(10, 30) * 50
        time.sleep_ms(wait_time)
        sound.operation()
        print(character[0])

        is_attacked = False
        input_start_time = time.ticks_ms()
        while time.ticks_diff(time.ticks_ms(), input_start_time) < 600:
            val = lightsensor.get_value()
            if val < THRESHOLD:
                is_attacked = True
                print(picture.SHURIKEN)
                time.sleep_ms(600)
        
        if is_attacked:
            if character[1] == "enemy":
                score += 1
                print(picture.OK)
                sound.correct()
            else:
                score -= 1
                print(picture.NG)
                sound.incorrect()
        else:
            if character[1] == "enemy":
                score -= 1
                print(picture.NG)
                sound.incorrect()
            else:
                score += 1
                print(picture.OK)
                sound.correct()

        time.sleep_ms(600)
        print("\n\n\n\n\n")

    if score >= WIN_SCORE:
        print(picture.SUCCESS.format(score))
        sound.success()
    else:
        print(picture.FAILED.format(score))
        sound.failed()
```

## Challenge: Changing Difficulty with New Actions

In Example Code 3-4-2 the player got points if they didn’t throw a *shuriken* at the Princess and King Meow. In this challenge, however, we’re going to raise the difficulty by requiring the player to **press A** when they see the **Princess**!

* Is it the Princess? ⇒ Press A
```
　 ∧M∧
  (ﾟーﾟ*)
⊂(|０|)⊃
　 ﾉ~~~ヽ
  ﾉ~~~~~ヽ
   ∪ ∪
```

* Is it King Meow? ⇒ Do nothing

```
 　 ∧＿∧
　　(  ﾟ∀ﾟ)
　　/~~〉〈/つ
　ノ ノ｜ ｜､
 ~(＿_)__)ゝ~~~
~~~~~~~~~~~
```

This will affect how the score is calculated as follows: you'll still get or lose the same points for attacking, but now you can also also gain or lose points if you press A when not attacking, depending on the target. We’ll also need to replace the Princess’s `”ally”` value with `”princess”` and King Meow’s with `”king”` in order to tell them apart!

|  |Attack|No Attack</br>and</br>Press A Button|No Attack</br>and</br>No Press|
|:---|:---:|:---:|:---:|
|Enemy</br>`"enemy"`|+1|-1|-1|
|Princess</br>`"princess"`|-1|**+1**|**-1**|
|King Meow</br>`"king"`|-1|**-1**|**+1**|

### Example Program

We’ll need to add and change the code in Example Code 3-4-2 in order. We’ll start by importing a `button_a` object at the beginning of the program in order to use the button.

###### Changed Code (Line 1)
```python=1
from pystubit.board import button_a, lightsensor, buzzer, Image, display
```

Next, we’ll change the values of each character’s ASCII art. Let’s change `”ally”` to `”princess”` and `”king”` for each character!

###### Changed Code (Lines 19-20)

```python=16
characters = (
    (picture.NINJA_A, "enemy"),
    (picture.NINJA_B, "enemy"),
    (picture.PRINCESS, "princess"),
    (picture.KING, "king")
)
```

Now let’s use the `button_a` object’s `was_pressed()` method to check whether the player has pressed the button! This method checks whether you’ve pressed A at some point in the past, so we need to clear its logs right before the part where the game needs to detect if you've pressed A. Otherwise it will end up counting times you pressed the A Button outside of that relevant time window!

###### New Code (Line 40)

```python=39
        is_attacked = False
        button_a.was_pressed()  # Clears log of button A being pressed
        input_start_time = time.ticks_ms()
        while time.ticks_diff(time.ticks_ms(), input_start_time) < 600:
            val = lightsensor.get_value()
```

Last, let’s make some changes to the code which decides if the player wins or loses points. You won’t need to change the part which sets `is_attacked` to `True`, meaning the player has attacked. When `is_attacked` is `False`, however, you need to set different processes for `”princess”` and `”king”` in addition to `”enemy”`!

Take a look at the table below as you change your code:

|  |No Attack</br>and</br>Press A Button|No Attack</br>and</br>No Press|
|:---|:---:|:---:|
|Enemy</br>`"enemy"`|-1|-1|
|Princess</br>`"princess"`|**+1**|**-1**|
|King Meow</br>`"king"`|**-1**|**+1**|

###### New and Changed Code (Lines 63-80)
```python=49
        if is_attacked:
            if character[1] == "enemy":
                score += 1
                print(picture.OK)
                sound.correct()
            else:  # Code here runs for both "princess" and "king"
                score -= 1
                print(picture.NG)
                sound.incorrect()
        else:
            if character[1] == "enemy":
                score -= 1
                print(picture.NG)
                sound.incorrect()
            elif character[1] == "princess":  # For "princess"
                if button_a.was_pressed(): # Button A pressed
                    score += 1
                    print(picture.OK)
                    sound.correct()
                else:  # Button A not pressed
                    score -= 1
                    print(picture.NG)
                    sound.incorrect()
            else:  # For "king"
                if button_a.was_pressed(): # Button A pressed
                    score -= 1
                    print(picture.NG)
                    sound.incorrect()
                else:  # Button A not pressed
                    score += 1
                    print(picture.OK)
                    sound.correct()
```

Now your program is finished! Check below to see what the finished program should look like. If you’re having trouble, try checking your code against the code below to see if there are any issues!

##### Example Code 4-1-1

```python=1
from pystubit.board import button_a, lightsensor, buzzer, Image, display
import picture
import sound
import random
import time

TIME_LIMIT = 30000
THRESHOLD = 2000
WIN_SCORE = 7
numbers = (
    picture.THREE,
    picture.TWO,
    picture.ONE,
    picture.ZERO
)
characters = (
    (picture.NINJA_A, "enemy"),
    (picture.NINJA_B, "enemy"),
    (picture.PRINCESS, "princess"),
    (picture.KING, "king")
)

def start_game():
    score = 0

    for number in numbers:
        print(number)
        buzzer.on("C6", duration=600)
        time.sleep_ms(600)

    start_time = time.ticks_ms()
    while time.ticks_diff(time.ticks_ms(), start_time) < TIME_LIMIT:
        character = random.choice(characters)
        wait_time = random.randint(10, 30) * 50
        time.sleep_ms(wait_time)
        sound.operation()
        print(character[0])

        is_attacked = False
        button_a.was_pressed()
        input_start_time = time.ticks_ms()
        while time.ticks_diff(time.ticks_ms(), input_start_time) < 600:
            val = lightsensor.get_value()
            if val < THRESHOLD:
                is_attacked = True
                print(picture.SHURIKEN)
                time.sleep_ms(600)

        if is_attacked:
            if character[1] == "enemy":
                score += 1
                print(picture.OK)
                sound.correct()
            else:
                score -= 1
                print(picture.NG)
                sound.incorrect()
        else:
            if character[1] == "enemy":
                score -= 1
                print(picture.NG)
                sound.incorrect()
            elif character[1] == "princess":
                if button_a.was_pressed():
                    score += 1
                    print(picture.OK)
                    sound.correct()
                else:
                    score -= 1
                    print(picture.NG)
                    sound.incorrect()
            else:
                if button_a.was_pressed():
                    score -= 1
                    print(picture.NG)
                    sound.incorrect()
                else:
                    score += 1
                    print(picture.OK)
                    sound.correct()

        time.sleep_ms(600)
        print("\n\n\n\n\n")

    if score >= WIN_SCORE:
        print(picture.SUCCESS.format(score))
        sound.success()
    else:
        print(picture.FAILED.format(score))
        sound.failed()
```


## Conclusion

### A Quick Review

In this lesson, we took another look at modules and packages. We also saw how using pre-made modules can help speed along the game making process!

On the Internet, you’ll find communities with lots of people who upload and share Python modules with each other. There are quite a few great modules out there, and finding one that you need for your program can make you a much more effective programmer! We’ll find out more about these communities starting in Topic 11!

### Coming Up Next Time!

In the next lesson, we’ll make a game using our Core Unit’s Accelerometer. We’ll also be learning about **list comprehension** which is a great way to store regularly-organized values like **1, 2, 3, 4, 5** in a list!
