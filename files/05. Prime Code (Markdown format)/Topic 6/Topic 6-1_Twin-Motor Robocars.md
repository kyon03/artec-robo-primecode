---
tags: Python_English, Topic 6
---

Python Robotics Course Lesson 21

Topic 6-1
Twin-Motor Robocars
===

Build a robocar and program it to drive forward, backward, and spin!

## In Topic 6...

In Topic 6 we’ll cover the various things that make object-oriented languages so special, and use them as we take a close look at how to write programs in an efficient way! We’ll start in **Topic 6-1** by making a robot car and define the **class** that we’ll use to control it! Next, in Topics 6-2 through 6-4 we’ll expand on classes by using **class inheritance** and **method overrides** to make robocars that can trace lines and be controlled remotely! This may be the first time you’ve heard the words inheritance and overrides, and we’ll be explaining each of these in order. We’ll start by reviewing **classes** in this lesson!

## In This Lesson...

We’ll take a look back at the **classes** we learned about in Lesson 8, Topic 2-3, which determine the shapes of objects with properties and methods. Then we’ll use two DC Motors to build a robocar, and define a unique class with the methods and properties we need to control it! We’ll also learn about **decorators**, which are a handy way to make functions and methods more functional without changing their content!

:::info
Lesson Introduction

■ Lesson 21 Contents

1. Classes in Review
2. All About Decorators
3. Building a Robocar
4. Challenge: Adding Features with Decorators



In this lesson, we’ll review the classes used in object-oriented programming languages.

We’ll start the lesson by making a simple example program to check how to define classes and call an object called an **instance** from a class!

Next, we’ll learn about a structure called a decorator. As the name suggests, decorators are used to decorate functions and methods and add features without you having to change them!
 
In the second half of the lesson, we’ll make a robot car and define the class that we’ll use to control it. We’ll make objects from this class to make our robocar make letters and shapes!



Finally, we’ll put everything together and use decorators to add a few methods to our robocar’s class.

Now let's start the lesson!

:::

## New Python Syntax

Let’s make a simple example program to review what we know about **classes**!

### Reviewing Classes

You can think of classes as a kind of cookie cutter. Just like one cookie cutter can decides the shape of the cookie, a class defines the shape of an object! When you make an object from a class, it will have properties and methods with the same names! The objects that we make using classes are called **instances**.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_1_E.png"/>

### Defining a Class

We use the **class** keyword to define a new class. Every indented line after the colon (`:`) in the class name is part of that class. We call the variables and functions that we make inside of a class **properties** and **methods**!
```
class MyClass: 	# This defines a class

    property = "..."	# This defines a property

    def method(self, ...): 	# This defines a method
        .
        .
```

Let’s say, for example, that we want to create a class that acts like a dog! We would write the following code to do it:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_2_E.png"/>

##### Example Code 3-2-1
```python==1
class Dog:
    voice = 'Arf!'  # This property is the dog\’s bark

    def bark(self):  # The dog barks with the sound set by the property
        print(self.voice)
```

Defining a property is just like defining a variable, but their methods aren’t exactly like a function’s methods. This is because you always have to set their first argument as **`self`**. This `self` shows the instance itself, and you can use a **.** to call properties and methods.

###### ★ You can actually use any name other than `self` in the first argument and the instance will be passed without a problem. That being said, try to keep using `self` unless you have a very good reason!

### Making and Instance from a Class

You can write the following code to make an instance:

```
instanceName = className()
```

Let’s try running code 3-2-1 for ourselves to make our `Dog` instance and call its properties and methods. `self` is called automatically as a method’s first argument, so we don’t need to set it up to be called here.

<pre class="prettyprint">
&gt;&gt;&gt; dog = Dog()
&gt;&gt;&gt; print(dog.voice)
Arf!
&gt;&gt;&gt; dog.bark()
Arf!
</pre>

### Constructors: A Special Kind of Method

A **constructor** is a method that is runs automatically when you create an instance of a class! You define this special method using the name **`_init_`**

```
class MyClass： 	# This defines a class

    def __init__(self, ...): 	# This is the constructor!
        .
        .
```

Constructors are usually used to set properties unique to an instance, or to run the methods required for their initial processes.

To give an example, let’s add a constructor which gives our dog a name to **Example Code 3-2-1**!

##### Example Code 3-4-1
###### New and Changed Code (Line 4, 5, and 8)

```python=1
class Dog:
    voice = 'Arf!'

    def __init__(self, name):  # This is the constructor!
        self.name = name  # Put the dog\’s name here
		
    def bark(self):
        print(self.name, self.voice)  # This will show what you set for each name
```

Now let’s run Example Code **3-4-1** to make two dogs named Fido and Rex and run their `bark()` methods!

<pre class="prettyprint">
&gt;&gt;&gt; fido = Dog("Fido")
&gt;&gt;&gt; rex = Dog("Rex")
&gt;&gt;&gt; fido.bark()
Fido Arf!
&gt;&gt;&gt; rex.bark()
Rex Arf!
</pre>

While the value of `voice` is a property shared across all instances, the `name` property will be different for each instance. A property like `voice` is called a **class member variable**, while ones like `name` are known as **instance member variables**!

## New Python Syntax

Let’s make a simple example program to learn about **decorators**!

### All About Decorators

A **decorator** is a kind of function. You can set a **function** in its argument and it will *return a brand new function**! This is a pretty advanced concept, but what we want to remember here is that decorators are functions with three special characteristics:

1. They can take functions as arguments
2. You can define new functions inside of them
3. Their return value is the function you set in 2!

Now let’s take a look at a real, live decorator. We’ll start by performing addition and multiplcation on multiple numbers, then make two functions which return the results.

```python=1
def addition(*args):  # This is addition
    result = 0
    for num in args:
        result += num
    return result

def multiplication(*args):  # This is multiplication
    result = 0
    for num in args:
        if(result == 0):
            result = num   # Substitutes only the first number
        else:
            result *= num
    return result
```

Let’s say that we wanted to add a feature which uses a `print` statement to show the results of these two functions. In order to do that, we need to define a decorator!

###### New Code (Line 1-5)
```python=1
def decorator(func):
    def new_func(*args):  # Defines a new function which takes func as an argument
        result = func(*args)  # Passes arguments to func before running
        print("Result:",result)  # Adds an additional process
    return new_func  # Returns the new function

def addition(*args):
    result = 0
    .
    .
    .
```

There are two ways to add decorators to a program. The first is to run the decorator function and store the return value in a variable before running  it again.

<pre class="prettyprint">
&gt;&gt;&gt; new_addition = decorator(addition)
&gt;&gt;&gt; new_addition(1, 2, 3)
Result: 6
</pre>

This would mean a lot of work for us! Another way would be to add `@decorator` directly before the name of the function. This keeps your code clean, too!

##### Example Code 4-1-1
###### New Code (Line 7, Line 14)
```python=1
def decorator(func):
    def new_func(*args):
        result = func(*args)
        print("Result:",result)
    return new_func

@decorator
def addition(*args):
    result = 0
    for num in args:
        result += num
    return result

@decorator
def multiplication(*args):
    result = 0
    for num in args:
        if(result == 0):
            result = num
        else:
            result *= num
    return result
```

Now let’s run **Example Code 4-1-1** and try calling our functions!

<pre class="prettyprint">
&gt;&gt;&gt; addition(1, 2, 3)
Result: 6
&gt;&gt;&gt; multiplication(2, 3, 4)
Result: 24
</pre>

And with that we’ve added a great way to show our results! Now you can see that decorators not only allow you to add features to a function without changing it, they also can be reused with different functions! Our example above assigns decorators to functions, but you can assign them to methods, too! **Example Code 3-4-1** below assigns the `repeat-twice` decorator to a method to run it twice!

##### Example Code 4-1-2
###### New Code (Lines 1-5,Line 13)

```python=1
def repeat_twice(func):
    def new_func(self):
        for _ in range(2):
            func(self)
    return new_func

class Dog:
    voice = 'Arf!'

    def __init__(self, name):
        self.name = name
	
    @repeat_twice
    def bark(self):
        print(self.name, self.voice)
```

##### Program Results
<pre class="prettyprint">
&gt;&gt;&gt; dog = Dog("Taro")
&gt;&gt;&gt; dog.bark()
Fido Arf!
Fido Arf!
</pre>

You can also assign multiple decorators at once. The code below adds and assigns the `repeat_three_times` decorator to run the method three more times!

##### Example Code 4-1-3
###### New Code (Lines 7-11, Line 19)

```python=1
def repeat_twice(func):
    def new_func(self):
        for _ in range(2):
            func(self)
    return new_func
    
def repeat_three_times(func):
    def new_func(self):
        for _ in range(3):
            func(self)
    return new_func

class Dog:
    voice = 'Arf!'

    def __init__(self, name):
        self.name = name
	
    @repeat_three_times
    @repeat_twice
    def bark(self):
        print(self.name, self.voice)
```

##### Program Results

<pre class="prettyprint">
&gt;&gt;&gt; dog = Dog("Taro")
&gt;&gt;&gt; dog.bark()
Fido Arf!
Fido Arf!
Fido Arf!
Fido Arf!
Fido Arf!
Fido Arf!
</pre>

## Building a Robocar

Now let’s start building our robocar. Get a copy of the building instructions for this lesson and follow the steps inside to build your robocar!

### Parts You’ll Need


* Core Unit x 1
* Robot Expansion Unit x 1
* Battery Box x 1
* DC Motor x 2
* Basic Cube (Black) x 2
* Basic Cube (Red) x 2
* Triangle (Red) x 2
* Half B (Gray) x 1
* Half B (Black) x 2
* Half B (Red) x 2
* Half C (White) x 4
* Half D (White) x 6
* Beam x 4
* Disk x 1

##### The Artec Block Shapes

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_3_E.png"/>


### Building the Robocar
Follow the link below to find instructions for building your robocar:

[Building a Robocar (Two Motors)](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_build_E.pdf)


## Programming a Robocar

Now we’re going to make a program to control the robocar you’ve just built!

### Reviewing DC Motor Methods
The `DCMotor` class you use to control DC Motors has the following methods:

##### `DCMotor` Class Methods

|method(argument)|Action|
|:---|:---|
|`__init__(pin)`|A constructor. This runs automatically at the start when you create an instance! Set the port your DC Motor is connected to as `"M1"` or `"M2"` in the `pin` argument. |
|`power(power)`|Set an output of **0** to **255** in the `power` to control the speed of your DC Motor. |
|`cw()`|Make a DC Motor spin clockwise. |
|`ccw()`|Make a DC Motor spin counterclockwise. |
|`stop()`|Cuts off power to the DC Motor port to make the DC Motor slowly stop spinning. |
|`brake()`|Applies the brakes by shorting power to the DC Motor port. |

While the `stop()` and `brake()` methods are both commands used to stop a DC Motor’s rotation, they take different amounts of time to stop as they use different ways to control the electricity inside of the motor!

Watch the video below to see how the `stop()` method makes the motor stop gradually while the `brake()` method stops it instantly!

##### `stop()` vs. `brake()`

<iframe src="https://player.vimeo.com/video/485365872" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>


### DC Motor Rotation and Robocar Directions

Take a look at the picture below to see how the direction your DC Motors rotate in affects the direction of your robocar’s wheels.

##### DC Motor and Wheel Directions

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_4_E.png"/>

This means that we can combine the rotations of our left and right DC Motors to control the direction the robocar drives in!

### DC Motor Rotation and Robocar Directions
|Action|Picture|DC Motor M1 (left wheel)|DC Motor M2 (right wheel)|
|:---|:---|:---|:---|
|Drive Forward|<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_5_E.png"/>|Counterclockwise|Counterclockwise|
|Reverse|<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_6_E.png"/>|Clockwise|Clockwise|
|Spin Left|<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_7_E.png"/>|Clockwise|Counterclockwise|
|Spin Right|<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_8_E.png"/>|Counterclockwise|Clockwise|
|Turn Left|<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_9_E.png"/>|Stop|Counterclockwise|
|Turn Right|<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_10_E.png"/>|Counterclockwise|Stop|


Spinning left or right means that your robocar will stay in one place, while turning will stop one wheel as the robocar pivots around it in a circle. **The radius of a spin is half the radius of a turn, which means that your robocar can change direction twice as fast by spinning!**

##### How Long Does it Take to Change Direction?

<iframe src="https://player.vimeo.com/video/485366477" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

### Defining the Robocar Class

We could define all of these actions as separate, but a defining them in a class with properties and methods makes managing things a lot easier. We can even use this class to add features to our robocar later in the lesson! Now we’re going to define a new class called `Robocar` and give it the following properties and methods.



##### Properties of the `Robocar` Class

|Property Name|What it Does|
|:---|:---|
|`dcm_l`|Stores an instance of the DC Motor class that lets us control the DC Motor connected to the **left wheel**. |
|`dcm_r`|Stores an instance of the DC Motor class that lets us control the DC Motor connected to the **right wheel**. |

##### Methods of the `Robocar` Class

|Method Name|What it Does|
|:---|:---|
|`__init__()`|Initialization function. Sets instances of the DC Motor class and an initial speed for the DC Motor. |
|`move_forward()`|Drive forward.|
|`move_backward()`|Reverse.|
|`spin_left()`|Spin to the left.<br/>★ The left and right wheels turn in opposite directions.|
|`spin_right()`|Spin to the right.<br/>★ The left and right wheels turn in opposite directions.|
|`turn_left()`|Turn to the left.<br/>★ The left wheel stops while the right wheel rotates.|
|`turn_right()`|Turn to the right.<br/>★ The left wheel rotates while the right wheel stops.|
|`stop()`|Coast to a stop.|
|`set_speed_to_left()`|Set the speed of the left wheel from 1 to 10.|
|`set_speed_to_right()`|Set the speed of the right wheel from 1 to 10.|



#### ■ Defining a Constructor (the `__init__()` Method)

The `_init()` method is a constructor, and we can use it to create an instance of the `DCMotor` class and store it in a property. Whenever we create a `DCMotor` class instance, we need to set the port of the DC Motor as `"M1"` or `"M2"` in its argument. Let’s do the same thing with the `_init()` method and make it accept the port of the DC Motor as an argument!

```python==1
from pyatcrobo2.parts import DCMotor

class Robocar:    # pin_l  is for the left side, pin_r is for the right side
    def __init__(self, *, pin_l, pin_r):
        self.dcm_l = DCMotor(pin_l)
        self.dcm_r = DCMotor(pin_r)
```

Defining the method as shown on line 4 as `__init__(self, *, pin_l, pin_r)` forces every argument after `*` to be a **keyword argument**. Due to this, we’ll have to write the following code to make an instance of this class.

<pre class="prettyprint">
robo = Robocar(pin_l="M1", pin_r="M2")
</pre>

Next, we’ll called the `power()` method of the `DCMotor` class instance we just made and use it to set the initial speed of our DC Motors. Let’s make this **100** for now. And remember! We have to add `self.` to the front of any property we call!

###### New Code (Line 7, Line 8)

```python==1
from pyatcrobo2.parts import DCMotor

class Robocar:
    def __init__(self, *, pin_l, pin_r):
        self.dcm_l = DCMotor(pin_l)
        self.dcm_r = DCMotor(pin_r)
        self.dcm_l.power(100)
        self.dcm_r.power(100)
```

#### ■ Defining the Forward and Reverse Methods

Next we’re going to define the methods which drive our robocar forward and backward.

|Action|Method Name|`self.dcm_l`</br>(left wheel)|`self.dcm_r`</br>(right wheel)|
|:---|:---|:---|:---|:---|
|Drive Forward|`move_forward()`|`ccw()`: Counterclockwise|`ccw()`: Counterclockwise|
|Reverse|`move_backward()`|`cw()`: Clockwise|`cw()`: Clockwise|

Check the table above as you define this method.

###### New Code (Line 10-16)

```python==1
from pyatcrobo2.parts import DCMotor

class Robocar:
    def __init__(self, *, pin_l, pin_r):
        self.dcm_l = DCMotor(pin_l)
        self.dcm_r = DCMotor(pin_r)
        self.dcm_l.power(100)
        self.dcm_r.power(100)
    
    def move_forward(self):
        self.dcm_l.ccw()
        self.dcm_r.ccw()

    def move_backward(self):
        self.dcm_l.cw()
        self.dcm_r.cw()
```

#### ■ Defining the Left and Right Spin Methods

Next we’re going to define the methods which make our robocar spin left and right.

|Action|Method Name|`self.dcm_l`</br>(left wheel)|`self.dcm_r`</br>(right wheel)|
|:---|:---|:---|:---|:---|
|Spin Left|`spin_left()`|`cw()`: Clockwise|`ccw()`: Counterclockwise|
|Spin Right|`spin_right()`|`ccw()`: Counterclockwise|`cw()`: Clockwise|

Check the table above as you define this method.

###### New Code (Line 18-24)
###### ★ Make sure everything is indented correctly!

```python==14
    def move_backward(self):
        self.dcm_l.cw()
        self.dcm_r.cw()
        
    def spin_left(self):
        self.dcm_l.cw()
        self.dcm_r.ccw()

    def spin_right(self):
        self.dcm_l.ccw()
        self.dcm_r.cw()
```

#### ■ Defining the Left and Right Turn Methods

Now let’s define the methods which make our robocar turn left and right.

|Action|Method Name|`self.dcm_l`</br>(left wheel)|`self.dcm_r`</br>(right wheel)|
|:---|:---|:---|:---|:---|
|Turn Left|`turn_left()`|`brake()`: Brake|`ccw()`: Counterclockwise|
|Turn Right|`turn_right()`|`ccw()`: Counterclockwise|`brake()`: Brake|

Check the table above as you define this method.

###### New Code (Line 26-32)

```python==22
    def spin_right(self):
        self.dcm_l.ccw()
        self.dcm_r.cw()

    def turn_left(self):
        self.dcm_l.brake()
        self.dcm_r.ccw()

    def turn_right(self):
        self.dcm_l.ccw()
        self.dcm_r.brake()
```

#### ■ Defining the Stop Methods

We’ll also need to define two methods which stop our robocar!

|Action|Method Name|`self.dcm_l`</br>(left wheel)|`self.dcm_r`</br>(right wheel)|
|:---|:---|:---|:---|:---|
|Brakes Off</br>(coast to a stop)|`stop()`|`stop()`|`stop()`|
|Brakes On</br>(stop immediately)|`brake()`|`brake()`|`brake()`|

Check the table above as you define this method.

###### New Code (Line 34-40)

```python==30
    def turn_right(self):
        self.dcm_l.ccw()
        self.dcm_r.stop()

    def stop(self):
        self.dcm_l.stop()
        self.dcm_r.stop()

    def brake(self):
        self.dcm_l.brake()
        self.dcm_r.brake()
```

#### ■ Defining the Speed Methods

Last, we’ll define the methods that us you set the speed of each wheel.

|Action|Method Name|`self.dcm_l`</br>(left wheel)|`self.dcm_r`</br>(right wheel)|
|:---|:---|:---|:---|:---|
|Set left wheel’s speed fromt</br>1 to 10|`set_speed_to_left(speed)`|`power(power)`|None|
|Set right wheel’s speed fromt</br>1 to 10|`set_speed_to_right(speed)`|None|`power(power)`|

The methods let you set 10 levels of speed from **1** to **10** in the argument to control the speed of the wheel. However, these methods use the `power()` method of the `DCMotor` class, where we we have to set an output from **0** to **255** in the `power` argument! That means we have to use the following formula to convert these numbers from inside of the method:

```
power = speed * 25
```

Take a look at the picture below to see the power output for each level of speed!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_11_E.png"/>

The `speed` argument will throw an exception if we use a number outside of that 1-10 range or that isn’t an integer, but we can prevent this from the inside of the method, too!

###### New Code (Lines 42-53)

```python==38
    def brake(self):
        self.dcm_l.brake()
        self.dcm_r.brake()

    def set_speed_to_left(self, speed):
        speed = int(speed)                   #  Converts number to an integer
        speed = 1 if speed < 1 else speed    # Sets to 1 if less than 1
        speed = 10 if speed > 10 else speed  # Sets to 10 if higher than 10
        power = speed * 25
        self.dcm_l.power(power)

    def set_speed_to_right(self, speed):
        speed = int(speed)
        speed = 1 if speed < 1 else speed
        speed = 10 if speed > 10 else speed
        power = speed * 25
        self.dcm_r.power(power)
```


#### Checking Class Definitions
Now we’ve defined the properties and methods that our class needs. Make sure your code doesn’t have any mistakes by checking it against the example code below!

##### Example Code 6-3-1
```python==1
from pyatcrobo2.parts import DCMotor

class Robocar:
    def __init__(self, *, pin_l, pin_r):
        self.dcm_l = DCMotor(pin_l)
        self.dcm_r = DCMotor(pin_r)
        self.dcm_l.power(100)
        self.dcm_r.power(100)
    
    def move_forward(self):
        self.dcm_l.ccw()
        self.dcm_r.ccw()
        
    def move_backward(self):
        self.dcm_l.cw()
        self.dcm_r.cw()
        
    def spin_left(self):
        self.dcm_l.cw()
        self.dcm_r.ccw()

    def spin_right(self):
        self.dcm_l.ccw()
        self.dcm_r.cw()

    def turn_left(self):
        self.dcm_l.brake()
        self.dcm_r.ccw()

    def turn_right(self):
        self.dcm_l.ccw()
        self.dcm_r.brake()

    def stop(self):
        self.dcm_l.stop()
        self.dcm_r.stop()

    def brake(self):
        self.dcm_l.brake()
        self.dcm_r.brake()

    def set_speed_to_left(self, speed):
        speed = int(speed)                 
        speed = 1 if speed < 1 else speed
        speed = 10 if speed > 10 else speed
        power = speed * 25
        self.dcm_l.power(power)

    def set_speed_to_right(self, speed):
        speed = int(speed)
        speed = 1 if speed < 1 else speed
        speed = 10 if speed > 10 else speed
        power = speed * 25
        self.dcm_r.power(power)
```

## Making a Class File and Using it as a Module

We’ll be using the `Robocar` class we made here in future lessons. Let’s save it to our Core Unit so we can use it as a module.

#### ■ Saving a File to the Core Unit

Let’s take the following steps to save the file we made to our Core Unit.

1. Save the file on your computer with the name `vehicle(.py)`.
2. Click the **Files** button and move the file from 1 to your Core Unit.
3. Make sure the file has transferred, then click the **Files** button again to close the window.
4. Press the **Reset** button on your Core Unit.

#### ■ Importing a Class from a Module

Click the **New** button to make a new file. Now let’s import the `Robocar` class from the `vechicle(.py)` module and make an instance of it!

```python==1
from vehicle import Robocar

robo = Robocar(pin_l="M1", pin_r="M2")
```

Let’s make a program to call the methods below in order **every 1 second** and see how our robocar works!

1. Drive forward with `move_forward()`
2. Reverse with `move_backward()`
3. Spin left with `spin_left()`
4. Spin right with `spin_right()`
5. Turn left with `turn_left()`
6. Turn right with `turn_right()`
7. Apply the brakes with `brake()`

##### Example Code 6-4-1
###### New Code (Lines 5-17)

```python==1
from vehicle import Robocar
import time

robo = Robocar(pin_l="M1", pin_r="M2")
robo.move_forward()
time.sleep_ms(1000)
robo.move_backward()
time.sleep_ms(1000)
robo.spin_left()
time.sleep_ms(1000)
robo.spin_right()
time.sleep_ms(1000)
robo.turn_left()
time.sleep_ms(1000)
robo.turn_right()
time.sleep_ms(1000)
robo.brake()
```

### Test Driving the Robocar

Once you’ve checked that your robocar is driving correctly, try thinking of how you would code your robocar to drive the following ways. Don’t worry, you can find example code at the end of this lesson!

##### Making a Reverse S

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_12_E.png"/>


##### Making a Square

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_6-1/6-1_13_E.png"/>

##### Example Code 6-5-1 - Making a Reverse S

###### ★ Depending on how much power your batteries have remaining, your a DC Motors may not run at the same speed even if you use the same numbers in your program! Try adjusting the argument in the `time.sleep_ms()` methods if your robocar is having trouble making a reverse S!

```python==1
from vehicle import Robocar
import time

robo = Robocar(pin_l="M1", pin_r="M2")
robo.turn_right()
time.sleep_ms(6000)
robo.turn_left()
time.sleep_ms(6000)
robo.brake()
```

##### Example Code 6-5-2 - Making a Square

###### ★ In order to make a square, your robocar has to turn right and drive forward three times in a row. That means you can use a `for` statement to make your program nice and short!

```python==1
from vehicle import Robocar
import time

robo = Robocar(pin_l="M1", pin_r="M2")
robo.move_forward()
time.sleep_ms(2000)
for _ in range(3):
	robo.spin_right()
	time.sleep_ms(1000)
	robo.move_forward()
	time.sleep_ms(2000)
robo.brake()
```


## Challenge: Adding Features with Decorators

In part 4 we called the `time` module's `sleep_ms()` every time we wanted to change the time for a certain action. But if we could control the time by making an argument in each method, we could make our program much cleaner!

For example, you could take the three lines below that make your robocar drive forward for two seconds and stop:
```
robo.move_forward()
time.sleep_ms(2000)
robo.brake()
```

And combine them into one line like this:

```
move_forward(2000) 
```

Controlling the time means taking the amount of time as an argument and running the `time.sleep_ms()` method and the `brake()` method from our `Robocar` class, meaning we can use it as common process for all of our methods. That means that we can use the **decorators** we learned about in part 4 and add a time control feature to the six methods below without having to change them!

* `move_forward()` method
* `move_backward()` method
* `spin_left()` method
* `spin_right()` method
* `turn_left()` method
* `turn_right()` method

### Example Program

We'll add the `time_control()` method as a decorator to the `vehicle(.py)` file which defines our `Robocar` class. You can set a method in the `func` argument for the decorator to run, and it will return a new method which can run the additional feature!

New Code (Line 2, Lines 11-17)

```python==1
from pyatcrobo2.parts import DCMotor
import time		# Note that we’re importing a new time module here!

class Robocar:
    def __init__(self, *, pin_l, pin_r):
        self.dcm_l = DCMotor(pin_l)
        self.dcm_r = DCMotor(pin_r)
        self.dcm_l.power(100)
        self.dcm_r.power(100)
        
    def time_control(func):
        def new_func(self, duration=-1):
            func(self)
            if duration > 0:
                time.sleep_ms(int(duration))
                self.brake()
        return new_func
```

You can see here that a new `def new_func(self, duration=-1)` method has been added, which has a `duration` argument with a default value of `-1`. But why make `-1` the default value? Using this value means that even if we leave out the argument on lines 14-16, the method will act normally because the `time.sleep_ms()` and `brake()` methods won't run!

Now let’s decorate every one of our methods to finish the program!

##### Example Code 7-1-1

```python==1
from pyatcrobo2.parts import DCMotor
import time

class Robocar:
    def __init__(self, *, pin_l, pin_r):
        self.dcm_l = DCMotor(pin_l)
        self.dcm_r = DCMotor(pin_r)
        self.dcm_l.power(100)
        self.dcm_r.power(100)
        
    def time_control(func):
        def new_func(self, duration=-1):
            func(self)
            if duration > 0:
                time.sleep_ms(int(duration))
                self.brake()
        return new_func
    
    @time_control
    def move_forward(self):
        self.dcm_l.ccw()
        self.dcm_r.ccw()
        
    @time_control
    def move_backward(self):
        self.dcm_l.cw()
        self.dcm_r.cw()
    
    @time_control    
    def spin_left(self):
        self.dcm_l.cw()
        self.dcm_r.ccw()

    @time_control
    def spin_right(self):
        self.dcm_l.ccw()
        self.dcm_r.cw()

    @time_control
    def turn_left(self):
        self.dcm_l.brake()
        self.dcm_r.ccw()

    @time_control
    def turn_right(self):
        self.dcm_l.ccw()
        self.dcm_r.brake()

    def stop(self):
        self.dcm_l.stop()
        self.dcm_r.stop()

    def brake(self):
        self.dcm_l.brake()
        self.dcm_r.brake()

    def set_speed_to_left(self, speed):
        speed = int(speed)                 
        speed = 1 if speed < 1 else speed
        speed = 10 if speed > 10 else speed
        power = speed * 25
        self.dcm_l.power(power)

    def set_speed_to_right(self, speed):
        speed = int(speed)
        speed = 1 if speed < 1 else speed
        speed = 10 if speed > 10 else speed
        power = speed * 25
        self.dcm_r.power(power)
```


## Conclusion
### A Quick Review

In this lesson, we reviewed how to define methods and learned about a new kind of structure called a **decorator**. In the second half of the lesson, we made a robocar and made a unique class in order to control it. 

We also used the objects we created from our class to make our car drive in the shape of a reverse S and a square! You can write every program for your robocar without having to define a unique class, but it would be much harder to use those programs in code for another robocar!

Putting processes that could come in handy elsewhere into a class makes them easily reused in other programs. It also lets you add critical features to a program without having to do a lot of work!

### Coming Up Next Time!
In the next lesson we’ll be looking at **class inheritance** and **method overrides** in detail, which are a very important part of object-oriented programming languages. We’ll also be using these critical parts to teach our robocar to follow a black line by adding a **line tracing** feature!


