---
tags: Python_English, Topic 10
---

Python Robotics Course Lesson 38

Topic 10-2
Making a Light-Wave Communicator
===

Learn how to represent text strings on a computer!

## In This Lesson...

In this lesson we’ll learn how to represent volumes of data as well as techniques for handling character data on a computer!

:::info

Lesson Introduction

■ Lesson 38 Contents
1. Representing Data Volume (Bits and Bytes)
2. Showing Text Strings on a Computer
3. Transmitting Text Using a Light-Wave Communicator
4. Challenge: Transmitting Color Data with Light Signals


In this lesson we’ll student how to represent the volumes of data handled by a computer as well as how to show text strings on a computer!

At the start of the lesson we’ll introduce bits and bytes, two units used to represent volumes of data.

Next, we’ll take a look at how text strings are expressed on a computer!

In the second half of the lesson, we’ll make a communication system which uses light signals to send and receive single characters! Light-based communication allows you to send data farther at higher speeds, making it widely-used for Internet connections.

In the final challenge we’ll improve our program to make our communicator transmit LED colors in addition to text!

Now let's start the lesson!

:::

## Representing Data Volume

In Topic 10-1 we explained how calculation is performed inside of a computer. To put it simply, you can think of it as a collection of electronic circuits with switches that turn on and off.

Turning the switch off or on sends the circuit into either a LOW or a HIGH voltage state, and substituting these states with digital values of 0 (LOW) or 1 (HIGH) is what allows a computer to use binary to handle numbers.

And these are used for more than just numbers. The digits 0 and 1 are used to represent every piece of data a computer handles, including text, pictures, and movies!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_1_E.png"/>

The smallest unit of data on a computer is a **single digit** of 0 or 1, and this unit is known as a **bit**. The word bit was coined by combining the word **binary** with the word **digit**! Rather than using bits, we use units of **bytes** to represent amounts of data on a computer. Generally speaking, a single byte represents **eight bits**!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_2_E.png"/>

You’ll often find amounts of data used by a computer or smartphone followed by a **G**, which is short for **gigabyte**. The prefix giga means one billion (or 10<sup>9</sup>), meaning that one gigabyte is equal to one billion bytes of data!

##### Data Volume Prefixes

|Prefix|What does it mean?|
|:---|:---|
|K (kilo)|1,000 (10<sup>3</sup>)|
|M (mega)|1,000,000 (10<sup>6</sup>)|
|G (giga)|1,000,000,000 (10<sup>9</sup>)|
|T (tera)|1,000,000,000,000 (10<sup>12</sup>)|
|P (peta)|1,000,000,000,000,000 (10<sup>15</sup>)|

## Showing Text Strings on a Computer

In the previous section we introduced the idea that character data on a computer is represented using 0s and 1s. Each letter in the English alphabet, for example, is represented by eight bytes of data by default in Python!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_3_E.png"/>

The value which represents a single byte is known as a **character encoding** or a **code point**. In other words, a computer substitutes character data using these fixed numbers rather than handling the character data directly!

The **character set** determines which character encoding is assigned to each character. Originally there were different character sets used for different languages. The spread of the Internet in the modern age, however, has led to websites and web services being shared by people all over the world, making the multilingual **Unicode** character set very popular! Unicode is also the default character set in Python.

The table below shows examples of the Unicode character encoding assigned to different characters.

##### Character Encodings in Unicode

|Character|Character Encoding (Hexadecimal)|
|:---|:---|
|!|33 (0x21）|
|?|63 (0x3F）|
|A|65 (0x41）|
|B|66 (0x42）|
|C|67 (0x43）|
|a|97 (0x61）|
|b|98 (0x62）|
|c|99 (0x63）|

You can check the character encoding for any character using the `ord()` function!

##### Example Code 3-1-1
```python=1
A = "A"
a = "a"

print(ord(A))
print(ord(a))
```

##### Program Results
<pre class="prettyprint">
65
97
</pre>

You can also use the `chr()` function to convert a character encoding into a character! 

##### Example Code 3-1-2
```python=1
print(chr(65))
print(chr(97))
```

##### Program Results
<pre class="prettyprint">
A
a
</pre>

Converting a specific character into a character encoding defined in a character set like Unicode is called **encoding**. Converting a character encoding into a character, on the other hand, is known as **decoding**!

**UTF-8** and **UTF-16** are two of the most representative encoding formats in Unicode. The main difference between these formats is the data volume of each encoding. In other words, this is how many bytes are used to represent a single character!

A character can be one to six bytes in UTF-8. The letters of the English alphabet are a single byte a single character in the Japanese language is represented by three bytes.

If we were to use UTF-16, however, every letter and character in both of these languages would be shown as two bytes. This means that UTF-8 is more efficient for texts which use a lot of English, while UTF-16 would be the more efficient choice if you’re working with a lot of Japanese!

Python is set to use the UTF-8 encoding format by default.

We can use the `encode()` method in order to encode a string. Encoding a string converts it into a data structure known as a **byte string**. We’ll go into a more detailed explanation of byte strings in the next section. The `decode()` method, on the other hand, allows us to decode a byte string!

##### Example Code 3-1-3
```python=1
string = "abc"

# Encodes the string
encoded_str = string.encode("utf-8")  # Sets to utf-8 (this can be left out since UTF-8 is the default)
print(type(encoded_str))  # Byte string structure
print(encoded_str)  # Adds b to the beginning since this is a byte string


# Decodes the string
decoded_str = encoded_str.decode("utf-8")  # Sets to utf-8 (this can be left out since UTF-8 is the default)
print(type(decoded_str))  # Byte string structure
print(decoded_str)  # Same as string on line 1
```

##### Program Results

<pre class="prettyprint">
&lt;class 'bytes'&gt;
b'abc’
&lt;class 'str'&gt;
abc
</pre>

Run this program for yourself and you’ll see that b has been added in front of the string to show that it has been converted into a byte string!

## Byte Strings

A byte string is a single-byte sequence of data. Even if you put text strings or numerical values into a single byte of data, they would still be recognized as characters and numbers. A byte string, on the other hand, has no designated format or meaning! This means that they’re treated only as a sequence of 0s and 1s!

The character encoding we did in the previous examples used English, which makes it hard to tell that the results from our print statement are a byte string. Encode and print a Japanese character, however, and you will clearly see that it has been converted into a three-byte byte string!  

###### ★ Don’t worry if you can’t type in Japanese! Just copy and paste the example code below and run it!

##### Example Code 4-1-1
```python=1
string = "あ"  # This is a Japanese hiragana character
encoded_str = string.encode("utf-8")
print(encoded_str)
```

##### Program Results
<pre class="prettyprint">
b'\xe3\x81\x82'
</pre>

A backslash (`\`) comes before and after each byte of data. The `x` shows that these are hexadecimal values, meaning that two characters which follow it are equal to one byte!

###### ★ One digit in hexadecimal is equal to four bits, making two hexadecimal digits one byte!

In addition to character strings, you can also convert between numerical values and byte strings. We can use an integer’s `to_bytes()` method and the `int` `from_bytes()` method in order to do this.

##### Example Code 4-1-2
```python=1
num = 15777200

# Converts to a byte string
_bytes = num.to_bytes(3, "big")
print(_bytes)

# Converts byte string to an integer
_num = int.from_bytes(_bytes, "big") 
print(_num)
```

##### Program Results
<pre class="prettyprint">
'\xf0\xbd\xb0'
15777200
</pre>

We’ll set the number of bytes after converting in the first argument of the `to_bytes()` method. Since we can use exactly three bytes to show the value 15777200, let’s set the argument to 3! The `”big”` in the second argument stands for **big-endian**. Along with `”little”`, or **little-endian**, it’s one of two formats which determine the order of data converted into a byte string.

##### Big-Endian vs. Little-Endian

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_4_E.png"/>


Now let’s try changing the example code from above into little-endian.

##### Example Code 4-1-3
###### Changed Code (Line 3, Line 6)
```python=1
num = 15777200

_bytes = num.to_bytes(3, "little")
print(_bytes)

_num = int.from_bytes(_bytes, "little") 
print(_num)
```

##### Program Results

<pre class="prettyprint">
b'\xb0\xbd\xf0'
15777200
</pre>

You can see from the results of each program that the order of the byte string has changed! Using little-endian to convert a byte string converted in big-endian back into an integer will change the content of the string, so make sure that you check which format you’re using!

## Bitwise Operators

Now we’ve learned that volumes of data are measured in units called bits and bytes. You’ll find that Python as well as many other programming languages support operations which work one bit at a time on the binary level!

### Single-Bit Logical Operators (AND, OR, XOR)

You can use the following operators when you want to use logical operations of two pieces of data one bit a time.

|Operator|What is it?|
|:---|:---|:---|
|`&`|An AND operator||
|`|`|An OR operator||
|`^`|A XOR exclusive OR operator|

These operators mix and match bits in the same column to give you the following results:

* AND

|A|B|A AND B|
|:---|:---|:---|
|0|0|0|
|1|0|0|
|0|1|0|
|1|1|**1**|

* OR

|A|B|A OR B|
|:---|:---|:---|
|0|0|0|
|1|0|**1**|
|0|1|**1**|
|1|1|**1**|

* XOR

|A|B|A XOR B|
|:---|:---|:---|
|0|0|0|
|1|0|**1**|
|0|1|**1**|
|1|1|0|


Now let’s try these operators out for ourselves using 8 bit binary data!

In the previous lesson we defined binary, hexadecimal, and other numbers as text strings like `"0b101"` or `"0x1f"` in order to use them, but here we’ll leave out the `””` and define these numbers as numerical values.

```python=1
A = 0b10101111  # 175 in decimal
B = 0b11110101  # 245 in decimal
```

We’re going to take our A and B values above and work on them one bit at a time using the AND, OR, and XOR operators to see what results we get!

##### Example Code 5-1-1
###### New Code (Lines 4-6)
```python=1
A = 0b10101111
B = 0b11110101

print("AND", A & B)  # AND operator
print("OR", A | B)  # OR operator
print("XOR", A ^ B)  # XOR operator
```

##### Program Results
<pre class="prettyprint">
AND 165
OR 255
XOR 90
</pre>

While these logical operators work on A and B one bit at a time, the result is handled as a numerical value. This means that the result is rewritten in decimal when we use a `print` statement to show it! We can use the `bin()` function to convert the value into a binary string. Let’s try rewriting the example code above to look like the code below before running it!

##### Example Code 5-1-2
###### Changed Code (Lines 4-6)
```python=1
A = 0b10101111
B = 0b11110101

print("AND", bin(A & B))
print("OR", bin(A | B))
print("XOR", bin(A ^ B))
```

##### Program Results
<pre class="prettyprint">
AND 0b10100101
OR 0b11111111
XOR 0b1011010
</pre>

No we can see our results in binary!

### Flipping Bits with NOT

There’s another logical operator known as the NOT operator. You can use the `~` symbol for this operator to change the value of a bit. Let’s start by running the example code below and checking the result!

##### Example Code 5-2-1
```python=1
C = 0b10101010  # 170 in decimal

print("NOT", bin(~C))
```

##### Program Results
<pre class="prettyprint">
NOT -0b10101011
</pre>

You may have expected every 0 in the number to turn into 1 and vice versa. Our results, however, show that this isn’t the case!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_5_E.png"/>

This time we’ll see what our results are if we don’t use the `bin()` function!

##### Example Code 5-2-2
###### Changed Code (Line 3)
```python=1
C = 0b10101010  # 170 in decimal

print("NOT", ~C)
```

##### Program Results

<pre class="prettyprint">
-171
</pre>

Using the `~` operator to flip a number takes the value `x` and returns `-(x+1). This means that a `-` symbol will be added to a binary string, with 1 being added to the lowest bit!

### Bit Shifting

In addition to logical operators, Python also has operators which can be used to shift bits to the left or right one bit at a time!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_6_E.png"/>

You can use the `<<` operator to shift bits to the left, and the `>>` operator to shift bits to the right. You can also place a numerical value after each of these operators to set how many places you want to shift the number! Let’s try running the example code below and checking the result!

##### Example Code 5-3-1
```python=1
C = 0b1001

print("<< 1", bin(C << 1))  # Shifts 1 bit to the left
print("<< 2", bin(C << 2))  # Shifts 2 bits to the left
print(">> 1", bin(C>> 1))  # Shifts 2 bit to the right
print(">> 2", bin(C >> 2))  # Shifts 2 bits to the right
```

##### Program Results
<pre class="prettyprint">
&lt;&lt; 1 0b10010
&lt;&lt; 2 0b100100
&gt;&gt; 1 0b100
&gt;&gt; 2 0b10
</pre>

While there aren’t very many opportunities to work with operators on the bit level, they can come in quite handy if you need to cut down on the size of a piece of data or if you need to lower the processing cost for a calculation!

## Building a Light-Wave Communicator

In the first half of the lesson we learned how **all data on a computer including numbers, text, and images are represented by the binary digits 0 and 1**. We also learned that units of information are measure in **bits** and **bytes**! Representing data in this way has its merits when information is being exchanged between computers.

Computers exchange information by **converting data into an electronic signal** before sending it. When you transfer a program made on your computer to your Core Unit, for example, the changes between HIGH voltage and LOW voltage states are sent as an electrical signal through the USB cable. If we interpret a high voltage state as 1 and a low voltage state as a 0 in our signal, we can send this data one bit at a time!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_7_E.png"/>

Internet communication sends a vast amount of data at high speeds by **converting electronic signals into light signals** before transmitting them!

The cables used to transmit this data use a material known as **optical fiber**. The properties of optical fiber causes a **total reflection of any light which enters the cable over a certain angle called an angle of incidence**. The light will repeatedly bounce along the inside of the cable with zero loss, allowing it to transmit signals over far distances!

##### The Structure of Optical Fiber

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_8_E.png"/>

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_9_E.png"/>

Instead of using voltage, light-based communication uses flashes of light to send binary data.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_10_E.png"/>

In the second half of the lesson we'll take this model of light-based communication to make a **light-wave communication system which uses changes in brightness to transmit single-byte (or eight bit) characters**. We’ll start by building the Light-Wave Communicator itself!

##### Building the Device

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_11_E.JPG"/>

### Parts You'll Need

##### Parts
* Core Unit x 1
* Basic Cube (Black) x 4
* Basic Cube (Red) x 1
* Half B (Black) x 2
* Half C (White) x 6
* Half D (White) x 4

##### The Artec Block Shapes

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_12_E.png"/>

### Building a Light-Wave Communicator

Follow the link below to find instructions for building the Light Wave Communicator:

[Building a Light-Wave Communicator](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_build_E.pdf)


## Programming the Light-Wave Communicator

We’ll start by figuring how our communicator sends and receives single-byte (or eight bit) characters.

### How the Communicator Works

We’ll have to make the light signals for our communicator manually. Pushing the black block to the left will expose the light sensor, and pushing it to the right will hide it. In other words, doing this will either let the light through or block it, switching between bright and dark states to simulate the light flashing!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_13_E.png"/>

Computers communicating with each other will also sync their **transfer speed**, or the volume of data the send and receive each second to ensure that each computer receives data with zero loss!

##### Differing Transfer Speeds
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_14_E.png"/>
##### Synced Transfer Speeds
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_15_E.png"/>

However, since we’re sending our signals manually, there’s no way to accurately sync transfer speeds the way that computers do. This means that, though it’s going to slow down our transfer speeds, we’re going to make a rough estimate for how long the light stays on to make our binary 0s and 1s. The light sensor is exposed to light for one second or longer will make a 1, while a 0 will be any time shorter than one second!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_16_E.png"/>

Before we start sending any data, we’ll need to request that the person on the other end begins receiving data! We can do this by leaving the light on for over two seconds to make the request before we start sending the data from the top digit!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_17_E.png"/>

The method we’ve explained so far will allow us to send characters, which can all be represented by a single byte. There are cases, however, when interference during transmission can cause data ti be lost or even replaced by other data. This is why techniques have been developed to detect if there are any errors in the data a machine receives!

The simplest and most well known of this is a method known as a **parity check**. A parity check checks for  errors by adding a single bit called a **parity bit** to the end of a piece of data. There are two ways to add this bit known as **even parity** or **odd parity**.

* **Even Parity**
Even parity counts every 1 bit in the sent data. If the number of 1s is odd, it adds a 1 to the end of the data, and if the number of 1s is even it adds a 0. This keeps the number of 1 bits in the data **even**. This means that an error is detected if the number of 1 bits in the received data is odd!
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_18_E.png"/>

* **Odd Parity**
Even parity counts every 1 bit in the sent data. If the number of 1s is even, it adds a 1 to the end of the data, and if the number of 1s is odd it adds a 0. This keeps the number of 1 bits in the data **odd**. This means that an error is detected if the number of 1 bits in the received data is even!
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_19_E.png"/>

We can add the code to perform a parity check to a program pretty easily, so let’s try adding it to the program for our Light-Wave Communicator here!

### Making the Program

Now let’s move on to making a program which can transmit data using the method we reviewed in the last section. We’ll need to give this program the following two features:

* **Convert character data typed into the terminal into binary data**
* **Receive and show the character data contained in the signals it receives**

While these signals are sent manually, it would be pretty hard to calculate the signals without any help. The first feature of our communicator is going to help cut down on the work it takes to do this!

Now let’s put each of the features into the following functions:

|Function|What does it do?|
|:---|:---|
|`convert_char_to_signal()` Function|Creates binary data for any character typed into the terminal|
|`receiver()` Function|Receive and show the character data contained in the signals it receives|

#### ■ Making the `convert_word_to_signal()` Function

The picture below shows the steps taken to convert a character into a binary string to send a light signal. The `convert_char_to_signal()` does the conversion and adds a parity bit to make the data!

##### Converting Text to Binary and Adding a Parity Bit

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_20_E.png"/>

Now let’s follow these steps in order as we write the code that will do the conversion!

We’ll start by writing the `ord()` function. This function retrieves a character code in decimal when you type a character in the terminal:

```python=1
def convert_char_to_signal(character):
    char_code = ord(character)  # Retrieves character code in decimal
```

Next, our `bin()` function will convert the code into a binary string. Using the `bin()` function to convert adds a `0b` header to the string to indicate that this is a binary number. We don’t need this header, so we can slice the `0b` and keep only the part that follows it!

###### New Code (Line 3)
```python=1
def convert_char_to_signal(character):
    char_code = ord(character)
    binary = bin(char_code)[2:]  # Keeps only the part of the binary string which follows `0b`
```

Ever piece of character data we send will be in fixed, single-byte (or eight bit) chunks. However, the `bin()` function leaves the top 0 bit out of and value less than eight bits. This means that we’re going to use the text string `format()` method to make the string eight bits by adding a 0 in the top space, making sure that every character we type appears as an eight-bit string!

###### New Code (Line 4)
```python=1
def convert_char_to_signal(character):
    char_code = ord(character)
    binary = bin(char_code)[2:]
    binary = "{:0>8}".format(binary)  # Changes a string like "1000001" to "01000001"
```

Last we’ll add an even parity bit to the end and return the result.

###### New Code (Lines 6-11)
###### ★ We’ll use the `%` remainder operator to divide the number by two. If the remainder is 1, the number of 1 bits is odd!
```python=1
def convert_char_to_signal(character):
    char_code = ord(character)
    binary = bin(char_code)[2:]
    binary = "{:0>8}".format(binary)
    # Adds parity bit to the end
    if binary.count("1") % 2 == 1:  # If the number of 1 bits is odd
        signal = binary + "1"  # Adds 1 to the end to make an even number of bits
    else:  # If the number of 1 bits is even
        signal = binary + "0"  # Adds 0 to the end to keep the number of bits even
        
    return signal # Returns the result
```

Now let’s run the above code and call the function from the terminal. You should be able to see that the character has been converted into a binary signal that we can send!

<pre class="prettyprint">
&gt;&gt;&gt; convert_char_to_signal("A")
'010000010'
&gt;&gt;&gt; convert_char_to_signal("a")
'011000011'
</pre>

#### ■ Making the `receiver()` Function

Next we’ll put the process for the machine receiving the signal inside of the `receiver()`.

We’ll start by checking the values for when the light sensor is hidden versus when it’s exposed. **Let’s make a new file** and copy and paste the example code into it!

##### Retrieving the Light Sensor Values

```python=1
import time
from pystubit.board import lightsensor

while True:
    print(lightsensor.get_value())
    time.sleep_ms(500)
```

Once we’ve confirmed the values for both states, we need to decide on a threshold to distinguish between the two. This judgment needs to be very accurate, so let’s set two threshold values for it!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_21_E.png"/>

Now let’s **go back to the original program** and import the `time` module as well as a `lightsensor` object at the beginning of the program and add the code which defines our two thresholds!

###### New Code (Line 1-5)
```python=1
import time
from pystubit.board import lightsensor

threshold_low = 400  # Light sensor values are affected by your environment
threshold_high = 700  # so be sure to use your own thresholds here!
```

Now let’s use our two thresholds to write the code for our receiver process.

We’ll start by making it wait until the request is made. While it waits, this process will check the light sensor’s value every **10 milliseconds**. If the value goes over the `threshold_high` variable it will determine that the light is staying on. Next, it will count how long the light has stayed on and check whether it has been on for longer than 2,000 milliseconds. If the light turns off, this count will be reset!

###### New Code (Lines 20-30)
```python=20
def receiver():  
    while True:   
        print("Now waiting...")  # Shows a now waiting message
        count = 0  # Variable which counts how long the light has been on
        while True:
            time.sleep_ms(10)
            val = lightsensor.get_value()
            if val > threshold_high:  # If the light is on
                count += 1  
            elif val < threshold_low:  # If the light is off
                count = 0
```

If the light is turned off after the count reaches 200, the `while` statement loop on line 24 will end and the receiver will begin receiving data.

###### New Code (Lines 32-36)
```python=20
def receiver():  
    while True:   
        print("Now waiting...")
        count = 0
        while True:
            time.sleep_ms(10)
            val = lightsensor.get_value()
            if val > threshold_high:
                count += 1
            elif val < threshold_low:
                count = 0
            
            if count == 200:  # If the count is over 200 (this means that the light has been on for two seconds)
                while lightsensor.get_value() > threshold_low:  # Waits until the light turns off
                    time.sleep_ms(10)
                break  # Ends while statement on line 24
        print("Now receiving.")  # Shows a now receiving message
```

Once reception has started, the receiver will take each signal in order and convert it into binary. Let’s measure the amount of time the light stays on add a `”1”` to the end of the text string for a time of one second or more and a `”0”` to the end for a time of less than one second. It should repeat until it receives nine bits of data (this includes the parity bit)!

###### New Code (Lines 38-52)
```python=32            
            if count == 200:
                while lightsensor.get_value() > threshold_low:
                    time.sleep_ms(10)
                break
        print("Now receiving.")

        data = ""  # Variable which stores received data
        while len(data) < 9:  # Repeats until it receives 9 bits
            while lightsensor.get_value() < threshold_high: # Waits until the light turns off
                time.sleep_ms(1)
                pass
            start = time.ticks_ms()  # Starts measuring the amount of time the light is on
            while lightsensor.get_value() > threshold_low:  # Waits until the light turns off
                time.sleep_ms(1)
                pass
            diff = time.ticks_diff(time.ticks_ms(), start)  # Retrieves the amount of time the light has stayed on
            if diff >= 1000:  # Receives “1” if over 1 second
                data += "1" 
            else:  # Receives “0” if under 1 second
                data += "0"
            print(data)  # Shows data received to far to test how the program works
```

Since we’ve received our nine bits of data, we’ll check the parity bit at the end. The program will show an error message if it detects and error with the bit! If no error is detected, it will convert the binary string that it received into a character and show it in the terminal.

##### Converting to a Character After the Parity Check

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_22_E.png"/>

###### New Code (Lines 54-58)
```python=48
            if diff >= 1000:
                data += "1" 
            else:
                data += "0"
            print(data)
        
        if data.count("1") % 2 == 1:  # Throws an error if number of 1 bits is odd
            print("Data error detected.")  # Shows an error message
        else:  # If no error is detected
            data = data[:8]  # Slices to remove the parity bit
            char_code = int(data, 2)  # Converts into decimal character code
            character = chr(char_code)  # Converts character code into character  
            print("Received the character '{}'.".format(character))  # Shows which character was received
```

#### ■ Making the `main()` Function

Our `convert_char_to_signal()` and `receiver()` functions are now ready. Last, we’ll finish the program and make our communicator easier by adding the following to the `main()` function: an input prompt will appear when you press the A button, and the character the user types will be added to the `convert_char_to_signal()` function. We’ll then show the user the returned result before starting a new thread to run the `receiver()` function!

###### New Code (Lines 2-3, Lines 64-74)
```python=1
import time
import _thread  # Adds module to use multithreading
from pystubit.board import lightsensor, button_a  # Imports object to control the A button
```
```python=64
def main():
    _thread.start_new_thread(receiver, ())  # Runs on a separate thread
    
    while True:
        if button_a.is_pressed():  # Requests input when you press the A button
            character = input(“Type a character >>> ")
            if len(character) == 1:  # Won\’t convert two characters or more to avoid errors
                signal = convert_char_to_signal(character)
                print(signal)
            else:
                print("Please enter only one character.")  
```


### Checking the Program

Now we’ll run the finished program and call the `main()` function from the terminal. We’ll send the following characters as practice and see if our communicator receives them!

|Function|`convert_word_to_signal()` Return Value|
|:---:|:---|
|"A"|"010000010"|
|"a"|"011000011"|
|"?"|"001111110"|

##### Running the Code
<pre class="prettyprint">
>>> main()
Now waiting...
Type a character >>> a
011000011
Now receiving.
0
01
011
0110
01100
011000
0110000
01100001
011000011
Received the character 'a'.
</pre>

If you’re still encountering errors that you can’t fix, check your code for mistakes or compare it line by line with the finished example below!

##### Example Code 7-3-1

```python=1
import time
import _thread
from pystubit.board import lightsensor, button_a

threshold_low = 400  # Light sensor values are affected by your environment
threshold_high = 700  # so be sure to use your own thresholds here!

def convert_char_to_signal(character):
    char_code = ord(character)
    binary = bin(char_code)[2:]
    binary = "{:0>8}".format(binary)
    
    if binary.count("1") % 2 == 1:
        signal = binary + "1"
    else:  
        signal = binary + "0"
        
    return signal


def receiver():  
    while True:   
        print("Now waiting...")
        count = 0
        while True:
            time.sleep_ms(10)
            val = lightsensor.get_value()
            if val > threshold_high:
                count += 1
            elif val < threshold_low:
                count = 0
            
            if count == 200:
                while lightsensor.get_value() > threshold_low:
                    time.sleep_ms(10)
                break
        print("Now receiving.")

        data = ""
        while len(data) < 9:
            while lightsensor.get_value() < threshold_high:
                time.sleep_ms(1)
                pass
            start = time.ticks_ms()
            while lightsensor.get_value() > threshold_low:
                time.sleep_ms(1)
                pass
            diff = time.ticks_diff(time.ticks_ms(), start)
            if diff >= 1000:
                data += "1" 
            else:
                data += "0"
            print(data)
        
        if data.count("1") % 2 == 1:
            print("Data error detected.")
        else:
            data = data[:8]
            char_code = int(data, 2)
            character = chr(char_code)  
            print("Received the character '{}'.".format(character))
            

def main():
    _thread.start_new_thread(receiver, ())
    
    while True:
        if button_a.is_pressed():
            character = input(“Type a character >>> ")
            if len(character) == 1:
                signal = convert_char_to_signal(character)
                print(signal)
            else:
                print("Please enter only one character.")    
```

## Challenge: Transmitting Color Data with Light Signals

In order to send a character in the previous section, we transmitted nine bits of data including the parity bit. Communications systems in the real world can send much more complex things like pictures and videos by increasing the amount of data they send!

That’s why in this challenge we’ll modify Example Code 4-3-1 and make a system which can transmit colors of LED light. These require a lot more data than characters!

### Representing Color Data

Remember that we can express the color of an LED using an `(R, G, B)` tuple representing the three primary colors of light. You can set the strength of these RGB values on your Core Unit using a value from 0 to 31. The strongest value, 31, is **11111** when converted to binary. This means that we can use **five bits** to represent each color of light. This lets us **16 bits** of data to represent the `(R, G, B)` value when we include a parity bit!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-2/10-2_23_E.png"/>

We can take this concept and use it to modify Example Code 4-3-1, making our communicator receive RGB color data and light up the LED at the top left in the same color!

### Modifying the Program

While the processes of the program won’t change much, we’ll have to change the following points:

##### Changing the Program
* The `convert_char_to_signal()` function will now take data as an RGB tuple. We’ll also change the name of the function to `convert_rgb_to_signal`.
* The `receiver()` will turn on the LED in the color of the data it receives.
* The `main()` will change to run the `convert_rgb_to_signal` when you press the A button.

Now let’s rewrite our code in order!

#### ■ Changing the `convert_char_to_signal()` Function

Let’s start by changing the function’s name to `convert_rgb_to_signal` to reflect its new function. This function will also have to take an `(R, G, B)` tuple in its argument rather than character data. This means that we’ll have to use a `for` statement to pull out element at a time and convert them into binary strings in order. Each color in our RGB value needs to be five bits. If it’s shorter than five bits, we’ll pad it with "0"s.

###### Changed Code (Lines 8-18)
```python=8
def convert_rgb_to_signal(rgb):
    signal = ""  # Variable for signals
    for val in rgb:
        binary = bin(val) [2:]  # Converts to binary string and pulls out everything after “0b”
        binary = "{:0>5}".format(binary)  # Pads with "0" if shorter than 5 bits
        signal += binary
        
    if signal.count("1") % 2 == 1:  # Adds parity bit
        signal += "1"
    else:
        signal += "0"
        
    return signal
```

#### ■ Changing the `receiver()` Function

The only thing we need to do here is change the process to make it receive 16 bits of data instead of nine bits. If there are no errors after the parity check, it will take five bit slices of the number at a time starting at the beginning and convert them into decimal. Well then take these values and use them to make an `(R, G, B)` tuple which lights up the top left LED at `(0, 0)`.

###### New and Changed Code (Line 3, Line 42, Lines 59-64)
```python=1
import time
import _thread
from pystubit.board , import lightsensor, button_a  # Imports a new object to control the LED display
```
```python=23
def receiver():    
    while True:         
        print("Now waiting...”)
        count = 0
        while True:
            time.sleep_ms(10)
            val = lightsensor.get_value()
            if val > threshold_high:
                count += 1
            else:
                count = 0
            
            if count == 200:
                while lightsensor.get_value() > threshold_low:
                    time.sleep_ms(10)
                break
        print("Now receiving.")

        data = ""
        while len(data) < 16:  # Change this to 16 bits
            while lightsensor.get_value() < threshold_high:
                time.sleep_ms(1)
                pass
            start = time.ticks_ms()
            while lightsensor.get_value() > threshold_low:
                time.sleep_ms(1)
                pass
            diff = time.ticks_diff(time.ticks_ms(), start)
            if diff >= 1000:
                data += "1" 
            else:
                data += "0"
            print(data)
        
        if data.count("1") % 2 == 1:
            print("Data error detected.")
        else:       
            val_r = int(data[:5], 2)  # Retrieves the R value and converts it into decimal
            val_g = int(data[5:10], 2)  # G value
            val_b = int(data[10:15], 2)  # B value
            print("({}, {} ,{})".format(val_r, val_g, val_b))  # Shows received color data in the terminal to test program
            display.set_pixel(0, 0, (val_r, val_g, val_b))  # Lights up top left LED in the display
```

#### ■ Changing the `main()` Function

We’ll change this function to show tuples in the terminal rather than character data. The data retrieved by the `input()` function, however, is handled as a text string. This means that data typed into the terminal will turn into text data as shown below:

<pre class="prettyprint">
Input rgb &gt;&gt;&gt; (31, 31, 31)  # Becomes the text string "(31, 31, 31)"
</pre>

While we could snip the numbers out of the string in order, convert them into an integer using the `int()` method, and put them together in a tuple, this would be a lot of extra steps. What you may not know is that Python has a **convenient `eval()` function which can evaluate formulas in text strings**.

If we were, for example, to put a text string showing a mathematical formula into the `eval()` function’s argument, it would evaluate the operation and return the result!

<pre class="prettyprint">
&gt;&gt;&gt; eval("2+3")
5
</pre>

If you set a text string formatted as a tuple as an argument, it will evaluate it in the same way and return a tuple!

<pre class="prettyprint">
&gt;&gt;&gt; eval("(1, 2, 3)")
(1, 2, 3)  # The result is no longer "(1, 2, 3)", making it a tuple instead of a text string
</pre>

Now let’s take the `eval()` function and use it to convert the text strings we type in the terminal into tuples that we can pass to the `convert_rgb_to_signal()` function!

```
rgb = input("Type an RGB value >>> ")
rgb = eval(rgb)
```

Making these changes will make your code look like the program below. The `(R, G, B)` tuple that you need to type has three elements, so let’s treat anything other than three elements as an input error:

###### Changed Code (Lines 72-78)

```python=67
def main():
    _thread.start_new_thread(receiver, ())
    
    while True:
        if button_a.is_pressed():
            rgb = input("Type an RGB value >>> ")
            rgb = eval(rgb)  # Evaluates the string as a formula
            if len(rgb) == 3:  
                signal = convert_rgb_to_signal(rgb)
                print(signal)
            else:  # Treats a tuple with less than three elements as an input error
                print("Enter a tuple with three elements.") 
```

### Checking the Program

And now we’ve finished changing our program! Try running the finished product and see how it works!

##### Example Code 8-3-1

```python=1
import time
import _thread
from pystubit.board import button_a, lightsensor, display

threshold_low = 400
threshold_high = 700

def convert_rgb_to_signal(rgb):
    signal = ""
    for val in rgb:
        binary = bin(val) [2:]
        binary = "{:0>5}".format(binary)
        signal += binary
        
    if signal.count("1") % 2 == 1:
        signal += "1"
    else:
        signal += "0"
        
    return signal


def receiver():    
    while True:         
        print("Now waiting...")
        count = 0
        while True:
            time.sleep_ms(10)
            val = lightsensor.get_value()
            if val > threshold_high:
                count += 1
            else:
                count = 0
            
            if count == 200:
                while lightsensor.get_value() > threshold_low:
                    time.sleep_ms(10)
                break
        print("Now receiving.")

        data = ""
        while len(data) < 16:  # Data is 16 bits including the parity bit
            while lightsensor.get_value() < threshold_high:
                time.sleep_ms(1)
                pass
            start = time.ticks_ms()
            while lightsensor.get_value() > threshold_low:
                time.sleep_ms(1)
                pass
            diff = time.ticks_diff(time.ticks_ms(), start)
            if diff >= 1000:
                data += "1" 
            else:
                data += "0"
            print(data)
        
        if data.count("1") % 2 == 1:
            print("Data error detected.")
        else:       
            val_r = int(data[:5], 2)
            val_g = int(data[5:10], 2)
            val_b = int(data[10:15], 2)
            print("({}, {} ,{})".format(val_r, val_g, val_b))
            display.set_pixel(0, 0, (val_r, val_g, val_b))


def main():
    _thread.start_new_thread(receiver, ())
    
    while True:
        if button_a.is_pressed():
            rgb = input("Type an RGB value >>> ")
            rgb = eval(rgb)
            if len(rgb) == 3:
                signal = convert_rgb_to_signal(rgb)
                print(signal)
            else:
                print("Enter a tuple with three elements.")     
```

## Conclusion

### A Quick Review

In this lesson we learned how to represent volumes of data as well as how to show text data on a computer.

The volume of every piece of data on a computer, including numbers, letters, images, and videos, is shown in units called bits and bytes.

You can also use operators like `&`, `|`, `^`, and `~` to perform logic operations on data one bit at a time!

In the second half of the lesson, we introduced how real-world communication converts data into electrical and light signals before sending it. We then made a simple system which used light to send and receive single-byte characters!

Along with Topic 10-1, this lesson gave us a closer look at how computers handle data internally. While many of these concepts are quite difficult, being a good engineer is more than just memorizing a programming language: it’s also understanding the fundamentals of how computers work!

The concepts we’ll explore in Topics 10-3 and 10-4 are a lot more complex, so be sure not to give up as you take on these new challenges!

### Coming Up Next Time!

In the next lesson we’ll learn techniques to organize and save information in a **database** as well as how to use **regular expressions** to search for patterns in specific pieces of text!
