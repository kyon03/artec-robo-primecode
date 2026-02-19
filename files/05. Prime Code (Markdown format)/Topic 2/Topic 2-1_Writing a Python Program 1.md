---
tags: Python_English, Topic 2
---

Python Robotics Course Lesson 5

Topic 2-1
Writing a Python Program 1
===

Learn about two basic structures you can use to write a program, and data types that let you handle many values at once!

## In This Lesson...

There are three basic ways you can lay out a program: as a series of **sequential** commands, as a **loop**, and as multiple **branches**. In this lesson you'll learn about the first two of these, and a few new data types other than numbers and strings. At the end of the lesson, you'll test what you've learned by programming the Buzzer to play a song!

:::info
Lesson Introduction

■ Lesson 5 Contents

1. Sequence Data in Python: Lists, Dictionaries, and Tuples
2. Sequential and Looping Programs
3. Challenge: A Song in a Tuple



In this lesson you'll learn about sequence data, which can store collections of multiple data points together, and about two basic programming structures.

We'll start by talking about conditional statements, which you'll need to use to make a branching program. We'll also explain boolean data, which describes the results of conditions. On top of that we'll also need to discuss the operators you'll use to compare values and combine multiple conditions.

Once you've learned about the concepts, you can try writing an actual program that evaluates data and branches based on its findings.

Next we'll look at some example programs to learn about simple sequential and looping programs.

At the end of the lesson, you can try using tuples and loops yourself to write a program for the Core Unit's Buzzer. This lets you make an efficient program that can play a song!

Now let's start the lesson!
:::

## Sequence Data in Python

Let's start by learning about a type of data called **sequences**. A sequence is a collection of several data values, grouped together and arranged in a particular order. Python uses the following kinds of sequences:

* Lists ( **list** )
* Tuples ( **tuple** )
* Ranges (Collections of numbers you can find with the built-in function **range()**)

The following two data types are not technically considered sequences, but work similarly so they're often grouped with the sequences anyway.

* Dictionaries ( **dict** )
* Sets ( **set** )

Now let's look at three of these data types more closely: **lists**, **tuples**, and **dictionaries**.

### Lists
A **list** is a data type that stores many pieces of data by lining them up one after the other in order. Lists can be very handy if you want to put a lot of numbers or strings into a single object so you can use them all at once!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-1/2-1_1_E.png"/>

#### ■ Defining a List
You can make/define a list using square brackets, like this:

```
List Name = [ Item 0, Item 1, Item 2, ... ]
```

Write the code below into your terminal and run it to make a list called **food** that records what you have in your refrigerator!

<pre class="prettyprint">
&gt;&gt;&gt; food = ['milk', 'beef', 'egg', 'carrot']
</pre>

You can use `print()` to check the contents of a list you've made.

<pre class="prettyprint">
&gt;&gt;&gt; print(food)
['milk', 'beef', 'egg', 'carrot']
</pre>

#### ■ Retrieving an Item from a List
If you want to find and retrieve a specific item from a list, write the number of that item's position in the list in square brackets, like this:

```
List Name[Number]
```

Be careful, the positions in a list are counted starting at 0 (0th, 1st, 2nd...), not 1. Now try putting in each position number to retrieve its corresponding item!

<pre class="prettyprint">
&gt;&gt;&gt; print(food[0])
milk
&gt;&gt;&gt; print(food[1])
beef
&gt;&gt;&gt; print(food[2])
egg
&gt;&gt;&gt; print(food[3])
carrot
</pre>

If you use a negative number like -1 to specify the position, Python will start searching from the end of the list.

<pre class="prettyprint">
&gt;&gt;&gt; print(food[-1])
carrot
</pre>

#### ■ Retrieving Multiple Items from a List
You can also retrieve a set of items from a list at once as a **slice**. You can write a slice like this:

```
List Name[Start Point:End Point]
```
Let's try taking a slice from items 0 to 2 of the **food** list.

<pre class="prettyprint">
&gt;&gt;&gt; print(food[0:3])
['milk', 'beef', 'egg']
</pre>

In this code, the start point is 0 and the end point is 3. As you can see from the results, the value from the position you specify as the end point won't be included in the slice. That means your slice will contain the items **from the start point to the end point minus 1**.

You can also make slices that retrieve items from a list in intervals, like this:

```
List Name[Start Point:End Point:Interval]
```

If you specify an interval, the program will retrieve items from every (interval number) positions in the list between and including the start and end point, like this:

```
[Start Point], [Start Point + Interval x 1], [Start Point + Interval x 2], ...
```

You can use the code below to test this out.

<pre class="prettyprint">
&gt;&gt;&gt; print(food[0:4:2])
['milk', 'egg']
</pre>


#### ■ List Methods
You can use methods to add and remove items from lists!

|Method Name|Function|
|:---|:---|
|append()|Add an item to the end of a list|
|pop()|Delete the item from a specified position in a list|
|remove()|Delete the first item of the specified value from a list|

First, let's say we've just bought some `bacon` and want to add it to the list of food in the fridge using `append()`.

<pre class="prettyprint">
&gt;&gt;&gt; food.append('bacon')
&gt;&gt;&gt; print(food)
['milk', 'beef', 'egg', 'carrot', 'bacon']
</pre>

Now, if we've used up the eggs we had in the fridge, we can use `pop()` to take `'egg'` off the list. `'egg'` is in the 2nd position, so we can delete it with this code:

<pre class="prettyprint">
&gt;&gt;&gt; food.pop(2)
'egg'
&gt;&gt;&gt; print(food)
['milk', 'beef', 'carrot', 'bacon']
</pre>

The reason `'egg'` appears in the terminal after we run `food.pop(2)` is that the `pop()` method gives you the value it deletes as a return value. When you remove an item from a list it reassigns all its numbered positions, so if you take out item 2 again, the result will be `carrot`.

<pre class="prettyprint">
&gt;&gt;&gt; print(food[2])
carrot
</pre>

`pop()` isn't the easiest method to use to delete a specific item from a list since you need to remember the position of that item to use it. If you want to **delete an item with a specific value** instead, you can use the method `remove()`.

<pre class="prettyprint">
&gt;&gt;&gt; food.remove('carrot')
&gt;&gt;&gt; print(food)
['milk', 'beef', 'bacon']
</pre>

Unlike `pop()`, `remove()` doesn't have a return value.

### Tuples
**Tuples** are very similar to lists, except that unlike a list, you can't add or remove items from a tuple. Data types whose contents can be changed, like lists, are called **mutable** data types, while unchangeable ones like tuples are called **immutable** data types. This means that once you've made a tuple you can't add to, delete, or overwrite its contents.

At this point you might think tuples sound strictly less useful than lists and wonder why they exist in the first place! However, we can give you a couple good reasons why tuples exist in Python:

1. **Tuples are faster than lists**
In regular programming, the more flexible a function, method, or object is, the longer it will take for the program to process it. This means the fact that tuples can do less than lists can actually make them work faster. It may be difficult to notice this difference yourself given the power of modern computers, though.

2. **Tuples can be used as constants**
We've explained before why it's important to give variables and functions names that tell you what they do. If you're using a value in your program that won't be changing, saving that value in a variable with a name can still make it easier to tell what that value stands for (for example that 24 is the number of hours in a day, or that 3.141... is Pi). Variables like these that don't change value in a program are called **constants**. You can use tuples in this same way if you have a series of fixed values you want to save for use in your program.

#### ■ Defining a Tuple

You can make/define a tuple using parentheses, like this:

```
Tuple Name = (Item 1, Item 2, Item 3, ...)
```

For example, we can make a tuple that records the names of all the provinces that make up the Kanto region in Japan.

<pre class="prettyprint">
&gt;&gt;&gt; Kanto = ('Ibaraki', 'Tochigi', 'Gunma', 'Saitama', 'Chiba', 'Tokyo', 'Kanagawa')
</pre>

You can use **print()** to check the contents of a tuple, just like with lists.

<pre class="prettyprint">
&gt;&gt;&gt; print(Kanto)
('Ibaraki', 'Tochigi', 'Gunma', 'Saitama', 'Chiba', 'Tokyo', 'Kanagawa')
</pre>

#### ■ Retrieving Items from a Tuple
Like lists, positions in a tuple are numbered, starting with 0. You can retrieve an item from a tuple by writing its position number in square brackets the same way, too.

<pre class="prettyprint">
&gt;&gt;&gt; print(Kanto[2])
Gunma
</pre>

You can't add new items to a tuple after it's made, but you can overwrite the values of variables stored in a tuple.

<pre class="prettyprint">
&gt;&gt;&gt; a = (1, 2, 3)
&gt;&gt;&gt; print(a)
(1, 2, 3)
&gt;&gt;&gt; a = (4, 5, 6)
&gt;&gt;&gt; print(a)
(4, 5, 6)
</pre>

Be careful if you're trying to make a tuple with only one piece of data in it! If you write it as shown below, it won't be defined as a tuple.

<pre class="prettyprint">
&gt;&gt;&gt; b = (1)
&gt;&gt;&gt; print(b)
1
</pre>

If you write just `b = (1)`  like this, `b` will be defined as a variable with the value of `1`. This happens because parentheses are used both to make tuples and to write basic mathematical equations. To fix this problem, add a `,` after `1` to make it a tuple, like this:

<pre class="prettyprint">
&gt;&gt;&gt; b = (1,)
&gt;&gt;&gt; print(b)
(1,)
</pre>

### Dictionaries
Every item stored in a list or tuple is automatically assigned a number in order, starting with 0, and can be retrieved by specifying its corresponding number. **Dictionaries** are different from these data types because items stored in them are organized by **keys** assigned by the programmer instead of position numbers. 

#### ■ Defining a Dictionary
You can make/define a dictionary using curly brackets, and then use colons ( : ) to set up key/item pairs, like this:

```
Dictionary Name = {Key 1:Item 1, Key 2:Item 2, Key 3:Item 3, ...}
```

**Each key can only be assigned to one item in the dictionary.** If you try to assign two items to the same key, the second item will overwrite the first.

Let's try making a dictionary called `contact` to store someone's contact information. We'll record their first and last names and their telephone number by pairing each piece of information with keys called `first_name`, `last_name`, and `tel_no`.

<pre class="prettyprint">
&gt;&gt;&gt; contact = {'first_name':'Taro', 'last_name':'Tanaka', 'tel_no':'123-1234-0000' }
&gt;&gt;&gt; print(contact)
{'tel_no': '123-1234-0000', 'last_name': 'Tanaka', 'first_name': 'Taro'}
</pre>

Since items in a dictionary aren't saved in order, using `print()` with a dictionary won't necessarily give you the items in the same order you typed them in.

#### ■ Retrieving Items from a Dictionary
You can retrieve an item from a dictionary by writing its corresponding key in square brackets. Just write the name of the dictionary and the program will search its contents for the key.

```
Dictionary Name[Key]
```

Let's try finding the information we stored in the `contact` dictionary before.

<pre class="prettyprint">
&gt;&gt;&gt; print(contact['first_name'])
Taro
&gt;&gt;&gt; print(contact['last_name'])
Tanaka
&gt;&gt;&gt; print(contact['tel_no'])
123-1234-0000
</pre>

Strings, integers, and tuples can all be used as dictionary keys. A dictionary with integer keys may seem like the same thing as a list, however, with a dictionary you can choose to use integers that don't follow immediately after one another (like 2, 4, 6...) as keys if you want to.

#### ■ Adding Items to a Dictionary
You don't need to use a method to add an item to a dictionary. Instead write it like this in your code:

```
Dictionary Name[Key]=Value you want paired with the key
```

For example, we can add a postal code to the `contact` dictionary like this:

<pre class="prettyprint">
&gt;&gt;&gt; contact['postcode'] = '111-1111'
&gt;&gt;&gt; print(contact)
{'first_name': 'Taro', 'postcode': '111-1111', 'tel_no': '123-1234-0000', 'last_name': 'Tanaka'}
</pre>

#### ■ Dictionary Methods
To delete items from a dictionary, you can use the `pop()` method the same way you would for a list. Let's try deleting the postal code we just added to **contact**.

<pre class="prettyprint">
&gt;&gt;&gt; contact.pop('postcode')
'111-1111'
&gt;&gt;&gt; print(contact)
{'first_name': 'Taro', 'tel_no': '123-1234-0000', 'last_name': 'Tanaka'}
</pre>

The `pop()` method for dictionaries still shows you the value it deletes as a return value.

## Sequential Programming

Sequential programming (not to be confused with sequence data!) describes a program made up of a series of commands that run in the order they're written in. In Python, a sequential program runs each line of code starting from the top line and going down. All the programs we've written so far in the course have been set up with a sequential structure!

##### Ex.) The Welcome Board Program from Topic.1-4

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-1/2-1_2_E.png"/>


## Loops
A **loop** is a kind of program that gives a certain set of commands over and over again. Loops come in different kinds, the main two being **infinite loops**, which repeat forever, and **conditional loops**, which repeat as long as a specified condition is fulfilled.  Both of these kinds of loops are generally written using either a **while statement** or a **for statement**. One thing loops are particularly useful for is running each piece of data stored in a sequence like a list or tuple through the same process in order.

### Loops with **while** Statements
**while** statements are written as shown below. As long as the condition written to the right of `while` is fulfilled, the code below it will keep repeating!

```
while Conditional Statement:
	Program you want to repeat
	.
	.
	.
```

As we learned when writing functions in Topic 1-3, every line of code that goes inside a **while** loop needs to be indented! Meanwhile, conditional statements are written by using =, >, and < operators, which are called **relational operators** when you use them this way.

##### Some Relational Operators for Conditional Statements
|Relational Operator|Usage|Meaning|
|:---|:---|:---|
|==|a == b|A is equal to B|
|!=|a != b|A is not equal to B|
|<|a < b|A is less than B (doesn't include a==b)|
|>|a > b|A is greater than B (doesn't include a==b)|
|<=|a <= b|A is less than or equal to B (includes a==b)|
|>=|a >= b|A is greater than or equal to B (includes a==b)|

Be sure to remember that if you want a relational operator to check whether two data values are the same, you need to use `==`, not `=`!

#### ■ Loops with Conditions
Let's try using a conditional **while** statement with a relational operator to make a program that counts from 0 to 9. Write the code below into your editor, and don't forget to add the colon after the conditional statement!

##### 【Example Code 4-1-1】
```python=1
count = 0
while count < 10:
	print(count)
	count += 1
```

The code `count < 10` on line 2 is a conditional statement. With this condition, the code inside the loop will keep repeating as long as the value of the `count` variable is less than 10. Line 4, `count += 1`, performs the same operation as writing `count = count + 1`, making the value of `count` increase by 1 each time the code runs. This way of writing it saves space! You can do the same thing if you want to make the value decrease, just write `count -=1` instead of `count=count-1`.

Try running this program and see if it makes the numbers 0 to 9 appear in order in your terminal.

<pre class="prettyprint">
0
1
2
3
4
5
6
7
8
9
</pre>

### Loops with for Statements
**for** statements are used with lists, tuples, and the **range()** function (which we'll be explaining in this section). If we rewrite the program we made in the last section using a **for** statement instead of a **while** statement, it looks like this:

##### 【Example Code 4-2-1】
```python=1
for count in range(10):
	print(count)
```

Running this program should give you the same results as running the **while** statement version did.


This code may look difficult at first since it uses the operator `in` and the function `range()`, but once you know what each part of it does, it's really not that hard to follow!

#### ■ The **range()** Function

The `range()` function creates a list of integers based on a condition you set in its parameters. It has three different parameters, shown below.

```
range(Starting Integer, Final Integer, Interval)
```

The list of integers will be made up of every (interval) integer between the starting point and the ending point. You can leave the starting integer and interval unspecified, in which case the range will default to a start point of 0 and an interval of 1. The final integer won't be included in the range, so if your interval is 1, your range will be the starting interval to the final integer minus one.

The series of integers produced by the `range()` function is neither a list nor a tuple; it's a special kind of object called an **iterator**. If you run the following code in your terminal, you'll be able to see that the result is neither a tuple nor a list.

<pre class="prettyprint">
>>> range(10)
range(0, 10)
</pre>

#### ■ The **in** Operator

Using the operator `in` within a **for** statement lets the program go through every item in a list, tuple, dictionary, or range one-by-one. This means the code `for count in range(10)`: goes through the series of integers (0-9) created by `range(10)` and retrieves each one.

The `in` operator can also be used to check whether any item of a particular value exists in a list, tuple, or dictionary.

Now that we know about **in** and **range()**, let's look at Example Code 4-2-1 again.

```python=1
for count in range(10): 
	print(count)
```

What's happening here is the `in` operator retrieves each number from the set (0-9) created by `range(10)`, then saves its value in the `count` variable and runs the code inside the loop.

This might still be a bit confusing, but once you understand how `range()` works, you can use it to make a very easy countdown program!

##### 【Example Code 4-2-2】
```python=1
for count in range(9, -1, -1): 
	print(count)
```
##### Program Results
<pre class="prettyprint">
9
8
7
6
5
4
3
2
1
0
</pre>

By setting the interval using a negative integer, you can make a series of decreasing numbers (i.e. 9, 8, 7, 6,...), which lets you program a countdown!

### Loops with Lists and Tuples
As we mentioned briefly earlier, you can use the `in` operator in a **for** statement to retrieve each item from a list, tuple, or dictionary in order.

#### ■ Retrieving Items from a List in Order
In the next example program, we'll retrieve each item from a list of fruit prices called `fruits`, save it in a variable called `fruit`, and use the `print()` function to display the results in the terminal.

##### 【Example Code 4-3-1】
```python=1
fruits = ['apple', 'banana', 'cherry', 'orange']
for fruit in fruits:
	print(fruit)
```

##### Program Results
<pre class="prettyprint">
apple
banana
cherry
orange
</pre>

#### ■ Retrieving Items from a Tuple in Order
You can retrieve items in order from a tuple using the same method we used for a list. If we take the program above and just change the list **fruits** into a tuple, the results will be the same.

##### 【Example Code 4-3-2】
```python=1
fruits = ('apple', 'banana', 'cherry', 'orange')
for fruit in fruits:
	print(fruit)
```

##### Program Results
<pre class="prettyprint">
apple
banana
cherry
orange
</pre>

#### ■ Retrieving Items from a Dictionary in Order
If you use the same retrieval methods for dictionaries as we used for lists and tuples, you'll end up retrieving the dictionary's keys, not the data items saved in it. Remember, the order of items in a dictionary won't be the same as the order you added them in!

##### 【Example Code 4-3-3】
```python=1
fruits = {'apple':120, 'banana':80, 'cherry':300, 'orange':100}
for key in fruits:
	print(key)
```

##### Program Results
<pre class="prettyprint">
orange
apple
banana
cherry
</pre>

If you want to find the values of all the items in a dictionary, you'll need to use the method `values()`, like this:

##### 【Example Code 4-3-4】
```python=1
fruits = {'apple':120, 'banana':80, 'cherry':300, 'orange':100}
for value in fruits.values():
	print(value)
```

##### Program Results
<pre class="prettyprint">
100
120
80
300
</pre>

If you want both the keys and the values for all the items, you can use the method `items()`. This will give you each key-item pair as a tuple.

##### 【Example Code 4-3-5】
```python=1
fruits = {'apple':120, 'banana':80, 'cherry':300, 'orange':100}
for item in fruits.items():
	print(item)
```

##### Program Results
<pre class="prettyprint">
('orange', 100)
('apple', 120)
('banana', 80)
('cherry', 300)
</pre>


## Challenge: A Song in a Tuple
To complete this lesson, use tuples and **for** statements to program a song the Core Unit's Buzzer can play!

### Making the Program
The first thing you need to do is prepare your program to control the Core Unit's Buzzer. You can copy the code below and paste it into your editor.
```python=1
from pystubit.board import buzzer
import time
```

Next you'll need to make a tuple that contains the data for a song. The data for a song needs to contain data for each note in it, meaning a pitch and a duration for every sound that plays. That means you'll need to put tuples inside of tuples to make this work!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_2-1/2-1_3_E.png"/>

To keep things simple, let's just play each note from **C4 (Do)** to **C5 (Do)** in order for our song this time.

|#|Note|Duration|
|:---|:---|:---|
|0|C4 (Do)|600|
|1|D4 (Re)|600|
|2|E4 (Mi)|600|
|3|F4 (Fa)|600|
|4|G4 (Sol)|600|
|5|A4 (La)|600|
|6|B4 (Ti)|600|
|7|C5 (Do)|600|

The code below turns these notes into a tuple. As you can see, each item we're adding to the tuple is on a separate line. You can do this whenever you're making a tuple, dictionary, or list - it might be a lot of lines, but it's easy to read, so we recommend it if your line of items gets too long!

```python=1
from pystubit.board import buzzer
import time

music = (
	('C4', 600),
	('D4', 600),
	('E4', 600),
	('F4', 600),
	('G4', 600),
	('A4', 600),
	('B4', 600),
	('C5', 600),
	)
```

Now you can use a **for** statement to retrieve each item from your new tuple in order. In the example below, the retrieved items get saved in the variable `tone`. Then `tone[0]` retrieves the pitch of the note and `tone[1]` retrieves the duration.

##### 【Example Code 5-1-1】
```python=1
from pystubit.board import buzzer
import time

music = (
	('C4', 600),
	('D4', 600),
	('E4', 600),
	('F4', 600),
	('G4', 600),
	('A4', 600),
	('B4', 600),
	('C5', 600),
	)

for tone in music:
	buzzer.on(tone[0])
	time.sleep_ms(tone[1])
buzzer.off()
```

Make sure line 18 is not indented! Now try running this program and see how it works.


### What Does It Look Like Without Tuples or a for Statement?
If we remove the extra line breaks from Example Program 5-1-1, it becomes a seven line program, like this:

##### 【Example Code 5-2-1】
```python=1
from pystubit.board import buzzer
import time
music = ('C4',600),('D4',600),('E4',600),('F4',600),('G4',600),('A4',600),('B4',600),('C5',600),)
for tone in music:
	buzzer.on(tone[0])
	time.sleep_ms(tone[1])
buzzer.off()
```

If you wanted to write a program that did the same thing without using tuples or a **for** statement (as shown below), it would be nineteen lines long! This shows just how much more concise you can make your programs by using **for**.

##### 【Example Code 5-2-2】
```python=1
from pystubit.board import buzzer
import time
buzzer.on('C4')
time.sleep_ms(600)
buzzer.on('D4')
time.sleep_ms(600)
buzzer.on('E4')
time.sleep_ms(600)
buzzer.on('F4')
time.sleep_ms(600)
buzzer.on('G4')
time.sleep_ms(600)
buzzer.on('A4')
time.sleep_ms(600)
buzzer.on('B4')
time.sleep_ms(600)
buzzer.on('C5')
time.sleep_ms(600)
buzzer.off()
```
When you're done with this chapter, click here!

## Conclusion
### A Quick Review
In this lesson, we learned about sequence data, including tuples and lists, and we saw how making loops using **while** and **for** statements can help you write shorter, clearer programs! These are all fundamental tools we'll be using in lots of programs to come. If you're not sure you've got a solid grasp of them yet, go back and read through again!

### Coming Up Next Time!
In the next lesson we'll learn about another essential basic programming structure, **branches**!