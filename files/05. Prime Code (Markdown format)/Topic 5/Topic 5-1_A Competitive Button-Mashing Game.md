---
tags: Python_English, Topic 5
---

Python Robotics Course Lesson 17

Topic 5-1
A Competitive Button-Mashing Game
===

Learn how to manage data in variables and functions!

## In This Lesson...

Learn about the different ways your programs treat different types of data when they're stored in variables, and what it means that the variables and functions you make in your programs have different **scopes**.

Then we'll finish the lesson by programming a competitive button-mashing game!

:::info
Lesson Introduction

■ Lesson 17 Contents
1. Using Data in Variables
2. The Scope of Functions and Variables
3. Making a Button-Mashing Game
4. Challenge: Multi-Round Button-Masher


In this lesson you'll learn about scope, an important aspect of Python that comes into play whenever you make or use your own variables and functions in a program.

We'll start off by introducing some new Python syntax elements.

In previous lessons we've introduced many different data types, including numbers, strings, tuples, lists, and more, and explained how they can store information. If you look at the process of copying or modifying variable values closely, you'll see that these actions have different effects depending on what type of data you've stored in the variable. We'll take a look at several example programs so you can see the differences for yourself!

Next you'll learn about scope, a trait that decides where you can use variable and function names in your program. Understanding how scope works will let you use functions and variables more effectively in your programs!

We'll finish the lesson by programming a competitive button-mashing game and using the programming tricks you've learned to upgrade it into a multi-round game!

Now let's start the lesson!

:::

## New Python Syntax

Let's start off by introducing some new Python code you'll be using for the first time in this lesson.

### Using Data in Variables

Python has a lot of different types of data, including numbers, strings, tuples, lists, and more. In the programs we've written so far, we've just stored data in variables and used them without paying too much attention to the differences between the various data types. These data types can actually be divided into two broader types which act differently when you store them in variables: **values** and **references**. So, what are the differences between values and references? Let's talk about them!

#### ■ Value Data

Numeric, string, boolean (True/False), and tuple data are all **values**. If you make a copy of a variable with a value in it, it creates **a new object containing the same information**. This is a little hard to explain without seeing how it works for yourself, so let's take a look at the example code below.

##### Example Code 2-1-1

```python==1
a = 1        
b = a        
print(a, b)  

a += 1       
print(a, b)
```

Connect to your Core Unit and try running the example code. Your results should look like this:

##### Program Results

<pre class="prettyprint">
1 1
2 1
</pre>

`1 1` is the result of the `print()` function on line 3, and `2 1` is the result of the `print()` function on line 6. Line 2 copies the value of variable `a` to variable `b`, so the `print()` function on line 3 gives us the same number for both variables. Even though the numeric value of variables `a` and `b` is the same here, the program considers them two separate objects. This means that when we add `1` to the value of variable `a` on line 5, this changes the value of a to `2`, but does **not** change the value of variable `b`.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-1/5-1_1_E.png"/>

This example shows how when you copy a variable with value data stored in it, the program creates a replica of the data which is an entirely separate object from the data you copied.

You can get the same results using string data, since strings are also treated as value data.

##### Example Code 2-1-2
```python==1
a = "A"
b = a
print(a, b)

a += "B"
print(a, b)
```

##### Program Results

<pre class="prettyprint">
A A
AB A
</pre>

#### ■ Reference Data

While copying a variable with value data in it makes a new object with the value, copying a variable with reference data stored in it makes the program treat the original variable and the variable you copied its data to as **references to the same object**. Lists and dictionaries are both treated as reference data. This concept is a little hard to explain in words, so let's look at the next example code.

##### Example Code 2-1-3
```python==1
a = [0, 1, 2]
b = a
print(a, b)

a.append(3)
print(a, b)
```

##### Program Results

<pre class="prettyprint">
[0, 1, 2] [0, 1, 2]
[0, 1, 2, 3] [0, 1, 2, 3]
</pre>

Unlike in Example Code 2-1-1 and 2-1-2, adding an element to variable `a` on line 5 changes variable `b` in the same way! This is because the code on line 2 makes variable `a` and variable `b` refer to the same list object.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-1/5-1_2_E.png"/>

What it's important for you to understand here is that variables that act as reference data **don't always have to refer to the same object** in every part of the same program. Let's talk about how that works now!

#### ■ Updating and Reassigning Data in Variables

There are two ways to change the data stored in a variable: **updating** and **reassigning**. When you update a variable, it still refers to the same object, but the data in that object will be changed. If you reassign a variable instead of updating it, it no longer references the same object. Instead it will reference a different, newly created object. Once again this concept is easier to understand by looking at actual programs, so let's try two different ways of adding an item to a list to see the difference between updating and reassigning.

* Adding Items with the `append()` Method

This program works the same way as Example Code 2-1-3. Using the `append()` method updates the data stored in the variable, but doesn't change what object the variable refers to, so variable `a` and variable `b` will have the exact same contents.

```python==1
a = [0, 1, 2]
b = a
a.append(3)
print(a, b)
```

##### Program Results

<pre class="prettyprint">
[0, 1, 2, 3][0, 1, 2, 3]
</pre>

* Adding Items with the + Operator

Now let's try adding an item using the + operator instead. Now the calculation `a + [3]` on line 3 replaces the list that variable `a` referred to with a new list object that has the same data as the original list but with the item `[3]` added. Variable `a` has now been **reassigned** so it refers to this new list!

```python==1
a = [0, 1, 2]
b = a
a = a + [3]
print(a, b)
```

##### Program Results

<pre class="prettyprint">
[0, 1, 2, 3][0, 1, 2]
</pre>

The flow charts below lay out how these two different processes work!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-1/5-1_3_E.png"/>


This means that there are times when it might look like the data in a variable has been updated, but actually that variable has been reassigned to a newly created object instead! This is very important to remember if you're writing a program with multiple variables that reference the same object!

### Scope: Where a Name is Valid

Accidentally creating multiple variables or functions with the same name can cause all kinds of problems that stop your code from running or give you the wrong results. That means you need to avoid reusing names in your code as much as you can!

The example code below makes two functions, one that adds two numbers and one that multiplies two numbers, and names both of them `calculate`. What happens then if you try to call the function that adds two numbers to find the answer to **3+4** on line 7? Run the example code to find out!



##### Example Code 2-3-1

```python==1
def calculate(x, y):
    return x+y

def calculate(x, y):
    return x*y

answer = calculate(3, 4)
print(answer)
```

When you run this code, the function that gets called is the one that multiplies two numbers!

##### Program Results

<pre class="prettyprint">
12
</pre>

These results happen because the contents of the first `calculate` function were overwritten by the contents of the second one. The mistake in the example program is obvious enough that you probably wouldn't actually write something like that! However, sometimes when programmers are working on very large projects, using pieces of code someone else wrote in a new program, or working on one project as a group, they will accidentally reuse names!

Most programming languages, including Python, address this problem by including special tricks you can use that allow you to use multiple variables and functions with the same name in the same program. They do this by dividing programs into several different parts. If a name gets used in one part of a program, that same name can be reused as long as it's in a different part of the program. These parts are called **scope blocks**, and the area where a particular name belongs to a particular object is that name's **scope**.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-1/5-1_4_E.png"/>

#### ■ The Basic Scopes

Python has two basic kinds of scope.

* **Global scope**: The largest possible scope, this covers the entirety of a program (one program file).
* **Local scope**: This scope covers only a single function.

As shown below, local scopes are nested inside global scope, and can be nested inside each other too if you define a function inside another function.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-1/5-1_5_E.png"/>

Now let's use some example code to take a closer look at these two kinds of scope!

#### ■ Global Scope

Variables and functions defined in the outermost part of a program have **global scope**. In the example code below, the `x`, `y`, and `z` variables are defined with a global scope. Variables with global scope like these are called **global variables**.

##### Example Code 2-3-2

```python==1
x = 0

if True:
    y = 1

for z in range(5):
    pass

print(x)
print(y)
print(z)
```

##### Program Results

<pre class="prettyprint">
0
1
4
</pre>

In some programming languages, the code inside of **if** statements and **for** statements belongs to separate scopes from the rest of the code, but not in Python! The **print** function is in the same scope as the variables here, so it can find and correctly display their values.

#### ■ Local Scope

When you create a function, its contents will belong to a separate scope from the global scope. This is called **local scope**. In the example code below, the `x` variable defined in the global scope and the `x` variable defined inside of the function (i.e. with a local scope) work as two separate variables!

##### Example Code 2-3-3
```python==1
x = 0  # Defines the global variable

def print_x():
    x = 1  # Defines a local variable for the function print_x()
    print(x)

print(x)
print_x()
```

##### Program Results

<pre class="prettyprint">
0
1
</pre>

Variables with local scope like these are called **local variables**. The code `print(x)` on line 7 displays the value of global variable `x (0)`, while `print_x()` on line 8 displays the value of the local variable `x (1)`. As long as they have different scopes like this, you can make variables that share the same name without any problems!

#### ■  Parent and Child Scopes

As we talked about earlier, scopes can be nested inside each other, by defining a function within the global scope, then defining another function inside that function, and so on. In setups like this, the outer scope is also called the **parent scope** of the scopes inside of it.

In Python if you use a variable or function that isn't defined inside the scope you're working in, the program will search for it in that scope's parent scope, then the parent scope of that second scope, and so on.

##### Example Code 2-3-4
```python==1
def scope_parent():
    x = 0

    def scope_child():
        print(x)  # The x variable isn't defined in this scope, so the x variable from the parent scope is used

    scope_child()


scope_parent()
```

##### Program Results

<pre class="prettyprint">
0
</pre>

You can use variables and functions defined in your current scope's parent scope, but the reverse (using a variable/function defined in a child scope in the parent scope) isn't possible!  This means if you try to run code like the following, you'll get an error.

##### Example Code 2-3-5

```python==1
def scope_parent():
    x = 0

    def scope_child():
        print(x)


scope_child()
```

##### Program Results

<pre class="prettyprint">
Traceback (most recent call last):
  File "&ltstdin&gt", line 16, in &ltmodule&gt
NameError: name 'scope_child' isn't defined
</pre>

The function `scope_child()` is defined inside the function `scope_parent()`, which means if you go up to the global scope this function isn't "visible" and calling it will just cause an error!

#### ■ Changing Variables Across Scopes

By default, you can find the values of variables defined in parent scopes from their child scopes, but you can't change those values. However, you can change these variables by **declaring** the variables either **global** (for global variables) or **nonlocal** (for other variables) in your code!

##### Example Code 2-3-6

```python==1
x = 0

def change_x():
    global x  # This code allows you to modify the global variable x
    x = 1     # Changes the global variable x

def print_y():
    y = 0

    def change_y():
        nonlocal y  # This code allows you to modify the variable y from the parent scope
        y = 1       # Changes the parent scope's y variable

    change_y()
    print(y)  # The change_y function has changed the value of the y variable, so this prints 1

change_x()
print(x)  # The `change_x` function has changed the value of the global scope's `x` variable, so this prints 1
print_y()
```

##### Program Results

<pre class="prettyprint">
1
1
</pre>

## Making a Button-Mashing Game
In this lesson, we'll be making a competitive button-mashing game! This is a game for two players, where each player needs to hit either the A or B button as quickly as they can. The first one to press their button 30 times wins!

##### Completed Example Video
:::info
<iframe src="https://player.vimeo.com/video/482872761" width="640" height="360" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>
:::

### Outline of the Game Program

Your game needs to have the following features to work.

##### Your Game Needs to...

1. Record how many times button A is pressed
2. Record how many times button B is pressed and compare the two to determine the winner
3. Count down to signal the start of the game
4. Start the game when the A and B buttons are pressed simultaneously

Let's put all these features together to make a complete program as shown in the flowchart below. We'll make a number of these features as their own functions to make the program code easier to follow, too!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-1/5-1_6_E.png"/>

Now let's go and program all these features, one at a time!

### Recording Button Presses

We'll start by making the game record how many times button A has been pressed, but before we start programming, let's review what methods we have for checking button presses.

#### ■ Methods That Detect Button Presses

The **StuduinoBitButton** class has two methods you can use to find out when the Core Unit's buttons have been pressed:

* **The `is_pressed()` Method**
This method returns the value `True` if the button is being pressed and the value `False` if it isn't.
* ** The `was_pressed()` Method**
This method returns the value `True` if the button has been pressed in the past, and the value `False` if it hasn't.

The key to understanding the difference between these two methods is the **"in the past"** part.

As long as a button is being pressed, the `is_pressed()` method will keep giving you the value `True`. The `was_pressed()` method, on the other hand, only gives you the value `True` once (when you first press the button), and then becomes `False` as long as you keep holding the button down. If you let go of the button and then press it again, the `was_pressed()` method will give you the value `True` one more time!

The program below checks the number of button presses with both of these methods every 500 milliseconds. Try running it to see the difference for yourself!

##### Example Code 3-2-1
```python==1
from pystubit.board import button_a
import time

while True:
    _is_pressed = button_a.is_pressed()
    _was_pressed = button_a.was_pressed()
    print('is_pressed:', str(_is_pressed))
    print('was_pressed:', str(_was_pressed))
    time.sleep_ms(500)
```

Run the program above, then press button A and hold it down. As long as you keep the button held down, the `is_pressed()` method's value will be `True`, but the `was_pressed()` method will only give you the value `True` once, then switch to `False`.

##### Program Results
<pre class="prettyprint">
is_pressed:True
was_pressed:True
is_pressed:True
was_pressed:False
is_pressed:True
was_pressed:False
is_pressed:True
was_pressed:False
</pre>

In this game we don't want holding down a button to count as multiple button presses, so we need to use the method that only counts each button press once, which is `was_pressed`!

#### ■  Counting Repeated Button Presses

Now let's delete Example Code 3-2-1 and start writing a program that can count how many times button A has been pressed.

First import the object `button_a` so you can program button A, then create a variable to record the number of times button A has been pressed, called `count_a`. Set `count_a`'s starting value to 0!

```python==1
from pystubit.board import button_a

count_a = 0
```

Next, use a **while** statement to set up an endless loop, and make a condition inside that loop using an **if** statement and the `was_pressed` method to check whether button A has been pressed. If button A has been pressed, the program should add 1 to the value of the `count_a` variable and then display its results with the `print()` function.

##### Example Code 3-2-2
###### New Code (Lines 5-8)
```python==1
from pystubit.board import button_a

count_a = 0

while True:
    if button_a.was_pressed():
        count_a += 1
        print(count_a)
```

Now try running this program and see if it counts up every time you press button A like it's meant to!

### Recording B Button Presses and Finding the Winner

The next feature we'll add counts the number of presses on button B as well as button A and compares the two to judge the winner of the game. First use the code that counts the presses for button A to make another one for button B and add it to Example Code 3-2-2.

###### New and Changed Code (Line 1, Line 4, Lines 9-10)
```python==1
from pystubit.board import button_a, button_b

count_a = 0
count_b = 0

while True:
    if button_a.was_pressed():
        count_a += 1
    if button_b.was_pressed():
        count_b += 1
```
###### ★ Delete the `print()` function that was in Example Code 3-2-2!

Now you need to add a process that judges the winner of the game. To do this you'll first have to make the loop we've set up stop when one of the buttons has been pressed the right number of times (this is the condition marked by ① in the flowchart). To do this, change the `while True:` statement on line 6 to `while button_ a < 30 and button_b < 30:` so the loop only keeps going and counting presses until one of the buttons has been pressed 30 times. Now if the value of either the `button_a` or `button_b` variable reaches 30, your program will stop running this **while** loop!

After it's finished the **while** loop, the program can compare the values of the `count_a` and `count_b` variables and record which side has won the game in a variable called `winner` (this is the branching condition marked by ② in the flowchart). Sometimes the game might result in a draw, in which case the program will record `'Draw'` here instead.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-1/5-1_7_E.png"/>



###### New and Changed Code (Line 6, Lines 12-17)
```python==1
from pystubit.board import button_a, button_b

count_a = 0
count_b = 0

while count_a < 30 and count_b < 30:
    if button_a.was_pressed():
        count_a += 1
    if button_b.was_pressed():
        count_b += 1     

if count_a > count_b:
    winner = 'A'
elif count_a == count_b:
    winner = 'Draw'
else:
    winner = 'B'
```

Finally, let's make the program play a sound to let the players know when the game is over, and show the results (i.e. who won) on the LED display.

###### New and Changed Code (Line 1, Lines 19-20)
```python==1
from pystubit.board import button_a, button_b, display, buzzer

count_a = 0
count_b = 0

while count_a < 30 and count_b < 30:
    if button_a.was_pressed():
        count_a += 1
    if button_b.was_pressed():
        count_b += 1     

if count_a > count_b:
    winner = 'A'
elif count_a == count_b:
    winner = 'Draw'
else:
    winner = 'B'

buzzer.on('C4', duration=1000)  # Make the Buzzer play however long you want!
display.scroll(winner)
```

Now put all your code so far into a function called `start_game()` and make the program call that function to run it!

##### Example Code 3-3-1
###### New and Changed Code (Lines 3-21, Line 23)
###### ★ Add indents to the start of every line to place the code inside of the function `start_game()`.

```python==1
from pystubit.board import button_a, button_b, display, buzzer

def start_game():
    count_a = 0
    count_b = 0

    while count_a < 30 and count_b < 30:
        if button_a.was_pressed():
            count_a += 1
        if button_b.was_pressed():
            count_b += 1     

    if count_a > count_b:
        winner = 'A'
    elif count_a == count_b:
        winner = 'Draw'
    else:
        winner = 'B'

    buzzer.on('C4', duration=1000)
    display.scroll(winner)
    
start_game()  # Don't indent this line!
```

### Counting Down to the Start of the Game
Next let's make a countdown that signals the start of the game! We'll put this program inside of a function called `countdown()`.

The countdown will work by making numbers appear on the display and playing sound to go along with it.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-1/5-1_8_E.png"/>

Let's start by defining the `countdown()` function and programming the numbers 0-3 to appear on the LED display.

The code `range(3, 0, -1)` creates a sequence of numbers that counts "3, 2, 1." Put this range in a **for** statement to make a loop that goes through all the numbers, and put that through the `str()` function to turn the numbers into strings so the **StuduinoBitDisplay** class's `show()` method can make them appear on the display. We want the sounds to play at the same time the numbers appear on the LED display, so set the `show()` method's `delay` parameter to 0 seconds. The number 0 should be displayed for a different length of time than the other numbers, so write the code for that number separately from the **for** statement.

###### New Code (Lines 4-12)
###### ★ Lines 13 and on are omitted here.

```python==1
from pystubit.board import button_a, button_b, display, buzzer
import time

def countdown():
    for i in range(3, 0, -1):
        display.show(str(i), delay=0)
        time.sleep_ms(300)
        display.clear()
        time.sleep_ms(600)
    display.show(str(0), delay=0)
    time.sleep_ms(1000)
    display.clear()
.
.
.
```

Next let's add code to play sounds from the Buzzer. Play the note C4 with numbers 3-1 and G4 with the number 0 by adding code at the points shown below.

###### New Code (Line 7, Line 10, Line 13, Line 16)
###### ★ Lines 16 and on are omitted here.
```python==1
from pystubit.board import button_a, button_b, display, buzzer
import time

def countdown():
    for i in range(3, 0, -1):
        display.show(str(i), delay=0)
        buzzer.on('C4')
        time.sleep_ms(300)
        display.clear()
        buzzer.off()
        time.sleep_ms(600)
    display.show(str(0), delay=0)
    buzzer.on('G4')
    time.sleep_ms(1000)
    display.clear()
    buzzer.off()
.
.
.
```

Now all you need to do is make your program call the `countdown()` function before the `start_game()` function!

###### New Code (Line 38)
##### Example Code 3-4-1
```python==1
from pystubit.board import button_a, button_b, display, buzzer
import time

def countdown():
    for i in range(3, 0, -1):
        display.show(str(i), delay=0)
        buzzer.on('C4')
        time.sleep_ms(300)
        display.clear()
        buzzer.off()
        time.sleep_ms(600)
    display.show(str(0), delay=0)
    buzzer.on('G4')
    time.sleep_ms(1000)
    display.clear()
    buzzer.off()

def start_game():
    count_a = 0
    count_b = 0

    while count_a < 30 and count_b < 30:
        if button_a.was_pressed():
            count_a += 1
        if button_b.was_pressed():
            count_b += 1     

    if count_a > count_b:
        winner = 'A'
    elif count_a == count_b:
        winner = 'Draw'
    else:
        winner = 'B'

    buzzer.on('C4', duration=1000)
    display.scroll(winner)

countdown()
start_game()
```

### Pressing A and B to Start the Game
Now let's make it so the program only calls the `countdown()` and `start_game()` functions when buttons A and B are pressed at the same time. We can do this using the **StuduinoBitButton** class's `is_pressed()` method.

##### Example Code 3-5-1
###### New and Changed Code (Lines 38-41)
```python==38
while True:
    if button_a.is_pressed() and button_b.is_pressed():
        countdown()
        start_game()
```

Now we've added all the features we wanted in the game! Try running the program and see how it works!

## Challenge: Multi-Round Button-Masher

Example Code 3-5-1 can determine the winner for a single round button-mashing game, now see if you can modify it to judge which player can win three rounds first!

##### Hints
Create global variables to keep track of how many rounds each player has won (`winning_a` and `winning_b`, for example) and count the number of rounds inside the `start_game()` function. Since you'll need to change a global variable inside a function for this, remember to declare the global variable like we talked about before!

### Example Program

Modify Example Code 3-5-1 as shown below!

#### ■ Repeating the Game Until Someone Gets Three Wins

Make the values of variables `winning_a` and `winning_b` be set to 0 when you press the A and B buttons at the same time. These two variables are created as global variables. Make the `countdown()` and `start_game()` functions run inside a **while** loop that keeps repeating until one of the players' win counts reaches three. In the example program below, we've also made the game wait for 1000 milliseconds before starting a new round.

###### New and Changed Code (Lines 40-45)
```python==38
while True:
    if button_a.is_pressed() and button_b.is_pressed():
        winning_a = 0    # Change Player A's total win count
        winning_b = 0    # Change Player B's total win count
        while winning_a < 3 and winning_b < 3:
            countdown()
            start_game()
            time.sleep_ms(1000)    # Wait for one second between rounds
```

#### ■ Show the Winner

When the game is over, the program should compare both players' total win counts and judge who won. Then it scrolls the winning player's letter across the LED display.

###### New Code (Lines 46-49)
```python==38
while True:
    if button_a.is_pressed() and button_b.is_pressed():
        winning_a = 0    
        winning_b = 0    
        while winning_a < 3 and winning_b < 3:
            countdown()
            start_game()
            time.sleep_ms(1000) 
        if winning_a > winning_b:    # If Player A wins
            display.scroll("Winner is A.", delay=50)
        else:    # If Player B wins
            display.scroll("Winner is B.", delay=50)
```

#### ■ Counting Wins

When a player wins a round, the `start_game` function should add that win to their total win count. Since the variables that record the total win counts (`winning_a` and `winning_b`) are global variables, you need to declare them at the start of the function in order to make changes to them. That's the last modification the program needs!

###### New Code (Line 19, Line 31, Line 36)
```python==18
def start_game():
    global winning_a, winning_b    # Declare global
    count_a = 0
    count_b = 0
    
    while count_a < 30 and count_b < 30:
        if button_a.was_pressed():
            count_a += 1
        if button_b.was_pressed():
            count_b += 1

    if count_a > count_b:
        winner = 'A'
        winning_a += 1    # Add 1 to Player A's win count
    elif count_a == count_b:
        winner = 'Draw'
    else:
        winner = 'B'
        winning_b += 1    # Add 1 to Player B's win count

    buzzer.on('C4', duration=1000)
    display.scroll(winner)
```

## Conclusion
### A Quick Review
In this lesson we learned...

* That the data stored in variables is treated differently depending on whether it's a value or a reference
* About **updating** and **reassigning** (two ways to change data stored in a variable)
* About **scope** (the part of a program where a variable or function's name is valid)
* That scopes can be either **global** or **local**
* That declaring **global** or **nonlocal** variables lets you change the value of variables from different scopes

The concepts we talked about in this lesson are pretty difficult, but if you can master them they'll be very useful when you need to write larger programs!

### Coming Up Next Time!
In the next lesson, you'll learn how to make functions with flexible numbers of variables, and how to use **continue**, **break**, and **else** statements with your **for** and **while** statements!