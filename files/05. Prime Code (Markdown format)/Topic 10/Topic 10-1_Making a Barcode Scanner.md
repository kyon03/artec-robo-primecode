---
tags: Python_English, Topic 10
---

Python Robotics Course Lesson 37

Topic 10-1
Making a Barcode Scanner
===

Learn how numbers are represented on a computer!

## In This Lesson...

In this lesson you’ll learn about binary and hexadecimal, two ways to represent numbers which work great on a computer. We’ll then take what we’ve learned and use it to make a cash register which can scan simple barcodes and give you the total for products you want to buy!

:::info

Lesson Introduction

■ Lesson 37 Contents
1. Representing Numbers on a Computer (Binary, Octal, Hexadecimal)
2. Building a Barcode Scanner and Cash Register
3. Challenge: Adding Features


Topic 10 introduces a number of techniques involved in processing data on a computer.

In this lesson we’ll start by learning how numbers can be represented on a computer.
The numbers humans use usually include the digits 0 to 9, but these numbers are actually very hard for a computer to handle. This is where the binary, octal, and hexadecimal number systems come in, which are much more suitable for representing numbers on a computer!

In the second half of the lesson we’ll use blocks to make a barcode scanner and build a cash register system which can scan simple product barcodes and give you your total. We’ll have to use the binary number system we studied in the first half in order to make a program which scans the barcodes!

At the end of the lesson we’ll take on the challenge of adding new features to our cash register.

Now let's start the lesson!

:::

## Representing Numbers on a Computer

The number system that we normally use is called the **decimal** system. In decimal, **10** means **ten** and **100** means **one hundred**. This system uses **10 digits from 0 to 9 to represent every number**! One possible reason that decimal counts in units of 10 is because human beings have 10 fingers on their hands, and showing numbers this way makes it easy for us to count them!

On the other hand, decimal is incredibly hard for a computer to work with. In order to understand why this is, we need to take a closer look at what happens inside of a computer when it calculates numbers!

### How Computers Calculate

The heart of a computer is its CPU, and this is where calculations are performed. The heart of a computer is its CPU, and this is where calculations are performed. To make a simple comparison: a CPU is like a circuit made up of a huge collection of simple circuits which can be switched on or off.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_1_E.png"/>

This circuit goes into a **LOW** state to turn off when there’s a low amount of voltage applied to both ends of the LED. When there’s a high amount of voltage, the switch goes into a **HIGH** state to turn on! On a computer, these LOW and HIGH voltage states are converted into digital values of **0** and **1**. Combining circuits which perform logical operations using 0 and 1 allows you to perform calculations which are much more complex. You call this collection of circuits used for logical operations a **logic gate**!

### Logic Gates and Their Types

There are three basic types of logic gates: **AND** gates, **OR** gates, and **NOT** gates. These gates are identical to the `and`, `or`, and `not` logical operators in Python!

#### ■ AND Gates

An AND gate sets output Y to 1 (HIGH) only when **both** inputs A and B are 1 (HIGH).

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_2_E.png"/>

|Input A|Input B|Output Y|
|:---|:---|:---|
|0 (LOW)|0 (LOW)|0 (LOW)|
|1 (HIGH)|0 (LOW)|0 (LOW)|
|0 (LOW)|1 (HIGH)|0 (LOW)|
|1 (HIGH)|1 (HIGH)|1 (HIGH)|

If we were to compare an AND gate to an analog electronic circuit, if would be like the circuit in the picture below which shows two switches connected in series. This circuit will turn the LED’s value to 1 and make it light up only when both of the switches have a value of 1, meaning they’re on!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_3_E.png"/>

#### ■ OR Gates

An OR gate sets output Y to 1 (HIGH) when **either** input A or B is 1 (HIGH).

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_4_E.png"/>

|Input A|Input B|Output Y|
|:---|:---|:---|
|0 (LOW)|0 (LOW)|0 (LOW)|
|1 (HIGH)|0 (LOW)|1 (HIGH)|
|0 (LOW)|1 (HIGH)|1 (HIGH)|
|1 (HIGH)|1 (HIGH)|1 (HIGH)|

If we were to compare an OR gate to an analog electronic circuit, if would be like the circuit in the picture below which shows two switches connected in parallel. This circuit will turn the LED’s value to 1 and make it light up when either switch has a value of 1, meaning that it’s on!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_5_E.png"/>

#### ■ NOT Gates

A NOT gate takes an input and outputs the opposite state.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_6_E.png"/>

|Input A|Output Y|
|:---|:---|
|0 (LOW)|1 (HIGH)|
|1 (HIGH)|0 (LOW))|

A computer takes the three logic gates we’ve just learned about and combines them in order to calculate numbers. However, because logic gates only have two states of either 0 (LOW) or 1 (HIGH), a computer can’t calculate decimal numbers which have a value from 0 to 9 directly. This is why computers use a number system called **binary** instead of decimal!

### What is Binary?

In the world of decimal, if you calculate **10 + 10** the answer is always going to be **20**. However, we’re about to learn about binary. When you calculate **10 + 10** in binary, you get **100**! While this may look like **one hundred**, in binary this number actually means **four**! This is because all numbers in binary are represented by the digits **0** and **1**!

Calculate **10 + 10** in decimal and the numbers in the tens place would make **2**, but the number **2** doesn’t exist in binary. This means that the **1** is carried over to the next place to become **100**!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_7_E.png"/>

#### ■ Binary vs. Decimal

Look at the chart below to see which decimal number is represented by each column in binary. You’ll notice going up one place makes the number increase by two!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_8_E.png"/>

Now we’re going to take this chart and use to it see what the numbers 0 to 9 look like when you convert them to binary!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_9_E.png"/>

Now let’s compare our binary formula with the same formula converted into decimal. You’ll see that the results for each calculation are correct!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_10_E.png"/>

It’s time to practice by solving the following problems in binary and then converting the results into decimal!

##### The Problems
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_11_E.png"/>

##### The Answers
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_12_E.png"/>

#### ■ Using Binary in Python

In Python, you put `0b` in front of a number to indicate that it’s a binary number. The `b` here stands for **b**inary!

<pre class="prettyprint">
&gt;&gt;&gt; print(0b1111)
15
</pre>


From the results above, you can see that the program will handle it as a decimal value if you write it as is. If you enclose it in `””`, however, you can define it as a text string.

```python=1
binary = "0b100"
```

#### ■ Converting Binary to Decimal

If we want to interpret a text string as a binary number and convert it into a decimal value, we can use the `int()` function!

```
int(string, number indicating the number system of the string)
```

##### Example Code 2-3-1
###### New Code (Lines 2-3)
```python=1
binary = "0b100"
decimal = int(binary, 2)
print(decimal)
```

##### Program Results
<pre class="prettyprint">
4
</pre>

#### Converting from Decimal to Binary

On the other hand, we can use the `bin()` function if we want to convert a decimal value into a binary text string!

```
bin(decimal value)
```

##### Example Code 2-3-2

```python=1
decimal = 10
binary = bin(decimal)
print(binary)
```

##### Program Results
<pre class="prettyprint">
0b1010
</pre>

#### ■ Binary Addition Using Logic Gates

In order to perform addition with single-digit binary numbers, we can use the circuit below which combines the AND, OR, and NOT logic gates we’ve just learned about. This kind of logic gate is known as a **half adder**. **A** and **B** take input as a single-digit binary number, and the result of the operation is output in **s**um **S**, which is the ones place, and **c**arry **C**, which is the tens place.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_13_E.png"/>

##### Half Adder Input and Output Values
|A|B|C|S|
|:---|:---|:---|:---|
|0|0|**0**|**0**|
|0|1|**0**|**1**|
|1|0|**0**|**1**|
|1|1|**1**|**0**|

The actual calculation process when we input **1** for A and B would look like this:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_14_E.png"/>

You can see how this makes binary very convenient when a computer needs to perform calculations. However, binary values become longer the larger a number is, which makes these values very hard for humans to use!

This means that we need a number system which is easy for both humans and computers to understand. That system is called **hexadecimal**, and we’ll explain it in the next section!

### What is Hexadecimal?

Hexadecimal, or **hex**, is a system which uses the letters A-F in addition to the digits 0-9  to represent numbers, giving us a total of **16 values**! Let’s say we had a series of four-digit numbers using 0s and 1s. We can calculate the number of possible values as **2<sup>4</sup> = 16**, giving us a total of **16 possible values**. This means you can use a single digit in hexadecimal to represent up to **four digits** in binary!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_15_E.png"/>

The chart below shows what decimal numbers would look like in hexadecimal.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_16_E.png"/>

#### ■ Using Hexadecimal in Python

In Python, you put `0x` in front of a number to indicate that it’s a hexadecimal number.

<pre class="prettyprint">
&gt;&gt;&gt; print(0x1f)
31
</pre>

Now let’s define it as a text string to let our program know that this is a hexadecimal number!

```python=1
hexadecimal= "0x1f"
```


#### ■ Converting Hexadecimal to Decimal

Just like with binary, we can use the `int()` function if we want to interpret a text string as a hexadecimal number and convert it into a decimal value!

##### Example Code 2-3-3
###### New Code (Lines 2-3)
```python=1
hexadecimal= "0x1f"
decimal = int(hexadecimal, 16)  # Takes 16 in the second argument since we\’re converting to hex
print(decimal)
```

##### Program Results
<pre class="prettyprint">
31
</pre>

#### Converting from Decimal to Hexadecimal

If we want to convert a decimal number into a hexadecimal one, we can use the `hex()` function!

##### Example Code 2-3-4

```python=1
decimal = 31
hexadecimal = hex(decimal)
print(hexadecimal)
```

##### Program Results
<pre class="prettyprint">
0x1f
</pre>

In addition to hexadecimal, you’ll sometimes see the **octal** system used in place of binary!

### What is Octal?

Octal uses **eight digits** from 0 to 7 to represent every number! Since **2<sup>3</sup> = 8**, we can use octal to represent any three-digit binary number!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_17_E.png"/>

The chart below shows what decimal numbers would look like in octal.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_18_E.png"/>

#### ■ Using Octal in Python

In Python, you put `0o` in front of a number to indicate that it’s an octal number.

<pre class="prettyprint">
&gt;&gt;&gt; print(0o17)
15
</pre>

Now let’s define it as a text string to let our program know that this is an octal number!

```python=1
octal = "0o17"
```


#### ■ Converting Octal to Decimal

Just like with binary and hexadecimal, we can use the `int()` function if we want to interpret a text string as an octal number and convert it into a decimal value!

##### Example Code 2-3-5
###### New Code (Lines 2-3)
```python=1
octal = "0o17"
decimal = int(hexadecimal, 8)  # Takes 8 in the second argument since we\’re converting to octal
print(decimal)
```

##### Program Results
<pre class="prettyprint">
15
</pre>

#### ■ Converting Decimal to Octal

If we want to convert a decimal number into an octal one, we can use the `oct()` function!

##### Example Code 2-3-6

```python=1
decimal = 15
octal = oct(decimal)
print(octal)
```

##### Program Results
<pre class="prettyprint">
0o17
</pre>

### Conversion Practice with a Game

Let’s use the example game below in order to get a firmer grasp on converting back and forth between different number systems!

###### ★ Consider this game as extra practice outside of the lesson. Feel free to skip it if you’re running low on time!

##### Converting to Decimal

```python=1
import random

def start(mode="b"):  # The mode argument starts the game with the selected mode
    if mode == "b":  # Binary mode
        base = 2  # Binary
        _max = 15  # Maximum range of numbers (in decimal)
    elif mode == "o": # Octal mode
        base = 8  # Octal
        _max = 63  # Maximum range of numbers (in decimal)
    elif mode == "h":  # Hex mode
        base = 16  # Hexadecimal
        _max = 255  # Maximum range of numbers (in decimal)
    else:
        print("Please select mode 'b', 'o', or 'h'.")
        return
    
    score = 0  # The score
    for _ in range(5):  # Gives 5 questions in a row
        num = random.randint(0, _max)  # Picks a random decimal number from the maximum range
        if mode == "b":
            print("Binary:", bin(num))  # Converts number into binary and shows it
        elif mode == "o":
            print("Octal:", oct(num))  # Converts number into octal and shows it
        else:
            print("Hexadecimal:", hex(num))  # Converts number into hexadecimal and shows it
        answer = input("What is this number in decimal? >>>")  # Asks user to type the answer
        if num == int(answer):  # If the answer is correct
            print("Correct!")
            score += 1
        else:  # If the answer is incorrect
            print("Incorrect!")  
            print("The correct answer is ", num)
    print("Result:", str(score), "/5")  # Shows the score
```

##### Converting from Decimal

```python=1
import random

def start(mode="b"):  # The mode argument starts the game with the selected mode
    if mode == "b":  # Binary mode
        base = 2  # Binary
        message = "Binary >>>"  # Shows prompt asking user for the answer
        _max = 15  # Maximum range of numbers (in decimal)
    elif mode == "o":  # Octal mode
        base = 8  # Octal
        message = "Octal >>>"
        _max = 63
    elif mode == "h":  # Hex mode
        base = 16  # Hexadecimal
        message = "Hexadecimal >>>"
        _max = 255
    else:
        print("Please select mode 'b', 'o', or 'h'.")
        return
    
    score = 0  # The score
    for _ in range(5):  # Gives 5 questions in a row
        num = random.randint(0, _max)  # Picks a random decimal number from the maximum range
        print("Decimal:", num)  # Shows the problem
        answer = input(message)  # Asks the user to type the answer
        answer = int(answer, base)  # Converts number into decimal for the selected mode
        if num == answer:  # If the answer is correct
            print("Correct!")
            score += 1
        else:  # If the answer is incorrect
            print("Incorrect!")
            if mode == "b":
                print(“The correct answer is:", bin(num))
            elif mode == "o":
                print("The correct answer is:", oct(num))
            else:
                print("The correct answer is:", hex(num))
    print("Result:", str(score), "/5")  # Shows the score
```


## Building a Barcode Scanner

In the second half of the lesson, we’re going to apply what we’ve learned about binary to make a cash register which can scan simple barcodes! Follow the instructions below as you use blocks to build the register’s Barcode Scanner:

##### Building a Barcode Scanner
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_19_E.png"/>

### Parts You'll Need

##### Parts
* Core Unit x 1
* Robot Expansion Unit x 1
* Battery Box x 1
* Servomotor x 1
* IR Photoreflector x 1
* Sensor Connecting Cable (3-wire, 30cm) x 1
* Basic Cube (Gray) x 6
* Basic Cube (Red) x 2
* Half A (Gray) x 2
* Half B (Gray) x 1
* Half B (Black) x 1
* Half C (White) x 7
* Half D (White) x 8
* Beam x 4
* Gear (L) x 1
* Rack ×1

##### The Artec Block Shapes

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_20_E.png"/>

### Building a Barcode Scanner

Follow the link below to find instructions for building the scanner:

[Building a Barcode Scanner](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_build_E.pdf)

## Programming the Cash Register

Before we start programming, let’s take a look at how our Barcode Scanner scans barcodes.

### Scanning Barcodes

Look at any product you’ll find when out shopping at the convenience store or supermarket. You’ll see that it has a barcode printed on the package!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_21_E.png"/>

Below the barcode, you’ll find a series of numbers. These numbers are requested by the manufacturer, and they’re a unique identifier for that product and that product only! You can use this one number to manage the price, stock, sales figures, and lots of other data for the product.

Sensors weren’t quite good enough to scan numbers when barcodes were first developed, which is why they use a system of white spaces and bars instead. The Barcode Scanner we’ve just built will have trouble scanning the barcodes found on actual products, which is why we’ve prepared simple barcodes for you to use!

##### The Barcode Structure

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_22_E.png"/>

The barcodes we’ll use in this lesson represent four-digit binary numbers. Each one has four blocks. A black block represents the number **0** while a white block represents the number **1**!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_23_E.png"/>

The Barcode Scanner scans these four-digit binary numbers before converting them into decimal numbers from 0 to 15 that we can use!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_24_E.png"/>

While the barcodes you normally see are printed on paper or the product package itself, we’ll use blocks to make the barcodes in this lesson!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_25_E.png"/>

These four-block barcodes will be set in the center of the Barcode Scanner. The Servomotor will move the IR Photoreflector starting at the top column and scanning left to right.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_26_E.png"/>

### The Cash Register Processes

Your average cash register uses the number, or **EAN code**, on a product to manage multiple pieces of data associated with that product, like its name and price. The barcode scanner connected to the machine will scan the product barcode and the system will pull up the information automatically and show it on the screen!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_27_E.png"/>

We’ll register a dictionary containing product data in advance to use with our register. The scanner will scan the barcode and use the number to look up the name and price of the product!

##### Product Number, Name, and Price Data

|Product Number|Name|Price (Cents)|
|---:|:---|:---|
|1|Carrots (Carrots)|300|
|2|Onions (Onions)|200|
|3|Tofu (Tofu)|100|
|4|Eggs (Eggs)|230|
|5|Milk (Milk)|240|
|6|Apple Juice (Apple juice)|320|
|7|Sausage (Sausage)|430|
|8|Bacon (Bacon)|380|
|9|Potato Chips (Potato chips)|200|
|10|Ice Cream (Ice cream)|150|
|11|Chocolate (Chocolate)|350|
|12|Sugar (Sugar)|250|
|13|Soy Sauce (Soy sauce)|500|
|14|Rice (Rice)|2000|
|15|Tissues (Tissues)|400|

### Making the Program

We’ll start by making two classes inside of our program which act as the Barcode Scanner and cash register.

|Class|What does it do?|
|:---|:---|
|`BarcodeScanner`|The class for the Barcode Scanner. Uses the IR Photoreflector to scan the barcode and passes the scanned number to the cash register. |
|`Register`|The class for the cash register. Uses the number from the Barcode Scanner to search for the product before showing its name, price, and the total for the purchase so far. |

We’ll need to define the following properties and methods for each class.

##### BarcodeScanner Class Properties and Methods

|Property or Method|What is it?|
|:---|:---|
|`POSITIONS` Property|Servomotor angle for scanning each of the blocks of the barcode
|`THRESHOLD` Property|IR Photoreflector threshold to tell the difference between white and black blocks|
|`irp` Property|Object used to control the IR Photoreflector|
|`servo` Property|Object used to control the Servomotor|
|`__Init__()` Method|This initialization process runs when creating an instance of the class|
|`scan_barcode()` Method|Scans the barcode and returns its number|

##### Register Class Properties and Methods

|Property or Method|What is it?|
|:---|:---|
|`PRODUCT_DATA` Property|Dictionary containing product data|
|`Scanner` Property|`BarcodeScanner` class instance|
|`Total` Property|Total purchase of scanned products|
|`__Init__()` Method|This initialization process runs when creating an instance of the class|
|`Read()` Method|Shows the product name and price for the scanned code and adds the price to the total in `total`|
|`Show_total()` Method|Shows the total for the purchase stored in `total`|

We’ll use the A and B buttons on the Core Unit to control the register as follows:

* **Press A:** Scan barcode to show product name and price
* **Press B:** Show the total purchase amount for products which have been scanned so far

We’ll run the code for each button’s process as well as our `BarcodeScanner` and `Register` class instance functions inside of the `main()` function and run them.

Now let’s define each class and function in order!

### Defining the BarcodeScanner Class

We’ll follow the steps below to define the `BarcodeScanner` class.

#### ■ Setting Servomotor Angles to Scan the Barcode

Try running the example code below and find out which angles you’ll need your Servomotor to turn to in order to scan each bar of the barcode.

##### Example Code 4-4-1

```python=1
from pystubit.board import button_a, button_b
from pyatcrobo2.parts import Servomotor

servo = Servomotor("P15")
angle = 90
servo.set_angle(angle)

while True:
    # Turns the IR Photoreflector incrementally to the left each time you press the A button
    if button_a.was_pressed() and angle > 0:
        angle -= 1
        servo.set_angle(angle)
        print(angle)
    # Turns the IR Photoreflector incrementally to the right each time you press the B button
    elif button_b.was_pressed() and angle < 180:
        angle += 1
        servo.set_angle(angle)
        print(angle)
```

While this example code is running, the IR Photoreflector will move to the left when you press A and move to the right when you press B. Once the Servomotor has moved, you’ll see its current angle in the terminal. Check these numbers and use them to decide which angles your Servomotor should move to in order to scan each block in the barcode!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_28_E.png"/>

Now let’s open a new text file and declare the `BarcodeScanner` class inside of it! We’ll store the angles we found starting from the left (which is the top column of the barcode) in a tuple and put it in the `POSITIONS` property.

```python=1
class BarcodeScanner:
    POSITIONS = (30, 70, 110, 150)  # Put the Servomotor angles you found here
```

#### ■ Black vs. White IR Photoreflector Threshold

Let’s open a new program file and use it to run the example code below.

##### Example Code 4-3-2

```python=1
import time
from pyatcrobo2.parts import IRPhotoReflector

irp = IRPhotoReflector("P2")

while True:
    time.sleep_ms(500)
    print(irp.get_value())
```

Start by setting a black block under your IR Photoreflector. Next, replace it with a white block and check the values in the terminal for each one. Use the values you’ve found to set a threshold which can tell white blocks from black blocks!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-1/10-1_29_E.png"/>

Next, let’s return to our original program file and define the threshold we chose in the `THRESHOLD` property.

###### New Code (Line 3)

```python=1
class BarcodeScanner:
    POSITIONS = (30, 70, 110, 150)
    THRESHOLD = 1600  # Put your own threshold here
```

#### ■ Making the `__init__()` Method

Let’s define the `__init__()` method next. The `BarcodeScanner` class is what controls the IR Photoreflector and Servomotor. Now let’s set the pins for each part as arguments and create an instance of the class!

New Code (Line 2, Lines 7-9)

```python=1
class BarcodeScanner:
    from pyatcrobo2.parts import IRPhotoReflector, Servomotor
    
    POSITIONS = (30, 70, 110, 150)
    THRESHOLD = 1600
    
    def __init__(self, pin_irp, pin_servo):
        self.irp = self.IRPhotoReflector(pin_irp)  # IR Photoreflector
        self.servo = self.Servomotor(pin_servo)  # Servomotor
```

Remember that the scan begins on the left side of the barcode, which is the starting column. Let’s start by moving the Servomotor in advance to move the IR Photoreflector to the left.

###### New Code (Line 10)

```python=1
class BarcodeScanner:
    from pyatcrobo2.parts import IRPhotoReflector, Servomotor
    
    POSITIONS = (30, 70, 110, 150)
    THRESHOLD = 1600
    
    def __init__(self, pin_irp, pin_servo):
        self.irp = self.IRPhotoReflector(pin_irp)
        self.servo = self.Servomotor(pin_servo)
        self.servo.set_angle(self.POSITIONS[0])  # Moves to the leftmost block (this is where the top column is)
```

#### ■ Making the `scan_barcode()` Method

Now we’ll define the `scan_barcode()` method in which scans the barcodes for each product!

This method scans each block in order starting from the top column, detecting the color of each block and converting it into a `”0”` for black or `”1”` for white to convert the barcode into a four-digit binary string.

###### New Code (Line 1, Lines 14-21)
```python=1
import time  # Imported in order to use the sleep_ms() function

class BarcodeScanner:
    from pyatcrobo2.parts import IRPhotoReflector, Servomotor
    
    POSITIONS = (30, 70, 110, 150)
    THRESHOLD = 1600
    
    def __init__(self, pin_irp, pin_servo):
        self.irp = self.IRPhotoReflector(pin_irp)
        self.servo = self.Servomotor(pin_servo)
        self.servo.set_angle(self.POSITIONS[0])

    def scan_barcode(self):
        barcode_binary = "0b"  # Barcode shown as a binary string
        for pos in self.POSITIONS:  # Scans block color starting with the top column
            self.servo.set_angle(pos)
            time.sleep_ms(300)  # Short pause until the Servomotor finishes rotating
            val = self.irp.get_value()
            # Adds a "0” to the end for black or a "1” to the end for white
            barcode_binary += "0" if val < self.THRESHOLD else "1"
```

Now that we’ve converted the barcode into a four-digit binary text string, we’ll use the `int()` function to convert the string into a decimal value! This result will be returned to the caller as a return value. The IR Photoreflector will then move back to the left in order to smoothly scan the next barcode.

###### New Code (Lines 21-23)

```python=14
    def scan_barcode(self):
        barcode_binary = "0b"
        for pos in self.POSITIONS:
            self.servo.set_angle(pos)
            time.sleep_ms(300)
            val = self.irp.get_value()
            barcode_binary += "0" if val < self.THRESHOLD else "1"
        barcode_decimal = int(barcode_binary, 2)  # Converts binary into decimal
        self.servo.set_angle(self.POSITIONS[0])  # Returns to the left
        return barcode_decimal  # Returns scan result
```

Last, we’ll add code which plays a short sound from the buzzer after the IR Photoreflector value is retrieved. This will let us know that the barcode is currently being scanned! And now we’ve finished the `BarcodeScanner` class!

###### New Code (Line 4, Line 21)

```python=1
import time

class BarcodeScanner:
    from pystubit.board import buzzer  # Imports buzzer object
    from pyatcrobo2.parts import IRPhotoReflector, Servomotor
```
```python=15
    def scan_barcode(self):
        barcode_binary = "0b"
        for pos in self.POSITIONS:
            self.servo.set_angle(pos)
            time.sleep_ms(300)
            val = self.irp.get_value()
            self.buzzer.on("C5", duration=100)  # Buzzer plays a short note
            barcode_binary += "0" if val < self.THRESHOLD else "1"
        barcode_decimal = int(barcode_binary, 2)
        self.servo.set_angle(self.POSITIONS[0])
        return barcode_decimal
```

### Defining the Register Class

Next we’ll define the `Register` class.

#### ■ Registering Product Data

We’ll start by putting the 16 products we checked at the beginning of the section into a dictionary. This dictionary will be defined as the `PRODUCT_DATA` property. The product numbers will be used as keys, and each key is associated with a product name and price stored inside of the dictionary!

###### New Code (Lines 28-45)

```python=28
class Register:
    PRODUCT_DATA = {
        1: {"name": "Carrots", "price": 300},
        2: {"name": "Onions", "price": 200},
        3: {"name": "Tofu", "price": 100},
        4: {"name": "Eggs", "price": 230},
        5: {"name": "Milk", "price": 240},
        6: {"name": "Apple juice", "price": 320},
        7: {"name": "Sausage", "price": 430},
        8: {"name": "Bacon", "price": 380},
        9: {"name": "Potato chips", "price": 200},
        10: {"name": "Ice cream", "price": 150},
        11: {"name": "Chocolate", "price": 350},
        12: {"name": "Sugar", "price": 250},
        13: {"name": "Soy sauce", "price": 500},
        14: {"name": "Rice", "price": 2000},
        15: {"name": "Tissues", "price": 400},   
    }
```

#### ■ Making the `__init__()` Method

The `Register` class uses a feature from the `BarcodeScanner` class in order to calculate the total for the purchase. This means that we’ll need to keep an instance of the `BarcodeScanner` class inside of the `Register` class! Let’s set the instance as an argument in the `__init__()` method and store it in the `scanner` property. We’ll also define the variable which stores the total purchase amount as the `total` property.

###### New Code (Lines 47-49)

```python=28
class Register:
    PRODUCT_DATA = {
        1: {"name": "Carrots", "price": 300},
        2: {"name": "Onions", "price": 200},
        3: {"name": "Tofu", "price": 100},
        4: {"name": "Eggs", "price": 230},
        5: {"name": "Milk", "price": 240},
        6: {"name": "Apple juice", "price": 320},
        7: {"name": "Sausage", "price": 430},
        8: {"name": "Bacon", "price": 380},
        9: {"name": "Potato chips", "price": 200},
        10: {"name": "Ice cream", "price": 150},
        11: {"name": "Chocolate", "price": 350},
        12: {"name": "Sugar", "price": 250},
        13: {"name": "Soy sauce", "price": 500},
        14: {"name": "Rice", "price": 2000},
        15: {"name": "Tissues", "price": 400},   
    }
    
    def __init__(self, scanner):
        self.scanner = scanner  # BarcodeScanner class instance
        self.total = 0  # Product total
```

#### ■ Making the `scan()` Method

The `scan()` method uses features from the `BarcodeScanner` class to scan a barcode and use its number to search for product data. Scanning a wrong number (an unregistered number like 0, for example) which doesn’t exist in `PRODUCT_DATA` will cause an error. We’ll deal with any exceptions by handling them with a `try-except` statement!

###### New Code (Lines 51-57)

```python=47
    def __init__(self, scanner):
        self.scanner = scanner
        self.total = 0
    
    def scan(self):
        # Scans the barcode (runs the BarcodeScanner class method)
        product_num = self.scanner.scan_barcode()  # Retrieves the number
        try:  # Checks for errors with a try-except statement
            product = self.PRODUCT_DATA[product_num]  # Retrieves product data from product code
        except KeyError:  # If product code doesn\’t exist
            print("Read error.")  # Shows the error message
            return  # Process ends here
```

If we successfully scan a product number, the product name and price will appear in the terminal and the price will be added to the total.

###### New Code (Lines 58-61)

```python=51
    def scan(self):
        product_num = self.scanner.scan_barcode()
        try:
            product = self.PRODUCT_DATA[product_num] 
        except KeyError:
            print("Read error.")
            return   
        name = product["name"]  # Product name
        price = product["price"]  # Product price
        print(product["name"], ":", product["price"])  # Shows product name and price
        self.total += price  # Adds price to the total
```

#### ■ Making the `show_total()` Method

Run this method and the combined for every product scanned so far will appear as the total (the `total` property). Once the total has appeared, we’ll reset it to 0!

###### New Code (Lines 63-66)

```python=51
    def scan(self):
        product_num = self.scanner.scan_barcode()
        try:
            product = self.PRODUCT_DATA[product_num] 
        except KeyError:
            print("Read error.")
            return
        name = product["name"]
        price = product["price"]
        print(product["name"], ":", product["price"])
        self.total += price
    
    def show_total(self):
        if self.total != 0:  # Won\’t show when total is 0 even when called
            print("total: ", str(self.total))  # Shows total for products
            self.total = 0  # Resets to 0
```

And now we’ve defined both of the classes!

### Defining the `main()` Function

Last, we’ll put the processes which call the `Register` class methods if you press A or B into the `main()` function.

New Code (Line 2, Lines 70-79)

```python=1
import time
from pystubit.board import button_a, display  # Imports objects for both buttons
```

```python=70
def main():
    # Creates an instance of each class
    scanner = BarcodeScanner("P2", "P15")  #
    register = Register(scanner)

    while True:
        if button_a.is_pressed():  # Press the A button to scan the barcode and show the product name and price
            register.scan()
        if button_b.is_pressed():  # Press the B button to show the total
            register.show_total()
```

### Testing the Program

Check below to see what the finished program should look like. Now let’s run the program and call the `main()` function from the terminal! Press the A button and scan a few barcodes to see if their product names and prices appear in the terminal. Now press the B button and check if the total purchase amount appears!

##### Example Code 4-6-1

```python=1
import time
from pystubit.board import button_a, button_b

class BarcodeScanner:
    from pystubit.board import buzzer
    from pyatcrobo2.parts import IRPhotoReflector, Servomotor

    POSITIONS = (30, 70, 110, 150)
    THRESHOLD = 1600
    
    def __init__(self, pin_irp, pin_servo):
        self.irp = self.IRPhotoReflector(pin_irp)
        self.servo = self.Servomotor(pin_servo)
        self.servo.set_angle(self.POSITIONS[0])

    def scan_barcode(self):
        barcode_binary = "0b"
        for pos in self.POSITIONS:
            self.servo.set_angle(pos)
            time.sleep_ms(300)
            val = self.irp.get_value()
            self.buzzer.on("C5", duration=100)
            barcode_binary += "0" if val < self.THRESHOLD else "1"
        barcode_decimal = int(barcode_binary, 2)
        self.servo.set_angle(self.POSITIONS[0])
        return barcode_decimal
    

class Register:
    PRODUCT_DATA = {
        1: {"name": "Carrots", "price": 300},
        2: {"name": "Onions", "price": 200},
        3: {"name": "Tofu", "price": 100},
        4: {"name": "Eggs", "price": 230},
        5: {"name": "Milk", "price": 240},
        6: {"name": "Apple juice", "price": 320},
        7: {"name": "Sausage", "price": 430},
        8: {"name": "Bacon", "price": 380},
        9: {"name": "Potato chips", "price": 200},
        10: {"name": "Ice cream", "price": 150},
        11: {"name": "Chocolate", "price": 350},
        12: {"name": "Sugar", "price": 250},
        13: {"name": "Soy sauce", "price": 500},
        14: {"name": "Rice", "price": 2000},
        15: {"name": "Tissues", "price": 400},   
    }
    
    def __init__(self, scanner):
        self.scanner = scanner
        self.total = 0
    
    def scan(self):
        product_num = self.scanner.scan_barcode()
        try:
            product = self.PRODUCT_DATA[product_num] 
        except KeyError:
            print("Read error.")
            return
        name = product["name"]
        price = product["price"]
        print(product["name"], ":", product["price"])
        self.total += price
    
    def show_total(self):
        if self.total != 0:
            print("total: ", str(self.total))
            self.total = 0


def main():
    scanner = BarcodeScanner("P2", "P15")
    register = Register(scanner)

    while True:
        if button_a.is_pressed():
            register.scan()
        if button_b.is_pressed():
            register.show_total()
```

## Challenge: Adding an Inventory Management Feature

In addition to managing pricing and product data, more advanced register systems come with an abundance of features which help manage the store, including tracking sales and membership reward points!

In this challenge we’ll focus on an inventory management feature as we improve example code 4-6-1. We’ll add a new stock attribute to our product data and decrease the level of stock every time that barcode is scanned. Once the stock becomes low enough, we’ll make the name of the product scroll across the LED display to alert us that the inventory for that item is low!

The table below lists the initial stock of each product as well as the stock level at which we’ll receive an alert. Feel free to set any amount you’d like here!

##### Initial Inventory and Low Stock Settings

|Product Number|Product Name|Initial Stock|Low Stock Alert Level|
|---:|:---|:---|:---|
|1|Carrots (Carrots)|10|2|
|2|Onions (Onions)|10|2|
|3|Tofu (Tofu)|5|1|
|4|Eggs (Eggs)|10|2|
|5|Milk (Milk)|10|2|
|6|Apple Juice (Apple juice)|5|1|
|7|Sausage (Sausage)|5|1|
|8|Bacon (Bacon)|5|1|
|9|Potato Chips (Potato chips)|5|1|
|10|Ice Cream (Ice cream)|10|2|
|11|Chocolate (Chocolate)|5|1|
|12|Sugar (Sugar)|5|1|
|13|Soy Sauce (Soy sauce)|5|1|
|14|Rice (Rice)|5|1|
|15|Tissues (Tissues)|5|1|

### Making the Program

We'll use the settings in the table above as we introduce the steps to make this program.

#### ■ Adding Initial Stock and the Low Stock Alert Level

We’ll add a `stock` attribute which sets the initial stock and a `low` attribute which sets the alert level in the `PRODUCT_DATA` property of the `Register` class which manages our product data.

###### Changed Code (Lines 31-45)

```python=29
class Register:   
    PRODUCT_DATA = {
        1: {"name": "Carrots", "price": 300, "stock": 10, "low": 2},
        2: {"name": "Onions", "price": 200, "stock": 10, "low": 2},
        3: {"name": "Tofu", "price": 100, "stock": 5, "low": 1},
        4: {"name": "Eggs", "price": 230, "stock": 10, "low": 2},
        5: {"name": "Milk", "price": 240, "stock": 10, "low": 2},
        6: {"name": "Apple juice", "price": 320, "stock": 5, "low": 1},
        7: {"name": "Sausage", "price": 430, "stock": 5, "low": 1},
        8: {"name": "Bacon", "price": 380, "stock": 5, "low": 1},
        9: {"name": "Potato chips", "price": 200, "stock": 5, "low": 1},
        10: {"name": "Ice cream", "price": 150, "stock": 10, "low": 2},
        11: {"name": "Chocolate", "price": 350, "stock": 5, "low": 1},
        12: {"name": "Sugar", "price": 250, "stock": 5, "low": 1},
        13: {"name": "Soy sauce", "price": 500, "stock": 5, "low": 1},
        14: {"name": "Rice", "price": 2000, "stock": 5, "low": 1},
        15: {"name": "Tissues", "price": 400, "stock": 5, "low": 1},   
    }
```

#### ■ Adding the Low Stock Alert

The stock for a product will decrease when a barcode is scanned by the `Register` class’s `scan()` method. Once the level of stock matches the low stock amount, the name of the product will scroll across the LED display. 

###### New Code (Line 30, Lines 65-67)
```python=29
class Register:
    from pystubit.board import display  # Imports object in order to use the LED display
```
```python=54
    def scan(self):
        product_num = self.scanner.scan_barcode()
        try:
            product = self.PRODUCT_DATA[product_num] 
        except KeyError:
            print("Read error.")
            return
        name = product["name"]
        price = product["price"]
        print(product["name"], ":", product["price"])
        self.total += price
        product["stock"] -= 1  # Recudes stock by 1
        if product["stock"] == product["low"]:  # If low on stock
            self.display.scroll(name, delay=50)  # Scrolls product name across display
```

### Testing the Program

As shown above, we just needed to make a few simple changes to add an inventory management feature. Of course, actual cash registers use much more complex processes in their programs. Now let’s run our finished program and see how it works!

##### Example Code 5-2-1

```python=1
import time
from pystubit.board import button_a, button_b

class BarcodeScanner:
    from pystubit.board import buzzer
    from pyatcrobo2.parts import IRPhotoReflector, Servomotor

    POSITIONS = (30, 70, 110, 150)
    THRESHOLD = 1600
    
    def __init__(self, pin_irp, pin_servo):
        self.irp = self.IRPhotoReflector(pin_irp)
        self.servo = self.Servomotor(pin_servo)
        self.servo.set_angle(self.POSITIONS[0])

    def scan_barcode(self):
        barcode_binary = ""
        for pos in self.POSITIONS:
            self.servo.set_angle(pos)
            time.sleep_ms(300)
            val = self.irp.get_value()
            self.buzzer.on("C5", duration=100)
            barcode_binary += "0" if val < self.THRESHOLD else "1"
        barcode_decimal = int(barcode_binary, 2)
        self.servo.set_angle(self.POSITIONS[0])
        return barcode_decimal
    

class Register:
    from pystubit.board import display
    
    PRODUCT_DATA = {
        1: {"name": "Carrots", "price": 300, "stock": 10, "low": 2},
        2: {"name": "Onions", "price": 200, "stock": 10, "low": 2},
        3: {"name": "Tofu", "price": 100, "stock": 5, "low": 1},
        4: {"name": "Eggs", "price": 230, "stock": 10, "low": 2},
        5: {"name": "Milk", "price": 240, "stock": 10, "low": 2},
        6: {"name": "Apple juice", "price": 320, "stock": 5, "low": 1},
        7: {"name": "Sausage", "price": 430, "stock": 5, "low": 1},
        8: {"name": "Bacon", "price": 380, "stock": 5, "low": 1},
        9: {"name": "Potato chips", "price": 200, "stock": 5, "low": 1},
        10: {"name": "Ice cream", "price": 150, "stock": 10, "low": 2},
        11: {"name": "Chocolate", "price": 350, "stock": 5, "low": 1},
        12: {"name": "Sugar", "price": 250, "stock": 5, "low": 1},
        13: {"name": "Soy sauce", "price": 500, "stock": 5, "low": 1},
        14: {"name": "Rice", "price": 2000, "stock": 5, "low": 1},
        15: {"name": "Tissues", "price": 400, "stock": 5, "low": 1},   
    }
    
    def __init__(self, scanner):
        self.scanner = scanner
        self.total = 0
    
    def scan(self):
        product_num = self.scanner.scan_barcode()
        try:
            product = self.PRODUCT_DATA[product_num] 
        except KeyError:
            print("Read error.")
            return
        
        name = product["name"]
        price = product["price"]
        print(product["name"], ":", product["price"])
        self.total += price
        
        product["stock"] -= 1
        if product["stock"] == product["low"]:
            self.display.scroll(name,delay=50)
    
    def show_total(self):
        if self.total != 0:
            print("total: ", str(self.total))
            self.total = 0


def main():
    scanner = BarcodeScanner("P2", "P15")
    register = Register(scanner)

    while True:
        if button_a.is_pressed():
            register.scan()
        if button_b.is_pressed():
            register.show_total()
```

## Conclusion

### A Quick Review

In this lesson we learned about **binary**, **octal**, and **hexadecimal**, three number systems which work well with computers. We also learned that even computers which perform high-level calculations are composed of simple **logic gates** when you take a deeper look at them!

In the second half of the lesson, we made a simple cash register with a Barcode Scanner. This scanner turned black and white blocks into binary, then converted those binary values into decimal.

All of the concepts covered here are incredibly important fundamentals when it comes to understanding computers. Review them well and make sure you have a firm grasp of them as we move on to the next lesson!

### Coming Up Next Time!

In the next lesson we’ll learn how to **represent volumes of data** as well as **techniques for handling text data** on a computer!