---
tags: Python_English, Topic 5
---

Python Robotics Course Lesson 19

Topic 5-3
Input and Output
===

Let’s learn how to use files to import and export data!

## In This Lesson...

In this lesson you’ll learn how to import, or **load**, information from a file, and export, or **save** information to a file! We’ll also learn how to use Mu’s native plotter feature to make line graphs out of our data!

In the last half of the lesson, we’ll load a musical score from a text file and make a digital music box that plays the exact same notes!

<iframe src="https://player.vimeo.com/video/482883044" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

:::info
Lesson Introduction

■ Lesson 19 Contents
1. Saving and Loading Data
2. Using the Data Plotter
3. Making a Digital Music Box
4. Advanced Learning: More on Loading Data 



In this lesson you’ll learn how to import, or **load**, information from a file, and export, or **save** data to a file! We’ll also learn how to turn the data in our program editor into a graph.

We'll start off by introducing some new Python syntax elements.

Your computer will store the data inside of a variable temporarily, but it’s deleted once your program is finished or if you turn you computer off. This is why Python allows you to store data by saving it to an external file, and load data from any file you’ve saved. In this lesson we’ll learn how to save data to and load data from a file!

Next, we’ll also learn how to use the native plotter feature in our program editor to graph our data. Graphing your sensor values makes it easier to understand how they change.

In the second half of the lesson, we’ll get a handle on our new syntax as we make a program that loads song data from a text file and plays each note in order using the Buzzer. This allows you to easily change the song the program plays by changing which text file you load, just like changing the disc on a disc music box!

Finally, we’ll take a closer look at loading data and do some advanced learning by adding a section to your program that allows you to change the tempo of the song!

Now let's start the lesson!

:::

## New Python Syntax

Let's start off by introducing some new Python code you'll be using for the first time in this lesson.

### Saving and Loading Data
Let’s take a moment and think about all of the sensors in your ArtecRobo kit. Good examples of this would be using your Temperature Sensor to measure changes in temperature, or using your Light Sensor to tell when the sun rises and sets!

You can save your sensor values temporarily by using something like a variable or a list, but these values will disappear as soon as you turn your Core Unit off!

This is why Python allows you to store data by saving it to an external file, and load data from any file you’ve saved. Using external files lets you store any data you might need for a long, long time! Now let’s find out how exactly we can do it.

#### ■ Working with Files 1. Opening a File

In Python, you can use a basic `open()` function to make a new file or even open a file you’ve already saved. The return value of an `open()` function is what we call a **file object**.

```
open(fileName,mode)
```

The first argument of the `open()` function is where you set the name of the file you want to open. If the file you want to open is in a different folder than the program you’re running, you can just add a **path** that points to the folder.

Use a text string in the second argument of the function to set the access mode you’ll use to load the file. Take a look at the table below to see which modes you can set! The most common modes you’ll use are `w` to save a new file, `a` to **append** (add) data to a file, and `r` to open a file as read-only!

##### `open()` Argument Access Modes

|Mode Text String|Meaning|
|:---:|:---|
|'r'|Load a file as read-only. This is the default mode.|
|'w'|Open a file for writing only. This will overwrite any existing files!|
|'x'| Open a file for writing only. This causes an error if you use it with an existing file! |
|'a'|Open a file as write only and append any new data to the end of the file.|
|'+'| Use this to open a file that you can read and write to, allowing you to update it! |
|'t'|Open a file in text mode.|
|'b'|Open a file in binary mode.|
|'U'| Open in universal newline mode (we won’t be needing this one!)|

The `open()` function has a lot more arguments, but we’re giving a short overview here since we won’t be using them all.

Now let’s run the following code in your terminal. This creates and opens a new text file called **sample_file.txt** and saves a file object in a variable called `file`.

<pre class="prettyprint">
>>> file = open("sample_file.txt", "w")
</pre>

###### ★ Since we’re making a new file, we’ll open it in `w` mode.

#### ■ Working with Files 2. Saving a File

Next, let’s take a look at how we can save data to a file!

File objects have a `write()` method that allows you to do this. 

```
write(text string that you want to save)
```

This will save the text string you specified in the argument to an open file. Now, let’s run the following code in the terminal to save the text string **Hello Python!** to the file.

<pre class="prettyprint">
>>> file.write("Hello Python!")
13
</pre>

Run this code and you’ll see the integer **13**. This tells you the length of the text string that the `write()` method saved!

#### ■ Working with Files 3. Closing a File

Now that we’ve saved data to a file, let’s close it! We’ll need to call the file object’s `close()` method in order to do this. Always remember to close a file once you’ve finished with it, otherwise you might lose all of your data!

Try running the following code in your terminal.

<pre class="prettyprint">
>>> file.close()
</pre>

#### ■ Working with Files 4. Loading a File

**Loading** a file is when you open a file that you’ve already created. Let’s check where we saved the file before we start writing a program to load it.

Start by clicking **File** in the top bar of your editor.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_1_E.png"/>

You’ll see a list of files saved on your Core Unit at the bottom left of the window. Now see if you can find the file you saved!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_2_E.png"/>

Click the **File** button at the top of the screen one more time to close the window.

Now let’s open this file to load the data.

This time we’ll have to use ``’r’`` access mode to open the file as read-only. Write the following code into your editor,

```python==1
file = open("sample_file.txt", "r")
```

You’ll need to use file object methods like `read()` or `readline()` when you’re loading data from a file.

```
read(size of the data you want to load)
```
Loads and returns the amount of data specified in the argument. Leave this argument out and you’ll load the entire file!


```
readline()
```
Loads and returns a single line from the file.


Here we’re going to use the `read()` method to load and show data from our file! When you’re finished, don’t forget to close your file using the `close()` method!

##### Example Code 2-1-1
```python==1
file = open("sample_file.txt", "r")
data = file.read()
print(data)
file.close()
```

Try running this code. If you see the **Hello Python!** from your previous code appear in the terminal, it worked!

##### Program Results

<pre class="prettyprint">
Hello Python!
</pre>


#### Closing a File Using with Statements

So now you know that you’ll have to call a `close()` method every time you use the `open()` function to open a file. Kind of a pain, right? And if you forget to close a file, you might get a nasty surprise if you try to work with that file again! In order to prevent these sorts of errors, Python lets you use **with** statements to automatically close a file once it’s been processed. Look below to find out how to write a **with** statement!

```
with open(fileName,mode) as variable that stores the file object:
    .
    .
    .
```

Just like the code for **if** and **for** statements, you’ll need to indent the code that you want to run after you open and before you close the file. If we rewrite Example Code 2-1-1 using a **with** statement, it'll look like this:

##### Example Code 2-1-2

```python==1
with open("sample_file.txt", "r") as file:
    data = file.read()
    print(data)
```

As you can see, the processes needed to handle the file are much easier to understand than example 2-1-1!

### Using the Data Plotter

Did you know that Mu Editor has a plotter? This allows you to use `print` to regularly output numerical values and show the changes in those values using a line graph!

Let’s try doing this by graphing the changes in your Core Unit’s Light Sensor values.

We’ll start by writing code that retrieves the value of the Light Sensor every second.

```python==1
from pystubit.board import lightsensor
import time

while True:
    v = lightsensor.get_value()
    time.sleep_ms(1000)
```

Next, we’ll output the values using a `print` statement, but we need to keep one thing in mind. **We’ll need to use tuples** to graph this data, even if we only have one data point! That means we’ll need to add a `print` statement in the following format.

###### New Code (Line 6)

```python==1
from pystubit.board import lightsensor
import time

while True:
    v = lightsensor.get_value()
    print((v, ))
    time.sleep_ms(1000)
```

###### ★ Writing `(v, )` makes a tuple with only one data point.

Let’s run the program and click the **Plotter** button in the top bar of Mu. You’ll see a graph appear at the bottom right of the screen!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_3_E.png"/>

Try covering the Light Sensor with your hand and see if the data changes to a graph like the one in the picture here!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_4_E.png"/>

Using graphs makes it much easier to grasp how your sensor values change, so be sure to use them whenever you can!

## Making a Digital Music Box
The music box you see in the picture below is known as a cylinder music box. When the pins on the cylinder rotate, they pluck the pins on the comb to make music! In this sort of music box, the song on the comb and the comb that plays it come as a set. This means that it can only play one song!

##### A Cylinder Music Box

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_5_E.png"/>

A **disc music box**, however, lets you swap special discs with holes in them to play different songs! Discs music boxes are kind of like the CD and DVD players of today. Unlike a cylinder music box, the disc that stores the music and the player are separate objects!

##### A Disc Music Box
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_6_E.png"/>

And now we’re going to make a digital music box that has separate parts for the music and the player, just like a disc music box! To be exact, we’ll make the player by using a Python program to load the song data stored in a simple text file. Then we’ll play the song using your Core Unit’s Buzzer!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_7_E.png"/>

###### ★ **.txt** is the extension used for text files, while **.py** is used for Python program files!

### Saving Your Sample Song
Just as an example, we’ll use a sample text file containing the song data for **Amazing Grace** to check out how the program we’re going to make works! Let’s take the following steps to save the program to our Core Unit.

#### ■ Downloading the Sample Song
Right-click the **amazing_grace.txt** link below and choose **Save As...** in the menu. Check where the destination folder is before saving the folder.

[amazing_grace.txt](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/amazing_grace.txt)

###### ★ If you don’t have a download folder in mind, just save the file to your desktop!

The data inside of the text file looks like this!

##### A Look Inside of amazing_grace.txt
<pre class="prettyprint">
D4,eighth_note
G4,eighth_note
G4,half_note
B4,sixteenth_note
A4,sixteenth_note
G4,eighth_note
B4,half_note
A4,eighth_note
G4,eighth_note
G4,half_note
E4,quarter_note
D4,half_note
D4,eighth_note
G4,eighth_note
G4,half_note
B4,sixteenth_note
A4,sixteenth_note
G4,eighth_note
B4,half_note
A4,eighth_note
B4,eighth_note
D5,dotted_half_note
D5,half_note
B4,eighth_note
D5,eighth_note
D5,half_note
B4,eighth_note
A4,eighth_note
G4,eighth_note
B4,half_note
A4,quarter_note
G4,half_note
E4,quarter_note
D4,half_note
D4,eighth_note
G4,eighth_note
G4,half_note
B4,sixteenth_note
A4,sixteenth_note
G4,eighth_note
B4,half_note
A4,quarter_note
G4,dotted_half_note
G4,half_note
</pre>

#### ■ Saving the Song File to the Core Unit
Let’s take the following steps to transfer the **amazing_grace.txt** file you just downloaded to your Core Unit!

Make sure your Core Unit is connected to your PC, then click **Files** in the top bar of Mu. You’ll see a small window appear at the bottom right of the screen. The only files shown here are ones saved in a specific folder on your PC!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_8_E.png"/>

We’ll need to move the file we just downloaded into this folder. You can find this folder by clicking **Load** in the top bar. The location of the folder will be in the address bar of the window that appears!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_9_E.png"/>

Click the address bar to show the exact folder path. Now copy it!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_10_E.png"/>

Open Windows Explorer and paste the path we just copied into the address bar. Hit the **Enter** key to go to that folder. Now put the text file into this folder!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_11_E.png"/>

Go back to Mu Editor and click the **Files** button in the top bar to close the window. Click **Files** one more time to open the window and you’ll see the text file we just added! Drag and drop this file into the window on the left to copy the file.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_12_E.png"/>

You’ll see a status message at the bottom of the window as the file is copied. Once this message disappears, you’re done! And now we’re ready to play a song.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_13_E.png"/>


### ■ Program Outline
Next, let’s take a look at how our program is going to work. The flowchart below shows a simplified overview of the program we’ll be making.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_14_E.png"/>

This program is divided into two parts: the first part will define a couple of data points, and the second will load data from the file and play songs using the Buzzer!

### Defining the Note Data

We’ll find the song data in the text file, with each note to be played starting from the top and separated line by line. Look closely at a line and you’ll see that the note and the note length are separated, or **delimited** by a comma!

<pre class="prettyprint">
D4,eighth_note
G4,eighth_note
G4,half_note
.
.
.
</pre>

But our program still won’t know how long it should play each note! This means we’ll have to use Python to convert the note length into a specific time! We’ll make the base note a **quarter_note** with a length of 600 milliseconds, then use this to define the length of the other notes!

We’ll need to remember a couple of things: namely, that dotted notes are 1.5 times the length of a standard note, and that you can only use whole numbers in the `duration` argument of the **StuduinoBitBuzzer** class’s `on()` method. This means that, since we’ll get a floating point number as a result when we use division or multiply with decimal points, we’ll need to use the `int()` function to cut off any numbers after the decimal point and make a whole number!

```python==1
quarter = 600                              # This is a quarter note!
whole = quarter * 4                        # This is a whole note!
half = quarter * 2                         # This is a half note!
dotted_half = quarter * 3                  # This is a dotted half note!
dotted_quarter = quarter * 1.5                  # This is a dotted quarter note!
eighth = int(quarter / 2)                  # This is an eighth note!
dotted_eighth = (quarter / 2 * 1.5)                  # This is a dotted eighth note!
sixteenth = int(quarter / 4)               # This is a sixteenth note!
dotted_sixteenth = int(quarter / 4 * 1.5)  # This is a dotted sixteenth note!
```

Let’s make a dictionary called `notes_list` that uses the names for each note length as keys and the lengths of time we defined above as items. This way, we can load the text file and just put the name of the note length in the `[]` of `note_length[]` to get the length of time!

###### New Code (Line 11, Line 21)
```python==1
quarter = 600
whole = quarter * 4
half = quarter * 2
dotted_half = quarter * 3
dotted_quarter = int(quarter * 1.5)
eighth = int(quarter / 2)
dotted_eighth = int(quarter / 2 * 1.5)
sixteenth = int(quarter / 4)
dotted_sixteenth = int(quarter / 4 * 1.5)

notes_list = {
    'whole_note': whole,
    'half_note': half,
    'dotted_half_note': dotted_half,
    'quarter_note': quarter,
    'dotted_quarter_note': dotted_quarter,
    'eighth_note': eighth,
    'dotted_eighth_note': dotted_eighth,
    'sixteenth_note': sixteenth,
    'dotted_sixteenth_note': dotted_sixteenth,
}
```


### Importing a Text File to Control Your Buzzer

Now that we’re ready to go, let’s load the **amazing_grace.txt** text file you stored on your Core Unit and call each piece of data in order to be played by the Buzzer.

#### ■ Picking Note and Length Data from the File

Let’s start by importing a `buzzer` object.

###### New Code (Line 1)
```python==1
from pystubit.board import buzzer
```

Now use a **with** statement to run an `open()` function to open the text file. We’ll also need to use an **as** statement to store the file object we just opened inside of a variable called `file`.

###### New Code (Line 25)
```python==25
with open('amazing_grace.txt') as file:
```

The next step is to run a `readline()` method inside of our **with** statement to import the text strings inside of the file one line at a time. If you find that you’ve imported a blank line, try using a **break** to escape the loop. Check to see whether your program is working correctly by using a **print** statement!

###### New Code (Lines 26-49)
```python==25
with open('amazing_grace.txt') as file:
    while True:
        line = file.readline()
        if line == "":  # Uses a break to escape the loop if `line` is a blank text string
            break
        print(line)
```

When you run the program, the following text will appear in your terminal. While everything may look alright at a glance, notice how there’s a blank line between every line.

##### Program Results
<pre class="prettyprint">
D4,eighth_note

G4,eighth_note

G4,half_note

B4,sixteenth_note
.
.
.
</pre>

The `print()` function designates a line by adding a special **control character** to the end of each line. This means that each time it runs, a new line is created before appearing in the terminal. But looking at the results above, we can see that it creates two lines instead of one. The reason for this is because there’s already the same special control character at the end of the strings in the `line` variable, before the `print()` function even adds a control character to the line!

Using the `split()` method to convert a single-line text string to a list and then showing that list allows us to see `\n` or `\r\n` strings at the end of each line.

##### New and Changed Code (Lines 30-31)
```python==25
with open('amazing_grace.txt') as file:
    while True:
        line = file.readline()
        if not line:
            break
        note = line.split(',')
        print(note)
```

##### Program Results
<pre class="prettyprint">
['D4', 'eighth_note\n']
['G4', 'eighth_note\n']
['G4', 'half_note\n']
['B4', 'sixteenth_note\n']
.
.
.
</pre>

The expression **\n** is known as a **line field**, while **\r** is known as a **carriage return**. Here’s what each one does:

* Line Field (`\n`)
This special character means "send the cursor to the next line." It moves the cursor to the exact same position on the line below it.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_15_E.png"/>

* Carriage Return (`\r`)
This special character sends the cursor back to the start of the line.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_16_E.png"/>


You can think of moving to a new line as a combination of sending the cursor to the next line and placing it at the start of that line. In other words, you can make a new line by combining a line field with a carriage return.

This is true for Windows, which shows lines using `\r\n`. Mac OS, a Unix-based operating system, shows lines using `\n` because it doesn’t need carriage returns.
 

These special characters get in the way when we’re defining the keys in `notes_list`, and we can use a text string’s `replace()` method to get rid of them!

###### New Code (Lines 30-31)
```python==25
with open('amazing_grace.txt') as file:
    while True:
        line = file.readline()
        if not line:
            break
        line = line.replace('\n','')    # Converts `\n` to an empty string ``
        line = line.replace('\r','')     # Converts `\r` to an empty string ``
        note = line.split(',')
        print(note)
```

Run this program and you’ll see that it deletes both `\n` and `\r`!

##### Program Results
<pre class="prettyprint">
['D4', 'eighth_note']
['G4', 'eighth_note']
['G4', 'half_note']
['B4', 'sixteenth_note']
.
.
.
</pre>

■ Controlling a Buzzer with Fetched Notes and Lengths

Now that we’re here, the only thing we need to do is using the `buzzer` object’s `on()` method to play the notes we loaded from the file!

We’ll find the list data we need inside of the `note` variable, formatted as `[note,length]`. That means we can use `note[0]` to fetch the note and `note[1]` to fetch the length of that note. We’ll have to take our `notes_list` list and use `notes_list[note[1]` to convert the note length to the right length of time. Now all we need to do is set both of these as arguments in our `on()` method and our program is finished! Try running it to see how it works!

##### Example Code 3-5-1
###### New Code (Line 34)
```python==1
from pystubit.board import buzzer

quarter = 600
whole = quarter * 4
half = quarter * 2
dotted_half = quarter * 3
dotted_quarter = int(quarter * 1.5)
eighth = int(quarter / 2)
dotted_eighth = int(quarter / 2 * 1.5)
sixteenth = int(quarter / 4)
dotted_sixteenth = int(quarter / 4 * 1.5)

notes_list = {
    'whole_note': whole,
    'half_note': half,
    'dotted_half_note': dotted_half,
    'quarter_note': quarter,
    'dotted_quarter_note': dotted_quarter,
    'eighth_note': eighth,
    'dotted_eighth_note': dotted_eighth,
    'sixteenth_note': sixteenth,
    'dotted_sixteenth_note': dotted_sixteenth,
}

with open('amazing_grace.txt') as file:
    while True:
        line = file.readline()
        if not line:
            break
        line = line.replace('\n','')
        line = line.replace('\r','')
        note = line.split(',')
        print(note)
        buzzer.on(note[0], duration=notes_list[note[1]])
```

## Challenge: Changing the Length of a Quarter Note to Change the Speed of the Song


In the program we just made, we can’t change the playback speed of the song because the length of the quarter note is decided by the Python program itself. However, if we modify our code from **3-5-1**, we can change the playback speed every time we load the text file by putting the length of the quarter note into the text file itself!

###### ★ The quarter note is the base note, determining the length of every other note.z This means that changing the quarter note changes the speed of the whole song!


<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/5-3_17_E.png"/>

For this challenge you’ll need the text file containing the song data below. This adds **300** on the first line of the text file we used before, which will set the length of the quarter note in milliseconds. Other than, every other line is exactly the same as before. Follow the link below and use the same steps from earlier to download **amazing_grace_for_challenge.txt** and transfer it to your Core Unit.

[amazing_grace_for_challenge.txt](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_5-3/amazing_grace_for_challenge.txt)

##### 【 amazing_grace_for_challenge.txt 】
<pre class="prettyprint">
300
D4,eighth_note
G4,eighth_note
G4,half_note

And the rest is identical to the first file!
.
.
.
</pre>

##### Helpful Hints
* Try putting the code which defines the note data into a function called `define_notes()` and the code which loads the file and plays the Buzzer into a function called `play_music_box()`!



* Take the length of the quarter note as an argument in `define_notes()` and use it to calculate the length of the other notes. You’ll also need to call this function inside of the `play_music_box()` function.

* Take the file name of the song you’ll be playing as an argument of the `play_music_box()` function. It has to open the file and perform a special process only for the first line of the program. This process will pass the length of the quarter note to the `define_notes()` function.

Both functions will use your `notes_list` dictionary, so you’ll have to declare it as a global variable. Make this declaration inside of `define_notes()` and add the data there.

* If you want to test out your program, click the **Run** button in the top bar and type the following into the terminal to run it.

<pre class="prettyprint">
>>> play_music_box('amazing_grace_for_challenge.txt')
</pre>


### Example Program

You can make the program for this challenge by following the steps below.

#### ■ Defining `define_notes()`
The data used to define notes all fits inside of the `define_notes()` function. However, since the list `notes_list` will be used by another function in your program, you’ll have to declare it with a global scope to make it a global variable. Store the data in this variable after making the global declaration in order to keep it from being treated as a local variable inside of the function.

Remember to take the length of the quarter note as an argument in `define_notes()`. The example code **3-4-1** you put the length of the quarter note in the `quarter` variable, but instead you’ll make it an argument that you can use to calculate the length of the other notes.

Making these changes will make your code look like this.

###### New and Changed Code (Line 3, Line 5-13, Line 15)
```python==1
from pystubit.board import buzzer

notes_list = []    #  Declares this as a global variable

def define_notes(quarter):    # Changes quarter to an argument of the function
    whole = quarter * 4
    half = quarter * 2
    dotted_half = quarter * 3
    dotted_quarter = int(quarter * 1.5)
    eighth = int(quarter / 2)
    dotted_eighth = int(quarter / 2 * 1.5)
    sixteenth = int(quarter / 4)
    dotted_sixteenth = int(quarter / 4 * 1.5)

    global notes_list    # Declares as a global list
    notes_list = {
        'whole_note': whole,
        'half_note': half,
        'dotted_half_note': dotted_half,
        'quarter_note': quarter,
        'dotted_quarter_note': dotted_quarter,
        'eighth_note': eighth,
        'dotted_eighth_note': dotted_eighth,
        'sixteenth_note': sixteenth,
        'dotted_sixteenth_note': dotted_sixteenth,
    }
```

#### ■ Defining the `play_music_box()` Function

Put the code which loads the file and plays the Buzzer into a function called `play_music_box()`. This function takes the name of the file as an argument and uses it to open and load the file.

Only the first line of this file shows the length of the quarter note, while all other lines show the note and the note length. This means we’ll need to do something special for the first line!

In order to do that, we’ll have to make a variable called `count` to count the number of lines. Now we can run `define_notes(line)` when the first line is loaded and store the length of each note in `notes_list`.

Every line after this will play the loaded notes from the Buzzer, just like in example **3-5-1**.

Making these changes will make your code look like this.

##### New and Changed Code (Lines 28-30, Line 33, Lines 38-43)
```python==28
def play_music_box(file_name):
    with open(file_name) as file:    # Set the name of the file as an argument to open it
        count = 0    # Use the count variable to count the number of lines loaded
        while True:
            line = file.readline()
            count += 1    # Counts the number of lines loaded
            if not line:
                break
            line = line.replace('\n', '')
            line = line.replace('\r', '')
            if count == 1:    # This process runs when line 1 is loaded
                define_notes(int(line))  # The line variable is a text string. Use the `int()` function to convert it into numbers.
            else:    # This process runs for every line after line 1
                note = line.split(',')
                print(note)
                buzzer.on(note[0], duration=notes_list[note[1]])
```

Now your program is finished! The entire program will look like this one you’re done:

##### Example Code 4-1-1
```python==1
from pystubit.board import buzzer

notes_list = []

def define_notes(quarter):
    whole = quarter * 4
    half = quarter * 2
    dotted_half = quarter * 3
    dotted_quarter = int(quarter * 1.5)
    eighth = int(quarter / 2)
    dotted_eighth = int(quarter / 2 * 1.5)
    sixteenth = int(quarter / 4)
    dotted_sixteenth = int(quarter / 4 * 1.5)

    global notes_list

    notes_list = {
        'whole_note': whole,
        'half_note': half,
        'dotted_half_note': dotted_half,
        'quarter_note': quarter,
        'dotted_quarter_note': dotted_quarter,
        'eighth_note': eighth,
        'dotted_eighth_note': dotted_eighth,
        'sixteenth_note': sixteenth,
        'dotted_sixteenth_note': dotted_sixteenth,
    }

def play_music_box(file_name):
    with open(file_name) as file:
        count = 0
        while True:
            line = file.readline()
            count += 1
            if not line:
                break
            line = line.replace('\n', '')
            line = line.replace('\r', '')
            if count == 1:
                define_notes(int(line))
            else:
                note = line.split(',')
                print(note)
                buzzer.on(note[0], duration=notes_list[note[1]])
```

## Conclusion
### A Quick Review
In this lesson we learned...

* How to open and load data from external files and how to save data to a file.
*  How to use the `open()` function to open a file. `open()` functions return file objects.
* You can use the `write()` method with file objects to save data to a file, and methods like `read()` and `readline()` to load data from a file.
* How to use the `close()` method to close files.
* How to use **with** statements to close a file automatically once you’re done using it.
* About the native Plotter feature of Mu Editor.

You also used subjects you’ve learned in previous lessons for this project, like break statements in loops and scope! The program you made in this challenge was pretty advanced, but it was a good chance to put what you’ve learned to far to the test.

As you get a grasp of more advanced syntax, you’ll find that your code for complex processes will become much more concise. The syntax you’ll learn will only get more advanced from here, so keep trying until you get the hang of it!

### Coming Up Next Time!
In the next lesson we’ll learn how to use the terminal to input data into a program you’re currently running, and how to fix errors in your program without having to stop it!

