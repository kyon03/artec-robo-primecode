---
tags: Python_English, Topic 5
---

Python Robotics Course Lesson 20

Topic 5-4
Command Line Input and Exception Handling
===

Learn how to handle errors with ease!Learn how to handle errors with ease!

## In This Lesson...

In this lesson we’ll learn how to use the terminal to input data into a program you’re currently running, and how to fix errors in your program without having to stop it!

In the second half of the lesson, we’ll apply what we’ve learned to make a game of rock, paper, scissors. This game allows you to challenge the computer by entering the symbol of your choice into the terminal.
:::info
Lesson Introduction

■ Lesson 20 Contents

1. Input for a Running Program
2. Handling Errors Safely
3. Programming Rock, Paper, Scissors
4. Challenge: Making a New Game



In this lesson we’ll learn two things: how to make a running program accept terminal input, and how to properly handle errors that pop up when your program is running!

We'll start off by introducing some new Python syntax elements.

In Python, there’s a way to input text strings for a program using the terminal, even when that program is running. Here we’ll learn how to write a program that can change what it does depending on which text string you input!

Next, we’ll learn how to safely handle any errors that pop up when your program is running. Normally, a program will stop when an error occurs, refusing to move on to the next step. This is why Python has a built-in feature to catch and figure out what to do with any errors that happen!

In the second half of the lesson, we’ll use our new syntax to make a game of rock, paper, scissors. This game has the player challenge the computer by entering the symbol of their choice into the terminal.

Last, we’ll review what we learned and take on the challenge of turning rock, paper, scissors into an entirely new game!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!
:::
## New Python Syntax

Let's start off by introducing some new Python code you'll be using for the first time in this lesson.

### Input for a Running Program

Think of a program for a trivia game: this program would have to take answers from the player and then decide whether that answer is correct! This is where Python’s `input()` function comes in. This function is a simple command that you can use when you want a running program to accept user input!

#### ■ Using the `input()` Function

Calling an `input()` function while a program is running makes the terminal accept user input. Once you’ve called `input()` the program will pause temporarily while it waits for that input.


This function will return any input received in the terminal as a **text string**. You can also set the **text you want to appear in the terminal** as an argument in an `input()` function.

Now let’s take a look at how the `input()` function works by making a program that shows the name of a month when the user types the number of that month!

We’ll start by making a dictionary that uses the number of the month as a key and the name of the month as the definition. The program will use this data to look up the month entered by the user.

```python==1
dict_month_name = {
            '1': 'January',
            '2': 'February',
            '3': 'March',
            '4': 'April',
            '5': 'May',
            '6': 'June',
            '7': 'July',
            '8': 'August',
            '9': 'September',
            '10': 'October',
            '11': 'November',
            '12': 'December'
}
```

Next, we’ll make a function that accepts the user input and shows the name of the month. We’ll store the text string (that’s the month’s number) in a variable, look that variable up in the dictionary, and then use the `print()` function to show the name!

##### Example Code 2-1-1
###### New Code (Line 16, Line 18)

```python==1
dict_month_name = {
            '1': 'January',
            '2': 'February',
            '3': 'March',
            '4': 'April',
            '5': 'May',
            '6': 'June',
            '7': 'July',
            '8': 'August',
            '9': 'September',
            '10': 'October',
            '11': 'November',
            '12': 'December'
}

def get_month_name():
    month_num = input('Please input the month number >>> ')
    month_name = dict_month_name[month_num]
    print(month_name)
```

Now let’s run the program and call the `get_month_name()` function from the terminal!

##### Running the Code
<pre class="prettyprint">
&gt&gt&gt get_month_name()
Please input the month number &gt&gt&gt 
</pre>

You’ll see the text string that you set as an argument in the `input()` function. The program is ready for keyboard input after this, so enter any number from 1 to 12 and press the **Enter** key!

##### Inputting a Number
<pre class="prettyprint">
&gt&gt&gt get_month_name()
Please input the month number &gt&gt&gt 5
May
</pre>

The program will look up the name of the month in the `dict_month_name` dictionary we made and then show it! This is the most basic way to use the `input()` function. There is, however, something you want to keep in mind when using this function with your Studuino:bit!

#### ■ Studuino:bit and `input()`

Try changing example code 2-1-1 as shown below and you’ll find that the program won’t accept any input!

###### New Code (Line 21)
```python==1
dict_month_name = {
            '1': 'January',
            '2': 'February',
            '3': 'March',
            '4': 'April',
            '5': 'May',
            '6': 'June',
            '7': 'July',
            '8': 'August',
            '9': 'September',
            '10': 'October',
            '11': 'November',
            '12': 'December'
}

def get_month_name():
    month_num = input('Please input the month number >>> ')
    month_name = dict_month_name[month_num]
    print(month_name)

get_month_name()
```

##### Program Results

<pre class="prettyprint">
Please input the month number &gt&gt&gt Traceback (most recent call last):
  File "&ltstdin&gt", line 42, in &ltmodule&gt
  File "&ltstdin&gt", line 36, in get_month_name
KeyError: 
</pre>

After running the `input()` function, this program will move onto the next step without waiting for input, which means that it skips storing the text string in the `month_num` variable. This causes a `KeyError` that says that it can't find the key in the dictionary!

This is why you always need to call `input()` functions from the terminal when using your Studuino:bit!

### Handling Errors Safely

With the spread of the Internet, there are lots of people who use websites to sign up for services and buy different things. In order to do this, they need to use a registration form to enter their name, address, phone number, email address, and other necessary information.

But what happens if a user makes a mistake when they enter their information? If they put in the wrong phone number or email address, for example, the service provider won’t be able to send them very important news!

Similarly, if you use an `input()` function accept user input in Python, the program will stop because any mistakes in the entered text string will cause a problem with the next step.

You can try this out yourself by typing any other number than 1 to 12 with example code **2-1-1**.

<pre>
&gt&gt&gt get_month_name()
Please input the month number &gt&gt&gt 13
Traceback (most recent call last):
  File "&ltstdin&gt", line 1, in &ltmodule&gt
  File "&ltstdin&gt", line 36, in get_month_name
KeyError: 13
</pre>

This is why Python allows you to handle any errors that pop up as a special exception!

#### ■ Handling Errors with `try`-`except` Statements

The results you see above will end in a message that says `KeyError: 13`. This error means that the text string **13** doesn’t exist inside of the data in dictionary `dict_month_name`. Errors like `KeyError` are called **exceptions**, and you can use `try`-`except` statements to catch and handle them without stopping your program.

`try`-`except` statements are actually two statements: a `try` statement block that you use when an exception might occur, and an `except` statement block that reserves a special process for when an exception is found!


```
try:
    .
    .    # Process to run when an exception might happen
    .

except **Type of exception to look for**:
    .
    .    #  Process to run when an exception is found
    .

```

Now let’s see how this works and improve our program by adding a `try`-`except` statement to code **2-1-1** to catch the `KeyError` for any numbers other than 1 to 12, notify the user, and then ask them to type the number again!

Let’s start by thinking of where an error might happen inside of code **2-1-1**.

##### Example Code 2-1-1

```python==1
dict_month_name = {
            '1': 'January',
            '2': 'February',
            '3': 'March',
            '4': 'April',
            '5': 'May',
            '6': 'June',
            '7': 'July',
            '8': 'August',
            '9': 'September',
            '10': 'October',
            '11': 'November',
            '12': 'December'
}

def get_month_name():
    month_num = input('Please input the month number >>> ')
    month_name = dict_month_name[month_num]
    print(month_name)
```

In this program, the `KeyError` exception might happen on line 18, which searches `dict_month_name` for the text string entered by the user!

```python==18
    month_name = dict_month_name[month_num]
```

Let’s pack this code together as a `try` statement block and use our `except` statement block to catch the `KeyError` exception. When we catch an exception, we’ll also need to use the `print()` function to let the user know that the number they entered is invalid! 

#### New and Changed Code (Lines 18-21)
```python==16
def get_month_name():
    month_num = input('Please input the month number >>> ')
    try:
        month_name = dict_month_name[month_num]
    except KeyError:
        print('{} is invalid!'.format(month_num))
        
    print(month_name)
```

Now we have to ask the user to enter a number again if an exception is caught, then use the `print()` function to show the name of the month if they enter the right number!

In addition to `try`-`except` statements, you can also use `else` and `finally` statements. `else` statements tell a program what to do when an exception isn’t found, and `finally` statements tell it what to do regardless of what happens!

```
try:
    .
    .    # Process to run when an exception might happen.
    .

except **Type of exception to look for**:
    .
    .    #  Process to run when an exception is found.
    .
    
else:
    .
    .    # Process to run **only** when an exception isn’t found.
    .

finally:
    .
    .    # Final process to run regardless of if an exception is found or not.
    .

```

Here we’re going to add a new `else` statement. We can easily make the program request user input again by setting `get_month_name()` to call itself inside of the function!

###### New and Changed Code (Lines 23-25)
```python==16
def get_month_name():
    month_num = input('Please input the month number >>> ')
    
    try:
        month_name = dict_month_name[month_num]
    except KeyError:
        print('{} is invalid!'.format(month_num))
        get_month_name()  # This function calls itself before running
    else:
        print(month_name)
```

A function which is designed to call itself is known as a **recursive** function, and it can be used as a programming technique to handle particularly complex numerical calculations. Now let’s run our new-and-improved program and see how it works!

##### Program Results

<pre class="prettyprint">
&gt&gt&gt get_month_name()
Please input the month number &gt&gt&gt 13
13 is invalid!
Please input the month number &gt&gt&gt 12
December
</pre>

Now your program will continue to run correctly, even if you enter a wrong number!


#### ■ Exception Types in MicroPython

Python comes with a number of **exception classess**. These let you deal with various errors on a case-by-case basis, including `KeyError`! The MicroPython language used by your Studuino:bit allows you to use a portion of these exception classes.

See the table below for a list of these classes. These may be a bit difficult to understand now, but as we continue through each lesson you’ll find yourself gradually learning how to use each one as your knowledge deepens!

##### Exception Classes Available in MicroPython

|Exception Name|Definition|
|:---|:---|
|Exception|The base class for all exceptions.|
|AssertionError|Occurs when a specific condition set by an assert statement used in a program’s test environment is met.|
|ImportError|Used in situations like when a module definition can't be found using a `from`~`import` statement or when a specified namespace doesn't exist.|
|KeyboardInterrupt|Occurs when an interrupt character is typed while a command is being run.|
|KeyError|Occurs when the specified key doesn’t exist in a dictionary.|
|AttributeError|Occurs when an attribute, method, etc. called from an instance doesn’t exist.|
|RuntimeError|Occurs when a program tries to run.|
|SyntaxError|Occurs when incorrect syntax is present in a program.|
|TypeError|Occurs when incorrectly formatted objects are used in a function or elsewhere.|
|ZeroDivisionError|Occurs when attempting to divide by 0.|
|IndexError|Occurs when the index number for a list or other item is outside of the expected range.|
|MemoryError|Occurs when there’s not enough memory available for the program to use.|
|NameError|Occurs when using an undefined variable name.|
|NotImplementedError|Occurs when a particular process hasn’t been implemented.|
|OSError|Occurs when an error happens on the operating system level before running a command.|
|StopIteration|Occurs when a generator object runs out of return values.|
|SystemExit|This exception is thrown when Python closes.|
|ValueError|Occurs when a number outside of the expected range is given as an argument.|


You’ll get a message from MicroPython any time an error pops up. We’ve run into quite a few errors in the programs we’ve made so far. Now we’re going to pick a few classes out of the table and explain what they mean along with example code. From now on when you get an error, try figuring out what the message means and think about what might be causing it!

##### SyntaxError

`SyntaxError` occurs when there’s improper syntax in a program. The `else` statement on line 3 in the code below has been mistyped as `eles`, which causes this exception.



```python==1
if True:
    print('OK')
eles:
    print('NG!')
```

<pre class="prrtyprint">
Traceback (most recent call last):
  File "&ltstdin&gt", line 6
SyntaxError: invalid syntax
</pre>

##### ImportError

`ImportError` occurs when an invalid module, class, object, or other name is specified in a `from`-`import` statement. The `board` module defined in the `display` object on line 1 has been mistyped as `baord`, which causes this exception.

```python==1
from pystubit.baord import display

display.show('hello!')
```

<pre class="prrtyprint">
Traceback (most recent call last):
  File "&ltstdin&gt", line 2, in &ltmodule&gt
ImportError: no module named 'pystubit.baord'
</pre>

##### NameError

`NameError` occurs when there’s an error in the variable, function, or other name that you’re trying to use. The `foo` variable on line 1 in the code below has been mistyped as `hoo` in the `print()` function, which causes this exception.

```python==1
foo = 1

print(hoo)
```

<pre class="prrtyprint">
Traceback (most recent call last):
  File "&ltstdin&gt", line 6, in &lmodule&gt
NameError: name 'hoo' isn't defined
</pre>

##### IndexError

`IndexError` occurs when the specified index number inside of sequenced data like text strings, tuples, and lists doesn’t exist. Line 3 in the code below attempts to retrieve index number 3 despite the list defined on line 1 only having index numbers 0, 1, and 2, which causes this exception.

```python==1
numbers = [0, 1, 2]

print(numbers[3]) 
```

<pre class="prrtyprint">
Traceback (most recent call last):
  File "&ltstdin&gt", line 6, in &ltmodule&gt
IndexError: list index out of range
</pre>

##### KeyError

`KeyError` occurs when attempting to look for a definition in a dictionary using a key that doesn’t exist. Line 3 in the code below attempts to find a nonexistent key named `strawberry` in the dictionary defined on line 1, which causes this exception.

```python==1
dict = {'apple': 100, 'banana': 200, 'grape': 300}

print(dict['strawberry'])
```

<pre class="prrtyprint">
Traceback (most recent call last):
  File "&ltstdin&gt", line 6, in &ltmodule&gt
KeyError: strawberry
</pre>

##### AttributeError

`AttributeError` occurs when there’s a mistake in the method, property, or other name of a called object. The `scroll()` method defined in the `display` object on line 3 has been mistyped as `scrol`, which causes this exception.

```python==1
from pystubit.board import display

display.scrol('hello!')
```

<pre class="prrtyprint">
Traceback (most recent call last):
  File "&ltstdin&gt", line 6, in &ltmodule&gt
AttributeError: 'StuduinoBitDisplay' object has no attribute 'scrol'
</pre>


## Programming Rock, Paper, Scissors

Now let’s try using the `input()` function and exception handling ourselves as we build a game that lets you challenge the computer in rock, paper, scissors!

##### Completed Example Video
<iframe src="https://player.vimeo.com/video/482885125" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

##### The Rules of Rock, Paper, Scissors

You may already be familiar with how to play rock, paper, scissors, but let’s go over the rules one more time here.

* You play rock, paper, scissors by making one of these three shapes with your hand and throwing it at the same time as your opponent.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-4/5-4_1_E.png"/>

* The shapes both you and your opponent throw decide who wins and who loses! You win if you throw a shape stronger than your opponent’s, and draw if you both throw the same shape. In the event of a draw, you’ll have to do a rematch.
 
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-4/5-4_2_E.png"/>

* The strengths of each shape are as follows: Rock beats scissors, scissors beats paper, and paper beats rock.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-4/5-4_3_E.png"/>

And now we’re going to make a game of rock, paper, scissors where a human player can challenge the computer!

### ■ Game Program Outline

This game will need to have the following features:

##### Your game will need to...

1. Have the computer choose a shape at random and show it on the LED display.
2. Let the player type the name of their shape check to see if it's correct.
3. Show which shape the player chose on the LED display.
4. Determine who wins and shows the result on the LED display.

We’ll combine these functions and process them as shown in the picture below:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-4/5-4_4_E.png"/>

Now let’s make each of these features in order.

### Programming a Random Computer Shape and the LED Display

In the order shown below, let’s write the code to make the computer choose a random shape and show it on the LED display.

#### ■ Preparing LED Images for Each Shape

We’ll start by making separate images for rock, paper, and scissors in the LED display.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-4/5-4_5_E.png"/>

Let's import the `Image` class, then make an instance for each image. We’ll have to store the images we make inside of a dictionary called `patterns`.

```python==1
from pystubit.board import Image

img_rock = Image('00000:00000:01110:11110:01110:', color=(31, 0, 0))    # Rock
img_scissors = Image('01010:01010:01110:11110:01110:', color=(0, 31, 0))    # Scissors
img_paper = Image('01110:01111:11111:11111:01110:', color=(0, 0, 31))    # Paper

patterns = {'rock': img_rock, 'scissors': img_scissors, 'paper': img_paper}
```

#### ■ Picking and Displaying a Random Shape

We’ll put the processes we’re going to make here into a function called `start_game()`. Let’s import the `random` module we studied in Topic 4-4. We’ll then use the `choice()` method to make the computer choose a random shape from the list of keys in our `patterns` dictionary.

We can use the `keys()` method to retrieve the list of dictionary keys, but because this data is what we call an **iterator**, we’ll need to convert it into a list or tuple before making it an argument in the `choice()` method. As shown below, you’ll need use the `tuple()` function to make this data into a tuple inside of the `choice()` method.

<pre class="prettyprint">
random.choice(tuple(patterns.keys()))
</pre>

Add the code shown below to your program.

###### New Code (Line 2, Lines 10-11)
```python==1
from pystubit.board import Image
import random

img_rock = Image('00000:00000:01110:11110:01110:', color=(31, 0, 0))
img_scissors = Image('01010:01010:01110:11110:01110:', color=(0, 31, 0))
img_paper = Image('01110:01111:11111:11111:01110:', color=(0, 0, 31))

patterns = {'rock': img_rock, 'scissors': img_scissors, 'paper': img_paper}

def start_game():
    com = random.choice(tuple(patterns.keys()))
```

Next, let’s program the LED display to show each shape. We’ll start by importing a `display` object. Now we’ll set `com` to search through the keys in our `patterns` dictionary and store the image it retrieves it in the variable `pattern_com`. Once we’ve done that, we’ll set the variable as an argument in the `display` object’s `show()` method. To give the player enough time to see what shape it is, we’ll need to set the arguments `delay=2000` and `clear=True` to make each shape stay on the display for two seconds.

###### New Code (Line 1, Line 12)
```python==1
from pystubit.board import Image, display
import random

img_rock = Image('00000:00000:01110:11110:01110:', color=(31, 0, 0))
img_scissors = Image('01010:01010:01110:11110:01110:', color=(0, 31, 0))
img_paper = Image('01110:01111:11111:11111:01110:', color=(0, 0, 31))

patterns = {'rock': img_rock, 'scissors': img_scissors, 'paper': img_paper}

def start_game():
    com = random.choice(tuple(patterns.keys()))
    pattern_com = patterns[computer]
    display.show(pattern_com, delay=2000, clear=True)
```

Now let’s run the program and call the `start_game()` function from the terminal! Try calling it multiple times to see how the randomly-selected shape appears on the LED display.


### Inputting and Verifying the Player’s Shape / Showing the Player’s Shape on the LED Display
Next, we’ll need to add a function that allows for the player to type their shape and perform exception handling to check that it’s correct.

First we’ll use an `input()` function to accept the player’s input, and store the text string typed by the player in a variable called `player`. Just like we did before, we’ll have it search the keys in the `patterns` dictionary and store the retrieved image inside of a variable called `pattern_player`.

###### New Code (Line 14, Line 15)
```python==10
def start_game():
    com = random.choice(tuple(patterns.keys()))
    pattern_com = patterns[computer]
     
    player = input('Rock, paper, scissors... Shoot! >>>')
    pattern_player = patterns[computer]
    
    display.show(pattern_com, delay=2000, clear=True)
```

At this point, the program will work as expected if the text string in `player` matches a key in `patterns`. If it doesn’t match, however, a `KeyError` will occur. We’ll use a `try`-`except` statement to catch this exception. If an exception is caught, we’ll have to let the player know they made a mistake and that they need to type `rock`, `paper`, or `scissors` in all lowercase letters.

###### New and Changed Code (Lines 15-19)
```python==10
def start_game():
    com = random.choice(tuple(patterns.keys()))
    pattern_com = patterns[com]
    
    player = input('Rock, paper, scissors... Shoot! >>>')
    try:
         pattern_player = patterns[player]
    except KeyError:
        print('{} is invalid!'.format(player))
        print('Please type `rock`, `paper` or `scissors`.')

    display.show(pattern_com, delay=2000, clear=True)
```

If an exception is caught, the program will ask the player to type their shape again. If the shape is correct, it’ll be shown on the LED display. Let’s use what we learned in the previous chapter and add code for an `else` statement and a recursive function.

###### New and Changed Code (Lines 20-23)
```python==10
def start_game():
    com = random.choice(tuple(patterns.keys()))
    pattern_com = patterns[com]
    
    player = input('Rock, paper, scissors... Shoot! >>>')
    try:
         pattern_player = patterns[player]
    except KeyError:
        print('{} is invalid!'.format(player))
        print('Please type `rock`, `paper` or `scissors`.')
        start_game()
    else:
        display.show(pattern_player, delay=2000, clear=True)
        display.show(pattern_com, delay=2000, clear=True)
```

### Determining the Result and Showing it on the LED Display

Next, we’ll need to add a function that determines whether the player wins or loses, then shows the result on the LED display. Both the `com` and `player` variables have information for each shape, and we can compare these two to see who wins!

While there are many ways to make branches that determine who wins, let’s start by determining whether the match is a draw. This is super easy: all we have to do is compare the text string data inside of our two variables and see if they match! In the event of a draw, let’s save `Draw` in the `result` variable that records the result of the match.

###### New Code (Line 25, Line 26)
```python==10
def start_game():
    com = random.choice(tuple(patterns.keys()))
    pattern_com = patterns[com]
    
    player = input('Rock, paper, scissors... Shoot! >>>')
    try:
         pattern_player = patterns[player]
    except KeyError:
        print('{} is invalid!'.format(player))
        print('Please type `rock`, `paper` or `scissors`.')
        start_game()
    else:
        display.show(pattern_player, delay=2000, clear=True)
        display.show(pattern_com, delay=2000, clear=True)

        if com == player:
            result = 'Draw'
```

Now let’s write the code that determines who wins in the event there isn’t a draw! We’ll need to use multiple branches in order to figure out what combinations of player and computer shapes determine a win or a loss.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-4/5-4_6_E.png"/>

Let’s start by making an `if`-`elif`-`else` statement to separate processes depending on the computer’s shape. We’ll add processes for the player’s shape to this by using the *conditions* we studied in Topic 4-1! Now let’s add the code below:

###### New Code (Line 27, Line 33)
```python==25
        if com == player:
            result = 'Draw'
        else:
            if com == 'rock':
                result = 'Win' if player=='paper' else 'Lose'
            elif com == 'scissors':
                result = 'Win' if player=='rock' else 'Lose'
            else:  
                result = 'Win' if player=='scissors' else 'Lose'
```

Last, we’ll make the result of the game scroll across the LED display. In the event of a draw, we’ll also call `start_game()` as a recursive function at the end of the program!

###### New Code (Line 35, Line 37)
```python==25
        if com == player:
            result = 'Draw'
        else:
            if com == 'rock':
                result = 'Win' if player=='paper' else 'Lose'
            elif com == 'scissors':
                result = 'Win' if player=='rock' else 'Lose'
            else:
                result = 'Win' if player=='scissors' else 'Lose'
            
        display.scroll(result, delay=50)
        if result == 'Draw':
            start_game()
```

Now your program is finished! The completed program should look like this. Make sure that everything in your code is correct before trying it out for yourself!

##### Example Code 3-4-1
```python==1
from pystubit.board import Image, display
import random

img_rock = Image('00000:00000:01110:11110:01110:', color=(31, 0, 0))
img_scissors = Image('01010:01010:01110:11110:01110:', color=(0, 31, 0))
img_paper = Image('01110:01111:11111:11111:01110:', color=(0, 0, 31))

patterns = {'rock': img_rock, 'scissors': img_scissors, 'paper': img_paper}

def start_game():
    com = random.choice(tuple(patterns.keys()))
    pattern_com = patterns[com]
    
    player = input('Rock, paper, scissors... Shoot! >>>')
    try:
         pattern_player = patterns[player]
    except KeyError:
        print('{} is invalid!'.format(player))
        print('Please type `rock`, `paper` or `scissors`.')
        start_game()
    else:
        display.show(pattern_player, delay=2000, clear=True)
        display.show(pattern_com, delay=2000, clear=True)
    
        if com == player:
            result = 'Draw'
        else:
            if com == 'rock':
                result = 'Win' if player=='paper' else 'Lose'
            elif com == 'scissors':
                result = 'Win' if player=='rock' else 'Lose'
            else:
                result = 'Win' if player=='scissors' else 'Lose'
            
        display.scroll(result, delay=50)
        if result == 'Draw':
            start_game()
```

## Challenge: A Level Beyond Rock, Paper, Scissors

In this challenge we’ll modify example code ***3-4-1** to add a game after you play rock, paper, scissors and decide who wins!

##### The Rules of the New Game
* Depending on the result of rock, paper, scissors, the player and the computer will be assigned roles of Attacker or Defender.
* If the player is the Attacker, they will have to press A to attack before time runs out. The Defender will have to press B to defend before time runs out.
* The player wins if they make a successful attack. If the attack fails, the computer has successfully defended and the player will have to play rock, paper, scissors again.
* A successful defense from the player also results in a rematch of rock, paper, scissors. Failure to defend results in a loss for the player.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-4/5-4_7_E.png"/>

##### ■ Program Outline

This program works as follows:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-4/5-4_8_E.png"/>

The results of the game will be shown as both an image and as scrolling text reading `Victory`, `Defeat`, or `Defended`. For the images, we’ll be using ones already available in the **StuduinoBitImage(`Image`)** class.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-4/5-4_9_E.png"/>

This is a pretty advanced challenge, so follow along with the code below to write your program if you’re having trouble figuring it out!

### Example Program

The `display.scroll(result, delay=50)` on line 35 shows the result of rock, paper, scissors. We’ll need to delete this as this game decides a win or loss in a different way.

###### Removed Code (Line 35)
```python==25
        if com == player:
            result = 'Draw'
        else:
            if com == 'rock':
                result = 'Win' if player=='paper' else 'Lose'
            elif com == 'scissors':
                result = 'Win' if player=='rock' else 'Lose'
            else:
                result = 'Win' if player=='scissors' else 'Lose'
                       
        if result == 'Draw':
            start_game()
```

If rock, paper, scissors doesn’t end in a draw, the program will have to determine whether the A button or the B button has been pressed. To detect a press without having to hold the button down, we’ll use the `was_pressed` method from the **StuduinoBitButton** class. Since we’ll be using this method’s return values quite often later, we’ll store these values in variables `was_pressed_a` and `was_pressed_b`.

###### Changed Code (Line 1)
```python==1
from pystubit.board import Image, display, button_a, button_b
```
###### New Code (Line 38, Line 39)
```python==25
        if com == player:
            result = 'Draw'
        else:
            if com == 'rock':
                result = 'Win' if player=='paper' else 'Lose'
            elif com == 'scissors':
                result = 'Win' if player=='rock' else 'Lose'
            else:
                result = 'Win' if player=='scissors' else 'Lose'
                       
        if result == 'Draw':
            start_game()
        else:
            was_pressed_a = button_a.was_pressed()
            was_pressed_b = button_b.was_pressed()
```

The `was_pressed()` method will return `True` even if you’ve just pressed the button once. This means it will return `True` even if you’ve pressed the button outside of the game! To prevent this, we’ll run the `was_pressed()` method one time immediately before the LED display shows the shape. Then we’ll reset it. 

###### New Code (Line 23, Line 24)
```python==10
def start_game():
    com = random.choice(tuple(patterns.keys()))
    pattern_com = patterns[com]
    
    player = input('Rock, paper, scissors... Shoot! >>>')
    try:
         pattern_player = patterns[player]
    except KeyError:
        print('{} is invalid!'.format(player))
        print('Please type `rock`, `paper` or `scissors`.')
        start_game()
    else:
        display.show(pattern_player, delay=2000, clear=True)
        button_a.was_pressed()
        button_b.was_pressed()
        display.show(pattern_com, delay=2000, clear=True)
```

And now we’re ready to determine the result of the game. Use the table below as you use branches to determine the result of the game before storing it in a variable called `judgment`.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-4/5-4_7_E.png"/>

To prevent an exploit where you can force a win by pressing A and B at the same time, let’s make a condition to determine a win when only one of the buttons is pressed.

###### New Code (Line 43, Line 52)
```python==39
        else:
            was_pressed_a = button_a.was_pressed()
            was_pressed_b = button_b.was_pressed()
                
            if result == 'Win':
                if was_pressed_a is True and was_pressed_b is False:
                    judgment = 'Victory'
                else:
                    judgment = 'Defended'
            else:
                if was_pressed_a is False and was_pressed_b is True:
                    judgment = 'Defended'
                else:
                    judgment = 'Defeat'
```

Last, we’ll show the result of the game as an image. Next, we’ll make the result we stored in `judgment` scroll across the display!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-4/5-4_9_E.png"/>

Keep in mind that a result of `Defended` will result in a rematch of rock, paper, scissors, so let’s call the `start_game()` function at the end!

###### New Code (Line 54-63)
```python==39   
        else:
            was_pressed_a = button_a.was_pressed()
            was_pressed_b = button_b.was_pressed()
                
            if result == 'Win':
                if was_pressed_a is True and was_pressed_b is False:
                    judgement = 'Victory'
                else:
                    judgment = 'Defended'
            else:
                if was_pressed_a is False and was_pressed_b is True:
                    judgment = 'Defended'
                else:
                    judgement = 'Defeat'
                        
            if judgement == 'Victory':
                display.show(Image.SWORD, delay=2000)
                display.scroll(judgement, delay=50)
            elif judgement == 'Defeat':
                display.show(Image.SKULL, delay=2000)
                display.scroll(judgement, delay=50)
            else:
                display.show(Image.SQUARE, delay=2000)
                display.scroll(judgement, delay=50)
                start_game()
```

Now your program is finished! The entire program will look like this one you’re done: Run it and try it out for yourself!

##### Example Code 5-1-1
```python==1
from pystubit.board import Image, display, button_a, button_b
import random

img_rock = Image('00000:00000:01110:11110:01110:', color=(31, 0, 0))
img_scissors = Image('01010:01010:01110:11110:01110:', color=(0, 31, 0))
img_paper = Image('01110:01111:11111:11111:01110:', color=(0, 0, 31))

patterns = {'rock': img_rock, 'scissors': img_scissors, 'paper': img_paper}

def start_game():
    com = random.choice(tuple(patterns.keys()))
    pattern_com = patterns[com]
    
    player = input('Rock, paper, scissors... Shoot! >>>')
    try:
         pattern_player = patterns[player]
    except KeyError:
        print('{} is invalid!'.format(player))
        print('Please type `rock`, `paper` or `scissors`.')
        start_game()
    else:
        display.show(pattern_player, delay=2000, clear=True)
        button_a.was_pressed()
        button_b.was_pressed()
        display.show(pattern_com, delay=2000, clear=True)
    
        if com == player:
            result = 'Draw'
        else:
            if com == 'rock':
                result = 'Win' if player=='paper' else 'Lose'
            elif com == 'scissors':
                result = 'Win' if player=='rock' else 'Lose'
            else:
                result = 'Win' if player=='scissors' else 'Lose'

        if result == 'Draw':
            start_game()
        else:
            was_pressed_a = button_a.was_pressed()
            was_pressed_b = button_b.was_pressed()
                
            if result == 'Win':
                if was_pressed_a is True and was_pressed_b is False:
                    judgement = 'Victory’
                else:
                    judgment = 'Defended'
            else:
                if was_pressed_a is False and was_pressed_b is True:
                    judgment = 'Defended'
                else:
                    judgement = 'Defeat'
                        
            if judgement == 'Victory':
                display.show(Image.SWORD, delay=2000)
                display.scroll(judgement, delay=50)
            elif judgement == 'Defeat':
                display.show(Image.SKULL, delay=2000)
                display.scroll(judgement, delay=50)
            else:
                display.show(Image.SQUARE, delay=2000)
                display.scroll(judgement, delay=50)
                start_game()
```


## Conclusion
### A Quick Review

In this lesson we learned...

* How to use the `input()` function to accept user input for a program which is currently running.
* How to use `try`-`except`-`else`- statements to catch any errors, or **exceptions** that happen and keep your program running smoothly!
* About the different sorts of **exception class** types available in Python and MicroPython.

Errors can happen no matter what kind of program you’re making. While you should always do your best to prevent them from happening, you need to be ready to deal with them when they pop up! The exception handling we’ve studied in this lesson might be hard to get the hang of, but try to think of how you can make use of once you get used to making programs.

### Coming Up Next Time!
In the next lesson, we’ll take a closer look at the special features of object-oriented programming languages as we build a robocar!