---
tags: Python_English, Topic 1
---

Python Robotics Course Lesson 4

Topic 1-4
Objects in Python
===

Learn the basics of Python objects!

## In This Lesson...
Python is what we call an **object-oriented** programming language. In this lesson you'll learn the basics of the **objects** mentioned in that term. At the end of the lesson, you'll use objects to make the Core Unit's LED display into an electronic welcome sign!

:::info

Lesson Introduction

■ Lesson 4 Contents

1. Objects in Python
2. Using Methods
3. Challenge: An Electronic Welcome



Python is what we call an **object-oriented programming language**. In this lesson you'll learn what these "objects" are and how they can manage variables and functions.

We'll start by explaining the concept of objects and the methods and properties they contain.

Next we'll talk about strings as an example of a common type of object and practice programming using their methods.

At the end of the lesson, you can try using string methods yourself

to make a welcome message scroll across the Core Unit's display, just like in this video.

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!
:::


## Objects in Python
In programming, the term **object** refers to a collection of data, including variables and functions. Python in particular treats all kinds of data, including numbers and strings, as objects. Variables that belong to an object are called **properties**, and functions that belong to an object are called **methods**. To understand how objects work, let's think of a person as an object, and see what a person's properties and methods would be.

### An Object's Properties
A property is a **type of quality** an object can have, the way a person has types of qualities like a name, an age, a gender, or a home address. Likewise, every object in Python has a set group of qualities it can have. For example, image data (like a photo or diagram) has qualities like height, width, color, and date created.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-4/1-4_1_E.png"/>

### An Object's Methods
Methods are kinds of **actions an object can do**, in the same way a person can do things like eat, sleep, or talk. Objects in Python all have various methods they can use to do different things. If you're using an image file on a computer and editing it (cropping it, adjusting brightness, resizing it, etc.) or saving the edits, you're using that image's methods.

#### ■ How is a Method Different from a Function?
Methods and functions serve very similar purposes, but you need to use a different format to call them in your program.

To call a function like `print()`, `int()`, or the others we've used so far, you write it like this:

```
Function name(Parameter 1, Parameter 2, ...)
```

To call a method, you write it like this instead:

```
Object name.Method name(Parameter 1, Parameter 2, ...)
```

Methods are different from functions because they belong to specific objects. The format for methods should be read like "run object A's method B," so the **dot** between the object name and the method name acts like a possessive **'s**.

### Programming with Objects
Programming using special features of objects like methods and properties is called **object-oriented programming**. There are many other important concepts in object-oriented programming, like  **inheritance**, **encapsulation**, and **polymorphism**, but for now just remember that objects have properties and methods, and we'll get to the other concepts later in the course!


## Using String Methods
As we mentioned earlier, everything in Python is an object, including numbers. That means that numbers and strings have their own methods! In this chapter we'll talk about a few easy-to-use methods for strings. If you want to learn more methods for numbers and strings, you can check out the official Python documentation (though this is a bit of an advanced topic at this stage).

[For Numeric Data - Python 3.6 Documentation](https://docs.python.org/ja/3.6/library/stdtypes.html#numeric-types-int-float-complex)

[For String Data - Python 3.6 Documentation](https://docs.python.org/ja/3.6/library/stdtypes.html#text-sequence-type-str)



### Capitalization Methods
The method `lower()` makes uppercase letters into lowercase letters, while the method `upper()` makes lowercase letters uppercase! Try typing the code below into your editor to see how these methods work.

##### 【 Example Code 3-1-1】
```python=1
text = 'Python'
low = text.lower()
up = text.upper()
print(low)
print(up)
print(text)
```

When you run this code, the following results should appear in your terminal:

<pre class="prettyprint">
python
PYTHON
Python
</pre>

The code we used gives the command to use the methods `lower()` and `upper()` to capitalize and uncapitalize the string data `'Python'` from the string object `text`. Unlike when you use built-in functions like `print()`, you don't need to specify the string as a parameter in each method separately!

These two methods give you capitalized and uncapitalized strings as their return values, but the original string data stored in the object remains unchanged. That's why when you run `print(text)` at the end of the program, it prints the string ``Python` with the same capitalization you used when you assigned it as the value of **text** at the start.

Methods like these that don't overwrite the data you put into them are called **non-destructive methods**. Some other non-destructive methods you can use for strings are `title()` (to put a string into title case) and `capitalize()` (to capitalize only the first letter of a string).

If you do want to replace the string data stored in the text object, you can use the = operator to replace text's value with the method's return value, like this:

<pre class="prettyprint">
text = text.lower()
</pre>

### Searching for Text in Strings
The `find()` method lets you search a string for specific text and characters. Set the text you want to search for as a string in the **find()** method's parameters, and the method will tell you where in the original string that text can be found by counting letters from the start of the string and giving you the position as an integer.

Try running the code below to see how this works!

##### 【Example Code 3-2-1】
```python=1
text = 'tomato'
print(text.find('a'))
print(text.find('o'))
print(text.find('p'))
```

##### （Program Results）
<pre class="prettyprint">
3
1
-1
</pre>

You'll notice these results say the letter a is 3rd in this string, one less than you'd expect. This is because Python counts the first position is the string as **0**, not 1! If the text you're searching for appears multiple times in the string, like o here, the method tells you the position of the first instance of the text it finds (in this case **1**). If the text doesn't appear anywhere in the string, the return value will be **-1**.

Another method you can use to search for text in a string is `count()`, which tells you how many times the text you specify in its parameter appears in the original string.

##### 【Example Code 3-2-2】
```python=1
text = 'tomato'
print(text.count('o'))
```

##### （Program Results）
<pre class="prettyprint">
2
</pre>

### Replacing Text in Strings
The `replace()` method lets you change the text in a string. This method takes two parameters, one to specify the text you want to replace, and one to set the text you want to put in instead. When you set multiple parameters in a method or function, you need to separate them using **commas**, like this:

```
String Object.replace(Text to replace, Text to insert)
```

In the `replace()` method, the first parameter sets the text to replace and the second parameter sets the text to replace it with. The return value of this method is a string with the specified text replaced, but since this is another non-destructive method, it won't overwrite the original string unless you use the = operator. Let's try replacing the word `'sunny'` with `'rainy'` as an example...

```python=1
text = 'Today is a sunny day.'
text = text.replace('sunny', 'rainy')
print(text)
```
##### （Program Results）
<pre class="prettyprint">
'Today is a rainy day.'
</pre>

### Trimming Strings
**Trimming** a string means deleting extra spaces and line breaks from your text. In Python you can do this easily using a method called `strip()`. This method takes the string you set in its parameter and deletes blank spaces from the beginning and end to give you a trimmed-down version of the string.

##### 【Example Code 3-3-1】
```python==1
text = '    white_space'
print(text)
print(text.strip())
```
##### （Program Results）
<pre class="prettyprint">
    white_space
white_space
</pre>

`strip()` searches the beginning and end of a string, so it won't delete any spaces between words in your text.

##### 【Example Code 3-3-2】
```python=1
text = '    w h i t e _ s p a c e'
print(text)
print(text.strip())
```
##### Program Results
<pre class="prettyprint">
    w h i t e _ s p a c e
w h i t e _ s p a c e
</pre>

You can also use `strip()` to delete specific letters/text from a string, like this:

##### 【Example Code 3-3-3】
```python=1
text = '****Happy Birthday!!****'
print(text.strip('*'))
```
##### （Program Results）
<pre class="prettyprint">
Happy Birthday!!
</pre>

### Inserting Text with Methods
Let's say you want to send an email to a lot of people, and it's going to have the same text except at a couple points that need to be different. Python can help you save time on editing each mail if you write a template with placeholders for the text that changes, like this:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-4/1-4_2_E.png"/>

In this email, you can complete the message just by replacing the {○○} with each recipient's name. If you have a message like this as string data, you can use the method `format()` to insert specific text in place of every {○○} placeholder.

`format()` takes any text written inside curly brackets (`{}`) and replaces it with the string you set in its parameters. If you have multiple sets of curly brackets in the original string, you can set multiple strings as parameters and the method will replace each of the placeholders with a different string in order. Try running the code below to see how this works!

##### 【Example Code 3-4-1】
```python=1
text = '{} and {} are {}.'
print(text.format('An apple', 'an orange', 'fruits'))
print(text.format('A cat', 'a dog', 'animals'))
```
##### （Program Results）
<pre class="prettyprint">
An apple and an orange are fruits.
A cat and a dog are animals.
</pre>

## Challenge: An Electronic Welcome
To complete this lesson, use what you've learned so far to make the Core Unit's LED display into an electronic welcome sign!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-4/1-4_3_E.png"/>

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_1-4/1-4_4_E.gif"/>


### Making the Program
This program displays the message **Welcome {Name}!** on the LED Display, letting you insert different names to welcome different people.

##### 【Example Code 4-1-1】
```python=1
from pystubit.board import display
 
message = 'Welcome {}!'
display.scroll(message.format('Takahashi'))
display.scroll(message.format('Imai'))
display.scroll(message.format('Sakurada'))
```

The word **Welcome** is always the same, so we just need to make a placeholder using curly brackets and then use the `format()` method to insert different names into the message. Use this code as a template to make your own program that scrolls your guest's name and a greeting of your choice across the display!

## Conclusion
### A Quick Review
In this lesson we learned about **objects** in Python. We've actually used objects and their methods to control the Core Unit in previous lessons too, like using the code `buzzer.on()` to play sound, or `display.scroll()` to scroll text across the LED display. We'll be using many more methods after this as well! It'd be impossible to memorize every method at once, so start by just trying to remember the ones you use the most.

### Coming Up Next Time!
In the next lesson we'll be learning about two different basic program structures, sequences and loops!
