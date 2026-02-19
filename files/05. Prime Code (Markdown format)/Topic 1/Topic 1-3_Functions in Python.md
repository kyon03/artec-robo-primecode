---
tags: Python_English, Topic 1
---

Python Robotics Course Lesson 3

Topic 1-3
Functions in Python
===
Learn how to use **functions** to keep your programs neat and tidy!

## In This Lesson...

We'll be learning about **functions**, which help you keep your code neat and tidy! At the end of this lesson, we'll take on the challenge of using functions to make a program that lets you change the key of a song playing from the Core Unit's Buzzer!


:::info
Lesson Introduction

■ Lesson 3 Contents

1. How to Use Functions
2. Python's Built-In Functions
3. Defining Functions
4. Explaining Code with Comments
5. Challenge: A Musical Function



In this lesson we'll learn about functions, which help keep programs neat and tidy.

We'll start by explaining what functions do and how to use them,

then introduce a few of the built-in functions that come with Python!

Learn how to make a function, then try making one that does simple calculations.

After that, we'll talk about how to use comments to leave notes that explain the variables and functions in your code!

At the end of the lesson, you can try using functions yourself to write a program for the Core Unit's Buzzer.

By using functions, you can change the tempo of a song just like in this video!

Now let's start the lesson!
:::

## Functions in Python

The programs that make the computer systems we use in real life work are often tens of thousands of lines long! The longer your program gets, the more important it is to be able to use **functions** to summarize parts of it to make it clean and concise. Clean code is also easier to follow when you read it, making system maintenance that much easier.

The programming term **function** originally comes from the world of mathematics. The functions we'll be using in Python aren't too different from the ones you'd use in an algebra class! When you write a mathematical function in the format `y = f(x)`, the "function" you're describing is a defined equation (f) that contains a variable called `x`, and when you assign a numeric value to x, you can solve the equation to find the value of `y`.

Let's look at the function below as an example.

```
f(x) = 2x + 3
```

With this function, the result of the equation will be **5** if the value of the `x` variable is **1**, but if the value of `x` is **2**, the result will be **7**.

```
f(1) = 2×1 + 3 = 5
f(2) = 2×2 + 3 = 7
```

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-3/1-3_1_E.png"/>

Basically, when you assign a value to a variable and then put it through some mathematical process to get a result, that's a function. In programming, the values you assign at the start of a function are called **parameters**, and the result you get at the end is called a **return value**. However, unlike in mathematics, **in programming you can also make functions that don't have parameters or return values!**

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-3/1-3_2_E.png"/>

### Built-In Functions in Python

There are a number of ready-to-use functions already included in the Python programming language! Many of the commands we've been using so far, like `print()`, `type()`, and `int()` are actually **built-in functions** of Python.


##### Built-In Functions We've Used So Far
|Function Name|Parameters|Return Value|What It Does|
|:---|:---|:---|:---|
|print()|Data (numbers, strings, etc.)|None|Displays the data it takes as a parameter on the screen/terminal.|
|int()|Numbers or strings|Integers|Takes non-integer data as its parameter and converts it into integer-type data.|
|float()|Numbers or strings|Floating-point numbers|Takes non-floating point data as its parameter and converts it into floating point number data.|
|str()|Numbers|Strings|Takes numeric data as its parameter and converts it into string data.|
|len()|Strings (and other data)|Length of data|Gives the length of the data in its parameter (e.g. number of letters in a string) as a return value.|

In a Python function, the data you put into the function is called a parameter, and a result the function gets from processing that data is called a return value. Some functions take just one parameter, some take two or more, and others take none! Likewise, some functions will give you a return value, but other functions don't. `print()`, for example, just displays the parameter you give it on screen.

There are lots of built-in functions in Python other than the ones listed above! If you want to learn about more of the built-in functions, you can look them up in the official Python documentation. The descriptions might be a little hard to understand at this point in the course, though, so we recommend waiting until you've completed more lessons to read it.

[Built-In Functions - Python 3.6 Documentation ](https://docs.python.org/ja/3.6/library/functions.html)

###  Defining Functions
You can also make (or define) functions of your own! To define a new function, use the keyword **def**, followed by the name of your function, a set of parentheses, and a colon., and write the names of any parameters you want your function to use inside the parentheses. Just like with variables, you can name your function anything you want as long as it's not a keyword.

You can start writing the code for the process you want your function to perform on the next line of the program, but there's one important thing you need to remember to do first! Every line of code you write that goes inside a function needs to have an indent (blank space) at the beginning. If a line isn't indented, Python won't be able to tell that line is supposed to be part of the function!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-3/1-3_3_E.png"/>

Normally we indent lines by pressing **Tab** at the beginning, but pressing **Space** four times works as well.

Let's practice coding functions by defining a function called **square** that takes a parameter called **x** and gives us the square of x as a return value, then put different numbers through it to see the results.

###### ★ Remember, the square of a number is that number multiplied by itself! When you're making functions, it helps to give them names like this that make it easy to tell what they do even without reading the code inside them.

**Start writing your code in Mu's editing area!** First, define a function with the name `square` and the parameter `x`, like this:

```python=1
def square(x):
```

Next comes the code inside the function. In this code we'll make a variable called `result` and assign it the value of parameter `x` squared. Don't forget to indent this line using the Tab key!


```python=1
def square(x):
    result = x**2
````

Now the function will assign a value to the **result** variable as its return value. To get a function to give you its return value, you can use a **return statement**, which is written like this:

```python=1
def square(x):
    result = x**2
    return result
```

Now you've fully defined a function! Click the **Run** button in the top menu bar to try running the program.

As you can see, just defining a function and then running the program doesn't give you any results. You actually need to call the function to use it! Type the following code into your terminal to call the function `square()` and calculate the value of 10 squared.

<pre class="prettyprint">
&gt;&gt;&gt; square(10)
100
</pre>

Writing a function in the `function name(parameter)` format like this calls the function! Let's try putting different numbers as the parameter to make sure the function squares them properly too.

* Calculating 123 squared

<pre class="prettyprint">
&gt;&gt;&gt; square(123)
15129
</pre>

* Calculating 1.2 squared

<pre class="prettyprint">
&gt;&gt;&gt; square(1.2)
1.44
</pre>

Now we've made a function that squares numbers, but we could do that by just using the ** operator without needing to define a function at all. What if we wanted to do a more complicated calculation several times in a long program, though? Writing the whole equation out every time would get really annoying! In that situation, making the equation into a function you just need to call and set the parameters for to get results can save you a lot of time and space in your program.

## Explaining Code with Comments
We've already talked about how it's important to give functions and variables easy-to-understand names.  However, sometimes names alone aren't enough to tell a person reading your program what the code does. If you need to give more detailed explanations, you'll have to leave **comments** in your code!

### Writing Comments
If you try to write regular text (not a string or a variable name or anything) in a computer program, it will cause an error. In Python, you can get around this by putting a # sign before the text to leave it as a comment in your program. A program with comments might look something like this:

```python=1
#  You can write comments in any language!
a = 10    # Set variable to 10
b = 20    # Set variable to 20

# The code below is supposed to overwrite the value of variable a, but the # in front of it turns it into a comment, so it won't run!
# a = 50
c = a + b

'''
by putting them between two sets of three quotation marks, like this!
'''

print(c)
```

If you run the program above, you'll get these results:

<pre class="prettyprint">
&gt;&gt;&gt;
raw REPL; CTRL-B to exit
&gt;OK

30
</pre>

The line `a = 50` would normally change `a`'s value to **50** in the middle of the program, but because that line was turned into a comment, the result is calculated using value assigned earlier with `a = 10`, making the result **30**.

Comments are just notes written in the code, which the program ignores when it runs. When you use a **#** sign to write a comment, everything from that sign to the end of the line becomes a comment. When you start a new line, your text becomes Python code again. As we saw with `#a = 50`, you can use a **#** sign in front of text that would usually give a command to make the program ignore it, if you want to write something in but not use it yet. Then when you want use it all you need to do is delete the # to make it work!

You can also use lines made up of three quotation marks (''' or """) to turn all the lines between them into a comment, like on lines 9-12. This is called a **here document**. You can use a here document if you want to write a long explanation or make the program ignore several lines of code at once.

Comments are useful because they help explain what your code is supposed to do if you show it to someone else, or come back to it later when you've forgotten the details yourself! You can use them, along with clear variable and function names, to make your code easy to understand.

## Challenge: A Musical Function
In **Topic 1-2** you used variables to change the tempo of a song playing through the Core Unit's Buzzer. This time, let's make a function with a parameter you can use to set six different keys (0-5) to change the musical register of a song!

#### Example Video
<iframe src="https://player.vimeo.com/video/482864114" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

### Making the Program
We'll start out with some set-up code to let this program control the Core Unit's Buzzer and timing. Copy the code below and paste it into your editor.

```python=1
from pystubit.board import buzzer
import time
```

Next we'll define a function called `play_music(key)`, which makes the Buzzer play a song and lets you set different keys as its parameter. Add the code shown in line 4 below to line 4 of your program.

```python=1
from pystubit.board import buzzer
import time

def play_music(key):
```

Now we're going to program the Buzzer to play the song "My Grandfather's Clock" by putting code that plays each note of the song in order into the `play_music()` function. Copy lines 5-29 of the code below and paste it into your program. Make sure every line of code you paste here is indented!

##### Example Code 4-1-1
```python=1
from pystubit.board import buzzer
import time

def play_music(key):
    buzzer.on(str(55+key*12))
    time.sleep_ms(600)
    buzzer.on(str(60+key*12))
    time.sleep_ms(600)
    buzzer.on(str(59+key*12))
    time.sleep_ms(300)
    buzzer.on(str(60+key*12))
    time.sleep_ms(300)
    buzzer.on(str(62+key*12))
    time.sleep_ms(600)
    buzzer.on(str(60+key*12))
    time.sleep_ms(300)
    buzzer.on(str(62+key*12))
    time.sleep_ms(300)
    buzzer.on(str(64+key*12))
    time.sleep_ms(300)
    buzzer.on(str(64+key*12))
    time.sleep_ms(300)
    buzzer.on(str(65+key*12))
    time.sleep_ms(300)
    buzzer.on(str(64+key*12))
    time.sleep_ms(300)
    buzzer.on(str(57+key*12))
    time.sleep_ms(600)
    buzzer.off()
```

Code commands that play specific musical notes through the Buzzer are written like this:

buzzer.on(str(MIDI note number in the default key+the key variable*12))
</pre>

To set a note in the `()` of the `buzzer.on()` command, you can use either **scientific pitch notation** (a note name followed by an octave number) or a **MIDI number** (a note number determined by the MIDI technical standards for audio). Whichever you use, you will have to make it into a string to use it in your code.

##### Notes and MIDI Numbers
|Scientific Pitch Notation|Solfa Notation|MIDI Note Number|
|:---|:---|:---|
|C3|Do 3|48|
|D3|Re 3|50|
|E3|Mi 3|52|
|F3|Fa 3|53|
|G3|Sol 3|55|
|A3|La 3|57|
|B3|Ti 3|59|
|C4|Do 4|60|
|D4|Re 4|62|
|E4|Mi 4|64|
|F4|Fa 4|65|
|G4|Sol 4|67|
|A4|La 4|69|
|B4|Ti 4|71|
|C5|Do 5|72|

For each octave you go up in pitch, the MIDI note number goes up by 12. This means that the difference between C4 and C3 (60-48) and F4 and F3 (65-53) will both be 12. 

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-3/1-3_4_E.png"/>

We can use this property of MIDI note numbers to make an equation that changes the key of the song. The equation will look like this: `50+key12`. It starts at C3 and adds 12 to the MIDI note number every time you increase the octave of the song. When you use a MIDI number to specify a note, it still needs to be a string, so we put the equation inside the built-in `str()` function. This makes the function convert the number from the equation's results into a string.


### Running the Program
Now we've finished defining the function! Click the **Run** button in the top menu bar to try running the program.

You can write code in the terminal to call the function defined in your program and run it! Type in the code below and press the **Enter** key to run it.

<pre class="prettyprint">
&gt;&gt;&gt; play_music(0)
</pre>

Once you've made sure the program plays the song, try running it again with the parameter set to a different number, try out each number from 1 to 5 and see if the song goes up an octave each time!

<pre class="prettyprint">
&gt;&gt;&gt; play_music(1)
&gt;&gt;&gt; play_music(2)
&gt;&gt;&gt; play_music(3)
&gt;&gt;&gt; play_music(4)
&gt;&gt;&gt; play_music(5)
</pre>

If you use an integer of 6 or higher, or -1 or lower, to set the parameter for this function, you'll cause an error. That's because putting those numbers into the `50+key*12` equation produces a number that's outside the range of MIDI note numbers (48-127) the Core Unit's Buzzer can play!

## Conclusion
### A Quick Review
In this lesson we learned how to use **functions** and **comments** in Python. There are still a lot of useful things you can do with functions we haven't talked about yet, though! You'll learn more about those as the course goes on. For now just make sure you understand how we used them in this lesson!

###  Coming Up Next Time!
In the next lesson we'll start learning about **objects**, the core of **object-oriented** programming languages like Python!