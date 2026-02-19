---
tags: Python_English, Topic 4
---

Python Robotics Course 16

Topic 4-4
An Infrared-Sensing Crocodile Game
===
Learn about random numbers so you can make a randomly moving robot!

## In This Lesson...
Learn about methods you can use with lists and how to generate random numbers! Then you can use lists and random numbers to make a roulette-style game with a robot crocodile that senses fingers in its mouth with an IR Photoreflector and randomly decides when to bite down!

<iframe src="https://player.vimeo.com/video/482871980" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

:::info
Lesson Introduction

■ Lesson 16 Contents

1. Methods for Lists
2. Lists and Generators
3. Using Randomness
4. Making a Crocodile Game
5. Challenge: Setting the Odds



In this lesson you'll learn how to use new list methods and random numbers to make a crocodile game that has a random chance to bite!!

Start the lesson by learning new methods you can use to edit data stored in lists, and how to use generators to process larger amounts of data on less powerful computers.

To make a randomly moving robot you'll also need to learn about random numbers, so we'll talk about those next! Random numbers are like dice rolls, and computer games often use them to activate random events like enemies appearing or dropping rare items.

You can then use what you've learned about lists and random numbers to make a roulette-style game with a digital lottery system to make a robot crocodile bite! Put a finger close to the IR Photoreflector in the crocodile's mouth and it might bite if it pulls the wrong lottery number!

At the end of the lesson, see if you can modify your game's program to set the chance of the crocodile biting to a constant probability!

Now let's start the lesson!

:::

## New Python Syntax
Let's start off by introducing some new Python code you'll be using for the first time in this lesson.

### Methods for Lists

Often when you're writing a program you'll want to be able to store a bunch of data together, and you've already learned some data structures you can use to do this, like **dictionaries** and **lists**. Python has a number of methods for using data from lists and dictionaries available already. Let's have a look at them!

#### ■ Adding New Items

There are three different methods you can use to add items to a list.

* `.append(x)`: Adds one item (x) to the end of a list.
* `.insert(i, x)`: Adds an item (i) at a specified position (x) in a list.
* `.extend(xs)`: Attaches the list (xs) to the end of another list.

##### ■ The `.append(x)` Method
The `.append(x)` method takes one argument and adds the value you give it to the end of a list.

##### Example Code 2-1-1
```python=1
xs = []      # Makes an empty list

xs.append(1) # Adds 1 to the end of the list
print(xs)    # Displays [1]

xs.append(2) # Adds 2 to the end of the list
print(xs)    # Displays [1, 2]

xs.append(3) # Adds 3 to the end of the list
print(xs)    # Displays [1, 2, 3]
```
##### Program Results

<pre class="prettyprint">
[1]
[1, 2]
[1, 2, 3]
</pre>

##### ■ The `.insert(i, x)` Method
The `.insert(i, x)` method takes two arguments, with the first argument specifying the position in the list you want to add a value at.

##### Example Code 2-1-2

```python=1
xs = [ '_0', '_1', '_2' ]
xs.insert(0, 0)  # Add the value 0 at the 0th position in the list
print(xs)        # Displays [0, '_0', '_1', '_2'] in the terminal

xs.insert(3, 2) # Add the value 2 at the 3rd position in the list
print(xs)       # Displays [0, '_0', '_1', 2, '_2']
```

##### Program Results
<pre class="prettyprint">
[0, '_0', '_1', '_2'] 
[0, '_0', '_1', 2, '_2']
</pre>

You can see from these results that the values that used to be in the position specified are now one position after the new value you've added.

##### ■  The `.extend(xs)` Method
The `extend(xs)` method is similar to the `append(x)` method, except that it adds a whole second list to the end of your original list instead of just one item.

##### Example Code 2-1-3
```python=1
xs = [ 0, 1, 2 ]
ys = [ 3, 4, 5 ]

xs.extend(ys) # Adds ys to the end of xs
print(xs)     # Displays [0, 1, 2, 3, 4, 5]
```

##### Program Results
<pre class="prettyprint">
[0, 1, 2, 3, 4, 5]
</pre>

#### ■ Deleting Items

Now let's look at three methods you can use to delete items from lists.

* `.pop(i)`: Deletes the item at the position specified by **i**, or the last item in the list if **i** isn't specified.

* `.remove(i)`: Deletes an item with the value specified by **i** from the list (if there is no item with that value in the list, this causes an error).

* `.clear()`: Deletes all the items in a list.

##### ■ The `.pop(i)` Method
The `.pop(i)` method takes one argument, which specifies an index number. The method deletes the value stored at that index from the list.

##### Example Code 2-2-1
```python=1
xs = [ 0, 1, 2, 3, 4 ]

xs.pop()  # Deletes the last item of xs
print(xs) # Displays [0, 1, 2, 3]

xs.pop(2) # Deletes the second item of xs
print(xs) # # Displays [0, 1, 3]
```

##### Program Results
<pre class="prettyprint">
[0, 1, 2, 3]
[0, 1, 3] 
</pre>

##### ■ The `.remove(i)` Method
The `.remove(i)` method takes one argument, which specifies a value. The method searches the list for items with that value and deletes the first one in the list.

##### Example Code 2-2-2
```python=1
xs = [ 0, 1, 0, 2, 0 ]

xs.remove(0) # Finds and deletes the value 0 from xs
print(xs)    # Displays [1, 0, 2, 0]
```

##### Program Results
<pre class="prettyprint">
[1, 0, 2, 0]
</pre>

##### ■ The `.clear()` Method
The `.clear()` method deletes all the items in a list.

##### Example Code 2-2-3
```python=1
xs = [ 0, 1, 2, 3, 4 ] # No matter how long the list is...
xs.clear()             # This deletes all of it.
print(xs)              # Displays [] (an empty list)
```

##### Program Results
<pre class="prettyprint">
[]
</pre>


### Lists and Generators

When you're programming for a micro-computer like the Studuino:bit Core Unit, it's important to keep the micro-computer's **memory** limitations in mind.

You can think of a computer's memory like a desk you're doing a project on. The larger your desk, the more work you can fit on it at once, and the larger a computer's memory is, the more work the computer can do at once! More powerful computers have more memory.

Your Core Unit's memory is limited, so if, for example, you make a very large list, it won't have enough memory space available, causing an error.

##### Example Code 2-3-1
```python=1
xs = []  # Makes a list with one million items in it
for i in range(1000000):
    xs.append(i)
```

Running this program will give you the error message `MemoryError: memory allocation failed, allocating XXX bytes`.

##### Program Results
<pre class="prettyprint">
Traceback (most recent call last):
  File "&lt;stdin&gt;", line 6, in &lt;module&gt;
MemoryError: memory allocation failed, allocating 4194304 bytes
</pre>

Python can deal with memory limitations like this using a kind of code called a **generator**. When you need a value for something, generators do just enough calculating to give you that value and no more.

One common generator is the `range()` function, which you've already been using to make loops with **for** statements! Generators like `range()` produce numbers in order.

Try running this program to see how that works!

##### Example Code 2-3-2
```python=1
gs = range(1000000) # This generator can make up to one million items
print(gs[999999])   # Checks the value of the millionth item (calculating only that value!)
```

##### Program Results
<pre class="prettyprint">
999999
</pre>

As we saw in Example Code 2-3-1, the Core Unit can't process a program with an actual million-item list, but by using a generator instead, we can make this work!

The function `range()` has three different arguments you can specify, each of which has a different purpose.

* `range(N)`: Makes a generator that can generate numbers from 0 to N - 1.

* `range(start, stop)`: Makes a generator that can generate numbers from **start** to **stop** - 1.

    Example: `range(1, 10)` ⇒ Generates numbers from 1 to 9.

* `range(start, stop, step)`: Makes a generator that can generate a number every interval, or **step**, between **start** and **stop** - 1.

    Example: `range(1, 10, 2)` ⇒ Generates every 2nd number between 1 and 9 (1, 3, 5, and so on).

Once you make a generator in a program, `you can't change it!` That means generators can act something like the constants you learned about in Topic 4-1.

##### Example Code 2-3-3
```python=1
gs = range(5)
gs[4] = 0 # If you try to overwrite a generator's value like this, it doesn't work!
```

##### Program Results
<pre class="prettyprint">
Traceback (most recent call last):
  File "&lt;stdin&gt;", line 4, in &lt;module&gt;
TypeError: 'range' object doesn't support item assignment
</pre>

If you want to turn agenerator into a regular list with values you can overwrite, use the function `list()`, like this:

##### Example Code 2-3-4
```python=1
gs = range(5)
xs = list(gs)
xs[4] = 0 # xs is a regular list, so you can change its contents!
```


### Using Randomness

Sometimes when you're programming something, you'll want to use random numbers. If you're programming a digital lottery, for example, you'll need to be able to pick a random number from all the possible winning and losing numbers in your lottery.

Python has a module called **random** you can use when you need to add randomness a program. Here are a few methods from it you can use:

* `random.random()`: Generates a random **real number** between 0 and 1.

* `random.randint(a, b)`: Generates a random **integer** between **a** and **b**.

* `random.randint(a, b)`: Generates a random **real number** between **a** and **b**.

* `random.choice(xs)`: Takes a list as argument **xs**, and retrieves a random item from that list.

#### ■ The `random()` Function
`random()` is the most basic random number generating function. It generates a random floating-point decimal between 0 and 1.

##### Example Code 2-4-1
```python=1
import random

for i in range(5):
    print(random.random())
```

##### Example Program Results
###### ★ Running this program multiple times should give different results each time!
<pre class="prettyprint">
0.7111824
0.4089468
0.09752237
0.8465231
0.8841982
</pre>

#### ■ The `random.randint(a, b)` Function
The function `random.randint(a, b)` generates random numbers from within the range you set in its arguments, but it only generates integers.

##### Example Code 2-4-2
```python=1
import random

for i in range(5):
    print(random.randint(1, 10))
```

##### Example Program Results
###### ★ Running this program multiple times should give different results each time!
<pre class="prettyprint">
4
4
6
3
2
</pre>

#### ■ The `random.uniform(a, b)` Function
`random.uniform(a, b)` works the same way as `random.randint(a, b)`, but it generates real numbers/floating-point numbers instead of integers.

##### Example Code 2-4-3
```python=1
import random

for i in range(5):
    print(random.uniform(1, 10))
```
##### Example Program Results
###### ★ Running this program multiple times should give different results each time!
<pre class="prettyprint">
2.791369
1.38526
2.406701
9.532597
6.098391
</pre>

#### ■ The `random.choice(xs)` Function
You can set the **xs** argument in `random.choice(xs)` using a piece of sequence data like a string, tuple, or list, and the function will pick out one item from that sequence randomly. This is probably the easiest function to use to make a digital lottery game!

##### Example Code 2-4-4
```python=1
from random import choice

# Make a fortune-telling list with items for great, good, average, and bad fortune in it!
fortune = ['Great Fortune', 'Good Fortune', 'Average Fortune', 'Bad Fortune']

for i in range(5):
    print(choice(fortune)) # Draw a random fortune!
```

##### Example Program Results
###### ★ Running this program multiple times should give different results each time!
<pre class="prettyprint">
Bad Fortune
Average Fortune
Good Fortune
Great Fortune
Bad Fortune
</pre>

## Building a Crocodile Game
Get a copy of the building instructions for this lesson and follow the steps inside to build a robot crocodile for your crocodile game!

### Parts You'll Need
* Core Unit x 1
* Robot Expansion Unit x 1
* Battery Box x 1
* Servomotor x 1
* IR Photoreflector x 1
* Sensor Connecting Cable (3-wire, 15 cm) x 1
* Basic Cube (Black) x 1
* Triangle (Gray) x 4
* Triangle (Red) x 4
* Half A (Gray) x 2
* Half C (White) x 4
* Half D (White) x 6
* Beam x 4

##### The Artec Block Shapes
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-4/4-4_1_E.png"/>

### Crocodile Game Building Instructions
[Crocodile Game Building Instructions](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-4/4-4_build_E.pdf)

## Programming Your Crocodile Game
Your goal in this project is make a crocodile game that does the following:

1. Starts when you press the A button
2. Draws digital lots from a pool of good and bad lots
3. Pulls a lot whenever the IR Photoreflector detects something in the crocodile's mouth
4. Makes the crocodile bite when it pulls a bad lot
5. Plays a sound when it pulls a good lot

Putting the programs for all these feature together in the following order makes a full program for your game!

##### The Crocodile Game: Program Outline
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-4/4-4_2_E.png"/>

Let's write all of these features as separate functions to keep this program organized. Just go through and program each one until you have a complete crocodile game program!

### Making Digital Lots
Let's start by making a list of numbers we can use as our pool of lots. We'll pick out some numbers from this list we want to count as bad lots, and save those numbers in another list. Whenever the game senses something in the crocodile's mouth, it will pick a lot from the pool, and if that lot's number is on the list of bad lots, the crocodile will bite down! For now let's write the code that sets up the bad lot list and make it into a function.

#### ■ The Size of the Lot Pool
The probability of drawing a bad lot is determined by the number of lots you designate as "bad" compared to the total number of lots in the pool. In this game, lots will get removed from the pool/list after you draw them, so your chances of drawing a bad lot will get higher the more lots you've already drawn!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-4/4-4_3_E.png"/>

Let's pick how many total lots and bad lots we want to start with and save the numbers as variables.

```python=1
lot_num = 30　
bad_lot_num = 3 
```

###### ★ A "lot" is any kind of object you pick out to decide something randomly, like a straw, a bingo ball, or a raffle ticket. The act of "picking lots" is actually where the word "lottery" comes from!

#### ■  Picking the Bad Lots

Let's define a new function to do this part. You can use the `list()` function and the `range()` function together to make a list with the same number of items in it as the total number of lots you chose earlier. Then make an empty list for bad lots, which we'll fill with randomly chosen numbers later.

###### New Code (Lines 4-6)
```python=1
lot_num = 30
bad_lot_num = 3

def choice_bad_lot():
    lots = list(range(1,lot_num+1)) # Makes list of all the numbers from 1 to lot_num.
    bad_lots = []
```

We'll use the `choice()` function from the **random** module to randomly pick out bad lot numbers. `choice()` will pick one random number from the lots list repeatedly until it's chosen the full number of bad lots we set earlier. When a number gets picked, it gets temporarily saved in a variable before it gets added to the **bad_lots** list with the `append()` method and deleted from the **lots** list with the `remove()` method.

###### New Code (Line 1, Lines 9-13)
```python=1
import random

lot_num = 30
bad_lot_num = 3

def choice_lost_lots():
    lots = list(range(1,lot_num+1))
    bad_lots = []
    for i in range(bad_lot_num):
        num = random.choice(lots)
        bad_lots.append(num)
        lots.remove(num)
    return bad_lots
```

Now you've finished setting up your lot pools!

### Detecting Fingers and Drawing Lots

Your crocodile robot's IR Photoreflector is there to detect when you put a finger in the crocodile's mouth. Let's start programming this feature by checking what the IR Photoreflector's value is when you put your finger in front of it.

#### ■ Picking an IR Photoreflector Threshold
Add the code below to check your IR Photoreflector's values.

###### New and Changed Code (Line 1, Line 3, Line 8, Lines 10-13)
```python=1
from pyatcrobo2.parts import IRPhotoReflector
import random
import time

lot_num = 30
bad_lot_num = 3

ir = IRPhotoReflector('P0')

while True:
    value = ir.get_value()
    print(value)
    time.sleep_ms(1000)
    
def choice_bad_lot():
    lots = list(range(1,lot_num+1))
    bad_lots = []
    for i in range(bad_lot_num):
        num = random.choice(lots)
        bad_lots.append(num)
        lots.remove(num)
    return bad_lots
```

Run the program above and check what values show up in the terminal when you place your finger a short distance away from the IR Photoreflector in the crocodile's open mouth.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-4/4-4_4_E.png"/>

##### Example Terminal Results
<pre class="prettyprint">
830
882
887
897
874
</pre>

Now take the values you found and use them to pick a **threshold** value the program can use to judge whether **your finger is close to the IR Photoreflector**. Once you've picked a threshold you won't need the four lines of code shown below any more, so you can go ahead and delete them!

```python=10
while True:
    value = ir.get_value()
    print(value)
    time.sleep_ms(1000)
```

#### ■ Drawing Lots When a Finger's Detected

Save the threshold value you've picked in a variable so you can use it. Now we need to make a new function for drawing lots. This function should run as soon as the game starts. Once the game starts the `list()` and `range()` functions will generate the pool of lots, and the `choice_bad_lot()` function we set up earlier will generate a list of bad lot numbers. After that, we need to add an endless loop that checks the IR Photoreflector's values. If the IR Photoreflector's value goes over the threshold value, the program can use the **random** module's `choice()` function to pick a random number from the **lots** list and save it in a variable.

###### New Code (Line 9, Lines 20-27)
```python=1
from pyatcrobo2.parts import IRPhotoReflector
import random
import time

lot_num = 30
bad_lot_num = 3

ir = IRPhotoReflector('P0')
ir_threshold = 800 

def choice_lost_lots():
    lots = list(range(1,lot_num+1))
    bad_lots = []
    for i in range(bad_lot_num):
        num = random.choice(lots)
        bad_lots.append(num)
        lots.remove(num)
    return bad_lots

def play_game():
    lots = list(range(1,lot_num+1)) # This is a different variable from the choice_bad_lot() function's lots.
    bad_lots = choice_bad_lot()
  
    while True:
        value = ir.get_value()
        if value > ir_threshold:
            num = random.choice(lots)
```

### Making the Crocodile Bite

Next we'll make the crocodile bite down if it draws a bad lot number and play a sound to let the player know they're safe when it draws a good lot number.

#### ■ The Biting and Safe Sound Functions

For the biting function, let's make the crocodile play a low note as it repeats the motion shown below three times.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-4/4-4_5_E.png"/>

The safe sound function should play a higher note to let the player know they won't be bitten, and pause the game for one second before it can draw another lot.

###### New Code (Lines 1-2, Line 12, Lines 14-21, Lines 23-25)
```python=1
from pystubit.board import buzzer
from pyatcrobo2.parts import IRPhotoReflector, Servomotor
import random
import time

lot_num = 30
bad_lot_num = 3

ir = IRPhotoReflector('P0')
ir_threshold = 800 

servo = Servomotor('P13')

def bite(): # The biting function
    buzzer.on('C4')
    for i in range(3):
        servo.set_angle(100)
        time.sleep_ms(200)
        servo.set_angle(135)
        time.sleep_ms(200)
    buzzer.off()

def safe_sound(): # The safe sound function
    buzzer.on('C7',duration=200)
    time.sleep_ms(1000)

def choice_bad_lot():
    lots = list(range(1,lot_num+1))
    bad_lots = []
    for i in range(bad_lot_num):
        num = random.choice(lots)
        bad_lots.append(num)
        lots.remove(num)
    return bad_lots

def play_game():
    lots = list(range(1,lot_num+1)) 
    bad_lots = choice_bad_lot()
  
    while True:
        value = ir.get_value()
        if value > ir_threshold:
            num = random.choice(lots)
```

#### ■ Using the Functions
Now make the program judge whether the lot it's drawn is good or bad and run either the `bite()` or `safe_sound()` function depending on the results. After the crocodile bites, the program should exit the endless loop and end the game. When the program draws a good lot, use the `remove()` method to erase that lot's number from the **lots** list.

###### New Code (Lines 44-49)
```python=1
from pystubit.board import buzzer
from pyatcrobo2.parts import IRPhotoReflector,Servomotor
import random
import time

lot_num = 30
bad_lot_num = 3

ir = IRPhotoReflector('P0')
ir_threshold = 800 

servo = Servomotor('P13')

def bite():
    buzzer.on('C4')
    for i in range(3):
        servo.set_angle(100)
        time.sleep_ms(200)
        servo.set_angle(135)
        time.sleep_ms(200)
    buzzer.off()

def safe_sound():
    buzzer.on('C7',duration=200)
    time.sleep_ms(1000)

def choice_bad_lot():
    lots = list(range(1,lot_num+1))
    bad_lots = []
    for i in range(bad_lot_num):
        num = random.choice(lots)
        bad_lots.append(num)
        lots.remove(num)
    return bad_lots

def play_game():
    lots = list(range(1,lot_num+1)) 
    bad_lots = choice_bad_lot()
  
    while True:
        value = ir.get_value()
        if value > ir_threshold:
            num = random.choice(lots)
            if num in bad_lots:
                bite()
                break
            else:
                safe_sound()
                lots.remove(num)
```

### Putting It All Together

Finally, let's make the functions that play sound when a new game starts and reset your crocodile game to its starting state. Then you just need to add some code to run all the functions in the order we laid out in the program outline in an endless loop to complete your game!

#### ■ The Crocodile Game: Program Outline
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-4/4-4_2_E.png"/>

##### Example Code 4-5-1
###### New and Changed Code (Line 1, Lines 27-30, Lines 32-33, Lines 60-66)
```python=1
from pystubit.board import buzzer,button_a
from pyatcrobo2.parts import IRPhotoReflector,Servomotor
import random
import time

lot_num = 30
bad_lot_num = 3

ir = IRPhotoReflector('P0')
ir_threshold = 800 

servo = Servomotor('P13')

def bite():
    buzzer.on('C4')
    for i in range(3):
        servo.set_angle(100)
        time.sleep_ms(200)
        servo.set_angle(135)
        time.sleep_ms(200)
    buzzer.off()

def safe_sound():
    buzzer.on('C7',duration=200)
    time.sleep_ms(1000)
  
def start_sound(): # This function plays a sound to signal the start of the game
    buzzer.on('C6',duration=200)
    buzzer.on('D6',duration=200)
    buzzer.on('E6',duration=200)

def reset(): # This function resets the crocodile game back to its starting state
    servo.set_angle(135) # Opens the crocodile's mouth

def choice_bad_lot():
    lots = list(range(1,lot_num+1))
    bad_lots = []
    for i in range(bad_lot_num):
        num = random.choice(lots)
        bad_lots.append(num)
        lots.remove(num)
    return bad_lots

def play_game():
    lots = list(range(1,lot_num+1)) 
    bad_lots = choice_bad_lot()
  
    while True:
        value = ir.get_value()
        if value > ir_threshold:
            num = random.choice(lots)
            if num in bad_lots:
                bite()
                break
            else:
                safe_sound()
                lots.remove(num)

# The code below determines the overall flow of the program
reset()
while True:
    if button_a.was_pressed():
        start_sound()
        play_game()
        time.sleep_ms(1000)
        reset()
```

## Challenge: Setting the Odds
In the program we just made, the probability of drawing a bad lot increases with every lot you draw from the pool. Now see if you can modify the program so the probability of drawing a bad lot doesn't change as you play!

##### Hints
Think carefully about what you can change stop the game's odds from changing. Look at the image below for ideas!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_4-4/4-4_6_E.png"/>

### Example Program
In Example Code 4-5-1, line 57, `lots.remove(num)`, deleted numbers from the pool after they were drawn. That means all you need to do to stop the game's odds from changing is to remove this line from the program!

###### Removed Code (Line 57)
```python=1
from pystubit.board import buzzer,button_a
from pyatcrobo2.parts import IRPhotoReflector,Servomotor
import random
import time

lot_num = 30
bad_lot_num = 3

ir = IRPhotoReflector('P0')
ir_threshold = 800

servo = Servomotor('P13')

def bite():
    buzzer.on('C4')
    for i in range(3):
        servo.set_angle(100)
        time.sleep_ms(200)
        servo.set_angle(135)
        time.sleep_ms(200)
    buzzer.off()

def safe_sound():
    buzzer.on('C7',duration=200)
    time.sleep_ms(1000)
  
def start_sound():
    buzzer.on('C6',duration=200)
    buzzer.on('D6',duration=200)
    buzzer.on('E6',duration=200)

def reset():
    servo.set_angle(135)

def choice_bad_lot():
    lots = list(range(1,lot_num+1))
    bad_lots = []
    for i in range(bad_lot_num):
        num = random.choice(lots)
        bad_lots.append(num)
        lots.remove(num)
    return bad_lots

def play_game():
    lots = list(range(1,lot_num+1)) 
    bad_lots = choice_bad_lot()
  
    while True:
        value = ir.get_value()
        if value > ir_threshold:
            num = random.choice(lots)
            if num in bad_lots:
                bite()
                break
            else:
                safe_sound()

reset()
while True:
    if button_a.was_pressed():
        start_sound()
        play_game()
        time.sleep_ms(1000)
        reset()
```

## A Quick Review
In this lesson we learned...

* Methods for adding removing items from lists
Add items with: `append()`, `insert()`, `extend()`
Delete items with: `pop()`, `remove()`, `clear()`
* How to use the functions `list()` and `range()` together to make lists
* How generators reduce memory usage
* About the random module and its functions:
`random()`, `randint()`, `uniform()`, `choice()`

You also used what you learned about random number generator to make a crocodile game that bites based the results of a digital lottery system!

## Coming Up Next Time!
In the next lesson we'll focus on learning new Python syntax you can use to write more advanced programs!