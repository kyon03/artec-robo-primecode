---
tags: Python_English, Topic 3
---

Python Robotics Course Lesson 12

Topic 3-4
Advanced Parameters
===
Learn about three different kinds of parameters used for functions and methods!

## In This Lesson...
Learn about **positional parameters**, **keyword parameters**, and **default parameters**, and how to use these different types of parameters with methods that control the LED display.
:::info
Lesson Introduction

■ Lesson 12 Contents

1. Three Kinds of Parameters
(Positional, Keyword, and Default Parameters)
2. Parameters in StuduinoBitDisplay Methods
3. Challenge: An Electronic Music Box



In this lesson you'll learn about three different kinds of parameters used for functions and methods! As an example, we'll look in detail at the parameters used by a couple of methods used to program the LED display.

We'll start by learning the difference between positional parameters, keyword parameters, and default parameters using a simple example program.

Then we'll talk about the parameters for the **show** and **scroll** methods from the **StuduinoBitDisplay** class, what kinds of parameters they are and what they do.

At the end of the lesson, you can use the **show** method's parameters to make a program that plays an animation in time with a song!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!
:::

## Three Kinds of Parameters
We've used a lot of different methods and functions in our programs so far. Sometimes, even when using the same method, we've used different numbers of parameters and different ways of writing them.

##### Ex. The StuduinoBitDisplay Class's scroll() Method
```python=1
from pystubit.board import display 

display.scroll('hello')
display.scroll('hello', 50)
display.scroll('hello', color=(15, 15, 15))
```

The reason you can write parameters for the same method in different ways like this is because Python has three different kinds of parameters: **positional parameters**, **keyword parameters**, and **default parameters**.

### Positional Parameters
Parameters that have to be in a particular order (which is to say, in particular positions) are called **positional parameters**. To see how these positional parameters work, try writing the program below, which displays a message a set number of times based on its parameters.

##### Example Code 2-1-1
```python=1
from pystubit.board import display 

def repeat_message(msg, num):
    for i in range(num):
        display.scroll(msg)
```


The function `repeat_message()` has two parameters. The first parameter (`msg`) represents the message you want to display, and the second parameter (`num`) represents how many times the program should display that message. When you set up parameters in this format, that makes them positional parameters.

Now let's try running the program. Once you've run a program that defines a function, it becomes possible to call that function from the terminal too! Type the following code into your terminal to call the function we just made:

<pre class="prettyprint">
&gt;&gt;&gt; repeat_message('hello', 3)
</pre>

Running the code above makes the message **hello** scroll across the LED display three times. Positional parameters always have to go in the same order, so if you flip the positions of the two parameters in this code, it will cause an error in the function and the program won't work! Try calling the function with its parameters reversed to see for yourself.

<pre class="prettyprint">
&gt;&gt;&gt; repeat_message(3,"hello")
Traceback (most recent call last):
  File "&lt;stdin&gt;", line 1, in &lt;module&gt;
  File "&lt;string&gt;", line 5, in repeat_message
TypeError: unsupported types for __lt__: 'int', 'str'
</pre>

The message `TypeError:` on the last line tells you that an error related to data types has occurred in your program. The rest of the error message is saying that an error occurred when running a method called `__lt__` because this method was given int-type (integer) and str-type (string) data it didn't know what to do with.

`__lt__(a, b)` is a special method for the relational operation "a < b" which is used for comparison processes in various objects. The error we're getting here happens because this method can't compare int-type data with str-type data. If you try to compare a string and an integer using < (as in the code below) and run it in your terminal, that causes the same error!</p>

<pre class="prettyprint">
&gt;&gt;&gt; 1 &lt; 2
True
&gt;&gt;&gt; 'aa' &lt; 'ab'
True
&gt;&gt;&gt; 1 &lt; 'a'
Traceback (most recent call last):
  File "&lt;stdin&gt;", line 1, in &lt;module&gt;
TypeError: unsupported types for __lt__: 'int', 'str'
</pre>

###### ★ The function compares integers just by checking which number has a higher value, and compares strings based on which would come first alphabetically (e.g. a string starting with 'aa' comes before 'ab'). The code 1 < 'a' compares a string and an integer, which the function doesn't have a way to do, so running it gives you an error message.

### Keyword Parameters
Parameters specified with the format "parameter name = value" to make a one-to-one correspondence between the parameter and its value are called **keyword parameters**. Let's try calling the function we made to test out positional parameters in the last section, but with keyword parameters this time.

<pre class="prettyprint">
&gt;&gt;&gt; repeat_message(msg='hello', num=3)
</pre>

This makes the message **hello** scroll across the display three times the same way it did before, so it might seem like there's no reason to use keyword parameters when you can do the same thing but faster with positional parameters. However, the advantage here is that with keyword parameters, reversing the order of the parameters won't cause errors in your code. Try running the code below in your terminal to see for yourself!

<pre class="prettyprint">
&gt;&gt;&gt; repeat_message(num=3, msg='hello')
</pre>

This time, no error occurred! If you add an asterisk when defining a new function, all the parameters listed after that will be keyword parameters. Modify Example Code 2-2-1 as shown below, then transfer it to your Core Unit.

##### Example Code 2-2-1
###### Changed Code (Line 3)

```python==1
from pystubit.board import display

def repeat_message(msg, *, num):
    for i in range(num):
        display.scroll(msg)
```

Now if you leave out the parameter name **num** and only write the value, it will cause an error.

<pre class="prettyprint">
&gt;&gt;&gt; repeat_message('hello', 3)
Traceback (most recent call last):
  File "&lt;stdin&gt;", line 1, in &lt;module&gt;
TypeError: function takes 1 positional arguments but 2 were given
</pre>

### Default Parameters
Parameters that have a certain predetermined value to use if you don't specify a different value for them when you call a function/method are called **default parameters**. You can add a default parameter to a function when you define it using the format "parameter name = default value". Let's modify Example Code 2-2-1 so the `num` parameter has a default value of 1.

###### Changed Code (Line 3)
```python=1
from pystubit.board import display 

def repeat_message(msg, *, num=1):
    for i in range(num):
        display.scroll(msg)
```

Now run this program. If you call the function in the terminal now, leaving out the `num` parameter will make the message scroll just once! That's because the program is using the default value (1) as the value of `num` in its code.

<pre class="prettyprint">
&gt;&gt;&gt; repeat_message('hello')
</pre>

## Parameters in StuduinoBitDisplay Methods
If you learn how to use positional, keyword, and default parameters together well, you'll be able to create more flexible functions and methods in your programs. The methods for your Core Unit's classes can do different things when you use them with different kinds of parameters. The `show()` and `scroll()` methods from the **StuduinoBitDisplay** class in particular are designed to do a number of different things depending on how you set their parameters.

### Parameters in `show()`
The **StuduinoBitDisplay** class's **show()** method is defined like this:

```
def show(iterable, delay=400, *, wait=True, loop=False, clear=False, color=None):
```

* **iterable**: Positional Parameter
Specify either a string or a data array (i.e. a list or tuple) from a **StuduinoBitImage**-class object in this parameter to make it appear on the LED display. If you're using only one **StuduinoBitImage** object, you can put it directly into the parameter without using a list/tuple, as well.

* **delay**: Positional Parameter, Default Parameter
This parameter sets how long the image or text you're displaying should stay on screen in milliseconds.

* **wait**: Keyword Parameter, Default Parameter
If this parameter is set to **True**, it makes your program wait until the **show()** method's display is finished before continuing to the next command.

* **loop**: Keyword Parameter, Default Parameter
If this parameter is set to **True**, the specified image/text displayed by **show()** will repeat.

* **clear**: Keyword Parameter, Default Parameter
If this parameter is set to **True**, all images from the LED display will be cleared after **show()** runs.

* **color**: Keyword Parameter, Default Parameter
This parameter sets the color of the image or text displayed by **show()**.

`show()` has six parameters in total, and all of them except `iterable` are default parameters. All of the parameters that come after **wait** are also keyword parameters, so you need to use that format to set them.

Now let's talk about what the settings for each of these parameters actually do.

#### ■ Setting the `iterable` Parameter
The `iterable` parameter's value can be set using strings or instances of the **StuduinoBitImage** class. You can also set multiple **StuduinoBitImage**-class instances at once if you combine them into a list or tuple.

```python=1
from pystubit.board import display, Image

display.show('hello')

img = Image('11111:11111:11111:11111:11111')
display.show(img)

img1 = Image('10101:01010:10101:01010:10101')
img2 = Image('01010:10101:01010:10101:01010')
display.show((img1, img2))   #Sets the parameter with the tuple (img1, img2)
```

#### ■ Setting the delay Parameter: Display Speed
The default value of this parameter is `delay=400`, so setting it to `delay=100`, for example, makes the display change four times faster.

```python=1
from pystubit.board import display 

display.show('hello', delay=100)
```

#### ■ Setting the wait Parameter: Pausing the Program
The program below runs a command that makes the Buzzer play a sound after it makes the word "hello" appear on the display. With the default parameter value `wait=True`, the Buzzer only plays after the text has been displayed.

```python=1
from pystubit.board import display, buzzer
import time

display.show('hello', wait=True)
buzzer.on('C4')
time.sleep_ms(1000)
buzzer.off()
```

If you set it to `wait=False` instead, the program will run the commands that make "hello" appear and make the Buzzer play more-or-less simultaneously.

###### Changed Code (Line 4)
```python=1
from pystubit.board import display, buzzer
import time

display.show('hello', wait=False)
buzzer.on('C4')
time.sleep_ms(1000)
buzzer.off()
```

#### ■ Setting the loop Parameter: Looping Displays

Setting this parameter to `loop=True` makes the `show()` method's code repeat the same way it would if you put it inside a `while True` : statement. To turn off the display image from a method like this, press the Core Unit's reset button.

```python=1
from pystubit.board import display 

display.show("hello", loop=True)
```

#### ■ Setting the clear Parameter: Clearing the Display
With this parameter set to its default value, `clear=False`, the last image displayed by the **show()** method will stay on the LED display. If you set `clear=True` instead, the image or text displayed will be cleared after the method is done.

```python=1
from pystubit.board import display 

display.show("hello", clear=True)
```

#### ■ Setting the color Parameter: Changing Display Colors
The default value of this parameter, `color=None`, uses a special object called **None**, which in this case makes the display's color red. In the example below, the color parameter is set to `color=(31, 31, 31)` using a tuple of RGB values. This particular value changes the LED display's color to white.

```python=1
from pystubit.board import display

display.show("hello", color=(31, 31, 31))
```

### Parameters in `scroll()`
The `scroll()` method is defined like this:

```
def scroll(string, delay=150, *, wait=True, loop=False, color=None):
```


* **string**: Positional Parameter
Specify the string you want to scroll across the LED display in this parameter.
* **delay**: Positional Parameter, Default Parameter
This parameter sets how long the string you're scrolling takes to move one pixel, in milliseconds.
* **wait**: Keyword Parameter, Default Parameter
If this parameter is set to **True**, it makes your program wait until your string is finished scrolling before continuing to the next command.
* **loop**: Keyword Parameter, Default Parameter
If this parameter is set to **True**, the text scroll will repeat itself.
* **color**: Keyword Parameter, Default Parameter
This parameter sets the color of the string displayed by **scroll()**.

`scroll()`'s methods are mostly the same as `show()`'s, so we don't need to explain all of them in detail again. The main differences between `scroll()` and `show()` are that `scroll()` can only be used with strings, and that the LED display is always cleared after `scroll()` so it has no **clear** parameter.

## Challenge: An Electronic Music Box

Now try using what you've learned so far to make an animation play on the LED screen in time with a song, like an electronic music box.

##### The ABC Song
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-4/3-4_1_E.png"/>

<iframe src="https://player.vimeo.com/video/482867850" width="640" height="360" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

### Making the Program
First write a program that makes the letters ABCDEFG appear on the LED display. We want the letters to appear on screen while the Buzzer is playing the song, so you'll have to set the value of the `wait` parameter to **False**. You also need to make the `delay` parameter match the length of the notes in the song. Basic quarter-notes last 600 milliseconds, while half-notes last 1200 milliseconds. That means we want the letters A-F to appear for 600 milliseconds each, and G should appear for 1200 milliseconds. Set the parameters to `delay=600` and `delay=1200` to match this.

```python=1
from pystubit.board import display

display.show('ABCDEF', delay=600, wait=False)
display.show('G', delay=1200, wait=False, clear=True)
```

Next, write the code that plays the song. The **StuduinoBitBuzzer** class's `on` method is defined using the parameters shown below. When you set a length of time in the `duration` parameter, the buzzer stops playing after that amount of time has passed.

```
def on(sound, *, duration=-1):
```

This means we need to set **duration** to `duration=600` for the quarter-notes and `duration=1200` for the half-note! There's also going to be a slight delay between running `display.show(...)` and the letters appearing on the LED display, so add an extra 500 millisecond delay before the Buzzer starts playing the song to make the letters and notes match up perfectly!


##### Example Code 4-1-1
###### New and Changed Code (Lines 1-2, Lines 5-11, Line 13)
```python=1
from pystubit.board import display, buzzer
import time

display.show('ABCDEFG', delay=600, wait=False) 
time.sleep_ms(500)
buzzer.on('C4', duration=600)
buzzer.on('C4', duration=600)
buzzer.on('G4', duration=600)
buzzer.on('G4', duration=600)
buzzer.on('A4', duration=600)
buzzer.on('A4', duration=600)
display.show('G', delay=1200, wait=False, clear=True) 
buzzer.on('G4', duration=1200)
```

## Conclusion
### A Quick Review
In this lesson you learned about these three different kinds of parameters:

* Positional Parameters ... Parameters that have to be in a particular order.
* Keyword Parameters - Parameters specified with the format "parameter name = value".
* Default Parameters - Parameters with a certain predetermined value to use if you don't specify a different value for them when you call the function/method.

Go over these parameters types carefully so you know which kinds to use when you're defining your own functions and methods later on!

### Coming Up Next Time!
In the next lesson we'll be starting Topic 4. In Topic 4 you can start building things with blocks, sensors, and motors as well as the Buzzer and LED Display, and use all the features of ArtecRobo and your Core Unit you learned about in Topic 3 to program them!