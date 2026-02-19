---
tags: Python_English, Topic 2
---

Python Robotics Course Lesson 7

Topic 2-3
Classes in Python
===
Learn about classes and how they define the type of an object like a number or string!

## In This Lesson...

Back in Topic 1-4, we learned that all data in Python are objects, and each object has its own methods and properties. Abstracting data by representing it as objects helps programmers develop programs more efficiently. These abstractions are also helpful because comparing data to physical objects and their qualities can make it easier to understand how programs work.

This time we'll learn about how to make programming objects of different types, like integer (int-type) objects and string (str-type) objects. In Python, the different types objects come in are called **classes**. Some classes, like integers and strings, are always available in Python by default, but it's also possible to make new classes of your own!

At the end of the lesson, you can get some hands-on experience with classes by writing a simple program using the various special classes that control your Core Unit.

:::info
Lesson Introduction

■ Lesson 7 Contents

1. Classes and Instances
2. More About Classes
3. Importing Data from Other Files
4. Challenge: A Temperature Display



In Lesson 4, you learned about objects and their methods and properties. In this lesson you can deepen your understanding of object-oriented programming by learning about classes and instances.

We'll start by writing an example program to help you learn what classes and instances are and how they relate to each other.

Next we'll explain how to make a class on your own and what kind of features you can give it.

Learn how you can use the Core Unit's existing data features in your programs by importing them!

At the end of the lesson, you can try making a program that uses the Core Unit's Temperature Sensor to make the Core Unit display its own internal temperature on the screen!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!
:::

## Classes and Instances
Putting aside some of their more complicated functions, classes are a lot like cookie cutters. You pick a class to use to make an object, and the class determines the type of object you make, just like picking out a cookie cutter to make a cookie determines the shape of the cookie you make. The specific object you make using a class (the cookie you cut out) is called an **instance** of that class.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-3/2-3_1_E.png"/>

Every instance you make is a different object, even if they're made with the same class. The same way you can make two cookies with the same shape but then put chocolate chips on top of one and nuts on top of the other, you can put different data into two instances of the same type.

Let's try making a class of our own to get a better idea of how classes and instances work.

### Defining a Class
To make/define a new class, you need to use the keyword **class**. Just like when using **if** to make a branching program, **for** to make a loop or **def** to make a function, follow your class's name with a colon and then write the code that defines the class on indented lines below.

```
class Class Name:
	Definition of the class (properties, methods, etc.)
	.
	.
	.
```

Now let's try making a class called `Dog`.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-3/2-3_2_E.png"/>


There aren't any special rules for how to name classes, but in Python it's customary to capitalize the name of a class so you can tell it apart from instances of the class. That's why we're calling this class `Dog` instead of `dog`.

Copy the following code into your editor, and don't forget to indent!

##### Example Code 2-1-1
```python=1
class Dog:
    voice = 'Arf!' 
    
    def bark(self):
        print(self.voice)
```

Now let's break down what this code does.

Line 2 defines a property for this class. Properties are set up as variables, like `voice = 'Arf!` here.

Lines 3-4 define a method. Methods are set up using **def** statements, just like functions. Unlike functions, however, methods always need to have a certain first parameter called `self`. Once you make an instance using this class, `self` will specify that particular instance. This lets you use code like `self.Property Name` or `self.Method Name()` to use properties or other methods that belong to the same instance within the definition of a method. In this case, we're using the `voice` property to define the `bark()` method on line 5, `print(self.voice)`.

Even if you don't plan to use `self` within the definition of a method, you still need to include it as a parameter, or else you'll get errors in your code. You can call this parameter something other than `self` if you want to, but since most programmers call it `self`, it's better to call it that unless you have a particular reason to use a different name.

### Making an Instance
Now let's make an instance using our new `Dog` class. Here's how you make an instance:

```
Instance Name = Class Name ()
```

Let's make an instance called `dog`, and then run its `bark()` method. Add lines 7-8 of the code below to Example Code 2-1-1, and make sure not to indent it.

##### Example Code 2-2-1
###### New Code (Lines 7-8)
```python=1
class Dog:
    voice = 'Arf!'

    def bark(self):
        print(self.voice)
        
dog = Dog()
dog.bark()
```

To define the method `bark()` we needed to include the `self` parameter, but we don't need to use `self` just to run the method. That means if you're using a method with parameters other than `self`, you can specify the parameter that came second in the method's definition first when you run it. Running this program should make the text `Arf!` appear in your terminal.

<pre class="prettyprint">
Arf!
</pre>

Now we've gone through the general process of making a class, making an instance of that class, and using its properties and methods, but there's actually still a lot more to classes we haven't talked about! Let's have a look at a few of their other features.

### Constructors
In the last program, we made the property `voice` for the entire `Dog` class, so every dog we make says`Arf!` But what if we want to make a property that's different for every dog we make, like the dog's name? A property that's different for every instance like that is called an **instance variable**, and you need to define it differently than you would define a property like **voice** that's shared by all instances of the same class (which is called a **class variable**).

There are a number of ways you can add properties as instance variables, but if you already know what you want those properties to be, you'll generally do it using a special kind of method called a **constructor**. A constructor is a kind of initialization (set up) process that assigns values for the variables/properties included in a new instance of a class as soon as that instance is created.

You can write a constructor using the code `__init__` (short for initialize), and yes, you do need to include the two underscores before and after `init.` Now let's rewrite Example Code 2-2-1 as shown below.

##### Example Code 2-3-1
###### New and Changed Code (Lines 4-5, Line 10, Line 12)
```python=1
class Dog:
    voice = 'Arf!'
    
    def __init__(self, name):
        self.name = name
    
    def bark(self):
        print(self.voice)
        
dog = Dog('Taro')
dog.bark()
print(dog.name)
```

##### Program Results
<pre class="prettyprint">
Arf!
Taro
</pre>

Take note of line 5 inside the `__init__()` method, `self.name = name`. This code makes it so if you specify a name for a dog when you make that dog's instance, the name you give it will be assigned as the value of the instance variable `name`.

This will be the same name as `self.name` and the parameter `name`, but it will be treated as a separate object. Usually Python programmers will give instance variables and constructor parameters matching names like this, but if you find this makes things too confusing, you can use different names for the variables and parameters if you want.

By the way, class variables (like `voice`), can be used even if you haven't created a specific instance of the class! You can't do this with instance variables, so even if you can run the code `print(Dog.voice)` just fine, `print(Dog.name)` will cause an error. Try it yourself!

<pre class="prettyprint">
&gt;&gt;&gt; print(Dog.voice)
Arf!
&gt;&gt;&gt; print(Dog.name)
Traceback (most recent call last):
  File "&lt;stdin&gt;", line 1, in &lt;module&gt;
AttributeError: type object 'Dog' has no attribute 'name'
&gt;&gt;&gt; 
</pre>

###### ★ When you run code you have written in the editing area, all the classes and instances you've set up in it will then be usable in the terminal too!

### Class Methods
Just like properties/variables can be either class variables or instance variables, methods can be either **class methods** or **instance methods**. Let's see what the difference is!

First of all, to make a class method, you need to write **@classmethod** in front of your **def** statement. Any method you make without **@classmethod** in front of it will be an instance method!

```
@classmethod
def Class Method Name():
```

You can run class methods using specific instances, but code like `Class Name.Class Method Name()` can run just fine even if you haven't made any instances of that class yet. Another difference is that when you run an instance method its results only affect the specific instance you run it with, but the results of running a class method will be reflected in every instance of that class. For example, let's make a class method called `change_voice()` that changes the barking sound saved in the class variable `voice`. Just add lines 10-12 of the code below to Example Code 2-3-1.

###### New Code (Lines 10-12)
```python=1
class Dog:
    voice = 'Arf!'
    
    def __init__(self, name):
        self.name = name
    
    def bark(self):
        print(self.voice)
        
    @classmethod
    def change_voice(self, voice):
        self.voice = voice
        
dog = Dog('Taro')
dog.bark()
print(dog.name)
```

The `change_voice()` method takes a new barking sound as a parameter, and overwrites the value of the class variable `voice` with the value specified in this parameter.

We'll need to make a second instance of this class if we want to compare the results of this method with the next version we make, so let's change the code to do that now.

##### Example Code 2-4-1
###### New and Changed Code (Lines 14-21)
```python=1
class Dog:
	voice = 'Arf' 
	
	def __init__(self, name):
		self.name = name
	
	def bark(self):
		print(self.voice)
		
	@classmethod
	def change_voice(self, voice):
		self.voice = voice

dog1 = Dog('Taro')
dog2 = Dog('Jiro')

dog1.bark()
dog2.bark()
dog1.change_voice('Woof!')
dog1.bark()
dog2.bark()
```

Running this code should make the results below appear in your terminal. What this shows us is that running the `change_voice()` method for `dog1` also affects `dog2`, changing its voice.
##### Program Results
<pre class="prettyprint">
Arf!
Arf!
Woof!
Woof!
</pre>

Now just delete `@classmethod` from line 10 and run the program again to see what happens!

##### Example Code 2-4-2
###### Removed Code (Line 10)
```python=1
class Dog:
	voice = 'Arf!' 
	
	def __init__(self, name):
		self.name = name
	
	def bark(self):
		print(self.voice)
		
	def change_voice(self, voice):
		self.voice = voice

dog1 = Dog('Taro')
dog2 = Dog('Jiro')

dog1.bark()
dog2.bark()
dog1.change_voice('Woof!')
dog1.bark()
dog2.bark()
```

##### Program Results
<pre class="prettyprint">
Arf!
Arf!
Woof!
Arf!
</pre>

Now that `change_voice()` is an instance method, running it only affects the instance you called it for (`dog1`).

This difference between class methods and instance methods means that it's easy to cause errors in your code if you're not careful when using class methods, since they affect all class instances when they run. We recommend that you stick to using instance methods as much as possible and only think about using class methods if there's something you really can't do with an instance method.

## Importing Data from Other Files
Now that you know all of that, you should be able to understand what the code we used in Topic 2-2 really did!

```python=1
from pystubit.board import Image

img = Image('11111:11111:11111:11111:11111:')
```

Your Core Unit has a pre-prepared class called the **Image** (or **StuduinoBitImage**) class, which is used to make images for the LED display. Line 3 of the code creates an instance of the **Image** class, and the parameter `'11111:11111:11111:11111:11111:'` sets the parameter the Image class's constructor method `__init__()` needs.

What line 1, `from pystubit.board import Image`, does is import (bring in) the **Image** class from another file where it's been defined into our current program's file.

So, the code `from pystubit.board import Image` basically means `import the class called Image` from `pystubit.board`." You can use `from-import` statements like this to import variables and functions as well as classes.

```
from Reference File's Location import Class/Variable/Function You Want
```

If you want to import multiple things from the same file at once, just write all of them and separate them with commas.

```
from Reference File's Location import Data1,Data2,Data3,...
```

The code below, which we've used a lot of times already, imports an object that's been set up ahead of time (an instance of the **StuduinoBitDisplay** class).

```python=1
from pystubit.board import display
```

The file **pystubit.board** contains a lot of classes that we've made specifically to operate the Studuino:bit Core Unit. It also contains specific instances of those classes (except for the Image class).

|Function|Class Name|Names of Instances Included in **pystubit.board**|
|:---|:---|:---|
|Buttons|StuduinoBitButton|button_a,button_b|
|Buzzer|StuduinoBitBuzzer|buzzer|
|LED Display|StuduinoBitDisplay|display|
|Image|StuduinoBitImage|None ★ Has the shortened class name "Image" instead|
|Temperature Sensor|StuduinoBitTemperature|temperature|
|Light Sensor|StuduinoBitLightSensor|lightsensor|
|Accelerometer|StuduinoBitAccelerometer|accelerometer|
|Gyroscope|StuduinoBitGyro|gyro|
|Compass (Magnetometer)|StuduinoBitCompass|compass|


If you write an asterisk after **import** (as shown below), that will import all the data from your reference file into the current program.

```python=1
from pystubit.board import *
```

This code is very useful if you want to use a lot of different parts of the Core Unit in a single program! You can still import just the classes and instances you need to use in the program if you prefer, though.

As you write more complicated programs, your programs will inevitably get longer, making it harder to remember which part of the program does what when you look at it. For programs like this, it can be helpful to set up parts of the program in separate files and use from-import statements to bring those parts into your main program.

## Challenge: A Temperature Display
Let's write a program that uses the Core Unit's Temperature Sensor to detect the device's current internal temperature and make it scroll across the LED display.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-3/2-3_3_E.gif"/>

### Making the Program
In this program we'll start by importing a class from our reference file and then making an instance of that class instead of importing an instance directly. Here's where you can find the reference files for each class you'll need.

|Function|Class Name|Reference File's Location|
|:---|:---|:---|
|LED Display|StuduinoBitDisplay|pystubit.dsply|
| Temperature Sensor |StuduinoBitTemperature|pystubit.sensor|

You can use the code below to import each of these classes.

```python=1
from pystubit.dsply import StuduinoBitDisplay
from pystubit.sensor import StuduinoBitTemperature
```

Next let's make an instance of each class. Neither of these classes needs you to set any parameters when you make an instance, so the code for that looks like this:

###### New Code (Lines 4-5)
```python=1
from pystubit.dsply import StuduinoBitDisplay
from pystubit.sensor import StuduinoBitTemperature

display = StuduinoBitDisplay()
temperature = StuduinoBitTemperature()
```

Now you just need to get the current temperature from the Temperature Sensor and make it scroll across the LED display. To find the temperature, you can use the `StuduinoBitTemperature` class's `get_celsius()` method. This method finds the temperature in units of degrees celsius (℃), and its return values are integers. You'll want to make this scroll across the display using the `StuduinoBitDisplay` class's `scroll()` method, but you'll need to turn the integer you get from `get_celsius()` into a string with `str()` first so the `scroll()` method can use it.

##### Example Code 3-1-1
###### New Code (Lines 7-8)
```python=1
from pystubit.dsply import StuduinoBitDisplay
from pystubit.sensor import StuduinoBitTemperature

display = StuduinoBitDisplay()
temperature = StuduinoBitTemperature()

value = temperature.get_celsius()
display.scroll(str(value))
```

### Running the Program

Once you've finished Example Code 3-1-1, try running it to see how it works!

This program should make a number with up to two decimal places that represents a temperature in degrees celsius scroll across your LED display. Remember though, this is the Core Unit's internal temperature, not the temperature of the room it's in!

## Conclusion
### A Quick Review
In this lesson you learned about **classes** and the role they play in making objects. Classes actually do a bunch of other things in object-oriented programming that we didn't learn about in this lesson, though! We'll talk about more of these in detail after you're done with the introductory part of the course. For now, just make sure you understand the features of classes we've introduced so far!

### Coming Up Next Time!
Congratulations! With this lesson, you've now completed your introduction to the basics of Python programming! In the next lesson you can start using these basics to take on some practical programming problems as you learn more about how to operate the Core Unit's LED display!