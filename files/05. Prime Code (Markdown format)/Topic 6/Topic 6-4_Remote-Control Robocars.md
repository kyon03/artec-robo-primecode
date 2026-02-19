---
tags: Python_English, Topic 6
---

Python Robotics Course Lesson 24

Topic 6-4
Remote-Control Robocars
===

Program a Robocar to move when you press buttons on a remote controller!

## In This Lesson...

Learn about **overriding special methods** and **the differences between class methods and instance methods**, Then use what you've learned to program a button-controlled Robocar!


:::info

Lesson Introduction

■ Lesson 24 Contents

1. Overriding Special Methods
2. Instance Member and Class Member Variables
3. Instance Methods and Class Methods
4. Making a Push-button Robocar
5. Challenge: Changing a Class into a Module



Remember how Python allows you to use + and - operators, convert data into text strings with the str() function, or how you can look up the length of sequenced data with the len() function? Well, in this lesson we're going to learn about an override method made especially for Python's native functions!

We'll start by reviewing these operators and the names of Python's native functions, and then write a program that can override a + operator for ourselves!

Next, we'll learn how the variables and methods we use in classes actually come in  two types: those used for instances and those used for classes! We'll be going into great detail about these, but fully understanding them is a great way to avoid the problems that come with programming using classes.

In the second half of the lesson, we'll take what we've learned and program your Robocar to take button input!

Last, we'll take on the challenge of converting the class we made in our program into a module that we can rewrite! Classes and modules can be very similar at times, and this challenge is a great way to learn how and where to use each one!

Now let's start the lesson!

:::

## New Python Syntax

Let's take a look at a simple example program so you can learn how overriding special methods works and how class and instance methods are different!

### Overriding Special Methods

Python contains certain special methods that define what operators like + and - or built-in functions like `str()` and `len()` do. Have a look at them in the tables below!

##### Special Method Names for Relational Operators
|Operator|Special Method Name|
|:---|:---|
|`==`|`__eq__(self, other)`|
|`!=`|`__ne__(self, other))`|
|`<`|`__lt__(self, other)`|
|`>`|`__gt__(self, other)`|
|`<=`|`__le__(self, other)`|
|`>=`|`__ge__(self, other)`|

#####  Special Method Names for Mathematic Operators
|Operator|Special Method Name
|:---|:---|
|`+`|`__add__(self, other)`|
|`-`|`__sub__(self, other)`|
|`*`|`__mul__(self, other)`|
|`//`|`__floordiv__(self, other)`|
|`/`|`__truediv__(self, other)`|
|`%`|`__mod__(self, other)`|
|`**`|`__pow__(self, other)`|

##### Special Method Names for Built-In Functions

|Function|Special Method Name
|:---|:---|
|`str()`|`__str__(self)`|
|`len()`|`__len__(self)`|

Special methods like these have two underscores on either side of their names, just line the constructor method ****init**()**.

You've actually already used a class enhanced by special method overriding in previous lessons! Specifically, the `StuduinoBitImage` class.

In the `StuduinoBitImage` class, the operator `+` (a.k.a. the `__add__()` method) is overridden to make it create a image pattern by putting two base patterns together.

In the example program below we've made an image pattern by making image instances with each of the colors it uses separately, then combinining them into a new instance using the `+` operator.

##### A Calculation with the `__add__()` Method Overridden

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-4/6-4_1_E.png"/>

```
    img_yellow = StuduinoBitImage("10000:01000:00000:01010:00000", color=(31, 31, 0))
    img_brown = StuduinoBitImage("10000:01000:00000:01010:00000", color=(4, 1, 1))
    img_giraffe = img_yellow + img_brown
```

Now let's try overriding methods ourselves!

#### ■ Method Overriding Practice

For our example, let's make a program that sorts the results of a school survey by class. The survey has three possible answers (A, B, and C) and we want to count how many people voted for each one. 'class_1' and 'class_2' collect the number of answers for each class and we can add them together to get the total, like this:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-4/6-4_2_E.png"/>

It would be hard to go through this same process for every class of every grade in the school! What if we try putting these results into a Pytho dictionary (called `dict`) and add them together with the `+` operator?

Well, normally we can't do that because the `+` operator doesn't have any methods that work with dictionaries. However, if we make a new class called `Data` with dictionaries in its properties and override the `+` operator's `__add__()` method, we can make the calculation we want work!

Let's declare the `Data` class and set dictionaries as an argument in its constructor (the `__init__()` method) so it can use them as properties.

```python=1
class Data:
    def __init__(self, _dict):
        self._dict = _dict
```
###### ★ `dict` is a reserved word in Python, so we're using `_dict` instead.

Next we need to override the `__add__()` method. We want it to take instances of the `Data` class in its arguments, then retrieve the all the keys from the dictionaries specified in order and combine them to make a new dictionary called `new`. That will let us use the dictionary `new` to perform the calculation we wanted to do before!

###### New Code (Lines 5-9)

```python=1
class Data:
    def __init__(self, _dict):
        self._dict = _dict

    def __add__(self, other):
        new = {}
        for key in self._dict.keys():
            new[key] = self._dict[key] + other._dict[key]
        return new

```

Now let's try assembling data for two classes and adding them together with `+` to see the results.

##### Example Code 2-1-1
###### New Code (Lines 12-15)

```python=1
class Data:
    def __init__(self, _dict):
        self._dict = _dict

    def __add__(self, other):
        new = {}
        for key in self._dict.keys():
            new[key] = self._dict[key] + other._dict[key]
        return new

class_1 = Data({"A" : 10, "B" : 12, "C" : 14})
class_2 = Data({"A" : 12, "B" : 8, "C" : 16})
total = class_1 + class_2
print(total)
```

#### Program Results

```
{'A': 22, 'C': 30, 'B': 20}
```

Overriding methods with special uses like this lets you make classes with useful additional features without messing up any of the pre-existing classes!

### Instance Member and Class Member Variables

Next let's talk about the difference between two kinds of variables/properties used with classes and objects: class member variables and instances member variables. These are usually called just class variables and instance variables.

Class variables belong to a class as whole while instance variables belong to individual instances. Let's look at the program below as an example.

##### Example Code 2-2-1

```python=1
class Dog:
    voice = "Arf!"		# Class variable
    
    def __init__(self, name, age):
        self.name = name		# Instance variable
        self.age = age			# Instance variable

```

In this class, the variable `voice` is defined as a class variable, while the variables `name` and `age` are defined as instance variables. To put it simply, variables that are defined directly under a class become class variables, while variables defined inside methods become instance variables.

Class variables can be called directly from a class object, but instance variables can't! Instance variables can only be called from the instance object they belong to.

##### Calling Class Variables

<pre class="prettyprint">
&gt;&gt;&gt; print(Dog.voice)
Arf!
</pre>

##### Calling Instance Variables

<pre class="prettyprint">
&gt;&gt;&gt; print(Dog.name)
Traceback (most recent call last):
  File "&lt;stdin&gt;", line 1, in &lt;module&gt;
AttributeError: type object 'Dog' has no attribute 'name'
</pre>

Class variables are tied to class objects and to instance objects that belong to that class as well. That means if you overwrite the value of a class variable (as shown with the variable `voice` below), the value of that variable when called in an instance will change as well.

##### Example Code 2-2-2
###### New Code (Lines 9-12)

```python=1
class Dog:
    voice = "Arf!"
    
    def __init__(self, name, age):
        self.name = name
        self.age = age

dog = Dog("Shiro", 3)
print(dog.voice)
Dog.voice = "Woof!"	# Overwrites class variable
print(dog.voice)	# Shows changed class variable
```

#### Program Results
	
<pre class="prettyprint">
Arf!
Woof!
</pre>

If you make a change to a variable from inside an instance instead, the variable's reference to the class variable will be removed and a new value attached to it, so it won't affect the variable's value in other instances.

##### Example Code 2-2-3
###### New and Changed Code (Lines 9-13)

```python=1
class Dog:
    voice = "Arf!"
    
    def __init__(self, name, age):
        self.name = name
        self.age = age

dog_1 = Dog("Shiro", 3)
dog_2 = Dog("Taro", 3)
print(dog_2.voice)  # Shows class member variable
dog_1.voice = "Woof!"	# Unable to reference class member variable. Stores new value.
print(dog_2.voice)  # Shows class member variable
```

#### Program Results
	
<pre class="prettyprint">
Arf!
Arf!
</pre>


### Instance Methods and Class Methods

Just like with variables, there's a difference between **instance methods** and **class methods**. To define a class method, you need to add the decorator `@classmethod` in front of it. In the code below, the method `bark_cl()` is define as a class method.

##### Example Code 2-3-1

```python==1
class Dog:
    voice = "Arf!"
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    @classmethod	# Decorator
    def bark_cl(self):	# Class method
        print(self.voice)
        
    def bark(self):     # Instance method
        print(self.voice)
```

To run an instance method from a class object, you need to have the method's `self` argument specified, but for a class method this argument can be left unspecified.

###### The argument `self` is left unspecified here, which causes an error.
					   
<pre class="prettyprint">
&gt;&gt;&gt; Dog.bark()
Traceback (most recent call last):
  File "&lt;stdin&gt;", line 1, in &lt;module&gt;
TypeError: function takes 1 positional arguments but 0 were given
</pre>

###### Setting the `self` argument with the class object itself lets the program run without error.
						  
<pre class="prettyprint">
&gt;&gt;&gt; Dog.bark(Dog)
Arf!
</pre>

###### Class methods can run even if `self` is unspecified
						  
<pre class="prettyprint">
&gt;&gt;&gt; Dog.bark_cl()
Arf!
</pre>

This difference exists because the `@classmethod` decorator attached to class methods automatically sets the class itself in the method's arguments when you run it. Calling an instance method inside its instance will automatically set that instance in the arguments as well.

## Building a Robocar

You can use the Robocar you built in Lesson 21 in this lesson as well! If you took it apart after that lesson, follow the instructions linked below to rebuild it.

[Building a Robocar (Lesson 21)](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_composition.pdf)

## Making a Button-controlled Robocar

Now let's write a program that lets you control your Robocar by pressing buttons to make it drive forward/backward and turn right/left.

##### Programming the Robocar
	   

<iframe src="https://player.vimeo.com/video/485732288" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>


We can divide this program into three broad parts: the main program, command registration, and running the car. 

##### Program Structure

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-4/6-4_3_E.png"/>

The most complex of these three parts is the command registration section. To record a command, you'll press Button A to select a the command and Button B to confirm it. We'll make the LED display show arrows pointing in different directions to show what command you currently have selected for this part.

##### Arrows Showing the Selected Direction

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-4/6-4_4_E.png"/>

When you press Button B to confirm a command, that command will be recorded in a list. The `run()` function then goes through the commands stored in the list and runs methods (`move_forward()`, `spin_right()`, etc.) from the `Robocar` class that match the commands. 

### Defining the Direction Class

The first thing we need to do is create a class called `Direction` to manage data describing the Robocar's motions. We'll  give the `Direction` class the properties and methods explained below.



##### The Direction Class's Properties

* Motion Constants

|Motion|Name|Value|
|:---|:---|:---|
|Drive forward|`FORWARD`|10|
|Turn right|`RIGHT`|20|
|Reverse|`BACKWARD`|30|
|Turn left|`LEFT`|40|

* `directions`: A tuple storing the motion constants

```
directions = (FORWARD, RIGHT, BACKWARD, LEFT)
```

* `images`: A dictionary storing the arrow images corresponding to each motion
		   
|Key (Property)|Value|
|:---|:---|
|`FORWARD`|`StuduinoBitImage.ARROW_N` (Arrow pointing up)|
|`RIGHT`|`StuduinoBitImage.ARROW_E` (Arrow pointing right)|
|`BACKWARD`|`StuduinoBitImage.ARROW_S` (Arrow pointing down)|
|`LEFT`|`StuduinoBitImage.ARROW_W` (Arrow pointing left)|

##### The Direction Class's Methods
																 
* **The `get_direction()` Method**
Takes an index number for the `directions` property as an argument, and returns the data from that index.

* **The `get_image()` Method**
Takes the properties `FORWARD`, `RIGHT`, `BACKWARD`, `LEFT` as arguments, then uses them as keys to retrieve values (image patterns) from the property `images`.

* **The `get_len()` Method**
Returns the length of the `directions` property.

This is a lot of information to think about all at once, but we'll explain the purpose of each property and method one by one as we write the code for them.

#### Defining the Motion Constants

We could record the motion commands in the list as strings like `move_forward` or `spin_right"`, but we're going to use numbers instead. Why?

Because micro-computers like your Core Unit have less memory they can use to process programs than regular computers do. It takes more memory for a computer to process a string than a number, so when you're writing programs for a micro-computer, you'll want to replace strings in with numbers in your code whenever it's possible. The difference in how much memory strings and numbers take up isn't as large in Python as it is in older programming languages like C, but strings will still use at least twice as much memory as numbers. Those small amounts of extra memory can add up into very significant amounts over the course of a long program, so don't underestimate the difference this makes!

We'll practice using numbers instead of strings in this lesson's program so you can start learning this technique. The tricky part of this is that using numbers instead of strings makes it a lot harder to tell what your code is supposed to do just by looking at it. However, we can make that easier by defining the numbers as constants with names attached to them!

Now that we've explained its purpose, we can start the program by defining the `Direction` class with the code below.
				  
```python==1
class Direction():

    FORWARD = 1
    RIGHT = 2
    BACKWARD = 3
    LEFT = 4
    directions = (FORWARD, RIGHT, BACKWARD, LEFT)
```

All four of these constants are class variables belonging to the `Direction` class, so their values can all be accessed directly from the class.

```
print(Direction.FORWARD)
```

On line 7, we store the four constants in a tuple, which will called the `directions` property from now on.

#### Making the Arrow Image Dictionary

Next let's make the dictionary that will store the arrow images corresponding to each motion. All the arrow images we need are already available in the StuduinoBitImage class, so we can just import that class and use them.

###### New Code (Line 1, Lines 11-16)
								
```python==1
from pystubit.board import Image

class Direction():

    FORWARD = 1
    RIGHT = 2
    BACKWARD = 3
    LEFT = 4
    directions = (FORWARD, RIGHT, BACKWARD, LEFT)

    images = {
        FORWARD: Image.ARROW_N,  # Arrow pointing up
        RIGHT: Image.ARROW_E,  # Arrow pointing right
        BACKWARD: Image.ARROW_S,  # Arrow pointing down
        LEFT: Image.ARROW_W,  # Arrow pointing left
    }

```

#### ■ Defining the Methods

Finally we need to define these three methods:

* The `get_direction()` Method
* The `get_image()` Method
* The `get_len()` Method

These methods will be used later when we write the code in the `regist()` function so we can switch selected commands when you press the buttons. It might not be clear yet why we need these particular methods, but you'll see why they're useful as we go. Let's define them as class methods so we can call them directly from the class, too.

We'll define the `get_direction()` method first. This method takes an index number for the `directions` property as an argument (called `index`), then retrieves the data stored at that index.

###### New Code (Lines 16-18)
				  
```python==1
class Direction():

    FORWARD = 10
    RIGHT = 20
    BACKWARD = 30
    LEFT = 40
    directions = (FORWARD, RIGHT, BACKWARD, LEFT)

    images = {
        FORWARD: Image.ARROW_N,
        RIGHT: Image.ARROW_E,
        BACKWARD: Image.ARROW_S,
        LEFT: Image.ARROW_W,
    }

    @classmethod
    def get_direction(self, index):
        return self.directions[index]

```

Next let's define the `get_image()` method, which we need to retrieve the arrow image that corresponds to the selected motion command. This method takes the four constants `FORWARD`, `RIGHT`, `BACKWARD`, `LEFT` (which are properties of this class) as arguments, then uses them as keys to retrieve values from the `images` dictionary.

###### New Code (Lines 20-22)
				
```python==16
    @classmethod
    def get_direction(self, index):
        return self.directions[index]

    @classmethod
    def get_image(self, direction):
        return self.images[direction]

```

The last method we need to define is `get_len()`, which retrieves the length of the `directions` property.

###### New Code (Lines 24-26)

```python==16
    @classmethod
    def get_direction(self, index):
        return self.directions[index]

    @classmethod
    def get_image(self, direction):
        return self.images[direction]
    
    @classmethod
    def get_len(self):
        return len(self.directions)

```

### Defining the regist() Function

The `regist()` function's job is to register the motion commands you select with the buttons, and store them in a list. This function works by following the steps laid out in the diagram below.

##### The regist() Function's Layout
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-4/6-4_5_E.png"/>

The `regist()` function uses the A and B buttons as well as the LED display, so let's import those classes into the program. Let's also create an empty list called `commands` as a global variable.

###### New and Changed Code (Line 1, Line 3)
```python==1
from pystubit.board import Image, button_a, button_b, display

commands = []  # List for recording commands

class Direction():
```

#### ■ Clearing the List

Now we can start writing the `regist` function! We want to use the global variable `commands` we just made in this function, so start by declaring the global variable. We also want this registration function to be clear of stored data each time you call it, so use the `clear()` method to empty the list at the start.

###### New and Changed Code (Lines 32-34)

```python==32
def regist():
    global commands
    commands.clear()
```
#### ■ Select and Display a Default State

Let's pick a command for the program to have selected by default when you start it up and make the LED display show it. Let's use the command with the index number 0, out of the four motion commands we have.

Make a variable called `index` and set its value to 0. Use the `index`  variable to set an argument in the `Direction` class's `get_direction()` method so it can retrieves the motion command numbers through that variable and then store them in another variable called `selected_direction`. After that, use the `selected_direction` variable to set the argument for the `Direction` class's `get_image()` method, and display the results using the `display` object's `show()` method.

###### New and Changed Code (Lines 36-38)

```python==32
def regist():
    global commands
    commands.clear()
    
    index = 0
    selected_direction = Direction.get_direction(index)
    display.show(Direction.get_image(selected_direction), delay=0)

```

#### ■ Selecting Commands with Button A

We're going to make the program keep checking whether the buttons are being pressed until you hold down Button B, so create a loop that keeps repeating until the value of a variable called `loop_flag` changes from `True` to `False`.

###### New Code (Lines 40-41)

```python==32
def regist():
    global commands
    commands.clear()
    
    index = 0
    selected_direction = Direction.get_direction(index)
    display.show(Direction.get_image(selected_direction), delay=0)

    loop_flag = True
    while loop_flag:

```

Next the program needs to determine when Button A has been pressed and increase the value of the `index` variable by 1 each time it counts a press. Make sure `index`'s value only gets changed after you let go of the button so the program doesn't register multiple presses when you hold a button down. If the value of `index` ever exceeds the highest index number in the `Direction` class's `direction` property (**Length - 1**) it will cause an error in the code, so use a conditional here to set its value back to 0 if that happens. After that, select the arrow image that corresponds to the motion command number at the new index number and make it appear on the LED display.

###### New Code (Lines 40-49)

```python==40
    loop_flag = True
    while loop_flag:
        if button_a.is_pressed():
            while button_a.is_pressed():  
                pass
            index = index + 1 if index < Direction.get_len() - 1 else 0
            selected_direction = Direction.get_direction(index)
            display.show(
                Direction.get_image(selected_direction), delay=0
            )
```

#### ■ Confirming and Looping Commands with Button B

The program should respond differently when you press the Button B quickly and which you hold it down. You'll need to use the `time` module to measure how long the button's been pressed for, so import that into the program.

###### New Code (Line 2)

```python==1
from pystubit.board import Image, button_a, button_b, display
import time

commands = []
```

First measure how long Button B is pressed for, and set `loop_flag` to `False` when the time reaches two seconds to make the loop stop. You can measure time on the Core Unit using the `ticks_ms()` function from the `time` module. Run this function once when the button is pressed before you start counting time and save the time it counts in a variable called `start`.

###### New Code (Lines 51-52)

```python==41
    loop_flag = True
    while loop_flag:
        if button_a.is_pressed():
            while button_a.is_pressed():  
                pass
            index = index + 1 if index < Direction.get_len() - 1 else 0
            selected_direction = Direction.get_direction(index)
            display.show(
                Direction.get_image(selected_direction), delay=0
            )
        if button_b.is_pressed():
            start = time.ticks_ms()
```

This program will loop to keep counting time for as long as the B Button is held down. Within this loop, you can use the `time` module's `ticks_diff()` function to find the difference between the most recent time from `ticks_ms()` and the time you started counting (stored in `start`). If this difference exceeds two seconds (2000 milliseconds), set the `loop_flag` variable to `False` and turn off the LED display. Then the program should wait until the button is released before **exiting the `while` loop with a `break` statement. **

###### New Code (Lines 54-59)

```python==51
        if button_b.is_pressed():
            start = time.ticks_ms()
            while button_b.is_pressed():
                if time.ticks_diff(time.ticks_ms(), start) > 2000:
                    loop_flag = False
                    display.clear()
                    while button_b.is_pressed():  # Wait until Button B is released
                        pass
                    break  # Running break will end the loop!
```

The reason we're using a `break` statement to end the `while` loop is that we're using an `else` statement to add the currently selected command to the list. The `else`  statement only runs when the `while` loop ends normally (i.e. when the B Button is released in less than two seconds). Let's make this part of the code flash the LED display quickly (for about 200 milliseconds) to let you know it's added your command to the list.

###### New Code (Lines 60-66)

```python==51
        if button_b.is_pressed():
            start = time.ticks_ms()
            while button_b.is_pressed():
                if time.ticks_diff(time.ticks_ms(), start) > 2000:
                    loop_flag = False
                    display.clear()
                    while button_b.is_pressed():
                        pass
                    break
            else:	# Make sure to indent the code correctly!
                commands.append(selected_direction)
                display.clear()  # Makes the LED display blink
                time.sleep_ms(200)
                display.show(
                    Direction.get_image(selected_direction), delay=0
                )
```

It took some time, but now we've finished defining the `regist()` function!

### Defining the `run()` Function

The `run()` function's job is to retrieve the commands recorded in the `commands` list and run matching methods from the `Robocar` class to make the Robocar drive. This function works by following the steps laid out in the diagram below.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-4/6-4_6_E.png"/>

This function controls the Robocar, so let's make an instance of the `Robocar` class for it to use.

###### New Code (Line 3, Line 6)

```python==1
from pystubit.board import button_a, button_b, display, Image
import time
from vehicle import Robocar

commands = []
robo = Robocar(pin_l="M1", pin_r="M2")
```

Now we can start writing the `run()` function. First you need to retrieve data (the motion command numbers) from the list and make the images appear on the LED display. 
			 
###### New Code (Lines 70-74)
```python==70
def run():
    global commands, robo  # You won't necessarily need to use the global variables in this section, but declaring them here makes the program easier to read.
	
    for direction in commands:
        display.show(Direction.get_image(direction), delay=0)

```

Now let's make the Robocar move based on the value of `direction`, using the code shown below. Using the properties from the `Direction` class in place of the motion command numbers here makes the code more understandable!

###### New Code (Lines 75-83)

```python==70
def run():
    global commands, robo
	
    for direction in commands:
        display.show(Direction.get_image(direction), delay=0)
        if direction == Direction.FORWARD:
            robo.move_forward()
        elif direction == Direction.BACKWARD:
            robo.move_backward()
        elif direction == Direction.LEFT:
            robo.spin_left()
        else:
            robo.spin_right()        
        time.sleep_ms(1000)	 # Set however long you want the car to drive here!
    robo.brake()  # Stop the car when all the other actions are complete.

```

### Defining the `main()` Function

Finally, let's make the `main()` function, which checks which of the two buttons is pressed and tells the `regist()` and `run()` functions when to run. This function works by following the steps laid out in the diagram below.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-4/6-4_7_E.png"/>

You can make the program do this using the following code:

###### New Code (Lines 86-95)

```python==86
def main():
    while True:
        if button_a.is_pressed():
            while button_a.is_pressed(): # Function runs after Button A is released
                pass
            run()
        if button_b.is_pressed():
            while button_b.is_pressed(): # Function runs after Button B is released
                pass
            regist()
```

Let's also make the Core Unit display a smiley face (like the one below) to let you know when it's ready to receive commands.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-4/6-4_8_E.png"/>

This image is available as a property of the `StuduinoBitImage` called `HAPPY`! The color it's lit up in is built into the `StuduinoBitImage` too, as the property `YELLOW`. That means you only need this one line of code to display this image:

```
display.show(Image.HAPPY, color=Image.YELLOW)
```

You can add this code to your program at the three points shown below.

###### New Code (Line 87, Line 93, Line 98)

```python==86
def main():
    display.show(Image.HAPPY, color=Image.YELLOW)  # New
    while True:
        if button_a.is_pressed():
            while button_a.is_pressed():
                pass
            run()
            display.show(Image.HAPPY, color=Image.YELLOW)  # New
        if button_b.is_pressed():
            while button_b.is_pressed():
                pass
            regist()
            display.show(Image.HAPPY, color=Image.YELLOW)  # New
```

### Testing Your Robocar

Now you've finished programming your Robocar controls! Just add a line at the end of the program to run the `main()` function, and you can try running the program!

```python==99
main()
```

If your Robocar isn't working right, compare the program you've written with the example code below to check for mistakes.

##### Example Code 4-5-1

```python==1
from pystubit.board import button_a, button_b, display, Image
import time
from vehicle import Robocar

commands = []
robo = Robocar(pin_l="M1", pin_r="M2")

class Direction():

    FORWARD = 1
    RIGHT = 2
    BACKWARD = 3
    LEFT = 4
    directions = (FORWARD, RIGHT, BACKWARD, LEFT)

    images = {
        FORWARD: Image.ARROW_N,
        RIGHT: Image.ARROW_E,
        BACKWARD: Image.ARROW_S,
        LEFT: Image.ARROW_W,
    }
    
    @classmethod
    def get_direction(self, index):
        return self.directions[index]

    @classmethod
    def get_image(self, direction):
        return self.images[direction]
    
    @classmethod
    def get_len(self):
        return len(self.directions)

							 

def regist():
    global commands
    
    commands.clear()
    index = 0
    selected_direction = Direction.get_direction(index)
    display.show(Direction.get_image(selected_direction), delay=0)
    loop_flag = True

    while loop_flag:
        if button_a.is_pressed():
            while button_a.is_pressed():
                pass
            index = index + 1 if index < Direction.get_len() - 1 else 0
            selected_direction = Direction.get_direction(index)
            display.show(
                Direction.get_image(selected_direction), delay=0
            )
        if button_b.is_pressed():
            start = time.ticks_ms()
            while button_b.is_pressed():
                if time.ticks_diff(time.ticks_ms(), start) > 2000:
                    loop_flag = False
                    display.clear()
                    while button_b.is_pressed():
                        pass
                    break
            else:
                commands.append(selected_direction)
                display.clear()
                time.sleep_ms(200)
                display.show(
                    Direction.get_image(selected_direction), delay=0
                )

def run():
    for direction in commands:
        display.show(Direction.get_image(direction), delay=0)
        if direction == Direction.FORWARD:
            robo.move_forward()
        elif direction == Direction.BACKWARD:
            robo.move_backward()
        elif direction == Direction.LEFT:
            robo.spin_left()
        else:
            robo.spin_right()        
        time.sleep_ms(1000)
    robo.brake()
        
def main():
    display.show(Image.HAPPY, color=Image.YELLOW)
    while True:
        if button_a.is_pressed():
            while button_a.is_pressed():
                pass
            run()
            display.show(Image.HAPPY, color=Image.YELLOW)
        if button_b.is_pressed():
            while button_b.is_pressed():
                pass
            regist()
            display.show(Image.HAPPY, color=Image.YELLOW)

main()
```

## Challenge: Defining Direction as a Module

In Example Code 4-5-1 we stored data about the Robocar's movements by making a class called `Direction`. However, **we can do the same thing by making a module instead of a class!** For this challenge, copy the program you've written into new file called `Direction(.py)` and save that file on your Core Unit so you can use it as a module!

### Example Program

First make a new program file, then save it on your computer with the file name `Direction(.py)`.

Next, copy the code shown below from your old program file and paste it into the new one. When you're done copying it, delete this section of the code from the old program file, or else change it into comments.

```python==8 
class Direction():

    FORWARD = 1
    RIGHT = 2
    BACKWARD = 3
    LEFT = 4
    directions = (FORWARD, RIGHT, BACKWARD, LEFT)

    images = {
        FORWARD: Image.ARROW_N,
        RIGHT: Image.ARROW_E,
        BACKWARD: Image.ARROW_S,
        LEFT: Image.ARROW_W,
    }
    
    @classmethod
    def get_direction(self, index):
        return self.directions[index]

    @classmethod
    def get_image(self, direction):
        return self.images[direction]
    
    @classmethod
    def get_len(self):
        return len(self.directions)

```

Now edit the code you've copied into your new file as shown below. Since the program files are split up now, you need to import the StuduinoBitImage class (`Image` for short) at the start of this file, remove the `@classmethod` decorator and the `self` argument, and change the indents in the code.

##### Direction.py

```python==1
from pystubit.board import Image

FORWARD = 1
RIGHT = 2
BACKWARD = 3
LEFT = 4
directions = (FORWARD, RIGHT, BACKWARD, LEFT)

images = {
    FORWARD: Image.ARROW_N,
    RIGHT: Image.ARROW_E,
    BACKWARD: Image.ARROW_S,
    LEFT: Image.ARROW_W,
}

def get_direction(index):
    return directions[index]

def get_image(direction):
    return images[direction]
    
def get_len():
    return len(directions)
```

Now transfer this file to your Core Unit. Click the **Files** button in the Menu bar and move the file from the Computer window to the Studuino:bit window.

Once you've done that, go back to your original file and make it import your new `Direction(.py)` module!

###### New Code (Line 4)
```python==1
from pystubit.board import button_a, button_b, display, Image
import time
from vehicle import Robocar
import Direction
```

Now you're done modifying the program! Try running it to see if it works the same way Example Code 4-5-1 does!

### Why Use a Module Instead of a Class?

Classes and modules have a lot in common, so as we've just seen, there are times when you can use either one without changing how your program works at all!

So, how do you when to use a module and when to use a class, then? Here a few simple guidelines you can use to decide:

##### Class vs. Module Guidelines

* If you need to use a number of instances that use the same methods but have different value stored in their properties, it's better to use a class.
* Conversely, if you're only planning to make one instance, it might be better to use a module. (For example, in Python no matter how many time you import a module in your program it won't make any new copies of that module, which can save valuable memory!)
* If you might want to use inheritance, it's better to use a class. (Modules can't be inherited from.)
* If you have several value-storing variables and want to use them as arguments in several functions, making them into properties and variables of a class will keep your code cleaner.

## Conclusion
### A Quick Review

In this lesson you learned about overriding the special methods that make operators and built-in functions work, and how variables and methods work differently when they belong to a class versus an instance. This is the last lesson of the basic studies part of the course! In future lessons you'll be writing a lot more code, so remember you can always come back and review Topic 6 if you get confused!

### Coming Up Next Time!

In Topic 7 we'll be showing you how to program games! These programs will be more complex than what you've done so far, so get ready!


																							   

																							   