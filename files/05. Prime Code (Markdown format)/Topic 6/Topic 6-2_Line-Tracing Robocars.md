---
tags: Python_English, Topic 6
---

Python Robotics Course Lesson 22

Topic 6-2
Line-Tracing Robocars
===

Add feature to your Robocar which lets it trace lines!

## In This Lesson...

In this lesson we’ll be learning about two new concepts which are a very important part of object-oriented programming languages: **class inheritance** and **method overrides**. We’ll then use class inheritance and method overrides to  build on the `Robocar` class we made in the last lesson, using it to make your Robocar follow black lines drawn on paper!

:::info

Lesson Introduction

■ Lesson 22 Contents

1. Class Inheritance and Method Overrides
2. Imported Modules and Object Handling
3. Making a Line Tracer
4. Challenge: Reading Landmarks to Change Speed



In this lesson, we’re going to learn how to make a new class which inherits the properties and methods of an existing one, as well as how to add new processes to the methods which were just inherited!

We'll start the lesson by making an example program very similar to a role-playing game as we take a close look at class inheritance and overrides, which let you overwrite what a method does!

Next, we'll revisit importing again to see what happens to imported modules and objects while a program runs and learn some stuff about importing which hasn't been explained yet!

In the second half, we'll take the Robocar you made in the last lesson and add a sensor to it in order to make it a line tracer! We can do this by using class inheritance with the Robocar class we made last time!

At the end of the lesson we’ll add another sensor to your Robocar and give it the ability to change speeds when it recognizes a landmark!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!

:::

## New Python Syntax

Let’s make a simple program to learn about **class inheritance** and **method overrides**!

### Class Inheritance

When a new class takes on the properties and methods of an old one, that’s called **class inheritance**! When you do this, the old class becomes the **superclass**, or the **parent class**, while the new class becomes the **subclass**, or the **child class**.

Now let’s take a concrete look at how class inheritance can come in very handy! Let’s say that we want to make a role-playing game where the character can pick from three jobs: **Soldier**, **Sorcerer**, or **Sage**. We would have to define these in-game classes as separate classes inside of our program, and make our character as an instance.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/6-2_1_E.png"/>

We can see here that these classes have a few things in common. For one, they all have three properties: **Name**, **HP** (health points), **MP** (mana points). They also have a method named **Strike**! We’ll go ahead and define a class for these shared elements, and make a new class for each job to inherit it. The new classes, or **subclasses**, inherit the properties and methods of the old class, or the **superclass**. This means that you only need to define the unique properties and methods of each subclass, which helps you to code efficiently!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/6-2_2_E.png"/>

Now let’s see how class inheritance works for ourselves.

#### ■ Inheriting a Class

You can write the following code to inherit a class. Once a subclass inherits the properties and methods of the superclass, we only need to add the properties and methods unique to that subclass!

```
class Superclass:
    .
    .  # Define properties and methods here
    .

class Subclass(Superclass):
    .
    .  # Define new properties and methods here
    .
```

Let’s get right to it by defining a superclass named **Human**!

```python=1
class Human():
    def __init__(self, name, hp, mp):
        self.name = name
        self.hp = hp
        self.mp = mp
    
    def strike(self):
        print(self.name, "strikes!")
```

Next, we’ll make the **Soldier** who will inherit our **Human** class!

##### Example Code 2-1-1
###### New Code (Lines 11-16)

```python=1
class Human:
    def __init__(self, name, hp, mp):
        self.name = name
        self.hp = hp
        self.mp = mp

    def strike(self):
        print(self.name, "strikes!")


class Soldier(Human):
    def slash(self):  # Soldier cuts with his sword
        print(self.name, "slashes!")

    def shield_guard(self):  # Soldier guards with his shield
        print(self.name, "guards!")
```

You might notice that our `Soldier` class is missing things like an `__init__()` and `strike()` method, but he can still use them since he inherited the `Human` class! Try running the code below and creating an instance in the terminal to see how it works!

<pre class="prettyprint">
&gt;&gt;&gt; character = Soldier("Kai", 100, 20)
&gt;&gt;&gt; character.strike()
Kai strikes!
&gt;&gt;&gt; character.slash()
Kai cuts down!
&gt;&gt;&gt; character.shield_guard()
Kai guards!
</pre>

### Method Overrides

You can also add code which will overwrite a method inherited from a superclass. This is known as **overriding** a method. Method overrides are simple: all you have to do is define a method with the same name in the subclass.

Take the `Soldier` subclass in Example Code 2-1-1 below as an example: it overrides the `strike()` method from the superclass!

##### Example Code 2-2-1
###### New Code (Line 12, Line 13)

```python=11
class Soldier(Human):
    def strike(self):  # Method override
        print(self.name, "strikes fiercely!")  # Changes “strikes!” to “strikes fiercely!”
		
    def slash(self):
        print(self.name, "slashes!")

    def shield_guard(self):
        print(self.name, "guards!")
```

Now let’s run our program, create an instance, and call the `strike()` method one more time to see what happens!

<pre class="prettyprint">
&gt;&gt;&gt; character = Soldier("Kai", 100, 20)
&gt;&gt;&gt; character.strike()
Kai strikes fiercely!
</pre>

And now we’ve overwritten the superclass’s method! In this example we've completely overwritten the superclass method, but since we've inherited this method's processes, we can add new processes to it as well! We can use the **`super()` function** in order to do this.

#### Calling a Superclass with the `super()` Function

The `super()` function is a function native to Python which allows you to call a superclass. Let's try using it in Example Code 2-1-1 to call our **Human** superclass `strike()` method from inside of the **Soldier** subclass `strike()` method!

##### Example Code 2-2-2
###### New Code (Line 13)

```python=11
class Soldier(Human):
    def strike(self):
        super().strike()  # Calls the superclass method
        print(self.name, "strikes fiercely!")
        
    def slash(self):
        print(self.name, "slashes!")

    def shield_guard(self):
        print(self.name, "guards!")
```

Let’s run the program again, then create an instance and call the `strike()` method.

<pre class="prettyprint">
&gt;&gt;&gt; character = Soldier("Kai", 100, 20)
&gt;&gt;&gt; character.strike()
Kai strikes!
Kai strikes fiercely!
</pre>

As you can see, the `strike()` method works perfectly! You can use `super()` functions just like this to use superclass methods in a subclass and make that subclass do entirely new things! 

### Handling Objects Imported to Multiple Places

Just like we defined multiple classes here, we’ll be continuing to make bigger programs where we have to import modules and objects into files and classes in order to use them!

In other programming languages this can cause errors, and even if it works it can consume a huge amount of memory. However, this problem doesn’t happen in Python since everything references the same object!

Let’s try importing a `display` instance from the `StuduinoBitDisplay` class and a `time` module into multiple places and look up their **id values**, which are unique values automatically assigned to every object.

Copy and paste the following code into Mu and try running it.

##### Example Code 2-3-1

```python==1
from pystubit.board import display
import time

print("display:", id(display))  # You can look up id values using the id() function
print("time:", id(time))

class Sample:
    def __init__(self):
        from pystubit.board import display
        import time

        print("display:", id(display))
        print("time:", id(time))

    def func(self):
        from pystubit.board import display
        import time

        print("display:", id(display))
        print("time:", id(time))


sample = Sample()
sample.func()
```

#### Program Results
###### ★ The id value of the  `display` object changes every time the program runs

<pre class="prettyprint">
display 1065514160
time: 1061294916
display 1065514160
time: 1061294916
display 1065514160
time: 1061294916
</pre>

Example Code 2-3-1 imports this module and object in three places. Notice how their id values are identical? This is how you know that they’re referencing the same object!

While you normally import objects and modules at the beginning of a program, keep in mind that you can also import them when defining a class or inside of a method or function if you only want to use them there!

## Adding Sensors to the Robocar

Now let’s add an IR Photoreflector and Color Sensor to the Robocar we built in Lesson 21!

### Parts You’ll Need

 
* Your Robocar from Lesson 21
* IR Photoreflector x 1
* Color Sensor x 1
* Sensor Connecting Cable (3-wire, 30cm) x 1
* Sensor Connecting Cable (4-wire, 30cm) x 1
* Basic Cube (White) x 1

##### The Artec Block Shapes
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/6-2_3_E.png"/>

### Building Instructions

If you’ve taken apart your Robocar from the last lesson, follow the instructions below to rebuild it:

[Building a Robocar (Two Motors)](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_build_E.pdf)

[Adding Sensors](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/6-2_build_E.pdf)


## Making a Line Tracer

Now we’re going to make your Robocar into a Line Tracer which can follow black lines that you draw on a piece of paper!


<iframe src="https://player.vimeo.com/video/485366912" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>


The simplest way to make our Line Tracer would be to use an IR Photoreflector. The Line Tracer will recognize the black line and the white paper and use that to control where it drives! 

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/6-2_4_E.png"/>

This method of control means it follows the line by continually moving toward and away from it.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/6-2_5_E.png"/>

While normally we would code the Line Tracer to turn left and right in order to do this, these processes are already in inside of the `Robocar` class we created in Lesson 21. That means we can create a new class for our Line Tracer by inheriting the `Robocar` class and overriding its methods!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/6-2_6_E.png"/>


## Making the Course
We’ll use the course below to test how our Line Tracer works.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/6-2_7_E.png"/>


Follow the link below to download your own copy of the course and print it out on A3 paper.

[Line Tracer Course_A3](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/6-2_course_A3.pdf)

If you can’t print A3 size paper, download the course below and print it out on two pieces of A4 paper. Cut off the dotted parts before taping them together!

[Line Tracer Course_A4](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/6-2_course_A4.pdf)

### Defining a New Subclass
We're going to load our `vehicle.py` file from Lesson 21, Topic 6-1 and import our `Robocar` class. Next, we'll create a **`Linetracer` class** to inherit it!

In our `Linetracer` class, we'll override the superclass's `__init()__` method and add the following new properties and methods to it.


##### New Properties

|Property Name|What It Does|
|:---|:---|
|`irp`|Stores an instance of the `IRPhotoReflector` class. |
|`threshold`|Threshold value which tells the IR Photoreflector the difference between black and white. |

##### Methods to Override

* **`__init__()` Method**
Creates an instance of the `IRPhotoReflector` class and stores it in the `irp` property. It also runs the `set_threshold()` method, which chooses the black/white threshold value used by the IR Photoreflector.

##### Methods to Add

* **`set_threshold()` Method**
Calculates the **IR Photoreflector black/white threshold value** and stores it in the `threshold` property. This value is calculated by taking the average of the IR Photoreflector's values when it's on the black line and when it's over the white paper.

* **`linetrace()` Method**
Takes the threshold value found by `set_threshold()` and uses it to control how the Line Tracer drives. To put this in concrete terms, the Line Tracer will use the superclass `turn_right()` method to turn right when it's over the white paper and the value is above the threshold, and the `turn_left()` method to turn left when it's on the black line and the value is below the threshold. This is what allows it to follow the line!

* **`start()` Method**
Controls when the `linetrace()` method runs to allow us to start our Line Tracer by pressing a button. Generally, these will be the only methods we’ll call in an instance we create.

We’re going to combine the methods above to perform the processes below in order:

##### Program Outline

1. Create an instance of the `Linetracer` class
2. Run the `__init__()` method
3. Run the `set_threshold()` method
4. Run the `start()` method
5. Press B to run the `linetrace()` method
6. Press B again to stop the Line Tracer
7. Repeat steps 5 and 6 forever

Now let’s code each one!

#### ■ Inheriting the `Robocar` Class

We’ll start by making a `Linetracer` subclass which inherits our `Robocar` class. Let’s import our `Robocar` class from the `vehicle(.py)` file stored on our Core Unit and make the `Linetracer` class as follows:

```python==1
from vehicle import Robocar

class Linetracer(Robocar):
```

###### ★ If you can’t find vehicle.py, download it to your PC using the link below before transferring it to your Core Unit!

[vehicle.py](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/vehicle.py)

#### ■ Declaring Method Names
Next, let's declare the names of the methods we're going to override as well as the new methods we're going to add. If you have an empty method in Python, you have to fill it with something or else it will cause an error! You can use **`pass` statements** if you don't have anything to run but still need to write something. Run a `pass` statement in a program and absolutely nothing will happen!

###### New Code (Lines 4-14)

```python==1
from vehicle import Robocar

class Linetracer(Robocar):
    def __init__(self):
        pass
    
    def set_threshold(self):
        pass
	
    def linetrace(self):
        pass
	
    def start(self):
        pass
```

#### ■ Overriding the `__init__()` Method

You can also find the `__init__()` method in our `Robocar` superclass. That means that we’ll have to override it here.

##### `Robocar` Class `__init__()` Method Definition

```
def __init__(self, *, pin_l, pin_r):
    self.dcm_l = DCMotor(pin_l)
    self.dcm_r = DCMotor(pin_r)
    self.dcm_l.power(100)
    self.dcm_r.power(100)
```

However, since we'll still need to do things that require this method like making `DCMotor` class instances and setting the speed of our DC Motors, we can use the `super()` function to call our superclass and run its `__init__()` method! Since this method takes the DC Motor's port as an argument, we'll also have to make the `__init__()` we overrode take the same argument.

###### New and Changed Code (Lines 4-5)

```python==1
from vehicle import Robocar

class Linetracer(Robocar):
    def __init__(self, *, pin_l, pin_r):
    	super().__init__(pin_l=pin_l, pin_r=pin_r)
```

Next, we’ll create an instance of the `IRPhotoReflector` class and store it in the `irp` property. When we create this instance, we'll also need to set the IR Photoreflector's port as an argument. This means adding `pin_irp` to the `__init__()` method's arguments!

New and Changed Code (Line 4, 5, and 8)

```python==1
from vehicle import Robocar

class Linetracer(Robocar):
    def __init__(self, *, pin_l, pin_r, pin_irp):
        from pyatcrobo2.parts import IRPhotoReflector
		
        super().__init__(pin_l=pin_l, pin_r=pin_r)
        self.irp = IRPhotoReflector(pin_irp)
```

Last, we’ll run the `set_threshold()` method to set the threshold value between black and white. You’ll have to write the method like this:

###### New and Changed Code (Line 9)

```python==1
from vehicle import Robocar

class Linetracer(Robocar):
    def __init__(self, *, pin_l, pin_r, pin_irp):
        from pyatcrobo2.parts import IRPhotoReflector
		
        super().__init__(pin_l=pin_l, pin_r=pin_r)
        self.irp = IRPhotoReflector(pin_irp)
        self.set_threshold()
```

#### ■ Making the `set_threshold()` Method

Now it's time to make the method which picks the threshold value we use to tell black from white! Some parts of this process need to be done outside of the program, and this requires the user to take the following steps:

##### What the User Does...

1. The LED display will show a W. Hold the Line Tracer with its IR Photoreflector over any white part of the paper
2. Press the A button
3. The LED display will show a B. Hold the Line Tracer with its IR Photoreflector over any part of the black line
5. Press the A button



<iframe src="https://player.vimeo.com/video/485368435" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>


The program will do the following in order to set a threshold value:

##### What the Program Does...

1. Show a W on the LED Display
2. Wait until the user presses the A button
3. Take the IR Photoreflector value and store it in a variable
4. Show a B on the LED Display
5. Wait until the user presses the A button
6. Take the IR Photoreflector value and store it in a variable
7. Take the average of the values from steps 3 and 6 and store the result in the `threshold` property

Here comes the tricky part: IR Photoreflector values can change quite a bit depending on the time of day, the lights you use, or the room you’re in! We can get around this by adjusting the values every time the program starts and setting our threshold based on the environment.

Now let’s write the code to do it!

We’ll need to import button A and display objects from pystubit.board before importing a `time` module. Let’s do this at the beginning of the program!

###### New and Changed Code (Lines 13-14)

```python==12
    def set_threshold(self):
        from pystubit.board import button_a, display
        import time
```

Next, we need to show a W on the display and have the program wait until the user presses the A button. The only problem is that your Core Unit doesn’t have a command which says **wait until the button is pressed**! As shown in the code below, we can do this by using a `while` statement with the button’s `is_pressed()` method and combining them with a `not` operator to make it wait until you press the button. This code will repeat for as long as the A button is **not** pressed. In other words, it will wait until you press the A button!

###### New and Changed Code (Lines 16-18)

```python==12
    def set_threshold(self):
        from pystubit.board import button_a, display
        import time

        display.show("W", delay=0)	# W stands for white!
        while not button_a.is_pressed():  # Repeats as long as the A button is not pressed
            pass
```

You can use the following code to make the program **wait until the button is released**:

```
while button_a.is_pressed():
    pass
```

Next, we want to look up the value of the IR Photoreflector and store it in the `val_white` variable, but we don’t want the program to move on to the next step while we’re holding down the A button. Let’s make the program wait until we release the button!

###### New and Changed Code (Lines 19-22)

```python==12
    def set_threshold(self):
        from pystubit.board import button_a, display
        import time

        display.show("W", delay=0)
        while not button_a.is_pressed():  # Waits until the A button is pressed
            pass
        val_white = self.irp.get_value()    # Gets the value of the IR Photoreflector
        display.clear()
        while button_a.is_pressed():    # Waits until the A button is released
            pass
```

Once we release the A button, we want to show a B on the LED display 500 milliseconds later. We’ll then have to press the A button again to get the IR Photoreflector value and store it in the `val_black` variable.

###### New and Changed Code (Lines 23-30)

```python==12
    def set_threshold(self):
        from pystubit.board import button_a, display
        import time

        display.show("W", delay=0)
        while not button_a.is_pressed():
            pass
        val_white = self.irp.get_value()
        display.clear()
        while button_a.is_pressed():
            pass
        time.sleep_ms(500)
        display.show("B", delay=0)	# B stands for black!
        while not button_a.is_pressed():
            pass
        val_black = self.irp.get_value()
        display.clear()
        while button_a.is_pressed():
            pass
```

Last, we’ll add the values of our `val_white` and `val_black` variables, then divide this number by 2 to get the average! We’ll take this average and convert it into an integer using our `int()` function before storing it in our `threshold` property.

###### ★ We have to convert this into an integer since the return value of the `IRPhotoReflector` class’s `get_value()` method is an integer!

###### New and Changed Code (Line 31)

```python==23
        time.sleep_ms(500)
        display.show("B", delay=0)
        while not button_a.is_pressed():
            pass
        val_black = self.irp.get_value()
        display.clear()
        while button_a.is_pressed():
            pass
        self.threshold = int((val_white + val_black) / 2)
```

#### ■ Checking the Code

Let’s see if the code we’ve written up to this point is working! Add the code below to the end of your program and click the **Run** button to run it. With your Core Unit connected via USB, use your course as you follow the steps above to set a threshold and see if it appears in the terminal!

```python==33
    def linetrace(self):
        pass

    def start(self):
        pass

robo = (pin_l="M1", pin_r="M2", pin_irp="P0")
print(robo.threshold)
```

#### ■ Making the `linetrace()` Method

Now let’s write the method which does the line tracing! This method will have to do the following:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/6-2_8_E.png"/>

Until we press the B button, this method will continue looking up the IR Photoreflector’s value and control how our Line Tracer drives. To do this, we’ll use a `while` statement and put our code together as follows:

###### New and Changed Code (Lines 34-42)

```python==33
    def linetrace(self):
        from pystubit.board import button_b

        while not button_b.is_pressed():  # Repeat until you press the B button
            val = self.irp.get_value()
            if val > self.threshold:
                self.turn_right()
            else:
                self.turn_left()
        self.brake()
```

#### ■ Making the `start()` Method

Last, we’ll have to make a method which starts our Line Tracer when we press the B button. This is simple! We just have to make code which repeatedly checks whether the B button is pressed, and runs the `linetrace()` method when it is!

###### New and Changed Code (Lines 45-49)

```python==44
    def start(self):
        from pystubit.board import button_b

        while True:
            if button_b.is_pressed():
                self.linetrace()
```

If we left the code like this, pressing the B button would call the `linetrace()` method immediately, running the `while` statement inside of the method that checks whether the condition is true. Since we’d still be pressing B, this would skip the code in the `while` statement before it gets a chance to run, so the Line Tracer wouldn't work at all!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/6-2_9_E.png"/>


To fix this, let’s add some code to the beginning of the `linetrace()` method which waits until we release the B button. A similar problem would happen if we press the B button to stop our Line Tracer: the program would go back to the `start()` method and immediately call the `linetrace()` method. So let’s make our program wait until we release the B button here, too!

###### New and Changed Code (Line 36, Line 37, Line 45, Line 46)

```python==33
    def linetrace(self):
        from pystubit.board import button_b

        while button_b.is_pressed():  # Waits until the B button is released
            pass
        while not button_b.is_pressed():
            v = self.irp.get_value()
            if v > self.threshold:
                self.turn_right()
            else:
                self.turn_left()
        self.brake()
        while button_b.is_pressed():  # Waits until the B button is released
            pass
```

And now we’ve defined our `Linetracer` class!

### Checking the Program
We’ll need to write code to call the `start()` method from an instance of the `Linetracer` class called `robo`. Save Example Code 4-3-1 as `main.py` on your PC, then click the **Files** button and transfer the file to your Core Unit. Once you transfer it, unplug the USB cable and press the Reset button on your Core Unit to run the program. Now use your Line Tracer’s course to see how it works!

##### Course Directions
###### ★ Your Line Tracer should follow the course clockwise along the outside edge of the line.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/6-2_10_E.png"/>

If your Line Tracer is having trouble, try checking your code against the code below to see if there are any issues!

##### Example Code 4-3-1

###### Changed Code (Line 56)

```python==1
from vehicle import Robocar

class Linetracer(Robocar):
    def __init__(self, *, pin_l, pin_r, pin_irp):
        from pyatcrobo2.parts import IRPhotoReflector

        super().__init__(pin_l=pin_l, pin_r=pin_r)
        self.irp = IRPhotoReflector(pin_irp)
        self.set_threshold()

    def set_threshold(self):
        from pystubit.board import button_a, display
        import time

        display.show("W", delay=0)
        while not button_a.is_pressed():
            pass
        val_white = self.irp.get_value()
        display.clear()
        while button_a.is_pressed():
            pass
        time.sleep_ms(500)
        display.show("B", delay=0)
        while not button_a.is_pressed():
            pass
        val_black = self.irp.get_value()
        display.clear()
        while button_a.is_pressed():
            pass
        self.threshold = int((val_white + val_black) / 2)

    def linetrace(self):
        from pystubit.board import button_b

        while button_b.is_pressed():
            pass
        while not button_b.is_pressed():
            val = self.irp.get_value()
            if val > self.threshold:
                self.turn_right()
            else:
                self.turn_left()
        self.brake()
        while button_b.is_pressed():
            pass

    def start(self):
        from pystubit.board import button_b

        while True:
            if button_b.is_pressed():
                self.linetrace()


robo = Linetracer(pin_l="M1", pin_r="M2", pin_irp="P0")
robo.start()
```


## Challenge: Reading Landmarks to Change Speed

In this challenge we’ll add a feature which lets your Line Tracer use its Color Sensor to detect the green and red landmarks on the course and use them to speed up or slow down!

##### Finished Challenge Video

:::info
<iframe src="https://player.vimeo.com/video/485731400" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>
:::

##### Landmark Color and Speed

|Landmark|Action|Methods|
|:---|:---|:---|
|Green|Speed up (accelerate)|`set_speed_to_left(10)`</br>`set_speed_to_right(10)`|
|Red|Slow down (decelerate)|`set_speed_to_left(4)`</br>`set_speed_to_right(4)`|

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/6-2_11_E.png"/>

Calibrating your Color Sensor for this challenge can be tricky, so let’s follow the steps below to make our program!

### Making the Program

We'll have to add processes for this challenge to the two methods from **Example Code 4-3-1** below 

* **The `__init__()` Method**
We’ll add a process which creates an instance of the `Color Sensor` class and stores it as a new property. We’ll also be calling a newly-defined method here which will be explained in detail after this!

* **The `linetrace()` Method**
This method will use the `set_speed_to_left()` and `set_speed_to_right()` methods from the `Robocar` super class to change the speed of our Line Tracer.

We’ll also need to make thresholds for our colors to make sure that the Color Sensor detects each color accurately. Now it’s time to define our new method!

* **The `set_threshold_cs()` Method**
Just like the `set_threshold()` method used the IR Photoreflector to set a threshold for black and white, this method finds the Color Sensor values for red, then green, and uses the results to set a threshold our Line Tracer can use to recognize the landmarks!

Now let’s change and add our methods!

#### ■ Changing the `__init__()` Method

In order to make a `ColorSensor` class instance, we’ll need to add a `pin_cs` argument where we can set the port of the Color Sensor. (Line 5)
Next, add code to import the `ColorSensor` class. (Line 7)
Now create the instance and store it in the `cs` property. (Line 11)
Last, run the `set_threshold_cs()` method. We’ll define this later! (Line 13)

###### New and Changed Code (Line 5, Line 7, Line 11, Line 13)

```python==4
class Linetracer(Robocar):
    def __init__(self, *, pin_l, pin_r, pin_irp, pin_cs):
        from pyatcrobo2.parts import IRPhotoReflector
        from pyatcrobo2.parts import ColorSensor

        super().__init__(pin_l=pin_l, pin_r=pin_r)
        self.irp = IRPhotoReflector(pin_irp)
        self.cs = ColorSensor(pin_cs)
        self.set_threshold()
        self.set_threshold_cs()
```

#### ■ Adding the `set_threshold_cs()` Method
Here we’re going to add a method to set the threshold our Color Sensor will use to detect the colors. The `ColorSensor` class has a method known as `get_values()`. This method returns a tuple with four values showing the levels of **red, green, blue, and brightness**.

In this challenge, we want our Color Sensor to detect a **red** landmark and **green** landmark. Now let’s see what sort of values the `get_values()` method returns for these colors!

Make a new file and run the following code:

```python==1
from pyatcrobo2.parts import ColorSensor
import time

cs = ColorSensor("I2C")

while True:
    print(cs.get_values())
    time.sleep_ms(500)
```

Scan each color and you should see values like the ones below:

###### ★ These values are only for reference. Actual values are affected by the type of paper and ink you use!

* Scan red and you get...
<pre class="prettyprint">
[76, 19, 14, 15]
</pre>

* Scan green and you get...
<pre class="prettyprint">
[56, 93, 40, 12]
</pre>

This may seem obvious, but we see here that the value for red has a higher level of red, while the value for green has a higher level of green! We can use this to detect the color of our landmark by taking the average red and green values and putting them into separate conditions as shown below:

###### ★ The conditions here use the reference values above in their formulas.
```
Red Threshold: (76 + 56) / 2 = 66
Green Threshold: (19 + 93) / 2 = 56

Red if red value is greater than 66
Green if green value is greater than 56
```

While things may look good so far, keep in mind that there are two more colors present on our course. Those colors are **white** paper and the **black** line! This means that our Line Tracer will have to tell the difference between black and white in addition to red and green.

Let’s see what sort of values we get for these colors!

###### ★ These values are only for reference. Actual values are affected by the type of paper and ink you use!

* Scan the white paper and you get...
<pre class="prettyprint">
[87, 85, 78, 35]
</pre>

* Scan the black line and you get...
<pre class="prettyprint">
[51, 39, 32, 9]
</pre>

Notice how these values are pretty close to red and green’s? That means our formula from before might not work correctly! This next part’s going to be tough, but we can detect all four colors by setting an **upper** and **lower** threshold for all four values: this means the values for **red**, **green**, **blue**, and **brightness**!

Let’s get started by setting our thresholds to detect the red landmark. Just like in `set_threshold()`, we need to press the A button to make our Color Sensor get the value of the color. What makes this different is that we want our LED display to show the letter R before we get the Color Sensor value.

###### New and Changed Code (Lines 35-45)

```python==35
    def set_threshold_cs(self):
        from pystubit.board import button_a, display
        import time

        display.show("R", delay=0)
        while not button_a.is_pressed():
            pass
        vals_red = self.cs.get_values()
        display.clear()
        while button_a.is_pressed():
            pass
```

Next, let’s set our thresholds to detect the red landmark. You will have to make a list with tuples containing the lower and upper values for each of the four levels.

```
self.threshold_red = [
	(lowest red value, highest red value),
	(lowest green value, highest green value),
	(lowest blue value, highest blue value),
	(lowest brightness value, highest brightness value),
]
```

We’ll also need to subtract 5 from our lowest values and add 5 to our highest values!

###### New and Changed Code (Lines 46-50)

```python==35
    def set_threshold_cs(self):
        from pystubit.board import button_a, display
        import time

        display.show("R", delay=0)
        while not button_a.is_pressed():
            pass
        vals_red = self.cs.get_values()
        display.clear()
        while button_a.is_pressed():
            pass
        self.threshold_red = []
        self.threshold_red.append((vals_red[0] - 5, vals_red[0] + 5))
        self.threshold_red.append((vals_red[1] - 5, vals_red[1] + 5))
        self.threshold_red.append((vals_red[2] - 5, vals_red[2] + 5))
        self.threshold_red.append((vals_red[3] - 5, vals_red[3] + 5))
```

Next, let’s set our thresholds to detect the green landmark. Make it wait 500 milliseconds after setting the red values, then follow the same steps to make a list with tuples for the highest and lowest values for each level.

###### New and Changed Code (Lines 51-63)

```python==35
    def set_threshold_cs(self):
        from pystubit.board import button_a, display
        import time

        display.show("R", delay=0)
        while not button_a.is_pressed():
            pass
        vals_red = self.cs.get_values()
        display.clear()
        while button_a.is_pressed():
            pass
        self.threshold_red = []
        self.threshold_red.append((vals_red[0] - 5, vals_red[0] + 5))
        self.threshold_red.append((vals_red[1] - 5, vals_red[1] + 5))
        self.threshold_red.append((vals_red[2] - 5, vals_red[2] + 5))
        self.threshold_red.append((vals_red[3] - 5, vals_red[3] + 5))
        time.sleep_ms(500)
        display.show("G", delay=0)
        while not button_a.is_pressed():
            pass
        vals_green = self.cs.get_values()
        display.clear()
        while button_a.is_pressed():
            pass
        self.threshold_green = []
        self.threshold_green.append((vals_green[0] - 5, vals_green[0] + 5))
        self.threshold_green.append((vals_green[1] - 5, vals_green[1] + 5))
        self.threshold_green.append((vals_green[2] - 5, vals_green[2] + 5))
        self.threshold_green.append((vals_green[3] - 5, vals_green[3] + 5))
```


#### ■ Changing the `linetrace()` Method

Let’s change our `linetrace()` method last. Start by having it check whether the color is red after getting the Color Sensor value. In order to compare our red, green, blue, and brightness values, use a `for` statement to check each value one-by-one. If the value is outside of the range, use a `break` statement to skip the `for` statement and tell your Line Tracer to recognize red only when the `for` statement has run successfully! Next, add an `else` statement to change the Line Tracer’s speed.

##### Example Code 5-1-1
###### New Code (Lines 76-82)

```python=65
    def linetrace(self):
        from pystubit.board import button_b

        while button_b.is_pressed():
            pass
        while not button_b.is_pressed():
            val = self.irp.get_value()
            if val > self.threshold:
                self.turn_right()
            else:
                self.turn_left()
            vals = self.cs.get_values()
            for i in range(len(vals)):
                if not (vals[i] >= self.threshold_red[i][0] and vals[i] <= self.threshold_red[i][1]):
                    break
            else:  # Run only when break statement includes for statement
                self.set_speed_to_left(4)
                self.set_speed_to_right(4)
				
        self.brake()
        while button_b.is_pressed():
            pass
```

Next, we’ll use similar code to detemine whether the landmark is green and change speeds if it is!

###### New Code (Lines 83-88)

```python=65
    def linetrace(self):
        from pystubit.board import button_b

        while button_b.is_pressed():
            pass
        while not button_b.is_pressed():
            val = self.irp.get_value()
            if val > self.threshold:
                self.turn_right()
            else:
                self.turn_left()
            vals = self.cs.get_values()
            for i in range(len(vals)):
                if not (vals[i] >= self.threshold_red[i][0] and vals[i] <= self.threshold_red[i][1]):
                    break
            else:
                self.set_speed_to_left(4)
                self.set_speed_to_right(4)
            for i in range(len(vals)):  # Code structure is same as red circle’s
                if not (vals[i] >= self.threshold_green[i][0] and vals[i] <= self.threshold_green[i][1]):
                    break
            else:
                self.set_speed_to_left(10)
                self.set_speed_to_right(10)			
				
        self.brake()
        while button_b.is_pressed():
            pass
```

##### ■ The Test Drive



###### Changed Code (Line 101)

```python==101
robo = Linetracer(pin_l="M1", pin_r="M2", pin_irp="P0", pin_cs="I2C")
robo.start()
```

Check below to see what the finished program should look like. Test your own program and check it against the example code if it’s not working correctly! 

##### Example Code 5-1-1

```python==1
from vehicle import Robocar

class Linetracer(Robocar):
    def __init__(self, *, pin_l, pin_r, pin_irp, pin_cs):
        from pyatcrobo2.parts import IRPhotoReflector
        from pyatcrobo2.parts import ColorSensor

        super().__init__(pin_l=pin_l, pin_r=pin_r)
        self.irp = IRPhotoReflector(pin_irp)
        self.cs = ColorSensor(pin_cs)
        self.set_threshold()
        self.set_threshold_cs()

    def set_threshold(self):
        from pystubit.board import button_a, display
        import time

        display.show("W", delay=0)
        while not button_a.is_pressed():
            pass
        val_white = self.irp.get_value()
        display.clear()
        while button_a.is_pressed():
            pass
        time.sleep_ms(500)
        display.show("B", delay=0)
        while not button_a.is_pressed():
            pass
        val_black = self.irp.get_value()
        display.clear()
        while button_a.is_pressed():
            pass
        self.threshold = int((val_white + val_black) / 2)

    def set_threshold_cs(self):
        from pystubit.board import button_a, display
        import time

        display.show("R", delay=0)
        while not button_a.is_pressed():
            pass
        vals_red = self.cs.get_values()
        display.clear()
        while button_a.is_pressed():
            pass
        self.threshold_red = []
        self.threshold_red.append((vals_red[0] - 5, vals_red[0] + 5))
        self.threshold_red.append((vals_red[1] - 5, vals_red[1] + 5))
        self.threshold_red.append((vals_red[2] - 5, vals_red[2] + 5))
        self.threshold_red.append((vals_red[3] - 5, vals_red[3] + 5))
        time.sleep_ms(500)
        display.show("G", delay=0)
        while not button_a.is_pressed():
            pass
        vals_green = self.cs.get_values()
        display.clear()
        while button_a.is_pressed():
            pass
        self.threshold_green = []
        self.threshold_green.append((vals_green[0] - 5, vals_green[0] + 5))
        self.threshold_green.append((vals_green[1] - 5, vals_green[1] + 5))
        self.threshold_green.append((vals_green[2] - 5, vals_green[2] + 5))
        self.threshold_green.append((vals_green[3] - 5, vals_green[3] + 5))
        
    def linetrace(self):
        from pystubit.board import button_b

        while button_b.is_pressed():
            pass
        while not button_b.is_pressed():
            val = self.irp.get_value()
            if val > self.threshold:
                self.turn_right()
            else:
                self.turn_left()
            vals = self.cs.get_values()
            for i in range(len(vals)):
                if not (vals[i] >= self.threshold_red[i][0] and vals[i] <= self.threshold_red[i][1]):
                    break
            else:
                self.set_speed_to_left(4)
                self.set_speed_to_right(4)
            for i in range(len(vals)):
                if not (vals[i] >= self.threshold_green[i][0] and vals[i] <= self.threshold_green[i][1]):
                    break
            else:
                self.set_speed_to_left(10)
                self.set_speed_to_right(10)
				
        self.brake()
        while button_b.is_pressed():
            pass

    def start(self):
        from pystubit.board import button_b

        while True:
            if button_b.is_pressed():
                self.linetrace()


robo = Linetracer(pin_l="M1", pin_r="M2", pin_irp="P0", pin_cs="I2C")
robo.start()
```
## Conclusion
### A Quick Review

In this lesson we learned about two new concepts: **class inheritance** and **method overrides**. Leveraging these means that you’ll be able to program much more efficiently, and that you’ll only have to write code once in order to reuse it in multiple places!

These days, more and more people are choosing to publish their programs as open source, letting other people use those programs for free. A programmer who makes a popular piece of open source software may find other programmers fixing their bugs or developing versions of their software with entirely new features.

If you think you might want to be one of those programmers in the future, what you’ve learned in this lesson will come in very handy!

### Coming Up Next Time!

In the next lesson, we’ll learn how to **inherit multiple classes** and **inherit multiple classes at the same time** as we build a controller equipped with an IR Photoreflector and Color Sensor to make a remote-control Robocar!
