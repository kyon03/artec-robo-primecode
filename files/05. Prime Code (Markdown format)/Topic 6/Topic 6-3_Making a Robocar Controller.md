---
tags: Python_English, Topic 6
---

Python Robotics Course Lesson 23

Topic 6-3
Making a Robocar Controller
===

Make a remote controller for your Robocar!

## In This Lesson...

In this lesson we’re going to learn how to **inherit multiple classes** as well as how to inherit these classes **at the same time**! In the second half of the lesson, we’ll take our `Robocar` class and make a new `Controller` class, inheriting them both to make a Remote-controlled Robocar!

<iframe src="https://player.vimeo.com/video/484348462" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

:::info

Lesson Introduction

■ Lesson 23 Contents
1. Inheriting Multiple Classes and Multiple Classes at the Same Time
2. Making a Remote-controlled Robocar
3. Challenge: Adding Signals to Your Controller 


In this lesson we’re going to learn how to inherit multiple classes as well as how to inherit them simultaneously!

We’ll start the lesson by taking a look at each of these two inheritance methods. You can use both of them to transfer the properties and methods of multiple classes, but there’s a key difference between them. We’ll see what that difference is by making a example program!

In the second half of the lesson we’ll build a controller and use it to control our Robocar. Our program will inherit both our Robocar and Controller classes to make a new class for our Remote-controlled Robocar.

In the last part of the lesson, we’ll take on the challenge of adding a new output signal to our controller and give our Robocar even more ways to move!

Now let's start the lesson!

:::

## New Python Syntax

In the previous lesson, you learned about class inheritance and how to overwrite methods from the base class. You aren’t limited to only inheriting from one base class - you can also inherit from multiple classed at once (multiple inheritance) or inherit from a previous derived class (multilevel inheritance). Let’s look at the differences between these kinds of inheritance next.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-3/6-3_1_E.png"/>

### Multilevel Class Inheritance

In Python, it’s possible to inherit from subclasses, too! In the code below, Class B inherits Class A, and then Class C inherits Class B. That means Class C will end up having the properties and methods of both Class A and Class B!

```
class A():
    .
    .
	
class B(A):
    .
    .
	
class C(B):
    .
    .
```

Let’s try writing some more concrete example code using role-playing game classes! In a lot of RPGs, characters can take on various jobs, or **"classes"** and level up those classes to learn new, more powerful combat abilities. Let’s program an RPG class called **Soldier** which can level up to become a **Knight** and then a **Royal Knight**, with each new class inheriting the old class’s abilities as well as learning new abilities!

##### Example Code 2-1-1

```python==1
class Soldier():		# The Soldier class
    def get_job(self):		# Display your character’s job class
        print("I'm a soldier.")
    def get_action(self):	# Display a list of actions your character can do
        print("Normal attack.")    

class Knight(Soldier):		# The Knight class
    def get_job(self):
        print("I'm a knight.")
    def get_action(self):
        super().get_action()        
        print("Strong attack.")

class RoyalKnight(Knight):	# The Royal Knight class
    def get_job(self):
        print("I'm a royal knight.")
    def get_action(self):
        super().get_action()
        print("Ultimate attack.")

character = RoyalKnight()
character.get_job()
character.get_action()
```

In this code, `Knight` inherits `Soldier` and then `RoyalKnight` inherits again from `Knight`. In the process of inheritance, the `get_job()` and `get_action()` methods both get overwritten. Now make the program in Mu Editor and try running it!

#### Program Results

<pre class="prettyprint">
I'm a royal knight.
Normal attack.
Strong attack.
Ultimate attack.
</pre>

When you run the `get_job()` method, it only runs the processes we defined for the `RoyalKnight` class. `get_action()`, on the other hand, runs all three processes from `Soldier`, `Knight`, and `RoyalKnight`.

The reason for this difference is that in `get_action()` we used the function `super()` to call a superclass. Calling a superclass with the `super()` like this to make a chain lets you use the methods from all the classes in your multilevel inheritance series.

### Multiple Class Inheritance

In Example Code 2-1-1 multilevel inheritance let us make a straightforward series of upgraded classes (Soldier ⇒ Knight ⇒ Royal Knight), but it won’t let us make the Sage class (shown earlier) inherit from both the Sorcerer and Monk classes.

In addition to multilevel inheritance, Python also lets classes inherit from two base classes at once through multiple inheritance. The program below makes Class C inherit from both Class A and Class B at once.

```
class A():
    .
    .
	
class B():
    .
    .
	
class C(A, B):  # List classes separated by commas to inherit from multiple classes
    .
    .
```

Now let’s use this same method to define the `Sage` class so it inherits abilities from both the `Sorcerer` and `Monk` classes!

##### Example Code 2-2-1

```python=1
class Monk():
    def heal(self):		# A healing spell
        print("Heal!")

class Sorcerer():
    def fireball(self):	# A fire spell
        print("Fireball!")

class Sage(Monk, Sorcerer):	# Inherit from both the Monk and Sorcerer
    def ice_blast(self):	# An ice spell
        print("Ice Blast!")

character = Sage()
character.heal()		# Inherited from the Monk
character.fireball()		# Inherited from the Sorcerer
character.ice_blast()		# New ability for the Sage
```

When you use multiple inheritance, you carry over the properties and methods of all your base classes. Now copy the code into your editor and try running it!

#### Program Results

<pre class="prettyprint">
Heal!
Fireball!
Ice Blast!
</pre>

Now an instance of the `Sage` class can call the methods it inherited from the `Sorcerer` and `Monk` classes!

#### Things to Remember in Multiple Inheritance

Inheriting all of the properties and methods from multiple classes at once in multiple inheritance can be useful, but what happens if those classes have methods with the same name in them?

In the following code we’ll test this by making a class called `Sub` that inherits from the classes `Super_1` and `Super_2`, both of which have the `__init__()` method.

##### Example Code 2-2-2

```python=1
class Super_1:
    def __init__(self):
        print("I'm Super 1.")

class Super_2:
    def __init__(self):
        print("I'm Super 2.")

class Sub(Super_1, Super_2):
    def __init__(self):
        super().__init__()

sub = Sub()
```

The `super()` function inside the `Sub` class’s `__init__()` method calls a superclass, but when we run this code, it still only runs the `Super_1` class’s `__init__()` method.

#### Program Results

<pre class="prettyprint">
I'm Super 1.
</pre>

This happens because the `super()` function in MicroPython can only call one class at a time. That means if you want to make a program like this, it’s better to use multilevel inheritance than multiple inheritance!

##### Example Code 2-2-3

```python=1
class Super_1:
    def __init__(self):
        print("I'm Super 1.")

class Super_2(Super_1):
    def __init__(self):
        super().__init__()
        print("I'm Super 2.")

class Sub(Super_2):
    def __init__(self):
        super().__init__()

sub = Sub()
```

When you run this code, it can call the `__init__()` methods from both class `Super_1` and class `Super_2`!

#### Program Results

<pre class="prettyprint">
I'm Super 1.
I'm Super 2.
</pre>

## Building the Controller

For the rest of this lesson, we’re going to be making a controller you can use as a remote control for your Robocar! Take the Robocar you built in Lesson 21 (Topic 6-1) and then build a controller with an IR Photoreflector and a Color Sensor in it.

### Parts You'll Need

* Lesson 21’s Robocar x 1
* IR Photoreflector x 1
* Color Sensor x 1
* Sensor Connecting Cable (3-wire, 30cm) x 1
* Sensor Connecting Cable (4-wire, 30cm) x 1
* Basic Cube (White) x 4
* Basic Cube (Gray) x 2
* Basic Cube (Red) x 1
* Basic Cube (Green) x 1
* Basic Cube (Blue) x 1
* Basic Cube (Yellow) x 1
* Half A (Gray) x 2
* Half C (White) x 6
* Beam x 1
* Axle x 2


##### The Artec Block Shapes

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-3/6-3_2_E.png"/>

### Building Instructions

If you took apart your Robocar after Lesson 22, follow the instructions linked below to rebuild it.

[Building a Robocar (Lesson 21)](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_build_E.pdf)

[Building a Controller](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-3/6-3_build_E.pdf)


## Programming Robocar Controls

For this program, we’ll make a class called `Controlbot` that inherits from both the `Robocar` class you made to control the Robocar in Lesson 22, and a new `Controller` class that operates your controller.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-3/6-3_3_E.png"/>

If you make separate classes for all the parts and features of a project you build, it’ll come in handy in cases like this where you can reuse the classes you’ve made as parts in a new project!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-3/6-3_4_E.png"/>

Let’s start the program by defining the controller’s class, and them move on to the class that operates the controller and the Robocar together.

### Defining the Controller Class

We can split the code for your controller into two different sections for the right and left sides of the controller.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-3/6-3_5_E.png"/>

When you turn the blocks on their axles this puts different blocks in front of the controller’s two sensors, which makes them broadcast the signals (strings) shown below.

##### Signals Sent from the Left Side

|Color of Block in Front of the IR Photoreflector|Broadcast Signal (String)|
|:---|:---|
|White Block|`"on"`|
|No Block|`"off"`|

##### Signals Sent from the Right Side

|Color of Block in Front of the Color Sensor|Broadcast Signal (String)|
|:---|:---|
|Blue|`"b"`|
|Red|`"r"`|
|Green|`"g"`|
|No Block|`"none"`|

###### ★ `"b"`, `"r"`, and `"g"` stand for the first letter of their colors.
###### ★ The yellow block on the right side will be used in the challenge in the next chapter!

Now let’s define a **`Controller` class** with the following properties and methods it can use with the signals from the controller’s sensors.

##### The Controller Class’s Properties
|Property|What It Does|
|:---|:---|
|`irp`|Stores an instance of the `IRPhotoReflector` class. |
|`cs`|Stores an instance of the `ColorSensor` class. |
|`threshold_irp`|A threshold value used to determine if a white block is in front of the IR Photoreflector|

##### The Controller Class’s Methods

|Method|What It Is|Arguments|Return Value|
|:---|:---|:---|:---|
|`set_controller()`|A constructor that takes the place of the `__init__()` method. |`pin_irp`: Name of the IR Photoreflector’s port</br>`pin_cs`: Name of the Color Sensor’s port|None|
|`get_signals()`|A method that receives signals from the controller. |None|`(signal_l , signal_r)`</br>A tuple storing the left and right side signals|

We could use the `__init__()` method as a constructor here as usual, but we know that we’re going to be inheriting from the `Controller` class as well as another class (`Robocar`) later in the program. If we use `__init__()` in both of them the two `__init__()`  methods will conflict with each other!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-3/6-3_6_E.png"/>


Instead we’ll make the `set_controller()` method take the role of constructor for this class, and call it later in the `__init__()` method for the `Controlbot` subclass!

Now let’s define the two methods for this class. 

#### ■ The `set_controller()` Method

First declare the `Controller` class, and then declare the `set_controller()` method inside it. The `set_controller()` method will create instances of the `IRPhotoReflector` and `ColorSensor` classes, so we need to make `pin_irp` and `pin_cs` to find the names of the ports the sensors are connected to and specify them in the arguments for those instances. Adding an asterisk (`*`) in front of the argument makes it into a forced keyword argument!

```python==1
class Controller:

    def set_controller(self, *, pin_irp, pin_cs):
```

Next let’s make an instance of our `IRPhotoReflector` and `ColorSensor` classes. These will take the port numbers we put in `set_controller()`’s argument! We’ll have to store them in the instance's `irp` and `cs` properties, respectively.

###### New Code (Lines 4-8)

```python==1
class Controller:

    def set_controller(self, *, pin_irp, pin_cs):
        from pyatcrobo2.parts import IRPhotoReflector
        from pyatcrobo2.parts import ColorSensor
        
        self.irp = IRPhotoReflector(pin_irp)
        self.cs = ColorSensor(pin_cs)
```

Last, we’ll set a threshold value which lets the IR Photoreflector determine if there’s white block in front of it. Make a new program and run the following code. Once it’s running, check what your IR Photoreflector’s values are when the block is there and when it isn’t! We’ll take the average of both results to use as a threshold value.

###### ★ Sunlight contains infrared light, too! Make sure your IR Photoreflector isn’t in direct sunlight when looking up your values!

##### Checking Your IR Photoreflector Values

```python==1
from pyatcrobo2.parts import IRPhotoReflector
import time

irp = IRPhotoReflector("P0")

while True:
    print(irp.get_value())
    time.sleep_ms(500)
```

Let’s store our threshold value in the `threshold_irp` property. We won’t be needing a threshold for our Color Sensor this time. Instead of using a threshold, we’ll be using the `ColorSensor` class’s `get_colorcode()` method to directly detect red and green!

###### New Code (Line 9)

```python==1
class Controller:

    def set_controller(self, *, pin_irp, pin_cs):
        from pyatcrobo2.parts import IRPhotoReflector
        from pyatcrobo2.parts import ColorSensor
        
        self.irp = IRPhotoReflector(pin_irp)
        self.cs = ColorSensor(pin_cs)
        self.threshold_irp = 500  # Put the threshold value you found here
```

Remember the last lesson? In order to make your Robocar into a Line Tracer, we made a method to set the IR Photoreflector’s threshold from inside of the program and find the right threshold for our environment! We could make another method here, but we’ll skip that part as we don’t want our code to get too long.

#### ■ Defining the `get_signals()` Method

Next, we’ll define a `get_signals()` method which receives the signals sent from the controller’s right and left sides. The left side will compare the threshold value stored in the `threshold_irp` property with the IR Photoreflector’s value to decide which signal to broadcast.

###### New Code (Lines 11-16)

```python==1
class Controller():
   
    def set_controller(self, *, pin_irp, pin_cs):
        from pyatcrobo2.parts import IRPhotoReflector
        from pyatcrobo2.parts import ColorSensor
        
        self.irp = IRPhotoReflector(pin_irp)
        self.cs = ColorSensor(pin_cs)
        self.threshold_irp = 500
    
    def get_signals(self)        
        val_irp = self.irp.get_value()
        if val_irp > self.threshold_irp:
            signal_l = "on"  # White block is in front of Robocar
        else:
            signal_l = "off"  # No block present
```

The right side will use the `ColorSensor` class’s `get_colorcode()` method to decide which signal to broadcast. Run this method and it will return one of the following property names defined in the `ColorSensor` class.

##### Return Values of the `get_colorcode()` Method

Color|Property Name|Value|
|:---|:---|:---|
|Red|ColorSensor.COLOR_RED|1|
|Green|ColorSensor.COLOR_GREEN|2|
|Blue|ColorSensor.COLOR_BLUE|3|
|White|ColorSensor.COLOR_WHITE|4|
|Yellow|ColorSensor.COLOR_YELLOW|5|
|Orange|ColorSensor.COLOR_ORANGE|6|
|Purple|ColorSensor.COLOR_PURPLE|7|
|Unknown|ColorSensor.COLOR_UNDEF|0|

These values are treated as constants, so if you’d like to use a conditional statement to check which color it is write code like the one shown below to call the properties from an instance or a class:

```
from pyatcrobo2.parts import ColorSensor

cs = ColorSensor("I2C")
color = cs.get_colorcode()
if color == cs.COLOR_RED: 	# Won’t compare if you write a number, like color == 1
    print("red")
else:
    print("not red")
```

And now we need to write the following code to decide which signal to broadcast!

###### New Code (Lines 18-26)

```python==11
    def get_signals(self):        
        val_irp = self.irp.get_value()
        if val_irp > self.threshold_irp:
            signal_l = "on"
        else:
            signal_l = "off"
        
        color = self.cs.get_colorcode()
        if color == self.cs.COLOR_RED:
            signal_r = "r"
        elif color == self.cs.COLOR_GREEN:
            signal_r = "g"
        elif color == self.cs.COLOR_BLUE:
            signal_r = "b"
        else:
            signal_r = "none"
```

Last, let’s set our two `signal_l` and `signal_r` signals to be returned as tuples. Now we've finished defining our `Controller` class!

###### New Code (Line 28)

```python==11
    def get_signals(self):
        val_irp = self.irp.get_value()
        if val_irp > self.threshold_irp:
            signal_l = "on"
        else:
            signal_l = "off"
        
        color = self.cs.get_colorcode()
        if color == self.cs.COLOR_RED:
            signal_r = "r"
        elif color == self.cs.COLOR_GREEN:
            signal_r = "g"
        elif color == self.cs.COLOR_BLUE:
            signal_r = "b"
        else:
            signal_r = "none"
            
        return (signal_l, signal_r)
```


#### ■ Why Not Use `get_colorcode()` in Lesson 22?

In last lesson’s challenge we used a Color Sensor to recognize the color of landmarks on the course, but in this lesson we were able to use the `get_colorcode()` method to do the same thing! Since this method uses a pre-set threshold on your Core Unit to detect color, we can get a pretty accurate reading for objects like blocks which have consistent coloring. However, the color of printed items can change depending on the ink and type of paper, which can lead to inaccurate readings. This is why we had to set a threshold based on our printout in order to recognize the color!

### Defining a Class for a Remote-controlled Robocar
We’ll be defining our Remoted-controlled Robocar class by inheriting our `Robocar` class from Lesson 21 along with our new `Controller` class. This class performs the follow actions depending on which signal the controller broadcasts.

##### Left Side Broadcast Signals and Actions

|Controller</br>Signal|Action|
|:---|:---|
|`"on"`|Perform action decided by right signal|
|`"off"`|Apply brakes to stop|

##### Right Side Broadcast Signals and Actions

|Controller</br>Signal|Action|
|:---|:---|
|`"b"`|Drive forward (left signal is `"on"`)|
|`"r"`|Spin right (left signal is `"on"`)|
|`"g"`|Spin left (left signal is `"on"`)|
|`"none"`|Coast to a stop|


##### Methods to Override

* The `__init__()` Method
We'll be overriding the `__init__()` method we inherited from the `Robocar` class and running the `Controller` class's `set_controller()` method instead. If you want to make any changes to the speed of our Robocar by changing the speed of the wheels, we’ll do that inside of this method, too!

##### Methods to Add

* The `start()` Method
In order to use our controller to control the Robocar, we’ll call this method from the instance. We can manage how our Robocar drives by repeatedly calling the `get_signals()` method we inherited from the `Controller` class.

Now let’s define our two methods in order!

#### ■ Overriding the `__init__()` Method

We’ll be adding onto our code from before here. We’ll start by importing the `Robocar` class from  `vehicle(.py)` on our Core Unit.

###### New Code (Line 1)

```python==1
from vehicle import Robocar

class Controller():
    def set_controller(self, *, pin_irp, pin_cs):
```

Next, we'll write the code for our new `Controlbot` class under the code which defines our `Controller` class. `Controlbot` inherits the `Robocar` class first and the `Controller` class second.

This class's constructor is the `__init__()` method, and we'll need it to take arguments from both the `Robocar` class's `__init__()` method and the `Controller` class's `set_controller()` method. This means we have to write our code as shown below:

###### New Code (Lines 36-38)

```python==34
class Controlbot(Robocar, Controller):

    def __init__(self, *, pin_l, pin_r, pin_irp, pin_cs):
        super().__init__(pin_l=pin_l, pin_r=pin_r)  # Inherited from Robocar class
        self.set_controller(pin_irp=pin_irp, pin_cs=pin_cs)  # Inherited from Controller class
```

If we want to change the speed of our Robocar, we can call the `Robocar` class’s `set_speed_to_left()` and `set_speed_to_right()` methods here.

###### New Code (Lines 39-40)

```python==34
class Controlbot(Robocar, Controller):

    def __init__(self, *, pin_l, pin_r, pin_irp, pin_cs):
        super().__init__(pin_l=pin_l, pin_r=pin_r)
        self.set_controller(pin_irp=pin_irp, pin_cs=pin_cs)
        self.set_speed_to_left(5)  # Set any level of speed from 1-10
        self.set_speed_to_right(5)  # Set any level of speed from 1-10 
```

#### ■ Defining the `start()` Method
Next, we’re going to define a `start()` method which decides what our Controlbot should do based on the signal it receives! This method will have to do the following:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-3/6-3_7_E.png"/>

Let's start by writing code to use `get_signals()` to get the signal and call the `brake()` method if the left side (that means the index is **0**) broadcasts an `"off"` signal!

###### New Code (Lines 42-47)
```python==42
    def start(self):
        
        while True:
            signals = self.get_signals()
            if signals[0] == "off":
                self.brake()  # Use the brakes to stop
```

Next, when the left side broadcasts `"on"`, we need a four-part condition for actions controlled by signals sent from the right side (that means the index is **1**).

###### New Code (Lines 48-56)
```python==42
    def start(self):
        
        while True:
            signals = self.get_signals()
            if signals[0] == "off":
                self.brake()
            else:
                if signals[1] == "b":
                    self.move_forward()  # Drive forward
                elif signals[1] == "r":
                    self.spin_right()  # Spin right
                elif signals[1] == "g":
                    self.spin_left()  # Spin left
                else:
                    self.stop()  # Stop without using the brakes
```

Now we’ve fully defined our `Controlbot` class!


### Checking the Program

Now let’s check if our class is working correctly by adding code to the end of our program to make an instance of it and call the `start()` method!

```python==56
robo = Controlbot(pin_l="M1", pin_r="M2", pin_irp="P0", pin_cs="I2C")
robo.start()
```

The completed program should look like this. Let’s check to see if it works alright!

##### Example Code 4-3-1
```python==1
from vehicle import Robocar

class Controller():
    def set_controller(self, *, pin_irp, pin_cs):
        from pyatcrobo2.parts import IRPhotoReflector
        from pyatcrobo2.parts import ColorSensor
        
        self.irp = IRPhotoReflector(pin_irp)
        self.cs = ColorSensor(pin_cs)
        self.threshold_irp = 500
    
    def get_signals(self):        
        val_irp = self.irp.get_value()
        if val_irp > self.threshold_irp:
            signal_l = "on"
        else:
            signal_l = "off"
        
        color = self.cs.get_colorcode()
        if color == self.cs.COLOR_RED:
            signal_r = "r"
        elif color == self.cs.COLOR_GREEN:
            signal_r = "g"
        elif color == self.cs.COLOR_BLUE:
            signal_r = "b"
        else:
            signal_r = "none"
            
        return (signal_l, signal_r)


class Controlbot(Robocar, Controller):

    def __init__(self, *, pin_l, pin_r, pin_irp, pin_cs):
        super().__init__(pin_l=pin_l, pin_r=pin_r)
        self.set_controller(pin_irp=pin_irp, pin_cs=pin_cs)
        self.set_speed_to_left(5)
        self.set_speed_to_right(5)

    def start(self):
        while True:
            signals = self.get_signals()
            if signals[0] == "off":
                self.brake()
            else:
                if signals[1] == "b":
                    self.move_forward()
                elif signals[1] == "r":
                    self.spin_right()
                elif signals[1] == "g":
                    self.spin_left()
                else:
                    self.stop()


robo = Controlbot(pin_l="M1", pin_r="M2", pin_irp="P0", pin_cs="I2C")
robo.start()
```

If your Robocar is still connected via USB, click the **Run** button to run your program!

If you want to unplug your Robocar, save this program with the name `main(.py)` on your computer, and click the **Files** button before transferring it to your Core Unit. Press the **Reset** button on your Core Unit to run the program and see how it works!

If `vehicle(.py)` isn’t on your Core Unit, download it using the link below and transfer it!

[vehicle.py](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-2/vehicle.py)

## Challenge: Adding a Signal to Your Controller

In this challenge we're going to modify Example Code 4-3-1 by adding a new signal for the yellow block on the right side of our controller which lets our Robocar drive backward!

##### New Broadcast Signals (Right Side)

|Color|Broadcast Signal (String)|
|:---|:---|
|Blue|`"b"`|
|Red|`"r"`|
|Green|`"g"`|
|**Yellow**|**`"y"`**|
|No Block|`"none"`|

##### New Robocar Actions

|Broadcast Signal|Action|
|:---|:---|
|`"b"`|Drive forward (left signal is `"on"`)|
|`"r"`|Spin right (left signal is `"on"`)|
|`"g"`|Spin left (left signal is `"on"`)|
|**`"y"`**|**Reverse** (left signal is `"on"`)|
|`"none"`|Coast to a stop|


### Example Program

If you understand Example Code 4-3-1, you can easily add a new action for a signal.

* Adding a Signal
In Example Code-4-3-1, lines **19-27** decide which signals are sent by the right side of our controller. This means that we can just add another conditional statement to set a signal for the yellow block.

* Adding an Action
Lines **46-53** in Example Code-4-3-1 control the actions your Robocar does for each signal sent by the right side of our controller. Here we can just add another conditional statement which calls the **`move_backward()` method** when the Robocar receives a **"y"** signal.

Making these two changes makes the program look like this:

##### Example Code 5-1-1
###### New Code (Lines 28-29, Lines 57-58)

```python==1
from vehicle import Robocar

class Controller():
    def set_controller(self, *, pin_irp, pin_cs):
        from pyatcrobo2.parts import IRPhotoReflector
        from pyatcrobo2.parts import ColorSensor
        
        self.irp = IRPhotoReflector(pin_irp)
        self.cs = ColorSensor(pin_cs)
        self.threshold_irp = 500
    
    def get_signals(self):
        val_irp = self.irp.get_value()
        if val_irp > self.threshold_irp:
            signal_l = "on"
        else:
            signal_l = "off"
        
        color = self.cs.get_colorcode()
        if color == self.cs.COLOR_RED:
            signal_r = "r"
        elif color == self.cs.COLOR_GREEN:
            signal_r = "g"
        elif color == self.cs.COLOR_BLUE:
            signal_r = "b"
        elif color == self.cs.COLOR_YELLOW:
            signal_r = "y"
        else:
            signal_r = "none"
            
        return (signal_l, signal_r)


class Controlbot(Robocar, Controller):

    def __init__(self, *, pin_l, pin_r, pin_irp, pin_cs):
        super().__init__(pin_l=pin_l, pin_r=pin_r)
        self.set_controller(pin_irp=pin_irp, pin_cs=pin_cs)
        self.set_speed_to_left(5)
        self.set_speed_to_right(5)

    def start(self):
        while True:
            signals = self.get_signals()
            if signals[0] == "off":
                self.brake()
            else:
                if signals[1] == "b":
                    self.move_forward()
                elif signals[1] == "r":
                    self.spin_right()
                elif signals[1] == "g":
                    self.spin_left()
                elif signals[1] == "y":
                    self.move_backward()
                else:
                    self.stop()


robo = Controlbot(pin_l="M1", pin_r="M2", pin_irp="P0", pin_cs="I2C")
robo.start()
```

## Conclusion

### A Quick Review

In this lesson we learned both how to **inherit multiple classes** and how to inherit those classes **at the same time**! While multiple inheritance makes it even easier to reuse your code, you may find yourself having trouble if you don’t have a strong grasp of how it’s all put together! Take the time to clearly understand the basics you’ve learned here and use it to gain even more mastery over classes!

### Coming Up Next Time!

In the next lesson, we’ll learn **a very special way to override** operators and Python’s native functions, as well as the difference between **class methods** and **instance methods**! Then use what you’ve learned to program a remote-controlled Robocar!