---
tags: Python_English, Topic 8
---

Python Robotics Course Lesson 32

Topic 8-4
An Arm Robot Control System
===

Build an easy-to-use arm robot control system!

:::info

Lesson Introduction

■ Lesson 32 Contents
1. New Python Syntax: Sets
2. Building an Arm Robot Control System
3. Challenge: Additional Commands


In this lesson you’ll make a system that lets people with no programming knowledge control an arm robot just by writing simple commands in the terminal!

We’ll start out by learning about a new Python syntax element called “sets.” Sets can store many values at once, just like lists and dictionaries. Sets in programming are much like sets in mathematics, and you can perform special set operations like union, difference, and intersection with them. Have a look at some example programs to see how sets work for yourself!

In the latter half of the lesson you’ll build a system that lets people control an arm robot with simple commands instead of having to write their own program. Think about how to analyze commands typed into the terminal to turn them into commands that control your robot.

Finally you can take on the challenge of adding a new command using conditions!

Now let's start the lesson!

:::

## In This Lesson...
Commercially available robot arms are sold alongside software that lets the buyer give commands to the robot without knowing any programming themselves!

In this lesson you’ll make a system like this yourself so your arm robot can be controlled just by writing simple commands in the terminal!

## New Python Syntax
Before we start building our robot arm, let’s learn about a new kind of Python syntax called **sets**.
### What are Sets?
A set is a type of data that stores many values at once, just like lists, dictionaries, and tuples. To define a set (or **set-type** data), list the data you want to store in curly brackets, like this:

<pre>
my_set = {1, 2, 3}
</pre>

Now let’s take a look at how sets work.

#### ■ Set Data Structure
Unlike the index numbers in a list or a the keys in a dictionary, data stored in a set **has no set order**. Furthermore, every piece of data in a set must be unique, with no repeated values.

<pre>
my_set_OK = {1, 2, 3, 4, 5}  # No numbers are repeated, so this works!
my_set_NG = {1, 2, 2, 3, 3}  # This set has repeated numbers, so it doesn’t work!
</pre>

The data structure for lists and dictionaries is often compared to a series of boxes, with each piece of data in a separate box. By contrast, a set is more like a big bag with a bunch of data thrown into it.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-4/8-4_1_E.png"/>


#### ■ Methods for Sets

The following three methods can be used with sets.

|Method|What It Does|
|:---|:---|
|`add()`|Adds items to the set. If the same item is already in the set the addition will be ignored, but won’t cause an error. |
|`remove()`|Remove the specified item from the set. |
|`clear()`|Remove all the items in the set. |

Check out the example code below to see how these three methods are used.

##### Example Code 2-1-1
<pre class="prettyprint linenums:1 lang-py">
# Define your set
my_set = {1,2,3}
print("Set definition")
print(my_set)

# Use add() to add an item
my_set.add(4)
print("Add the item 4 with the add() method")
print(my_set)

# Use add() to add an item
my_set.add(3)
print("Add the item 3 with the add() method")
print(my_set)

# Use remove() to delete the specified item
my_set.remove(3)
print("Remove the item 3 with the remove() method")
print(my_set)

# Use clear() to delete all items in the set
my_set.clear()
print("Remove all items with the clear() method")
print(my_set)
</pre>

##### Program Results
<pre class="prettyprint">
Set definition
{3, 1, 2}
Add the item 4 with the add() method
{4, 1, 2, 3}
Add the item 3 with the add() method
{4, 1, 2, 3}
Remove the item 3 with the remove() method
{4, 1, 2}
Remove all items with the clear() method
set()
</pre>

Take special note of line 12, where we add the item 3 with the `add()` method. The number 3 is already included in the set, so running this code has no effect on the results we see. As you can see from this, adding a repeated item to a set just causes that code to be ignored.
### Set Operations
The concept of sets is borrowed from the world of mathematics, where a “set” is a collection of different objects. Sets in programming can be added and subtracted from each other just like in math, using three basic operations called **union**, **difference**, and **intersection**.

As an example, let’s take two collections of numbers and try using set operations on them.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-4/8-4_2_E.png"/>



#### ■ Union Operations

If you combine A and B to make a new collection containing all the items from both of them, that makes the **union** of A and B. If any items are found in both sets, the repeated items will be merged into one in the union.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-4/8-4_3_E.png"/>

Python has operators you can use to perform set operations available by default. The operator for the union operation is the vertical bar (`|`) symbol. 

##### Example Code 2-2-1
<pre class="prettyprint linenums:1 lang-py">
# Define Set A and Set B
A = {1, 2, 3, 5}
B = {3 ,4, 5, 6, 7}

# Find the union of Set A and Set B
AB = A | B
print(AB)
</pre>

##### Program Results
<pre class="prettyprint">
{1, 2, 3, 4, 5, 6, 7}
</pre>


#### ■ Difference Operations

If you take sets A and B and remove all the items in set A that are also in set B, what remains is the **set difference** of A and B.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-4/8-4_4_E.png"/>

The operator for the difference operation in Python is the `-` symbol.

##### Example Code 2-2-2
<pre class="prettyprint linenums:1 lang-py">
# Define Set A and Set B
A = {1, 2, 3, 5}
B = {3 ,4, 5, 6, 7}

# Find the difference of Set A and Set B
AB = A - B
print(AB)
</pre>

##### Program Results
<pre class="prettyprint">
{1, 2}
</pre>

#### ■ Intersection Operations

If you take sets A and B and find all the items that are in both of them, a new set of just those repeated items is the **intersection** of A and B.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-4/8-4_5_E.png"/>

The operator for the intersection operation in Python is the ampersand (`&`) symbol.

##### Example Code 2-2-3
<pre class="prettyprint linenums:1 lang-py">
# Define Set A and Set B
A = {1, 2, 3, 5}
B = {3 ,4, 5, 6, 7}

# Find the intersection of Set A and Set B
AB = A & B
print(AB)
</pre>

##### Program Results
<pre class="prettyprint">
{3, 5}
</pre>


#### ■ Exclusive Disjunction Operations

Along with union, difference, and intersection, there’s another commonly used set operation called an **exclusive disjunction**. This operation is a little hard to explain with words, but pretty simple if you look at a diagram. To find the exclusive disjunction of sets A and B, take a set of all the items in A and B and then remove all the items that were in both A and B.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_8-4/8-4_6_E.png"/>

The operator for the exclusive disjunction operation in Python is the caret (`^`) symbol.

##### Example Code 2-2-4
<pre class="prettyprint linenums:1 lang-py">
# Define Set A and Set B
A = {1, 2, 3, 5}
B = {3 ,4, 5, 6, 7}

# Find the exclusive disjunction of Set A and Set B
AB = A ^ B
print(AB)
</pre>

##### Program Results
<pre class="prettyprint">
{6, 1, 2, 7, 4}
</pre>
### Making Set-Type Data from List-Type Data
You can make a data set from a list you already have by using the **set() function**.

In the example code below, we’ll make a data set called `myset` from a list with repeated items in it called `mylist`.

#### Example Code 2-3-1
<pre class="prettyprint linenums:1 lang-py">
# Define the list
mylist = [1, 2, 2, 3, 3, 3, 4, 5, 6, 7]

# Use the set() function to make a set from a list
myset = set(mylist)
print(myset)
</pre>

#### Program Results
<pre>
{1, 2, 3, 4, 5, 6, 7}
</pre>

In this example, we made a list (`[1, 2, 2, 3, 3, 3, 4, 5, 6, 7]`) with repeated items in it. As you can see, the resulting set (`{1, 2, 3, 4, 5, 6, 7}`) doesn’t have any repeating items. When you change a list into a set like this, repeated items will automatically be removed.
### Why Use Sets?
Here are two important things you’ve learned about sets from the explanations so far:

1. Sets have no repeating items.
2. There are special operations you can do with sets.

Number 1 here means sets are very useful for storing information without repeats, like employee ID numbers and phone numbers. It also means sets won’t be useful for storing information with repeated items, like people’s names or birthdates. You can also use sets to help manage data by purging repeated items from lists when you don’t need them.

Number 2 lets us use sets to do things like pull items from categories of “animals with trait A,” “animals with trait B,” and “animals with trait C” to make a new category of “animals with traits A, B, and C.”

<pre>
animals = A & B & C
</pre>

A lot of the programs we’ve made using lists and dictionaries before could actually be made much simpler by using sets. Learning the strengths and weaknesses of different data types like this will let you write cleaner code!
## Giving Arm Robots Commands
Not everyone who needs to operate an arm robot understands programming! This is why arm robots use specialized, simplified robot programming languages instead of normal programming languages like Python. 

You can accomplish something like this yourself by using the `ArmRobot` class you made in the last lesson to build a system that lets people who don’t know Python control your arm robot just by typing simple commands into the terminal!

##### Arm Robot Commands

|Command|What the Robot Does|
|:---|:---|
|`pick`|Grab and lift an object|
|`place`|Set an object down|
|`move A`|Rotate the robot’s body and move to a specified location (`A`)|
|`transport A B`|Move an object from one specified location (`A`) to another specified location (`B`)|

In place of **A and **B** in these commands, let’s use numbers or colors that correspond to positions on the field we’ve been using since lesson 30. The actual commands you type in the terminal will look like this:

##### Giving Commands

<pre class="prettyprint">
--&gt; move 1  # Move to Position 1
--&gt; pick    # Pick up an object
--&gt; move 2  # Move to Position 2
--&gt; lift    # Place a held object
--&gt; transport green blue  # Move an object from the green circle to the blue circle
</pre>

Each command corresponds to a method from the `ArmRobot` class.

* **pick**
<pre>
ArmRobot.pick()
</pre>

* **place**
<pre>
ArmRobot.place()
</pre>

* **move A**
<pre>
ArmRobot.move_to(num) OR ArmRobot.move_to_color(color)
</pre>

* **transport A B**
<pre>
ArmRobot.move_to(num) OR ArmRobot.move_to_color(color)
ArmRobot.pick()
ArmRobot.move_to(num) OR ArmRobot.move_to_color(color)
ArmRobot.place()
</pre>

Now let’s start writing a program that will let us use these commands to control the arm robot!


#### ■ Turning the `ArmRobot` Class into a Module

First let’s take the `ArmRobot` class you made in the previous lesson and turn it into a module.

If you deleted your program from that lesson, copy and paste the code below into a new file. Save the file with the name `armrobot(.py)` and save a copy on your Core Unit too.

##### The ArmRobot Module

###### ★ Be sure to adjust the thresholds on lines 15-16 and the angle of the robot arm’s claw in its grabbing motion on line 28 to make sure they work.

<pre class="prettyprint linenums:1 lang-py">
class ArmRobot:
    from pyatcrobo2.parts import DCMotor, IRPhotoReflector, ColorSensor
    import myservo
    import time

    COLOR_POSITIONS = {
        "grey": 1,
        "red": 2,
        "green": 3,
        "undef": 4,
        "blue": 5,
        "yellow": 6,
    }
    
    threshold_high = 900  # Adjust this value to work with your IR Photoreflector!
    threshold_low = 650  # Adjust this value to work with your IR Photoreflector! 

    def __init__(self, *, pin_arm, pin_hand, pin_body, pin_irp, pin_cs):
        self.arm = self.myservo.MyServomotor(pin_arm)
        self.hand = self.myservo.MyServomotor(pin_hand)
        self.body = self.DCMotor(pin_body)
        self.irp = self.IRPhotoReflector(pin_irp)
        self.cs = self.ColorSensor(pin_cs)
        self.position = 1

    def pick(self):
        self.myservo.syncRotateServos(600, (self.arm, 170), (self.hand, 135))
        self.myservo.syncRotateServos(600, (self.hand, 60))  # Adjust this angle so your robot arm has a secure hold on the block
        self.myservo.syncRotateServos(600, (self.arm, 90))

    def place(self):
        self.myservo.syncRotateServos(600, (self.arm, 165))
        self.myservo.syncRotateServos(600, (self.hand, 135))
        self.myservo.syncRotateServos(600, (self.arm, 90), (self.hand, 90))

    def rotate(self, duration=-1, speed=50, direction="cw"):
        self.body.power(speed)
        if direction == "cw":
            self.body.cw()
        else:
            self.body.ccw()
        if duration != -1:
            self.time.sleep_ms(duration)
            self.body.brake()

    def stop(self):
        self.body.brake()

    def move_to(self, num):
        diff = num - self.position
        if abs(diff) > 3:
            diff = diff - 6 if diff > 0 else diff + 6
        direction = "cw" if diff > 0 else "ccw"
        self.rotate(direction=direction)
        for _ in range(abs(diff)):
            while not self.irp.get_value() > self.threshold_high:
                pass
            while not self.irp.get_value() < self.threshold_low:
                pass
        self.stop()
        self.position = num

    def recognize_color(self):
        color = self.cs.get_colorcode()
        if color == self.cs.COLOR_RED:
            color_name = "red"
        elif color == self.cs.COLOR_GREEN:
            color_name = "green"
        elif color == self.cs.COLOR_BLUE:
            color_name = "blue"
        elif color == self.cs.COLOR_YELLOW:
            color_name = "yellow"
        else:
            color_name = "undef"
        return color_name

    def move_to_color(self, color):
        num = self.COLOR_POSITIONS[color]
        self.move_to(num)
</pre>

#### ■ Importing the `ArmRobot` Module and Making an Instance

Start a new program file and begin your program by importing the `ArmRobot` class from the module you just created. Then you can make an instance from that class.

<pre class="prettyprint linenums:1 lang-py">
from armrobot import ArmRobot

armrobot = ArmRobot(pin_arm="P13", pin_hand="P14", 
                    pin_body="M1", pin_irp="P0", pin_cs="I2C")
</pre>

#### ■ Defining the Usable Commands

Next let’s define the usable commands in different categories as sets. After that we can make the program check to make sure the commands it receives don’t contain errors.

###### ★ Make sure to enter all the position numbers as strings! All the commands received from the terminal will be treated as strings, so the numbers need to match.

###### New Code (Lines 6-8)

<pre class="prettyprint linenums:1 lang-py">
from armrobot import ArmRobot

armrobot = ArmRobot(pin_arm="P13", pin_hand="P14", 
                    pin_body="M1", pin_irp="P0", pin_cs="I2C")

COMMANDS = {"pick", "place", "move", "transport"}  # Usable commands
POSITIONS = {"1", "2", "3", "4", "5", "6"}  # Position numbers
COLORS = {"grey", "red", "green", "blue", "yellow", "undef"}  # Position colors
</pre>

#### ■ Receiving Commands

Your control system will receive commands from the user with the function `input()`. Store the command the user enters in a standard variable called `data`, then use the `split()` method to make it into a list, divided with single spaces. For example, if the user enters `transport 1 2`, that will be turned into `["transport", "1", "2"]`.


###### New Code (Lines 10-13)

<pre class="prettyprint linenums:1 lang-py">
from armrobot import ArmRobot

armrobot = ArmRobot(pin_arm="P13", pin_hand="P14", 
                    pin_body="M1", pin_irp="P0", pin_cs="I2C")

COMMANDS = {"pick", "place", "move", "transport"}
POSITIONS = {"1", "2", "3", "4", "5", "6"}
COLORS = {"grey", "red", "green", "blue", "yellow", "undef"}

def main():  # Store other processes in this function
    while True:
        data = input("-->")  # Accept commands from the terminal
        command = data.split()  # Splits command strings with spaces and makes them into lists
</pre>

#### ■ Checking Commands for Mistakes

We can deal with typos in the commands by using a `try-except` statement. Specifically, let’s go through the steps below to check for mistakes within the `try` statement, and let it create an exception with a message attached if it finds any.

1. Does the input correspond to a command?
2. If the command is `move`, does it have only one argument?
3. If there’s no problem with 2, is the position number/color in the argument usable?
4. If the command is `transport`, does it have only two arguments?
5. If there’s no problem with 4, are the position numbers/colors in the arguments usable?

Check out the example code below to see how this checking process looks as a program. If the program finds an error, put the contents of that error in a variable called `msg` and put an instance of the `Exception` class into a `raise` statememt.

###### New Code (Line 12, Lines 15-31)

<pre class="prettyprint linenums:10 lang-py">
def main():
    while True:
        try:  # Find exceptions with a try statement
            data = input("-->")
            command = data.split()
            msg = None  # Variable for storing messages
            if not command[0] in COMMANDS:  # If the command entered isn’t usable
                msg = "'{}' is an invalid command.".format(command[0])
            elif command[0] == "move":  # If the command is “move”
                if not len(command) == 2:  # If there aren’t exactly two items including the argument, that’s an error
                    msg = "'move' only needs 1 argument."
                elif not (command[1] in POSITIONS or command[1] in COLORS):  # It’s also an error of the argument isn’t valid
                    msg = "'{}' is an invalid argument.".format(command[1])
            elif command[0] == "transport":
                if not len(command) == 3:  # If there aren’t exactly three items including the two arguments, that’s an error
                    msg = "'transport' only needs 2 arguments."
                elif not (command[1] in POSITIONS or command[1] in COLORS):  # If the first argument is invalid that’s an error too
                    msg = "'{}' is an invalid argument.".format(command[1])
                elif not (command[2] in POSITIONS or command[2] in COLORS):  # If the second argument is invalid that’s an error
                    msg = "'{}' is an invalid argument.".format(command[2])
            if msg:  # If you have a message stored, create an exception
                raise Exception(msg)  # Don’t create an exception if the message is “None”
</pre>




#### ■ Display the Contents of Detected Errors

The `except` statement catches the exception created with the `raise` statement. Then it can use the `print()` function to display the error message recorded in the exception. If there’s an error in the command it can’t be used to control the arm robot, so use a `continue` statement to move on to the next loop.

###### New Code (Lines 32-34)

<pre class="prettyprint linenums:10 lang-py">
def main():
    while True:
        try:
            data = input("-->")
            command = data.split()
            msg = None
            if not command[0] in COMMANDS:
                msg = "'{}' is an invalid command.".format(command[0])
            elif command[0] == "move":
                if not len(command) == 2:
                    msg = "'move' only needs 1 argument."
                elif not (command[1] in POSITIONS or command[1] in COLORS):
                    msg = "'{}' is an invalid argument.".format(command[1])
            elif command[0] == "transport":
                if not len(command) == 3:
                    msg = "'transport' only needs 2 arguments."
                elif not (command[1] in POSITIONS or command[1] in COLORS):
                    msg = "'{}' is an invalid argument.".format(command[1])
                elif not (command[2] in POSITIONS or command[2] in COLORS):
                    msg = "'{}' is an invalid argument.".format(command[2])
            if msg:
                raise Exception(msg)
        except Exception as e:  # Find exceptions
            print(e)  # Display the error message
            continue  # Move to the next loop
</pre>

#### ■ Making Commands Control the Arm

If the command entered doesn’t have any errors in it (so there’s no exception found), the program should make the robot follow that command! Keeping in mind that the `ArmRobot` class’s `move_to()` method takes integer (`int`) type data for its argument, we can write the program like this:

###### New Code (Lines 35-55)

<pre class="prettyprint linenums:32 lang-py">
        except Exception as e:
            print(e)
            continue
        else:  # If no exception found/no mistake in the command
            if command[0] == "pick":
                armrobot.pick()
            elif command[0] == "place":
                armrobot.place()
            elif command[0] == "move":
                if command[1] in POSITIONS:  # If the position is specified by number
                    armrobot.move_to(int(command[1])) # Turn the string into an integer!
                else:  # If the position is specified by color
                    armrobot.move_to_color(command[1])
            else:  # For "transport” commands
                if command[1] in POSITIONS:  # If the position is specified by number
                    armrobot.move_to(int(command[1]))
                else:  # If the position is specified by color
                    armrobot.move_to_color(command[1])
                armrobot.pick()
                if command[2] in POSITIONS: # If the position is specified by number
                    armrobot.move_to(int(command[2]))
                else:  # If the position is specified by color
                    armrobot.move_to_color(command[2])
                armrobot.place()
</pre>

#### ■ Test the Finished Program

Now your program is finished! Run the program and then run the `main()` function from the terminal so you can test out all four commands.

###### ★ You’ll need to use the arm robot field from the last lesson to test the `move` and `transport` commands.

###### ★ Place a block at Position 1 on the field and line up the arm robot with it to set up. 

##### Giving Commands

<pre class="prettyprint">
&gt;&gt;&gt; main()
--&gt;pick
--&gt;move 2
--&gt;place
--&gt;transport 2 3 
</pre>

The completed program should look like the example code below. If you’re program isn’t working, compare it to the example to check for mistakes!

##### Example Code 3-1-1

<pre class="prettyprint linenums:1 lang-py">
from armrobot import ArmRobot

armrobot = ArmRobot(pin_arm="P13", pin_hand="P14", 
                    pin_body="M1", pin_irp="P0", pin_cs="I2C")

COMMANDS = {"pick", "place", "move", "transport"}
POSITIONS = {"1", "2", "3", "4", "5", "6"}
COLORS = {"grey", "red", "green", "blue", "yellow", "undef"}

def main():
    while True:
        try:
            data = input("-->")
            command = data.split()
            msg = None
            if not command[0] in COMMANDS:
                msg = "'{}' is an invalid command.".format(command[0])
            elif command[0] == "move":
                if not len(command) == 2:
                    msg = "'move' only needs 1 argument."
                elif not (command[1] in POSITIONS or command[1] in COLORS):
                    msg = "'{}' is an invalid argument.".format(command[1])
            elif command[0] == "transport":
                if not len(command) == 3:
                    msg = "'transport' only needs 2 arguments."
                elif not (command[1] in POSITIONS or command[1] in COLORS):
                    msg = "'{}' is an invalid argument.".format(command[1])
                elif not (command[2] in POSITIONS or command[2] in COLORS):
                    msg = "'{}' is an invalid argument.".format(command[2])
            if msg:
                raise Exception(msg)
        except Exception as e:
            print(e)
            continue
        else:
            if command[0] == "pick":
                armrobot.pick()
            elif command[0] == "place":
                armrobot.place()
            elif command[0] == "move":
                if command[1] in POSITIONS:
                    armrobot.move_to(int(command[1]))
                else:
                    armrobot.move_to_color(command[1])
            else:
                if command[1] in POSITIONS:
                    armrobot.move_to(int(command[1]))
                else:
                    armrobot.move_to_color(command[1])
                armrobot.pick()
                if command[2] in POSITIONS:
                    armrobot.move_to(int(command[2]))
                else:
                    armrobot.move_to_color(command[2])
                armrobot.place()
</pre>

## Challenge: Additional Commands
For this lesson’s challenge, add a new command that uses the Color Sensor to detect the color of the block and makes the robot move differently depending on that color.

##### The New Command

|Command|What It Does|
|:---|:---|
|`recognize COLOR A`|Check the color of the block the robot is holding and move it to the specified position (`A`) if it matches the specified color (`COLOR`). If the color is different, set the block down again. |

This command can be used like this:

<pre class="prettyprint">
--&gt;recognize red 3  # If the block held is red, mode it to Position 3
</pre>

Now let’s edit Example Code 3-1-1 to make this new command usable!
### Example Program Changes
We’ll walk you through one way to add this command to the program with the steps below.

#### ■ Adding recognize to the Commands

Add `"recognize"` to the set `COMMANDS`.

###### Changed Code (Line 3)

<pre class="prettyprint linenums:1 lang-py">
from armrobot import ArmRobot

COMMANDS = {"pick", "place", "move", "transport", "recognize"}  # Add “recognize”
POSITIONS = {"1", "2", "3", "4", "5", "6"}
COLORS = {"grey", "red", "green", "blue", "yellow", "undef"}
</pre>

#### ■ Dealing with Exceptions

You’ll need to add some code to make the program raise an exception if there are mistakes in `recognize` (e.g. not enough arguments, invalid argument settings), just like the other commands. 

###### New Code (Lines 30-36)

<pre class="prettyprint linenums:10 lang-py">
def main():
    while True:
        try:
            data = input("-->")
            command = data.split()
            msg = None
            if not command[0] in COMMANDS:
                msg = "'{}' is an invalid command.".format(command[0])
            elif command[0] == "move":
                if not len(command) == 2:
                    msg = "'move' only needs 1 argument."
                if not (command[1] in POSITIONS or command[1] in COLORS):
                    msg = "'{}' is an invalid argument.".format(command[1])
            elif command[0] == "transport":
                if not len(command) == 3:
                    msg = "'transport' only needs 2 arguments."
                if not (command[1] in POSITIONS or command[1] in COLORS):
                    msg = "'{}' is an invalid argument.".format(command[1])
                if not (command[2] in POSITIONS or command[2] in COLORS):
                    msg = "'{}' is an invalid argument.".format(command[2])
            elif command[0] == "recognize":  # For “recognize” commands
                if not len(command) == 3:  # The command needs two arguments so the list should have three items in it
                    msg = "'recognize' only needs 2 arguments."
                elif not command[1] in COLORS:  # If the fist argument is only a color
                    msg = "'{}' is an invalid argument.".format(command[1])
                elif not (command[2] in POSITIONS or command[2] in COLORS):  # If the second argument is a position number or color
                    msg = "'{}' is an invalid argument.".format(command[2])
            if msg:
                raise Exception(msg)
</pre>

#### ■ Making the Command Control the Arm

Finally, add the code that makes the arm robot move according to the `recognize` command.

###### New and Changed Code (Line 52, Lines 63-71)

<pre class="prettyprint linenums:39 lang-py">
 except Exception as e:
            print(e)
            continue
        else:
            if command[0] == "pick":
                armrobot.pick()
            elif command[0] == "place":
                armrobot.place()
            elif command[0] == "move":
                if command[1] in POSITIONS:
                    armrobot.move_to(int(command[1]))
                else:
                    armrobot.move_to_color(command[1])
            elif command[0] == "transport":  # Change the else here into an elif
                if command[1] in POSITIONS:
                    armrobot.move_to(int(command[1]))
                else:
                    armrobot.move_to_color(command[1])
                armrobot.pick()
                if command[2] in POSITIONS:
                    armrobot.move_to(int(command[2]))
                else:
                    armrobot.move_to_color(command[2])
                armrobot.place()
            else:  # For "recognize” commands
                armrobot.pick()  # Pick up a block and check its color
                color = armrobot.recognize_color()
                if color == command[1]:  # Go to the second argument if the color detected matches the color specified in the first argument
                    if command[2] in POSITIONS:
                        armrobot.move_to(int(command[2]))
                    else:
                        armrobot.move_to_color(command[2])
                armrobot.place()  # Put the held block down
</pre>


#### ■ Testing the Program

The completed program should look like the example code below. Run the program to see how it works!

##### Example Code 4-1-1

<pre class="prettyprint linenums:1 lang-py">
from armrobot import ArmRobot

COMMANDS = {"pick", "place", "move", "transport", "recognize"} # Add “recognize”
POSITIONS = {"1", "2", "3", "4", "5", "6"}
COLORS = {"grey", "red", "green", "blue", "yellow", "undef"}

armrobot = ArmRobot(pin_arm="P13", pin_hand="P14", 
                    pin_body="M1", pin_irp="P0", pin_cs="I2C")

def main():
    while True:
        try:
            data = input("-->")
            command = data.split()
            msg = None
            if not command[0] in COMMANDS:
                msg = "'{}' is an invalid command.".format(command[0])
            elif command[0] == "move":
                if not len(command) == 2:
                    msg = "'move' only needs 1 argument."
                if not (command[1] in POSITIONS or command[1] in COLORS):
                    msg = "'{}' is an invalid argument.".format(command[1])
            elif command[0] == "transport":
                if not len(command) == 3:
                    msg = "'transport' only needs 2 arguments."
                if not (command[1] in POSITIONS or command[1] in COLORS):
                    msg = "'{}' is an invalid argument.".format(command[1])
                if not (command[2] in POSITIONS or command[2] in COLORS):
                    msg = "'{}' is an invalid argument.".format(command[2])
            elif command[0] == "recognize":  # For “recognize” commands
                if not len(command) == 3:  # The command needs two arguments so the list should have three items in it
                    msg = "'recognize' only needs 2 arguments."
                elif not command[1] in COLORS:  # If the fist argument is only a color
                    msg = "'{}' is an invalid argument.".format(command[1])
                elif not (command[2] in POSITIONS or command[2] in COLORS):  # If the second argument is a position number or color
                    msg = "'{}' is an invalid argument.".format(command[2])
            if msg:
                raise Exception(msg)
except Exception as e:
            print(e)
            continue
        else:
            if command[0] == "pick":
                armrobot.pick()
            elif command[0] == "place":
                armrobot.place()
            elif command[0] == "move":
                if command[1] in POSITIONS:
                    armrobot.move_to(int(command[1]))
                else:
                    armrobot.move_to_color(command[1])
            elif command[0] == "transport":  # Change the else here into an elif
                if command[1] in POSITIONS:
                    armrobot.move_to(int(command[1]))
                else:
                    armrobot.move_to_color(command[1])
                armrobot.pick()
                if command[2] in POSITIONS:
                    armrobot.move_to(int(command[2]))
                else:
                    armrobot.move_to_color(command[2])
                armrobot.place()
            else:  # For "recognize” commands
                armrobot.pick()  # Pick up a block and check its color
                color = armrobot.recognize_color()
                if color == command[1]:  # Go to the second argument if the color detected matches the color specified in the first argument
                    if command[2] in POSITIONS:
                        armrobot.move_to(int(command[2]))
                    else:
                        armrobot.move_to_color(command[2])
                armrobot.place()  # Put the held block down
</pre>
## Conclusion

### A Quick Review
In this lesson you programmed a system that lets users control an arm robot by just typing in simple commands! The system you made only took single-line commands, but with all the programming techniques you’ve learned so far, you could also make a system that reads text files with several lines of commands in them and controls the arm robot with that. We didn’t put that challenge in the lesson, but we encourage you to try it out to test your skills if you have time!
### Coming Up Next Time!
In the next lesson we’ll start learning how to write programs that run multiple parallel processes at the same time! There’ll be plenty of examples so you can see for yourself how these programs work differently from our programs so far.