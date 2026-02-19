---
tags: Python_English, Topic 3
---

Python Robotics Course Lesson 11

Topic 3-3
The LED Display: Advanced Controls
===
Learn about more advanced methods for programming the LED display!

## In This Lesson...
Learn about special methods and operations for the StuduinoBitImage class, and write advanced programs for the LED display!
:::info
Lesson Introduction

■ Lesson 11 Contents

1. Combining Images with the + Operator
2. Shifting Images
3. Challenge: A Shifting Lightshow



In this lesson, you'll learn two new ways to control images on your LED display!

First, you'll learn how to combine images using the + operator. The + operator can be used to add numbers together, but if you use it with two instances of the StuduinoBitImage class, it can combine those two image patterns together to make a new image instance! Using this simple technique lets you make multi-colored image patterns!

Second, you'll learn how to use methods that shift images in different directions. You can use these methods to make images move across the LED display, like scrolling text!

At the end of the lesson, you can use both image combination and shifting to make a moving light show!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!
:::

## Two Ways to Program the LED Display
There are several different techniques you can use to make pictures and text appear on your LED display. Let's compare two different ways you can make the LED display light up in the pattern shown below as an example.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_1_E.png"/>

### ① Use methods from the StuduinoBitDisplay class to program each LED separately
The first technique is using the `set_pixel()` method from the StuduinoBitDisplay class to specify particular LEDs by their position and programming each one separately.

```
StuduinoBitDisplay.set_pixel(x, y, color)
- Make the LED at the specified coordinates (x,y) light up in the specified color.
```

Each LED in the Core Unit's display has specific X and Y coordinates you can use to describe its position.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_2_E.png"/>

The program below uses these coordinates to make the LED display light up in the pattern shown in the image above. The object `display`, which is imported in line 1, is an instance of the **StuduinoBitDisplay** class.

##### Example Code 2-1-1
```python=1
from pystubit.board import display

RED = (31, 0, 0)
display.set_pixel(1, 1, RED)
display.set_pixel(3, 1, RED)
display.set_pixel(0, 3, RED)
display.set_pixel(4, 3, RED)
display.set_pixel(1, 4, RED)
display.set_pixel(2, 4, RED)
display.set_pixel(3, 4, RED)
```

### ② Use patterns made with the StuduinoBitImage class to program LEDs

The second technique is to create an image pattern as an instance of the **StuduinoBitImage** class, and then use the **StuduinoBitDisplay** class's`show()` method to make that pattern appear on your display.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_3_E.png"/>

When you're using the `__init__()` method to make an instance of the **StuduinoBitImage** class, you have to describe LED patterns using a string made up of numbers separated by colons, instead of coordinates. Each 5 numbers separated by a colon make one line of LEDs. 1s stand for lit LEDs and 0s stand for unlit ones. Patterns look like this:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_4_E.png"/>

You can also set the color for an LED pattern using the format `color=(R, G, B)` in the`__init__()`method, like this:

```
StuduinoBitImage('00000:01010:10001:01110', color=(31, 0, 0))
```

Put an instance you've made like this into the **StuduinoBitDisplay** class's`show()`method to make the LED display light up according to your pattern!

```python=1
from pystubit.board import display,Image

image = Image('00000:01010:00000:10001:01110:', color=(31, 0, 0))
display.show(image)
```
###### ★ The name of the StuduinoBitImage class can be shortened to just`Image`, as it is when it's imported in line 1 of this code. That means you can create an instance by writing `image = Image(...)` instead of `image = StuduinoBitImage(...)`.

Saving a pattern like this as an image lets you reuse it whenever you want, but that's not the only thing you can do with it! In the next chapter we'll explain how you can use saved images as bases to make new images!

## Putting Images Together
The image below is a yellow and brown two-colored picture of a giraffe. We could make this image by using the`set_pixel()`method from the **StuduinoBitDisplay** class to light up each LED one by one, but that would take an awful lot of lines of code.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_5_E.png"/>

What if instead we look at this image as two separate images put together? So far we've used the + operator to perform mathematical addition and to concatenate strings. If you use the + operator with two instances of the **StuduinoBitImage** class, though, it can combine those two image patterns together to make a new image instance!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_6_E.png"/>

### Combining Images with the + Operator
Let's try using the + operator to make an instance of the giraffe image above! First you'll have to make two patterns with the **StuduinoBitImage** class, one of the yellow LEDs from the picture called `img_yellow` and the other of the brown LEDs called `img_brown`.

###### ★ image is shortened to img in these names so the variable names don't get too long!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_7_E.png"/>

```python=1
from pystubit.board import display,Image

YELLOW = (31, 31, 0)
BROWN = (4, 1, 1)
img_yellow = Image("10000:01000:00000:01010:00000",color=YELLOW)
img_brown = Image("01000:00000:01000:00100:01010",color=BROWN)
```

Then combine the two images with a + operator to make a new image called `giraffe`, and put that new image into the **StuduinoBitImage** class's `show()` method to make it appear on the display!


##### Example Code 3-1-1
###### New Code (Lines 7-8)
```python=1
from pystubit.board import display, Image

YELLOW = (31, 31, 0)
BROWN = (4, 1, 1)
img_yellow = Image("10000:01000:00000:01010:00000", color=YELLOW)
img_brown = Image("01000:00000:01000:00100:01010", color=BROWN)
img_giraffe = img_yellow + img_brown
display.show(img-giraffe)
```

Now your program is finished! Try running it to see how it works!

### Combining Colors
The giraffe image we just made was made up of two patterns with no overlapping colors in them. What happens if you combine patterns that do have overlapping parts? The colors from the two patterns will be combined!

* R (red) value in the combined image ＝ Image 1's R value ＋ Image 2's R value
* G (green) value in the combined image ＝ Image 1's G value ＋ Image 2's G value
* B (blue) value in the combined image ＝ Image 1's B value ＋ Image 2's B value

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_8_E.png"/>

This means you can combine images made up of red, green, and blue colors to make an image with yellow, cyan, magenta, and white colors, too!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_9_E.png"/>

Now try writing the program below to make an image with seven colors in it!

```python=1
from pystubit.board import display,Image

img_red = Image("11111:11111:11111:11111:11111", color=(31, 0, 0))
img_green = Image("11111:11111:11111:11111:11111", color=(0, 31, 0))
img_blue = Image("11111:11111:11111:11111:11111", color=(0, 0 ,31))

img_yellow = img_red + img_green
img_cyan = img_green + img_blue
img_magenta = img_red + img_blue
img_white = img_red + img_green + img_blue
```

Store all of the image patterns you've made in a tuple, then use a **for** statement to make the program cycle through all of them, making them appear on the LED display in order. Adding a **while** statement around the whole thing turns it into a looping light show!

##### Example Code 3-2-1
###### New Code (Line 12, Lines 14-16)
```python=1
from pystubit.board import display,Image

img_red = Image("11111:11111:11111:11111:11111", color=(31, 0, 0))
img_green = Image("11111:11111:11111:11111:11111", color=(0, 31, 0))
img_blue = Image("11111:11111:11111:11111:11111", color=(0, 0 ,31))

img_yellow = img_red + img_green
img_cyan = img_green + img_blue
img_magenta = img_red + img_blue
img_white = img_red + img_green + img_blue

imgs = (img_red, img_green, img_blue, img_yellow, img_cyan, img_magenta, img_white)

while True:
    for img in imgs:
        display.show(img)    
```

###### ★ The phrase`for a in A:` makes a looping program that takes each value stored in a tuple or list specified by`A` and runs the code in the loop once with each of those values assigned to the variable`a`. The program stops looping when it's run through all the values stored in `A`.

Now try running this program and see how it works!

### Animating with Colors
We've seen that it's fairly simple to make new images by combining multiple images with different colors in them using the + operator. Now let's try using that technique to make an animation that alternates two images of a turtle to make it look like it's swimming!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_10_E.png"/>

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_11_E.gif"/>

#### ■ Writing the Program

Part of the two images above is the same - namely, the green-colored turtle shell part! With the following code, we can make three images (the shell and two different versions of the legs), and combine them to make two different turtle images.


```python=1
from pystubit.board import display,Image

YELLOW = (31, 31, 0)
GREEN = (0, 31, 0)

img_shell = Image("00000:01110:01110:01110:00000:", color=GREEN)  #Shell
img_hands_and_legs1 = Image("00100:10001:00000:00000:10001:", color=YELLOW)  #Left turtle's legs
img_hands_and_legs2 = Image("00100:00000:10001:00000:01010:", color=YELLOW)  #Right turtle's legs

img_turtle1 = img_shell + img_hands_and_legs1 # Left image
img_turtle2 = img_shell + img_hands_and_legs2 # Right image
```

Let's call these two images `img_turtle1`and `img_turtle2`and make them alternate on the display to animate the turtle!

##### Example Code 3-3-1
###### New Code (Lines 13-15)
```python=1
from pystubit.board import display,Image

YELLOW = (31, 31, 0)
GREEN = (0, 31, 0)

img_shell = Image("00000:01110:01110:01110:00000:", color=GREEN)
img_hands_and_legs1 = Image("00100:10001:00000:00000:10001:", color=YELLOW)
img_hands_and_legs2 = Image("00100:00000:10001:00000:01010:", color=YELLOW)

img_turtle1 = img_shell + img_hands_and_legs1
img_turtle2 = img_shell + img_hands_and_legs2

while True:
    display.show(img_turtle1)
    display.show(img_turtle2)
```

Now try running this program and see how it works!


## Shifting Images

The **StuduinoBitImage** class comes with a set of methods you can use to make new images by shifting images you already have a certain number of pixels left, right, up, or down! Using these methods with **for** statements makes it easy to animate images scrolling across the LED display.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_12_E.png"/>

### Shifting Left and Right
You can use the methods `shift_left()` and `shift_right()` to shift an image to the left or right.

```
StuduinoBitImage.shift_left(n)
-Returns a new image made by shifting the original image n pixels to the left.
```

```
StuduinoBitImage.shift_right(n)
- Returns a new image made by shifting the original image n pixels to the right.
```

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_13_E.png"/>

Let's try it out by writing a program that makes images of a vertical line shifted 1 pixel to the right and left (as shown above) and puts them together into an animation of the line moving.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_14_E.gif"/>

#### ■ Writing the Program
First we'll make the image of a vertical line in the center of the LED display, called `img_line`.Use the **StuduinoBitDisplay** class's `show()` method to make your image appear on the display.

```python=1
from pystubit.board import display, Image

img_line = Image("00100:00100:00100:00100:00100:", color=(31, 0, 0))

display.show(img_line)
```

Next, use the `shift_left()` and `shift_right()` methods to make images of `img_line` shifted 1 pixel left and right appear before and after `img_line`.

##### Example Code 4-1-1
###### New Code (Line 5, Line 7)
```python=1
from pystubit.board import display, Image

img_line = Image("00100:00100:00100:00100:00100:", color=(31, 0, 0))

display.show(img_line.shift_left(1))  #Image shifted 1 pixel left
display.show(img_line)
display.show(img_line.shift_right(1))  #Image shifted 1 pixel right
```

This makes an animation of the line moving back and forth! Now run the program and see how it works!

You can also use negative numbers to set the parameters for the `shift_left()` and `shift_right()` methods. This makes the image shift in the opposite direction, meaning `shift_left(-1)` and `shift_right(1)` will create the same image as their result. Try rewriting Example Code 4-1-1 using negative parameters and see if you can get the same results!

##### Example Code 4-1-2
###### Changed Code (Line 5, Line 7)

```python=1
from pystubit.board import display, Image

img_line = Image("00100:00100:00100:00100:00100:", color=(31, 0, 0))

display.show(img_line.shift_right(-1))  #Same result as img_line.shift_left(1)
display.show(img_line)
display.show(img_line.shift_left(-1))  #Same result as img_line.shift_right(1)
```

### Shifting Up and Down
You can use the methods `shift_up()` and `shift_down()` to shift an image up or down.

```
StuduinoBitImage.shift_up(n)
- Returns a new image made by shifting the original image n pixels up.
```

```
StuduinoBitImage.shift_down(n)
- Returns a new image made by shifting the original image n pixels down.
```

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_15_E.png"/>

Let's try it out by writing a program that makes images of a horizontal line shifted 1 pixel up and down (as shown above) and puts them together into an animation of the line moving.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_16_E.gif"/>

#### ■ Writing the Program

First we'll make the image of a horizontal line in the center of the LED display, called `img_line`. Use the **StuduinoBitDisplay** class's `show()` method to make your image appear on the display.

```python=1
from pystubit.board import display,Image
import time

img_line = Image("00000:00000:11111:00000:00000:", color=(31, 0, 0))

display.show(img_line)
```

Next, use the `shift_up()` and `shift_down()` methods to make images of `img_line` shifted 1 pixel up and down appear before and after `img_line`.

##### Example Code 4-2-1
###### New Code (Line 5, Line 7)
```python=1
from pystubit.board import display,Image
import time

img_line = Image("00000:00000:11111:00000:00000:", color=(31, 0, 0))

display.show(img_line.shift_up(1))
display.show(img_line)
display.show(img_line.shift_down(1))
```

This makes an animation of the line moving up and down! Now run the program and see how it works!

You can use negative numbers to set the parameters for these methods, too. Just like the left/right methods, **shift_up(-1)** and **shift_down(1)** create the same image as their results. Try rewriting Example Code 4-2-1 using negative parameters and see if you can get the same results!

##### Example Code 4-2-2
###### Changed Code (Line 5, Line 7)
```python=1
from pystubit.board import display,Image
import time

img_line = Image("00000:00000:11111:00000:00000:", color=(31, 0, 0))

display.show(img_line.shift_down(-1))
display.show(img_line)
display.show(img_line.shift_up(-1))
```

### Animating with Shift Methods
You can use these shift methods you've been learning to animate text scrolling across the LED display, like the **StuduinoBitDisplay** class's `scroll()` method does. For example, you can try using the code below to animate the letter A scrolling repeatedly across the display.

```python=1
from pystubit.board import display

while True:
    display.scroll('A')
```

#### ■ Writing the Program
First, make an image of the letter A you want to make scroll.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_17_E.png"/>

```python=1
from pystubit.board import display, Image

img_A = Image("01100:10010:11110:10010:10010:", color=(31, 0, 0))
```

Next, we need to know how far the image would have to shift from its original position in each direction so it goes off the side of the display. It needs to shift 4 pixels to the left, or 5 pixels to the right. Now to simplify this program, we can use the fact that `shift_left(-n)` and `shift_right(n)` have the same results to write the whole thing using only the `shift_left()` method.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_18_E.png"/>
###### ★ shift_left(0) creates an image identical to the original image.

Use a **for** statement and the built-in function `range()` to make images shifted from right to left one column at a time, and use the **StuduinoBitDisplay** class's `show()` method to make it appear on the display. Using negative numbers in the `range()` function's parameters lets us use a range from -5 to 4 in the code below.

###### Changed Code (Lines 5-7)
```python=1
from pystubit.board import display, Image

img_A = Image("01100:10010:11110:10010:10010:", color=(31, 0, 0))

while True:
    for n in range(-5, 5):
        display.show(img_A.shift_left(n))
```
###### ★ Take note: the code range(-5, 5) makes a series of numbers that does not include 5!

Try running your finished program to see if it gives you the same results as using the `scroll()` method!

## Challenge: A Shifting Lightshow
Now try using what you've learned so far to make a light show with two shifting colored lines!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_19_E.gif"/>

### Making the Program
Make images of one vertical and one horizontal line and use the `shift_right()` and `shift_down()` methods to make shifted versions of them. Use the ＋ operator to put your lines together and make an animation like the one above!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_3-3/3-3_20_E.png"/>

First make images of a red, horizontal line and a blue, vertical line, then combine them and make them appear on the display.


```python=1
from pystubit.board import display, Image

img_horizontal_line = Image("11111:00000:00000:00000:00000:", color=(31, 0, 0))
img_vertical_line = Image("10000:10000:10000:10000:10000:", color=(0, 0, 31))

img = img_vertical_line + img_horizontal_line
display.show(img)
```

Next use a **for** statement to make your horizontal line shift down and your vertical line shift right, one pixel at a time. Combine those images and have them appear in the display! When you're done, run the program and see how it works!

###### New and Changed Code (Lines 6-8)
```python==1
from pystubit.board import display, Image

img_horizontal_line = Image("11111:00000:00000:00000:00000:", color=(31, 0, 0))
img_vertical_line = Image("10000:10000:10000:10000:10000:", color=(0, 0, 31))

for n in range(5):
    img = img_vertical_line.shift_right(n) + img_horizontal_line.shift_down(n)
    display.show(img)
```

Now try making the lines move in the opposite directions. If you add a **-1** as a third parameter in `range()`, as in `range(4, -1, -1)`, it creates a sequence of numbers counting down one by one (like 4, 3, 2, 1, 0). Set this parameter and make an image!

###### New Code (Lines 9-11)
```python=1
from pystubit.board import display, Image

img_horizontal_line = Image("11111:00000:00000:00000:00000:", color=(31, 0, 0))
img_vertical_line = Image("10000:10000:10000:10000:10000:", color=(0, 0, 31))

for n in range(5):
    img = img_vertical_line.shift_right(n) + img_horizontal_line.shift_down(n)
    display.show(img)
for n in range(4, -1, -1):
    img = img_vertical_line.shift_right(n) + img_horizontal_line.shift_down(n)
    display.show(img)
```

Finally put all of this inside a **while** statement to make it into an endless animation loop! Try running it to see how it works!

##### Example Code 5-1-1
###### New and Changed Code (Lines 6-12)
```python=1
from pystubit.board import display, Image

img_horizontal_line = Image("11111:00000:00000:00000:00000:", color=(31, 0, 0))
img_vertical_line = Image("10000:10000:10000:10000:10000:", color=(0, 0, 31))

while True:
    for n in range(5):
        img = img_vertical_line.shift_right(n) + img_horizontal_line.shift_down(n)
        display.show(img)
    for n in range(4, -1, -1):
        img = img_vertical_line.shift_right(n)+img_horizontal_line.shift_down(n)
        display.show(img)
```

## Conclusion

###  A Quick Review
In this lesson you learned how to make more advanced animations for the LED display using the + operator and shift methods from the **StuduinoBitDisplay** class!

### Coming Up Next Time!
In the next lesson we'll learn more about the parameters you use when running functions and methods, and the different varieties they come in. In particular, we'll look at the parameters for the `show()` and `scroll()` methods from the **StuduinoBitDisplay** class and how to use them!