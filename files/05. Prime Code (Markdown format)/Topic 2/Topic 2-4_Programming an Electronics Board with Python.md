---
tags: Python_English, Topic 2
---

Python Robotics Course Lesson 8

Topic 2-4
Programming an Electronics Board with Python
===

Program the Core Unit's LED display with Python!

## In This Lesson...
Now that you've learned the fundamentals of Python, you can start applying them to practical programming challenges by writing programs for your Core Unit's LED display! Learn about the methods for each of the Core Unit's classes so you can use them as tools to complete your own programming projects! We've also included some advanced challenges for you to test your skills. See if you can solve them without looking at the example code!

:::info
Lesson Introduction

■ Lesson 8 Contents

1. Example Project ①: Displaying Images
2. Example Project ②: Animated Images
3. Example Project ③: Electronic ABCs
4. Example Project ④: Your Own Light Show



In this lesson, you'll use the Python programming techniques you've learned in Lessons 1-7 to write a bunch of different programs for the LED display.

The first example project makes basic image patterns for the LED display by controlling LEDs one at a time.

The second example project creates simple animations by making the display switch back and forth between different images.

The third example project uses the two display control methods you've learned to alternate between showing words and letters, making a program that teaches the ABCs!

The last example project makes the whole LED display gradually change color like a light show!

By making these example projects, you'll learn about many new things you can do with the Core Unit!
If you finish all the example projects, try coming up with your own project and build that too!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!
:::

## Example Project ①: Displaying Images
The LED display can display not only strings, but also simple 5 x 5 pixel images. Let's practice by writing a program that displays the smiley face shown below.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_1_E.png"/>

### Writing the Program
First you'll need to make an image using the **StuduinoBitImage** class. This class's constructor (its `__init__()` method) takes strings that describe LED patterns as its parameter. These strings are written in the format shown below. Each 〇 would be replaced with either a 1 or a 0, which corresponds to a particular LED in the display. 1 means that an LED should be turned on, 0 means it should be turned off.

```
'○○○○○:○○○○○:○○○○○:○○○○○:○○○○○:'
```

The five numbers between each colon in this string correspond to one horizontal row of LEDs in the display, counting **Row 1:Row 2:Row 3:Row 4:Row 5** in order from left to right. You can code the smiley face image shown above in this format like this:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_2_E.png"/>


With this code added, your program should look like this:

##### Example Code 2-1-1
```python=1
from pystubit.board import display, Image

image = Image('00000:01010:00000:10001:01110:')
display.show(image)
```

Line 1 imports the **StuduinoBitImage** class (under its shortened name `Image`) and an instance of the **StuduinoBitDisplay** class called `display`. Line 3 creates an instance of the **StuduinoBitImage** class that codes the smiley face image pattern. Finally on line 4 the `show()` method displays the image in the LED display. Now try running this program and see how it works!

### Advanced Challenge
Try making this image of an airplane appear on the LED display!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_3_E.png"/>


#### ■ Example Program
You can write the string for the airplane image's pattern like this:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_4_E.png"/>


That means this code will make the airplane image appear on  the LED display:

##### Example Code 2-2-1
```python=1
from pystubit.board import display, Image

image = Image('00100:00100:01110:11111:01010:')
display.show(image)
```


## Example Project ②: Animated Images
Displaying two or more images in a row on the LED display lets you make simple animations! For example, repeatedly switching between the two images below makes a little animation of a walking dog.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_5_E.png"/>

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_6_E.gif"/>


Let's try writing a program that makes this animation happen!

### Writing the Program
The "full color LEDs" in the Core Unit's LED display can each produce mixtures of red, blue, and green light. Red, blue, and green are considered the primary colors of light, because  combining different levels of those three colors lets you create any color in the light spectrum.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_7_E.png"/>

You can set what color you want your LEDs to light up in your program. Color didn't matter in the programs we've written up to this point, so they just lit up in their default color of red.

You can set your LEDs' colors before or after making an image - either is fine! To set the colors after making an image, you can use the **StuduinoBitImage** class's `set_base_color()` method. This method has one parameter, which you can set using a tuple made up of three integers from 0 to 31. Each of these integers sets the brightness for one of the primary light colors (in order: red, green, blue).

```
StuduinoBitImage.set_base_color( (Red Brightness, Green Brightness, Blue Brightness) )
- Changes the color of all the lit LEDs in an image to the color specified.
```

Now let's take this information and use it to write our program! First we need to import the **StuduinoBitImage** class (under its shortened name `Image`) and the **StuduinoBitDisplay** class instance called `display`.

```python=1
from pystubit.board import display, Image
```

Next we need to make two **Image** class instances, one for each of the patterns we're using in our animation.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_8_E.png"/>


###### New Code (Lines 3-4)
```python=1
from pystubit.board import display, Image

image1 = Image('00000:00100:11001:11110:01001')
image2 = Image('00000:01000:11001:11110:00110')
```

Next, change the color of both of the images to yellow. You can make yellow light by combining equal levels of red and green light! This means the tuple `(31,31,0)` makes the color yellow when you set it as the parameter of the `set_base_color()` method.

###### New Code (Lines 6-7)
```python=1
from pystubit.board import display, Image

image1 = Image('00000:00100:11001:11110:01001')
image2 = Image('00000:01000:11001:11110:00110')

image1.set_base_color((31, 31, 0))
image2.set_base_color((31, 31, 0))
```

Now to complete the animation, you need the LED display switch repeatedly between these two images. To do this, set up an endless loop using a **while** statement, and call the `display` object's `show()` method inside the loop.

##### Example Code 3-1-1
###### New Code (Lines 9-11)
```python=1
from pystubit.board import display, Image

image1 = Image('00000:00100:11001:11110:01001')
image2 = Image('00000:01000:11001:11110:00110')

image1.set_base_color((31, 31, 0))
image2.set_base_color((31, 31, 0))

while True:
    display.show(image1)
    display.show(image2)
```

Now your program is finished! Try running it to see how it works!

#### ■ Changing Animation Speeds
The `show()` method can take a number of different parameters, the second of which lets you adjust how long an image is displayed for. So, if you want to change the speed of your animation, you can put a length of time in milliseconds in the `show()` method's second parameter to set how long each image stays on screen. If you want to make your animated dog run faster, for example, you can change your code so each image appears for 100 milliseconds (0.1 seconds), like this:

##### Example Code 3-1-2
###### Changed Code (Lines 10-11)
```python=1
from pystubit.board import display, Image

image1 = Image('00000:00100:11001:11110:01001')
image2 = Image('00000:01000:11001:11110:00110')

image1.set_base_color((31, 31, 0))
image2.set_base_color((31, 31, 0))

while True:
    display.show(image1, 100)
    display.show(image2, 100)
```

### Advanced Challenge

Try to program an animation of a rotating propeller by making these four images alternate over and over! 

###### ★ Hint: You can make aqua/cyan-colored light by combining equal levels of blue and green light! That means cyan written in a tuple is (0,31,31).

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_9_E.png"/>

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_10_E.gif"/>


#### ■ Example Program

You can write the strings for the four propeller images' patterns like this:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_11_E.png"/>

The code below creates an instance for each of these images, changes the LED display's light color, and makes the images appear in an alternating loop to create a propeller animation.

##### Example Code 3-2-1
```python=1
from pystubit.board import display, Image

image1 = Image('11100:01100:00100:00110:00111')
image2 = Image('10000:11000:11111:00011:00001')
image3 = Image('00001:00111:11111:11000:10000')
image4 = Image('00111:00110:00100:01100:11100')

image1.set_base_color((0, 31, 31))
image2.set_base_color((0, 31, 31))
image3.set_base_color((0, 31, 31))
image4.set_base_color((0, 31, 31))

while True:
    display.show(image1, 200)
    display.show(image2, 200)
    display.show(image3, 200)
    display.show(image4, 200)
```

## Example Project ③: Electronic ABCs
Combining the LED display methods let's you make a program that alternates between showing letters and words that begin with those letters. A program like this could help kids learn their ABCs!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_19_E.gif"/>

The example program we'll make displays the following letters and words in order, then repeats them in a loop.

1. Display the letter **A** for 1500 milliseconds with the **show()** method
2. Scroll the word **Apple** across the screen with the **scroll()** method
3. Wait for 1 second with **time.sleep_ms(1000)**`
4. Display the letter **B** for 1500 milliseconds with the **show()** method
5. Scroll the word **Banana** across the screen with the **scroll()** method
6. Wait for 1 second with **time.sleep_ms(1000)**`
7. Display the letter **C** for 1500 milliseconds with the **show()** method
8.  Scroll the word **Carrot** across the screen with the **scroll()** method
9.  Wait for 1 second with **time.sleep_ms(1000)**`

### Writing the Program
First we need to import  the **StuduinoBitDisplay** class instance called **display**, and the module **time**, which has a method we need to use to make the program pause.

```python=1
from pystubit.board import display
import time
```

Next let's make the letter **A** and the word **Apple** appear on the display. The `scroll()` method can take a second parameter that sets how long the scroll lasts, just like the parameter in `show()`. If you set this parameter to 50 (as shown below), the text will scroll for only 50 milliseconds, only moving one LED to the left.

###### New Code (Lines 4-5)
```python=1
from pystubit.board import display
import time

display.show('A', 1500)
display.scroll('Apple', 50)
```

Use the `time` module's `sleep_ms()` method to add one second pauses, then display **B** and **Banana** and **C** and **Carrot** in order. Finally put all of this inside an endless **while** loop!

##### Example Code 4-1-1
###### New and Changed Code (Lines 4-13)
```python=1
from pystubit.board import display
import time

while True:
    display.show('A', 1500)
    display.scroll('Apple', 50)
    time.sleep_ms(1000)
    display.show('B', 1500)
    display.scroll('Banana', 50)
    time.sleep_ms(1000)
    display.show('C', 1500)
    display.scroll('Carrot', 50)
    time.sleep_ms(1000)
```

Now your program is finished! Try running it to see how it works!

### Advanced Challenge
Modify your program so the display shows an image of the fruit or vegetable you're naming between showing the first letter and the word!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_20_E.gif"/>

You can make the images look like this:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_21_E.png"/>


#### ■ Example Program
To make the images, you'll need to import the **StuduinoBitImage** class (under its shortened name **Image**). Then make an instance for each image pattern and use the method `set_base_color()` to change their colors! Use the **StuduinoBitDisplay** class's `show()` method to display them at the right points in the program, and you're done.`

##### Example Code 4-2-1
###### New Code (Lines 4-9, Line 13, Line 17, Line 21)
```python=1
from pystubit.board import display, Image
import time

image_apple = Image('00100:01110:11111:11111:01110:')
image_apple.set_base_color((31, 0, 0))
image_banana = Image('00010:00110:00110:01100:11000:')
image_banana.set_base_color((31, 31, 0))
image_carrot = Image('01110:01110:01110:01110:00100:')
image_carrot.set_base_color((31, 10, 0))

while True:
    display.show('A', 1500)
    display.show(image_apple, 1500)
    display.scroll('Apple', 50)
    time.sleep_ms(1000)
    display.show('B', 1500)
    display.show(image_banana, 1500)
    display.scroll('Banana', 50)
    time.sleep_ms(1000)
    display.show('C', 1500)
    display.show(image_carrot, 1500)
    display.scroll('Carrot', 50)
    time.sleep_ms(1000)
```



## Example Project ④: Your Own Light Show
Each LED in the Core Unit's display lets you set its color separately! That means you can program your own light show by making the screen gradually change color!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_12_E.gif"/>

### Writing the Program

Each individual LED has specific X and Y coordinates you can use to describe its position. Just remember that the grid starts at (0,0) in the upper-lefthand corner and the y coordinate numbers get bigger as they go down!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_13_E.png"/>

To light up a specific LED, you'll need to use the method `set_pixel()` from the StuduinoBitDisplay class. Set the x-coordinate of the LED you to light up in `set_pixel()`'s first parameter, and the y-coordinate in its second parameter. Then you can set the color you want the  LED to light up in as the third parameter.

```
StuduinoBitDisplay.set_pixel(x-coordinate, y-coordinate, LED Color)
- Makes the LED at the specified x-y coordinates light up in the specified color.
```

Let's practice by writing a program that makes the LEDs at coordinates `(1, 1)` and `(4, 3)` light up red, as shown in the example image above.

##### Example Code 1-1-1
```python=1
from pystubit.board import display

display.set_pixel(1, 1, (31,0,0))
display.set_pixel(4, 3, (31,0,0))
```

#### ■ Programming a Light Show

Let's program a light show where the LEDs change color in the order red ➡ yellow ➡ green ➡ cyan ➡ blue ➡ magenta. Let's also make the colors change starting on the left side of the display and have the color spread across the display to the right.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_14_E.png"/>

Making this whole process happen by writing one line of code every time every LED changes color would get extremely tedious, so we'll do it by using a **for** statement instead. First let's write the part that makes the whole display light up red, starting with the upper leftmost LED `(0, 0)`.

#### ■ Lighting Up a Column of LEDs
First let's make the column of LEDs with the x-coordinate 0 light up. We want to keep the x-coordinate as 0 while going through each of the y-coordinates 0, 1, 2, 3, and 4 in order, so let's write the code like this:

```python=1
from pystubit.board import display
import time

display.clear()

for y in range(0, 5):
    display.set_pixel(0, y, (31, 0, 0))
    time.sleep_ms(100)
```

The code `display.clear()` is included on line 4 so any LEDs that are turned on when you start the program will be turned off. This should help while you're testing the program. Line 6, `for y in range(0, 5)`:, creates a loop that runs the code inside it once for each of the values in the range 0-4. On line 7, `display.set_pixel(0, y,(31, 0, 0))` uses the variable y as its second parameter, which makes the loop specify each of the coordinates `(0, 0)`, `(0, 1)`, `(0, 2)`, `(0, 3)`, and `(0, 4)` in order and turn on those LEDs. Line 8, `time.sleep_ms(100)`, makes the program wait for 100 milliseconds (0.1 seconds) before lighting up the next LED. Without this line, all the LEDs in the column would light up at the same time!

Now try running this program and make sure the LEDs light up in order!

#### ■ Lighting Up the Whole LED Display

Next we can add code that repeats this process for each column in the display. Just make another **for** statement where the x-coordinate goes through each value from 0 to 4 in the same way. Each time the x-coordinate changes, the code will go through all the possible y-coordinates again, gradually illuminating the whole screen!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_15_E.png"/>

###### New and Changed Code (Lines 6-9)
```python=1
from pystubit.board import display
import time

display.clear()

for x in range(0, 5):
    for y in range(0, 5):
        display.set_pixel(x, y, (31, 0, 0))
        time.sleep_ms(100)
```

Make sure that the code `for y in range(0, 5)`: on line 7 is inside the loop set up by `for x in range(0, 5)`: on line 6! This is what makes the program go through all the values of variable y together with each new value of variable `x`. Also make sure that the first parameter of `display.set_pixel(x, y, (31, 0, 0))` on line 8 is set using the x variable.

Now you have a program that lights up the whole display, LED-by-LED! Try running it to see how it works!

#### ■ Lighting Up LEDs in Different Colors

To complete the program, we need to add some code that makes the LEDs turn different colors over and over. We'll make the colors change in the order shown below by writing this sequence of colors as tuple and using it with a **for** loop. Conveniently, nothing stops tuples from storing other tuples, so we can put all the tuples that encode these colors together into a single tuple called **colors**.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_16_E.png"/>

###### New Code (Line 4)
```python=1
from pystubit.board import display
import time

colors = ((31, 0, 0), (31, 31, 0), (0, 31, 0), (0, 31, 31), (0, 0, 31), (31, 0, 31))

display.clear()

for x in range(0, 5):
    for y in range(0, 5):
        display.set_pixel(x, y, (31, 0, 0))
        time.sleep_ms(100)
```

If we make a **for** loop that retrieves each value stored in `colors` one-by-one and then runs the rest of the code with that value set as a parameter in the `set_pixel()` method, the LED display will light up in the sequence of colors we've planned. Just add/change the lines of code shown below to make it happen!

###### New and Changed Code (Lines 8-12)
```python=1
from pystubit.board import display
import time

colors = ((31, 0, 0), (31, 31, 0), (0, 31, 0), (0, 31, 31), (0, 0, 31), (31, 0, 31))

display.clear()

for color in colors:
    for x in range(0, 5):
        for y in range(0, 5):
            display.set_pixel(x, y, color)
            time.sleep_ms(100)
```

Line 8, `for color in colors`:, assigns each value stored in the tuple `colors` to the variable `color` in order. Every time the value of the `color` variable changes, that also changes the value of the third parameter of `display.set_pixel(x, y, color)` on line 11, making the loop turn the display a different color every time it runs.

Once you've tested this code to make sure it works, you can put all the code after `for color in colors`: inside a **while True**: endless loop to make your light show keep going forever!

##### Example Code 4-1-2
###### New and Changed Code (Lines 8-13)
```python=1
from pystubit.board import display
import time

colors = ((31, 0, 0), (31, 31, 0), (0, 31, 0), (0, 31, 31), (0, 0, 31), (31, 0, 31))

display.clear()

while True:
    for color in colors:
        for x in range(0, 5):
            for y in range(0, 5):
                display.set_pixel(x, y, color)
                time.sleep_ms(100)
```

Now your program is finished! Feel free to try out different values for the parameter of the `time.sleep(100)` method on line 13 to see how your light show looks at different speeds!

### Advanced Challenge
See if you can re-write the example program so the LEDs light up line-by-(horizontal) line!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_17_E.png"/>

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-4/2-4_18_E.gif"/>

#### ■ Example Program
Unlike the last program, this time we want to go through each x-coordinate before changing the y-coordinate. That means we need switch the order of the **for** loops that go through the values of the `x` and `y` variables. We also want to make a full line of LEDs (x-coordinates 0-4) light up at once, so we need to move `time.sleep_ms(100)` to a different point in the program too. With these changes made, the program should look like this:

##### Example Code 5-1-2
###### Changed Code (Lines 8-9, Line 11)
```python=1
from pystubit.board import display
import time

colors = ((31, 0, 0), (31, 31, 0), (0, 31, 0), (0, 31, 31), (0, 0, 31), (31, 0, 31))

display.clear()

while True:
    for color in colors:
        for y in range(0, 5):
            for x in range(0, 5):
                display.set_pixel(x, y, color)
            time.sleep_ms(100)
```

The key difference is that line 10 now starts the loop that goes through the y-coordinates `for y in range(0, 5):` while line 11 starts the loop that goes through the x-coordinates `for x in range(0, 5):`. Also note the indentation of line 13 `time.sleep_ms(100)` - this indentation makes the program wait for 100 milliseconds after each line of LEDs lights up.

## Conclusion
###  A Quick Review
In this lesson you used the fundamental Python programming techniques you learned in Topics 1.1-2.3 to write a bunch of different programs for the LED display. There are lots of other programming projects you could do using the Core Unit's Buzzer and sensors, too! If you go back and re-read the previous lessons after finishing this one, we think you'll be able to better understand all the programming you've learned so far! We definitely recommend you take some time at this point to go back and review them.

### Coming Up Next Time!
We've now completed the introductory part of the course! In the next lesson we'll start using the Robot Expansion Unit and learning about DC Motors, Servomotors, and three new kinds of sensors.

###### ★ The short-term version of the course ends here!