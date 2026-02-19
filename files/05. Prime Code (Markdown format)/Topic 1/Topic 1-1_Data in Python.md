---
tags: Python_English, Topic 1
---

Python Robotics Course Lesson 1 

Topic 1-1
Data in Python
===
Learn about the different kinds of data used in Python, like numbers and strings!

## In This Lesson...
Start out by learning how to use the software and hardware we'll be using to study Python in this course, then move on to learn about the most common types of data in programming: numbers and strings. At the end of the lesson you can test what you've learned by making your name appear on the LED display!

:::info
Lesson Introduction

■ Lesson 1 Contents

1. Our Study Materials (Studuino:bit)
2. Installing Mu (A Python Text Editor)
3. Before You Start a Lesson
4. Using Numbers and Strings
5. Operations with Numbers and Strings
6. Challenge: Get Your Name On Screen



In this lesson you'll set up the study tools and software you need for this course set up, and then learn about two kinds of data computers can use: numbers and strings.

We'll start the lesson by introducing the Studuino:bit learning tool.

then walk you through installing Mu, which is the special text editor you'll need to use to write Python programs.

After that we'll go over the set up routine you should use every time you start a new lesson.

Learn how to use numeric and string data in Python by running a simple program.

We'll also talk about operators and how to use them to do things like solve math problems and put strings together.

At the end of the lesson you can try programming the Studuino:bit Core Unit to make your name scroll across the LED display, just like in this video!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!
:::

##### Figure. Studuino:bit Core Unit
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_1_E.png"/>

We can use the many functions of the Studuino:bit Core Unit to practice different kinds of programming! For example, you can make an animated light show by programming the lights in the LED display to change colors in a specific order.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_2_E.gif"/>

The Studuino:bit Core Unit also includes buttons, a Light Sensor, a Temperature Sensor, an Accelerometer, a Gyroscope, and a Compass it can use to detect information about its surroundings. You can use Python programs to control all of these different parts!

##### Figure. Studuino:bit Core Unit without casing
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_3_E.png"/>


## Getting a Python Text Editor
To get started writing programs in Python, you'll need a special text editor to write them in. There are plenty of different Python editors out there, but in this course we'll be using an **open source** editor called **Mu**.
###### ★ Open source - A term that describes software with source code (the text of the program that makes up the software, also called just "code") that's freely available to download, edit, and redistribute, at no cost.

### Mu's Layout
The layout of the Mu editor is pretty simple! It doesn't include any highly-advanced functions, but that makes it easier for beginners to use.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_4_E.gif"/>

###  Installing Mu
Mu is available for both Windows and Mac. If you don't have Mu installed on the computer you're using, follow the links below to download the installer for your operating system.

##### Download Mu for Windows
* 64-bit version
https://www.artec-kk.co.jp/artecrobo2/data/mu-editor_64bit.exe

* 32-bit version
https://www.artec-kk.co.jp/artecrobo2/data/mu-editor_32bit.exe

##### Download Mu for Mac
https://www.artec-kk.co.jp/artecrobo2/data/mu-editor.zip

Now that you have an editor, you can type all the sample Python programs from this course into it and try running them to see how they work for yourself! This kind of hands-on approach to learning programming might be tough at times, but don't worry, you'll get the hang of it!

## Before You Start a Lesson
There are a lot of different lessons in this course, but there are a few things you should do before you start each one!

###  Connecting the Core Unit
You'll be sending the programs you write in your code editor to the Studuino:bit Core Unit from your computer via USB cable. You can use the USB cable that comes with your Core Unit to connect it to the computer .

1. Plug the bigger end of the USB cable into your computer.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_5_E.png"/>

2. Plug the smaller end of the USB cable into your Core Unit.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_6_E.png"/>

### Opening the Editor
1. Double click this icon to start up Mu.
 
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_7_E.png"/>

2. The first time you open Mu, a screen asking what mode you want to use it in will appear. Select **Artec Studuino:Bit MicroPython** from the list shown, then click the **OK** button. The next time you start Mu it will automatically open in the mode you've selected.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_8_E.png"/>

3. Click the **REPL**<sup>*</sup> button in the middle of the upper part of the window.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_9_E.png"/>

###### ★ REPL is short for "**R**ead-**E**val-**P**rint **L**oop," meaning a looping process that reads the information you type in, evaluates it, and then shows you the results. This loop process can be used to interactively execute Python source code.

4. When you have REPL running, a dialogue window will appear at the bottom of the screen. When this window is open, your computer is communicating with the Core Unit, so don't unplug the USB cable. To stop REPL, just click the REPL button again.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_10_E.png"/>
## Numeric Data in Python
Computer programs can handle a lot of different kinds of data, from numbers to letters to pictures to sound to video. Let's start by talking about the most basic form of data: **numbers**.

###  Using Numeric Data
When you think about using computers to do math, the first kind of computer that comes to mind is probably a calculator. You can actually use Python to solve math problems the same way you would with a calculator!

#### ■  Types of Numeric Data
When you use a calculator you don't need to think about the difference between integer and decimal numbers, but computer programs treat these as two separate types of data.

* Integers
0, natural numbers (1, 2, 3...), and negative numbers (-1, -2, -3...) are all considered **integers**.

* Floating-Point Numbers
Numbers with decimal points (1.01, 3.14, etc.) are considered **floating-point numbers**.

#### ■ Mathematical Operators　
Mathematical calculations are also called **operations**. You can use Python to perform the four basic operations (addition, subtraction, multiplication, and division), as well as many more complicated ones!

In programming languages, symbols that stand for calculations (like + and -) are called **operators**. The operators for addition and subtraction in Python are still + and -, but multiplication is * (asterisk) instead of x, and division is / (slash) instead of ÷.

|Types of Operator|Operator|
|:---|:---|
|Addition|+|
|Subtraction|-|
|Multiplication| * |
|Division|/|


Let's try writing programs to solve math problems!

#### ■ Adding Integers
Let's start with a simple equation: **1+2**. Programs that only take a single line of code like this one can be typed into the REPL window and run instantly! This window is usually called the **terminal window**, or just the **terminal**. We'll be calling it the terminal in this course too!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_11_E.png"/>

Click next to the >>> on the last line in the terminal, and type in the following code there.

<pre class="prettyprint">
&gt;&gt;&gt; 1+2
</pre>


**Press the Enter key to run your code!** The result of your calculation (3) should appear in the terminal as shown below.

<pre class="prettyprint">
&gt;&gt;&gt; 1+2
3
&gt;&gt;&gt;
</pre>

###### ★ If you can't enter text in the terminal, that means your computer isn't communicating with the Core Unit. Make sure your USB cable is securely plugged in, then press the REPL button and close the terminal. Press REPL again to reopen the terminal and try again.

#### ■ Adding Floating-Point Numbers
Next let's try an addition problem using decimals, **1.2+1.8.** Type in the equation, then press **Enter** to run your code.

###### ★ You can't delete messages in the terminal! Type in your new equation after the previous lines, like this:

<pre class="prettyprint">
&gt;&gt;&gt; 1+2
3
&gt;&gt;&gt; 1.2+1.8
3.0
&gt;&gt;&gt;
</pre>


The result of the equation is still 3, but because we used floating-point number data, the result is appears in the terminal as **3.0**, with a decimal point.

#### ■ Subtracting Integers and Floating-Point Numbers
Now let's try a subtraction equation using decimals, 4.9-0.9.

<pre class="prettyprint">
&gt;&gt;&gt; 1+2
3
&gt;&gt;&gt; 1.2+1.8
3.0
&gt;&gt;&gt; 4.9-0.9
4.0
&gt;&gt;&gt;
</pre>

When both of the numbers in a subtraction equation are floating-point numbers, the result will also be shown as a floating-point number!

#### ■ Multiplying Integers
Now let's multiply two integers together: **4x9**. Remember to use an asterisk (*) instead of an x as your multiplication sign!

<pre class="prettyprint">
&gt;&gt;&gt; 1+2
3
&gt;&gt;&gt; 1.2+1.8
3.0
&gt;&gt;&gt; 4.9-0.9
4.0
&gt;&gt;&gt; 4*9
36
&gt;&gt;&gt;
</pre>

When both of the numbers in a multiplication equation are integers, the result will also be shown as an integer. However, if either number is a floating-point number, the result will be a floating-point number!

#### ■ Dividing Integers
For our last equation let's divide one integer by another, 54÷6. Remember to use a slash (/) instead of a ÷ as your division sign!

<pre class="prettyprint">
&gt;&gt;&gt; 1+2
3
&gt;&gt;&gt; 1.2+1.8
3.0
&gt;&gt;&gt; 4.9-0.9
4.0
&gt;&gt;&gt; 4*9
36
&gt;&gt;&gt; 54/6
9.0
&gt;&gt;&gt;
</pre>
You'll notice that the result appears as **9.0**, not **9**! The result (or quotient) of a division operation will always be a floating-point number, even if the numbers divided evenly.

#### ■ Other Operators

In addition to the four basic operators we just tested, Python also has operators for these more complicated operations:

|Types of Operator|Operator|
|:---|:---|
|Floor Division (finding the result of a division operation minus any remainder)|//|
|Modulo (finding only the remainder of a division operation)|%|
|Exponentiation (multiplying a number by itself)| ** |

##### (Example Equations)

<pre class="prettyprint">
&gt;&gt;&gt; 10//3
3
&gt;&gt;&gt; 10%3
1
&gt;&gt;&gt; 2**3
8
&gt;&gt;&gt; 
</pre>

#### ■ The Order of Operations
When you use multiple operators in an equation, certain operations always happen before others. Operations with the higher priority in the order of operations always happen before operations with lower priority in the same equation.

##### The Order of Operations in Python (From Highest to Lowest Priority)

|Operator|Types of Operator|
|:---|:---|
| ** |Exponentiation|
| *, /, //, % |Multiplication, Division, Floor Division, Modulo|
|+, -|Addition, Subtraction|

Just like in regular math, if you have both a + and a * in your code, the * operation will happen first. If you want to make a calculation happen before the rest of the equation regardless of the order of operators, put it in parentheses **( )**!


##### (Example Equations)

<pre class="prettyprint">
&gt;&gt;&gt; 9+2*3
15
&gt;&gt;&gt; (9+2)*3
33
&gt;&gt;&gt;
</pre>



###  Changing Numeric Data Types
As we've seen in our calculations so far, integers and floating-point numbers are treated as different types of data by your computer. In programming languages, including Python, all data is sorted into different **data types**.

#### ■ Finding a Number's Data Type
You can find out what data type a piece of data belongs to using a command called `type()`.

```
type(Data)
・・・Find the data type of a piece of data.
```

Try putting the code below into your terminal to see what the data types for integers and floating-point numbers are.

<pre class="prettyprint">
&gt;&gt;&gt;type(1)
&lt;class 'int'&gt;
&gt;&gt;&gt; type(1.1)
&lt;class 'float'&gt;
&gt;&gt;&gt; 
</pre>


The text after `<class`(`'int'` or `'float'`) is the name of that kind of number's data type. int is short for integer, and float is short for floating-point number!

#### ■ Changing a Number's Data Type
Now, what if we want to make the terminal say 9 instead of 9.0? We'll have to change the data type of the number (from floating-point to integer, in this case). In Python you can change a number's data type using these commands:

```
int(Data you want to make into an integer)　
・・・ Changes the type of the data you choose to integer (int).
```

```
float(Data you want to make into a floating-point number)　
・・・ Changes the type of the data you choose into a floating-point number (float).
```

Try running the code below to see how these commands change the types of your data.

###### ★ Make sure the _int()_ and _float()_ commands go inside the _type()_ command that checks the type of your data!

<pre class="prettyprint">
&gt;&gt;&gt; type(float(1))
&lt;class 'float'&gt;
&gt;&gt;&gt; type(int(1.1))
&lt;class 'float'&gt;
&gt;&gt;&gt;
</pre>

The above results tell us that the number 1.1 has had its data type changed to integer, but, you might ask, what happens to the extra 0.1 when we do this? The answer is that using the int() command on a number removes any values after the decimal point.

<pre class="prettyprint">
&gt;&gt;&gt; int(1.1)
1
&gt;&gt;&gt;
</pre>


This means the code above effectively turns 1.1 into the integer 1 and then finds its data type.

## String Data in Python
The next most common data type after numbers is what we call **strings**.

### Using String Data
In programming languages, a series of letters or words is called a **string**. This includes letters and characters from outside the English alphabet too!

#### ■ Making a String
If you just write a string of letters in the terminal and run it, you'll get an error, like this:

<pre class="prettyprint">
&gt;&gt;&gt; hello
Traceback (most recent call last):
  File "&lt;stdin&gt;", line 1, in &lt;module&gt;
NameError: name 'hello' isn't defined
&gt;&gt;&gt; 
</pre>

Don't worry about the content of this error message for now, we'll explain what it means in Lesson 1-2 when we talk about **variables**.

To make text into a string in Python, you need to put either single quotations ( ' ) or double quotations ( " ) on each side of the text. That will stop the error we saw above from happening.


<pre class="prettyprint">
&gt;&gt;&gt; 'hello'
'hello'
&gt;&gt;&gt;
</pre>

Don't forget to put quotations around your text when you want to use it as a string! In Python there's no real difference between strings made with single quotations versus double quotations.

#### ■ Displaying Strings in the Terminal
If you want to make a string of letters appear as text in your terminal with Python, use the print() command!

```
print(Text)
・・・ Displays the string you type in as text in the terminal.
```

Try running the code below to see how it works!

<pre class="prettyprint">
&gt;&gt;&gt; print('hello')
hello
&gt;&gt;&gt;
</pre>

Unlike the text we typed in, the text the terminal displays doesn't have quotations around it.

#### ■ Using Special Characters in Strings
Now we know that quotation marks are used to turn text into a string, but what do we do if we want single or double quotation marks to appear in the text of the string we're making?

Let's say we want to make a string with the text **hello 'python'**. If we use that as-is, we'll get an error, like this:

<pre class="prettyprint">
&gt;&gt;&gt; print('hello 'python'')
Traceback (most recent call last):
  File "&lt;stdin&gt;", line 1
SyntaxError: invalid syntax
&gt;&gt;&gt;
</pre>

This error happens because the computer thinks the ’ in front of `python` is ending the string started by the ’ in front of `hello`. There are a couple different solutions you can use in a case like this to make your string print properly.

* Solution 1
If you want to use single quotations in the text of your string, you can use double quotations to mark the beginning and end of the string. Likewise you can mark the beginning and end of the string with single quotations if you want to use double quotations in the text.

<pre class="prettyprint">
&gt;&gt;&gt; print("hello 'python'")
hello 'python'
&gt;&gt;&gt; print('hello "python"')
hello "python"
&gt;&gt;&gt;
</pre>

* Solution 2
If you put a backslash **( \ )** in front of a quotation mark, the computer will know that quotation mark is part of the text of the string. A **\** used in this way is called an **escape sequence**!

###### ★ Remember to use a backslash \ and not a forward slash!

<pre class="prettyprint">
&gt;&gt;&gt; print('hello \'python\'')
hello 'python'
&gt;&gt;&gt;
</pre>

You can also use \ to put line breaks in the text of your string. Typing **\n** in a string turns the letter **n** into a line break. This is also an escape sequence!

<pre class="prettyprint">
&gt;&gt;&gt; print('hello\npython')
hello
python
</pre>

#### ■ Connecting Strings
If you use the addition operator + between two strings in your code, that connects the two strings together. As shown below, connecting the two strings **abc** and **def** like this makes the string **abcdef**. The technical name for this operation is **concatenation**!

<pre class="prettyprint">
&gt;&gt;&gt; 'abc'+'def'
'abcdef'
&gt;&gt;&gt;
</pre>

#### ■ Repeating a String
Use the * multiplication operator to repeat a string multiple times. As shown below, multiplying the string **abc** by 3 repeats abc three times, making the string **abcabcabc**. The technical name for this operation is **replication**!

<pre class="prettyprint">
&gt;&gt;&gt; 'abc'*3
'abcabcabc'
&gt;&gt;&gt;
</pre>

### Converting Strings and Numbers
The number **123** and the string **'123'** are completely different types of data to your computer. Most of the time you'll want to use numbers as numeric data so you can make calculations with them, but sometimes you might want to change their data type and so you can use them as text instead.

#### ■ Turning Numbers into Strings
We can find out how many characters long a string is using a command called `len()`.

###### ★ The name of the command **len()** is short for "length," because it finds the length of a string!

```
len(String data)
・・・ Finds the number of characters in a string.
```

Let's try using `len()` to find the number of letters in the string **python**.

<pre class="prettyprint">
&gt;&gt;&gt; len('python')
6
&gt;&gt;&gt;
</pre>

Using the `len()` command could be an easy way to tell how many digits long a number is, but because `len()` doesn't accept numeric data, putting numbers into it directly causes an error.

<pre class="prettyprint">
&gt;&gt;&gt; len(123456)
Traceback (most recent call last):
  File "&lt;stdin&gt;", line 1, in &lt;module&gt;
TypeError: object of type 'int' has no len()
</pre>

That means if you want to use `len()` like this, first you need to turn the number into a string. The `str()` command lets you do just this!

###### ★ **str** is short for "string"!
```
str(Data you want to make into a string)
- Changes the data you choose into a string.
```

Now let's try running this code using `str()` to change data types...

<pre class="prettyprint">
&gt;&gt;&gt; len(str(123456))
6
</pre>

Converting numbers into strings and strings into numbers lets you do plenty of useful tricks like this one, so programmers do it all the time!

#### ■ Turning Strings into Numbers
To change a string of numbers back into numeric data, you'll need to use the `int()` and `float()` commands you learned earlier.

The code below takes the strings **'123'** and **'456'**, converts them into numeric data using `int()`, the adds them together for a result of **579**.

<pre class="prettyprint">
&gt;&gt;&gt; int('123')+int('456')
579
&gt;&gt;&gt;
</pre>

## Challenge: Get Your Name On Screen
To complete this lesson, use what you've learned about string data to make your name scroll across the Core Unit's LED display!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_12_E.gif"/>

### Making the Program
So far we've just been typing single lines of code into the terminal and running them, but making a program that controls the Core Unit will require multiple lines of code. To write multi-line programs, you'll have to use the editing area instead.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_13_E.png"/>

First try copying the two lines of code below and pasting them into the editing area. It just takes two lines of code to make a word scroll across the display!

###### ★ A more detailed explanation of this code will appear in later lessons!

```python=1
from pystubit.board import display
display.scroll('Please write your name here.')
```

##### How to Copy
1. Highlight the two lines of code above, then right-click the highlighted area and select **Copy** from the context menu that appears.
2.  Click on the editing area in Mu, then right-click to open another context menu and select **Paste**.


To explain this code just a bit, the first line is a command that brings the set of programming tools you need to control the LED display (named `display`) into your program.

```
from pystubit.board import display
```

The second line uses a command called `scroll()` from that set to make a string scroll across the display. You can put the string you want to display in the **()** in the `scroll()` command.

```
display.scroll('Please write your name here.')
```

Now replace the string `'Please write your name here.'` with your name! Make a string of your name, written in English letters (since the Core Unit's display can't use characters from other languages).

You can use `scroll()` to display numbers, but those numbers need to be a string, not numeric data! You can accomplish this with either of these two methods:

* Put quotation marks around numbers to turn them into a string

```python=1
from pystubit.board import display
display.scroll('123')
```

*  Use the **str()** command to turn numbers into a string

```python=1
from pystubit.board import display
display.scroll(str(123))
```

### Running the Program
To run the program you've written, click the Run button in the top bar of the editor.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_14_E.png"/>

When you click **Run**, the terminal will open and a message (shown below) will appear. After that, your name will scroll across the Core Unit's LED display!

<pre class="prettyprint">
MicroPython v1.10-222-g1afc30c-dirty on 2019-06-17; ESP32 module with ESP32
Type "help()" for more information.
&gt;&gt;&gt; 
&gt;&gt;&gt; 
&gt;&gt;&gt; 
&gt;&gt;&gt; 
&gt;&gt;&gt; 
raw REPL; CTRL-B to exit
&gt;OK
</pre>


### Saving the Program
You can save programs by clicking the **Save** button in the editor's top bar. Pick a name for your program and a folder to save it in. The program will be saved as a program file, with the file extension **.py**.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_15_E.png"/>

Computers use file extensions to tell the differences between different kinds of files, so when a file has the extension .py, the computer knows it's a Python program.

If you want to open a program you've saved before and edit it, click the Open button in the editor and select the file you want.。

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-1/1-1_16_E.png"/>

## Conclusion
###  A Quick Review
In this lesson we learned...

* The parts of the Studuino:bit Core Unit
* How to use an editor to write programs in Python
* How to use numeric data
* How to use string data

There are many other data types in Python, like **lists**, **tuples**, and **dictionaries**. You can learn more about these data types when you get to **Lesson 2-1**!

### Coming Up Next Time!
In **Lesson 1-2** we'll be learning how to use **variables** to save data so you can use it later in your program!


