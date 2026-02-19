---
tags: Python_English, Topic 1
---

Python Robotics Course Lesson 2

Topic 1-2
Variables in Python
===
Learn to save data in **variables**!

## In This Lesson...

Learn about **variables** and how they let you save data within a program, then test what you've learned by using variables to change the tempo of a song playing on the Core Unit's Buzzer!

:::info
Lesson Introduction

■ Lesson 2 Contents

1. How to Use Variables
2. Data Types and Variables
3. Operations with Variables
4. Challenge: Change the Tempo



In this lesson you'll learn about variables and how they let you save data within a program.

We'll start by explaining what variables do and how to use them,

then see how a variable's data type changes to match the type of data you store in it.

After that we'll look at a number of example programs to see how variables are used to make calculations in Python.

At the end of the lesson, you can try using variables yourself to write a program for the Core Unit's Buzzer.

The Buzzer will play a song just like in this video, but you can change how long each note plays by using a variable!

Now let's start the lesson!
:::


## Variables in Python

In **Topic 1-1** we used REPL to perform simple mathematical calculations. For these problems you could just enter a single line of code and see the results right away, but most of the programs we write from now on will perform calculations across many lines of code instead. In programs like this, sometimes you want to use the results of a calculation from an earlier line in the code.

Let's use this math problem as an example:

##### Finding Speed and Time to Reach a Destination
You get in a car at Point A and start driving toward Point B, which is 50 km away. When 16 minutes have passed, you've traveled 20 km. How many minutes will it take you to drive the remaining 30 km to your destination?

We can find the answer to this problem with the following two equations:

* Average speed while driving 20 km = 20 km ÷ 16 minutes = 1.25 km per minute
* Time needed to drive the remaining distance = 30 km ÷ 1.25 km per minute = 24 minutes

To solve the problem, we needed to use the results of the first equation in the second equation! If we want to solve these problems with a computer program, we need to store the average speed we found with the first equation somewhere so we can use it in the second equation. The place we store that information is called a **variable**. Variables are often compared to boxes, but boxes that store data!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-2/1-2_1_E.png"/>

You can define a variable using this format:

```
Variable name = Data you want to store in the variable
```

Putting data into a variable for storage like this is called assigning a value to a variable. In math, "A = B" means "A is equal to B," but in Python, it means "the value of A is assigned to B." To write "A is equal to B" in Python, we use two = signs together, like this: **A==B**

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-2/1-2_2_E.png"/>


### Using Variables
Now let's try using variables to solve the math problem we looked at earlier.

First, write the following code into the terminal to calculate the average speed of a car driving 20 km in 16 minutes, then press **Enter**.

###### ★ We've named the variable that stores the car's speed v, short for velocity, the physics term for how fast an object moves in a specified direction.

<pre class="prettyprint">
&gt;&gt;&gt; v = 20/16
&gt;&gt;&gt;
</pre>

The results of the equation won't be shown in the terminal when you store them in a variable like this. If you want to see the results of this equation, you'll have to use the **print()** command! Add the following lines to the code you've written so far.

<pre class="prettyprint">
&gt;&gt;&gt; v = 20/16
&gt;&gt;&gt; print(v)
1.25
&gt;&gt;&gt;
</pre>

###### ★ Don't put quotation marks around the name of a variable you're putting in the print() command's parentheses! Using quotation marks will make the command read the variable name as a string instead.

This codes lets us confirm that the variable **v** is storing the results of our last equation. Now we can use this variable to calculate how long it will take to drive the remaining 30 km!

<pre class="prettyprint">
&gt;&gt;&gt; 30/v
24.0
&gt;&gt;&gt;
</pre>

Now we've solved the whole problem by programming both equations!

### Naming Variables
You can give a variable pretty much any name you like! Names that consist of only one letter like **a**, **b**, or **c** will all work fine as far as the computer's concerned. However, when you're writing a program you'll often need to make a lot of different variables, so using names like a or b may make it difficult to remember which variable is which.

This means you'll usually want to give variables names that tell you what kind of data is stored in them, for example calling variables that record how old someone is and where they live **age** and **address**.

In Python, if you want to give a variable a name made up of multiple words, you should use an underscore ( _ ) to connect the two words, like **last_name** or **first_name** for variables storing someone's first and last names. This way of writing is called **snake case**, because it lies flat like a snake. Another way to connect words to make variable names is to leave no space between the words and capitalize the second word (but not the first). This method is called **camel case** because it has a hump in the middle like a camel's back!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-2/1-2_3_E.png"/>

#### ■ Unusable Variable Names

Some words (called **keywords**) can't be used as variable names in Python because the language already uses them to mean something else, like **print()** for example. If a word is a keyword in Python, you won't be able to define a variable with that word as its name. If at some point in the course you get an error when trying to run sample code from the lesson or another source, and the only change you made to the code was variable names, check to make sure you didn't use a keyword to name a variable. To find out what words are keywords, go to the URL below to check the official Python documentation.

[Keywords - Python 3.6 Documentation](https://docs.python.org/3.6/reference/lexical_analysis.html#keywords)

### Overwriting Data in a Variable
When you've made a variable and assigned a value to it, you can replace that value by **overwriting** it with different data. Try running the code below in your terminal to see how overwriting works!

<pre class="prettyprint">
&gt;&gt;&gt; a = 5
&gt;&gt;&gt; a = 10
&gt;&gt;&gt; print(a)
10
&gt;&gt;&gt;
</pre>

First we assigned the value **2** to variable **a**, but then we overwrote it with the value **10**, so the result of **print(a)** became **10**.

You can also use the current value of a variable in an equation that overwrites that value with its results, like this:

<pre class="prettyprint">
&gt;&gt;&gt; f = 100
&gt;&gt;&gt; f = f + 100
&gt;&gt;&gt; print(f)
200
&gt;&gt;&gt;
</pre>

In this code, variable **f** is assigned the value of **100**, then the program overwrites the value of **f** with the results of the equation "the value of f (100) plus 100." As a result, the value of **f** at the end of the program is **200**.

## Data Types and Variables
We learned about data types like numbers and strings in the last lesson, and these data types apply to variables as well! A variable's data type matches the type of the data assigned to that variable as its value. You can find the data type of a variable using the **type()** command, as usual.

*  Find the data type of a variable storing an integer

<pre class="prettyprint">
&gt;&gt;&gt; number = 100
&gt;&gt;&gt; type(number)
&lt;class 'int'&gt;
&gt;&gt;&gt;
</pre>

* Find the data type of a variable storing a string

<pre class="prettyprint">
&gt;&gt;&gt; string = 'abc'
&gt;&gt;&gt; type(string)
&lt;class 'str'&gt;
&gt;&gt;&gt;
</pre>

### Overwriting Variables with a Different Data Type
Variables in Python are very flexible! Some programming languages won't let you change the data type of a variable once you've assigned it. In these languages you can't overwrite a variable's value with data of a different type, but in Python you can!


<pre class="prettyprint">
&gt;&gt;&gt; data = 4
&gt;&gt;&gt; type(data)
&lt;class 'int'&gt;
&gt;&gt;&gt; data = 4.0
&gt;&gt;&gt; type(data)
&lt;class 'float'&gt;
&gt;&gt;&gt; data = '4'
&gt;&gt;&gt; type(data)
&lt;class 'str'&gt;
&gt;&gt;&gt;
</pre>

This code assigns data of three different types (integer, floating-point number, and string) to the same variable, and the data type of the variable changes to match each time, no problem!

## Programming with Variables
Now let's use what we've learned so far to write two different programs using variables and see how they work!

### Calculating Sales Price with Tax Included
Saving the sales tax rate as a variable lets you calculate the price of multiple products with tax included!

<pre class="prettyprint">
&gt;&gt;&gt; tax = 1.1
&gt;&gt;&gt; apple = 100*tax
&gt;&gt;&gt; orange = 120*tax
&gt;&gt;&gt; banana = 250*tax
&gt;&gt;&gt; print(apple, orange, banana)
110.0 132.0 275.0
</pre>

###### ★ You can use the _print()_ command to display several different pieces of data by separating them with commas!


### Displaying Multiple Messages with Shared Text
Saving a string of text you want to appear in multiple messages as a variable and connecting it with a + operator lets you display messages with simpler code! In the example below we've used a variable to store the text string **is now loading** so we can use it in loading notifications for different programs.

<pre class="prettyprint">
&gt;&gt;&gt; load = ' is now loading.'
&gt;&gt;&gt; print('data1'+load)
data1 is now loading.
&gt;&gt;&gt; print('data2'+load)
data2 is now loading.
</pre>

## Challenge: Change the Tempo
To complete this lesson, use what you've learned about variables to control the Core Unit's Buzzer!

#### Example Video
<iframe src="https://player.vimeo.com/video/482865736" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

### Making the Program
The code below programs the Buzzer to play the first four bars of the Japanese Frog Song. Don't worry, you don't need to understand everything in this program yet! Just highlight and copy the whole program and paste it into the editing area in Mu.

##### Example Code 5-1-1
```python=1
from pystubit.board import buzzer
import time

duration = 600

buzzer.on('C4')
time.sleep_ms(duration)
buzzer.on('D4')
time.sleep_ms(duration)
buzzer.on('E4')
time.sleep_ms(duration)
buzzer.on('F4')
time.sleep_ms(duration)
buzzer.on('E4')
time.sleep_ms(duration)
buzzer.on('D4')
time.sleep_ms(duration)
buzzer.on('C4')
time.sleep_ms(duration)
buzzer.off()
```

You can see that a variable is being defined on line 4, `duration=600`. The name `duration` refers to the length of time the Buzzer will keep playing each note in the song. This is the data the variable stores.

The commands `buzzer.on(...)` from line 6 onward tell the buzzer to play specific notes. The strings in the **()**, like **C4** and **D4**, all indicate different notes. The `time.sleep_ms()` commands that come after these tell the program to wait for a number of milliseconds (specified in the parentheses) before it gives the next command. These commands use the value of the `duration` variable to determine how many milliseconds they tell the program to wait before it starts playing the next note of the song.

###### ★ One millisecond (abbreviated ms) is one 1/1000th of a second, so 600 ms is 0.6 seconds.


In the last line, the `buzzer.off()` command makes the Buzzer stop playing.

The thing you should pay attention to in this code for now is how the `duration` variable sets the time for every `time.sleep_ms()` command in the program. This means that if you change the value assigned to `duration`, you'll change how long every note in the song plays!

### Running the Program
Click the **Run** button in the top menu bar to run the program and see how it works. Then try changing the value of the `duration` variable and running the program again! Try out different values and see how they change the tempo of the song.

## Conclusion
### A Quick Review
In this lesson we learned about **variables**, and how they store data. Variables can store data of all different types, not just numbers and strings. They'll be showing up again in later lessons, so make sure you understand how they work!

### Coming Up Next Time!
In the next lesson we'll be learning how to use **functions** to keep programs short and understandable!
