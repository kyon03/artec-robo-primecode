---
tags: Python_English, Topic 2
---

Python Robotics Course Lesson 6

Topic 2-2
Writing a Python Program 2
===

Learn about branching programs that run only under specific conditions!

## In This Lesson...
In Topic 2-1 you learned about sequential and looping programs, but there's still one important kind of program structure we haven't talked about: **branches**. Let's learn the basics of branching programs and then try using them to make the Core Unit's sensors control the LED display!

:::info
Lesson Introduction

■ Lesson 6 Contents

1. Conditions and Booleans
2. Conditional Branches
3. Challenge: Programming with Sensors



In this lesson you'll learn about branches, another important basic programming structure like sequences and loops.

We'll start by talking about conditional statements, which you'll need to use to make a branching program. We'll also explain boolean data, which describes the results of conditions. On top of that we'll also need to discuss the operators you'll use to compare values and combine multiple conditions.

Once you've learned about the concepts, you can try writing an actual program that evaluates data and branches based on its findings.

At the end of the lesson, you can try making a program that uses the Core Unit's sensors to control the LED display and make it light up automatically when the Light Sensor detects its surroundings getting dark!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!
:::

## Conditions and Booleans
A branching program is a program that can decide whether to run parts of its code based on whether specific **conditions** are fulfilled or not. That means if you want to learn how to use branches, you need to understand conditions first!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-2/2-2_1_E.png"/>

### Relational Operators for Conditional Statements and Their Results

To make a conditional statement, you need to use a **relational operator**. We talked about these briefly when we learned about while statements in Topic 2-1, but let's take another look at them now.

|Relational Operator|Usage|Meaning|
|:---|:---|:---|
|==|a == b|A is equal to B|
|!=|a != b|A is not equal to B|
|<|a < b|A is less than B (doesn't include a==b)|
|>|a > b|A is greater than B (doesn't include a==b)|
|<=|a <= b|A is less than or equal to B (includes a==b)|
|>=|a >= b|A is greater than or equal to B (includes a==b)|

When you run a conditional statement with a relational operator in your code, you'll get a certain kind of result. What kind? Try running the code below (which uses `==` and `=`! operators) in your terminal and see for yourself!

<pre class="prettyprint">
&gt;&gt;&gt; 1 == 1
True
&gt;&gt;&gt; 1 != 1
False
</pre>

The results of a conditional statement come as a data type called **boolean** data, which has only two possible values: `True` and `False`. If the condition described in a condition statement is fulfilled, its result will be `True`, and if it isn't fulfilled, the result will be `False`.

###### ★ Notice that the **boolean values True and False are always capitalized!** If you write true or false instead when using them in your code, it won't work!

Now let's think about **while** statements again. A **while** statement makes the code inside it run in a loop as long as the conditional statement it starts with is fulfilled, and stops running it when the condition isn't fulfilled.

If we think of this in terms of boolean data, the program is evaluating the conditional statement (determining whether it's fulfilled), and returning a value of either True or False. If it gets a `True` value the loop runs, and if it gets a `False` value it stops.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-2/2-2_2_E.png"/>

This means you can use the code **while True**: (shown below) to make an **endless loop** that keeps repeating the code inside it indefinitely!

```
while True:
    The loop's program
    .
    .
    .
```

If you convert `True` and `False` into integer data, they'll turn into **1** and **0**.


<pre class="prettyprint">
&gt;&gt;&gt; int(True)
1
&gt;&gt;&gt; int(False)
0
</pre>

If you turn integers into boolean data instead, 0 will become `False`, and any other value will be `True`.

<pre class="prettyprint">
&gt;&gt;&gt; bool(0)
False
&gt;&gt;&gt; bool(1)
True
&gt;&gt;&gt; bool(10)
True
</pre>


Similarly, if you convert a string into boolean, a blank string will be `False` and any other string will be `True`.
<pre class="prettyprint">
&gt;&gt;&gt; bool('')
False
&gt;&gt;&gt; bool('a')
True
&gt;&gt;&gt; bool('b')
True
&gt;&gt;&gt; bool('  ')
True
</pre>

This means if you have a process and want to check if its results are 0 or an empty string, you can do that using a conditional statement with `==False` or `==True`, like this:

<pre class="prettyprint">
&gt;&gt;&gt; 1-1 == False
True
&gt;&gt;&gt; 10-2 == True
False
&gt;&gt;&gt; 10/3 == True
True
</pre>


###  Combining Conditions with Logical Operators
Let's say you're in a store deciding what to buy. Usually you have multiple conditions you're considering when picking something, like "clothes that are warm **and** not too expensive" or "ice cream with either chocolate **or** cookies on top." Programming is the same - you'll often want a program to decide when to run a branch based on multiple conditions together. To do this, you'll need to use **logical operators**.

There are three logical operators: **and**, **or**, and **not**. Of these, **and** and **or** are the ones you can use to combine conditions.

#### ■ The **and** Operator

You can use the operator **and** between two conditional statements to put them together, like this:

```
Condition 1 and Condition 2
```

A set of conditions combined with **and** like this has a boolean value of True only when BOTH of the conditions it's made up of are fulfilled. Otherwise, its value will be False.

|Condition 1's Results|Condition 2's Results|The and Operation's Results|
|:---|:---|:---|
|**True**|**True**|**True**|
|**True**|False|False|
|False|**True**|False|
|False|False|False|

Try running the code below in your terminal to see how this works!

<pre class="prettyprint">
&gt;&gt;&gt; 5 &lt; 10 and 'a' == 'a'
True
&gt;&gt;&gt; 5 &lt; 10 and 'a' == 'A'
False
&gt;&gt;&gt; 5 &gt; 10 and 'a' == 'a'
False
&gt;&gt;&gt; 5 &gt; 10 and 'a' == 'A'
False
</pre>

#### ■ The **or** Operator
You can use the operator **or** between two conditional statements to put them together, like this:

```
Condition 1 or Condition 2
```

A set of conditions combined with **or** has a boolean value of True if EITHER of the conditions it's made up of are fulfilled. Its value will be only be False if both of the combined conditions are not fulfilled.

|Condition 1's Results|Condition 2's Results|The **or** Operation's Results|
|:---|:---|:---|
|**True**|**True**|**True**|
|**True**|False|**True**|
|False|**True**|**True**|
|False|False|False|

Try running the code below in your terminal to see how this works too!

<pre class="prettyprint">
&gt;&gt;&gt; 5 &lt; 10 or 'a' == 'a'
True
&gt;&gt;&gt; 5 &lt; 10 or 'a' == 'A'
True
&gt;&gt;&gt; 5 &gt; 10 or 'a' == 'a'
True
&gt;&gt;&gt; 5 &gt; 10 or 'a' == 'A'
False
</pre>

#### ■ The **not** Operator
You can use the operator **not** to reverse the results of a conditional statement. That just means that the code `not True` will give you the result `False`, and `not False` will give you the result `True`. Try running the code below in your terminal to see how this works!

<pre class="prettyprint">
&gt;&gt;&gt; not 5 &lt; 10
False
&gt;&gt;&gt; not 5 &gt; 10
True
</pre>

## Branches
Branching programs are set up using what we call **if-else** statements, which look like this:

```
if Conditional Statement:
	Code that runs when the condition is fulfilled
	.
	.
else:
	Code that runs when the condition is NOT fulfilled
	.
	.
	.
```

If all you want to do is make a section of code run only when a certain condition is fulfilled, you don't need to include an **else** statement or code for it. Let's try looking at a simple branching program to get a feel for how **if-else** statements work.

### Programming Passing and Failing Scores
Some standardized tests, like language proficiency tests or driver's license tests, judge whether you've passed or failed based on a predetermined passing score. Tests like these are usually graded by computers, since there are so many people who take them that it would take much too long to grade them by hand, while a computer can quickly and efficiently calculate the score and judge whether it's a pass or a fail. Let's try making a pass/fail grading program like this ourselves!

#### ■ Program Layout

The program finds an ungraded test, and checks whether the score makes the passing grade (60 points). If the score passes, the test is added to the **passed** list, if not it gets added to the **failed** list. When it finishes looking through all the tests, it shows you the final **passed** and **failed** lists.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-2/2-2_3_E.png"/>

#### ■ Writing the Program
Let's start by putting together a list of five tests the program needs to evaluate.

|Test Number|Score|
|:---|:---|
|No.01|72|
|No.02|56|
|No.03|86|
|No.04|45|
|No.05|63|

We can store this information as a dictionary, where the test numbers are the keys and the scores are the items matched to the keys.

```python=1
data = {'No.01':72,'No.02':56,'No.03':86,'No.04':45,'No.05':63}
```

Then we'll make two empty lists to sort tests into: `passed` and `failed`.

###### New Code (Lines 2-3)
```python=1
data = {'No.01':72,'No.02':56,'No.03':86,'No.04':45,'No.05':63}

passed = []
failed = []
```

Next we need to make a looping program that judges whether each test score is passing or failing. A **for-in** statement will let us retrieve each item from a dictionary one-by-one, the same way it works for a list. Just remember that if you use **for-in** with a dictionary, it retrieves the dictionary's keys, not the stored data values.

```
for Key in Dictionary:
	Code you want to repeat
	.
	.
```

Now we want to retrieve the test numbers that act as the dictionary's keys and store them in a variable (which we'll call `number`). Then we can use the code `data[number]` to retrieve the values tied to those keys (the test scores)! After that, we can use a conditional statement to judge whether the test scores are 60 or above, and an **if-else** statement to make different processes happen for scores that meet the condition and those that don't. We'll use the method `append()` to add test numbers that correspond to passing scores to the list `passed`, and those that correspond to failing scores to the list `failed`.

###### New Code (Lines 6-10)
```python=1
data = {'No.01':72,'No.02':56,'No.03':86,'No.04':45,'No.05':63}

passed = []
failed = []

for number in data:
    if data[number] >= 60:
        passed.append(number)
    else:
        failed.append(number)
```

Finally, let's use the `print()` function to display the contents of both lists. Set multiple parameters in the `print()` function by separating them with `,` (commas), and you'll get that data displayed in a single block.

##### Example Code 3-1-1
###### New Code (Lines 12-13)
```python=1
data = {'No.01':72,'No.02':56,'No.03':86,'No.04':45,'No.05':63}

passed = []
failed = []

for number in data:
    if data[number] >= 60:
        passed.append(number)
    else:
        failed.append(number)

print('passed:', passed)
print('failed:', failed)
```

Now click the **Run** button in the top menu bar to try running this program and see how it works.

<pre class="prettyprint">
passed: ['No.03', 'No.01', 'No.05']
failed: ['No.02', 'No.04']
</pre>

#### ■ Branching Based on Multiple Conditions
**if** statements aren't limited to branches that run based on one condition being either fulfilled or not. They can also split into three or more branches by judging whether any, all, or no conditions from a group of conditions are fulfilled.

In the program we just made, 60 points or more was considered a passing score and anything else was failing. Instead, let's say this is an admissions test and students with scores that don't meet the passing grade of 60 but are still above 55 can go on a waitlist. We'll put their tests on a new list called `waitlist`.  One way we can do this is to add another **if-else** statement inside either section of our first **if-else** statement. Try making the changes shown below to Example Code 3-1-1, then run the code to see how it works!

##### Example Code 3-2-1
###### New and Changed Code (Line 5, Lines 11-14, Line 18)
```python=1
data = {'No.01':72,'No.02':56,'No.03':86,'No.04':45,'No.05':63}

passed = []
failed = []
waitlist = []

for number in data:
    if data[number] >= 60:
        passed.append(number)
    else:
        if data[number] >= 55:
            waitlist.append(number)
        else:
            failed.append(number)
			
print('passed:',passed)
print('failed:',failed)
print('waitlist:',waitlist)
```

##### Program Results
<pre class="prettyprint">
passed: ['No.03', 'No.01', 'No.05']
failed: ['No.04']
waitlist: ['No.02']
</pre>

The first line of this program, `if data[number] >= 60:`, checks whether a test score is 60 or more. If it isn't, the line `if data[number] >= 55` below it checks if the score is 55 or more.

Another way to program a process like this is to use an **elif** statement. You can insert **elif** statements (as many as you want!) with conditions between the **if** and **else** lines in an **if-else** statement. If the first **if** statement's condition isn't fulfilled, the program will go to check the following **elif** statement,  then the next if that one isn't fulfilled, and so on.

```
if Condition 1:
	Code that runs when Condition 1 is fulfilled
	.
	.
elif Condition 2:
	Code that runs when Condition 1 is not fulfilled, but Condition 2 is
	.
	.
elif Condition 3:
	Code that runs when Conditions 1 and 2 are not fulfilled, but Condition 3 is
	.
	.
else:
	Code that runs when NONE of the conditions are fulfilled
	.
	.
```

If we rewrite Example Code 3-2-1 using an **elif** statement, it'll look like this:

##### Example Code 3-2-2
###### Changed Code (Lines 11-14)
```python=1
data = {'No.01':72,'No.02':56,'No.03':86,'No.04':45,'No.05':63}

passed = []
failed = []
waitlist = []

for number in data:
    if data[number] >= 60:
        passed.append(number)
    elif data[number] >= 55:
    	waitlist.append(number)
    else:
        failed.append(number)
			
print('passed:',passed)
print('failed:',failed)
print('waitlist:',waitlist)
```

As you can see now, there's more than one way to write a program that accomplishes a specific task like this! Still, it's easy to lose track of what you're doing as programs get more complicated, so while we're getting used to this new structure let's try drawing diagrams of how programs are supposed to work before we start writing code for them.

## Challenge: Programming with Sensors
Sensors are used to gather information about a machine's surroundings, much like humans use their sensory organs to see, hear, feel, taste, and smell things. Most robots and even modern household appliances use sensors of some kind to adjust their operations automatically based on their surroundings.

Air conditioners, for example, use temperature sensors so they can cool a room's temperature just enough to make the desired temperature the user sets.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-2/2-2_4_E.png"/>

Newer air conditioners might also include a sensor that detects whether a person is in the room so the air conditioner can stop running when the room is empty, or direct cool air toward people in the room.

To complete this lesson, use what you've learned so far to make a program that branches based on information collected from the Core Unit's sensors!

### The Core Unit's Sensors
The Core Unit has four built-in sensors: the Light Sensor, the Temperature Sensor, the buttons, and the Motion Sensor. In this lesson, we'll just be using the Light Sensor.

|Sensor Type|What It Does|
|:---|:---|
|Light Sensor|Detects surrounding light|
|Temperature Sensor|Detects the temperature of the surroundings|
|Buttons|Detect whether they've been pressed|
|Motion Sensor|Detects acceleration, angular velocity, and magnetism|

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-2/2-2_5_E.png"/>



### Using the Light Sensor

Computers can't directly grasp information from physical qualities like temperature and the brightness of light, which we call **analog data**. When sensors detect analog data, it needs to be converted into **digital data** so the computer can process it. Basically, digital data is the information the sensors collect but expressed as numeric data.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-2/2-2_6_E.png"/>

Let's try writing a program to see what the Light Sensor's information about surrounding light levels looks like once it's been turned into numbers.

#### ■ Writing the Program

First, copy the two lines of code below and paste them into your editor.

```python=1
from pystubit.board import lightsensor
import time
```

Line 1 calls the object `lightsensor`, which we need to use to get data from the Light Sensor. Line 2 calls the object `time`, which we need so we can measure time in our program.

Next, let's write a looping program to check the Light Sensor's data every 500 milliseconds. Use the code `while True`: you learned about at the start of the lesson to set up an endless loop!

###### New Code (Line 4)
```python=1
from pystubit.board import lightsensor
import time

while True:
```

To get information about light levels from the Light Sensor, use the `lightsensor` object's `get_value()` method. This method will give you an integer that indicates a level of brightness as its return value. Let's store this return value in a variable called `value`, then use the `print()` function to display it in the terminal. We also want the program to wait 500 milliseconds before checking the light level again, so let's add a line of code using the `time` object's `sleep_ms()` method at the end of the program.

##### Example Code 4-2-1
###### New Code (Lines 5-7)
```python=1
from pystubit.board import lightsensor
import time

while True:
	value = lightsensor.get_value()
	print(value)
	time.sleep_ms(500)
```
###### ★ Be careful! If the time set on line 7, time.sleep_ms(500), isn't long enough, the program may find data too fast for the print() function to display it, causing the Mu editor to crash.

#### ■ Running the Program
Try running Example Code 4-2-1 while covering the Light Sensor with your hand and with the Light Sensor uncovered to see how that changes the numbers your program displays.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-2/2-2_7_E.jpg"/>

##### Example Values with the Light Sensor Uncovered:

<pre class="prettyprint">
4095
4095
4095
4095
4095
</pre>

##### Example Values with the Light Sensor Covered:

<pre class="prettyprint">
871
722
693
683
684
</pre>

The light levels the sensor detects are being displayed as integers from **0** to **4095**! Smaller numbers mean the sensor is detecting lower levels of light, and bigger numbers mean higher levels. So the higher the number, the brighter the sensor's surroundings are!

### Branching Programs Based on Light
Now you can use the Light Sensor's data about surrounding light to make a program that does different things in the dark than it does in the light! Let's write a program that turns on the Core Unit's LED display for a short time when the Light Sensor's values get low enough, like a night light that turns on automatically when it gets dark outside!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-2/2-2_8_E.gif"/>

#### ■ Program Outline
This program works by following the steps laid out in the diagram below. As you can see from the diagram, the loop that keeps checking the surrounding light indefinitely lets the program react as soon as the light changes, making the LED display turn on automatically.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-2/2-2_9_E.png"/>

#### ■  Finding a Threshold
Before you can start writing this program, there's one more thing you'll need to figure out: how does the computer decide whether the surroundings are dark?

The computer treats light as numeric data, so to tell if any level of light is dark or not, it needs to have a number it knows means "dark" to compare things to!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-2/2-2_10_E.png"/>

A number that you use as a boundary line between two opposing states (like light and dark or hot and cold) is called a **threshold** or **threshold value**. In this particular program, there's no way to determine one definitely correct threshold value. Just look at the values you've gotten from your Light Sensor so far and pick one that corresponds to a level of light you think should make the LED display turn on.

#### ■ Writing the Program

Once you've picked a threshold, you can use that value to write your program. Let's take the program we made to check the Light Sensor's values earlier and build on it.

First we'll need to add some code that lets the program give commands to the LED display. Copy the new lines from the code below (lines 2, 3, and 6) and paste them into the same places in the program you have in your editor.

###### New Code (Lines 2-3, Line 6)
```python=1
from pystubit.board import lightsensor
from pystubit.board import display
from pystubit.board import Image
import time

image = Image('11111:11111:11111:11111:11111:')

while True:
	value = lightsensor.get_value()
	print(value)
	time.sleep_ms(500)
```

Line 6 defines an image object called `image`, which stores a pattern telling the display which LEDs to light up. This particular pattern lights up all the LEDs in the display.

Next, we need to add the code for the program's branches inside the **while** loop. Copy the code below into your editor (except for the condition in line 10's **if** statement).

##### Example Code 4-3-1
###### New and Changed Code (Lines 10-13)
```python=1
from pystubit.board import lightsensor
from pystubit.board import display
from pystubit.board import Image
import time

image = Image('11111:11111:11111:11111:11111:')

while True:
	value = lightsensor.get_value()
	if value < 1000:	#Write your threshold value here instead of 1000
		display.show(image)
		time.sleep_ms(1000)
		display.clear()
```

Line 10 `if value < 1000:` makes the program branch based on the brightness threshold you picked earlier.

In line 11, `display.show(image)` uses the `show()` method from the LED display object `display` to make the LEDs light up according to the pattern stored in the image object `image`. The `show()` method controls the LED display, just like the `scroll()` method you've used before. You can use `show()` to make strings appear in the display one letter at a time, or to display a pattern stored in an image object like we're doing this time.

Finally on line 13, we'll use the `clear()` method from the object `display` to make all the LEDs in the display turn off again.

#### ■ Running the Program

Run Example Code 4-3-1 and see if the LED display lights up red when you cover the Light Sensor with your hand!

## Conclusion
### A Quick Review
In this lesson we learned...

* How to write conditional statements using relational operators
* How to combine conditions using the logical operators **and** and **or**
* That the results of conditional statements are boolean values (**True** or **False**)
* How to use **if-else** statements to make programs branch based on the results of conditional statements
* How to add more branches to an **if-else** statement using **elif** statements

Since you learned about branches and loops in Topic 2-1, you can set up much more intricate programs than just simple sequences of commands!

On top of that, you've also tried out using Core Unit's Light Sensor. Modern appliances use all kinds of sensors, including the ones we mentioned in this lesson and many more we didn't! Don't worry, we'll talk about how to use all of the Core Unit's other sensors in later lessons.

### Coming Up Next Time!
In the next lesson, we'll learn about classes, which are used to make objects!