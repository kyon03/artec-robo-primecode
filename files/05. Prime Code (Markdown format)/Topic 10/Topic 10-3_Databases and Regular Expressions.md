---
tags: Python_English, Topic 10
---

Python Robotics Course

Topic 10-3
Databases and Regular Expressions
===

Learn about managing data with databases and searching text with regular expressions!

## In This Lesson...

Learn how to store and manage information in **databases** and use **regular expressions** and **matching** to search for specific strings in collections of text.

:::info

Lesson Introduction

■ Lesson 39 Contents

1. What is a Database?
2. Regular Expressions and Matching
3. Making a Simple ID Database Authentication System
4. Challenge: Expanded Account Information

■ Lesson Introduction

In this lesson we're going to learn about a tool used to organize and store information called a database as well as regular expressions and matching, which can help you find search for exact strings in a piece of text.

We'll start the lesson by learning about the different types of databases as well as what they can do, then take on the challenge of building a database on the Core Unit!

Next we'll look at regular expressions. This is a method of writing patterns which you can use to look for and extract specific text strings. A good example would be using regular expressions to check whether a user has entered the correct email address and password when logging into a website!

In the second half of the lesson, we'll make a simple user authentication server which checks a person's username and password, just like a real website. This system will store account data using the database we made in the first half of the lesson!

At the end of the lesson, we'll modify the program for our authentication system as we take on the challenge off storing data other than usernames and passwords inside of the database!

Now let's start the lesson!

:::

## What is a Database?

There may be some of you reading this who haven’t heard the term **database** before, but even if you don’t know the word, you’re actually using databases all of the time through the internet! 

If you use a search engine like Google or Bing, you can access all kinds of data from around the world with the press of a button. The reason you can access that data is because there’s a place somewhere in the world where it’s being stored. One common way to store and organize this data is what we call a database!

### Database Basics

The information stored in a database is arranged according to special rules to help you search through or analyze it. Databases are divided into several different types (database models) based on what system they use to organize their data!

#### ■ Different Kinds of Databases

Here’s a brief introduction to five different kinds of database:

* **Card Index**
A system of storing collections information on single cards was an old-fashioned kind of database, but it isn’t commonly used any more.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-3/10-3_1_E.png"/>
* **Key-Value Database**
A simple database model that organizes information in sets of keys and values. This structure is light-weight and easy for computers to handle, so it’s often used for storing very large amounts of information.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-3/10-3_2_E.png"/>
* **Relational Database**
The most common database model which organizes data in tables. The values in one table can be the keys for another table, creating relationships between information in different tables. 
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-3/10-3_3_E.png"/>
* **Network Database**
A database model that organizes information in　a web, where pieces of data have “parent-child” relationships between them.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-3/10-3_4_E.png"/>
* **Object Database**
A database model where you can store a collection of information as a single object. This structure works well with object-oriented programming languages like Python, since it lets your store data without breaking the object format you’re using in your code.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-3/10-3_5_E.png"/>


Usually when making a database you’d pick what model to use based on which one is best suited to the kind of data you want to store and how much of it you have. Unfortunately, because MicroPython only has the tools for us to build **key-value model** databases, this is the only type we can use here. However, using just this one model should be enough for you to learn just how useful databases can be! Let’s start programming a database of our own and see what kind of features are available for us to use.

### Building a Database in MicroPython

MicroPython has a module called `btree` which you can use to make a key-value database. Much like dictionary data structures, databases made with the `btree` module let you access data that’s stored with keys, but there are two major differences between dictionaries and databases:

* The keys and values in a database are all **byte strings**.
* You can make a **range query** of the keys in a database.
 
Now let’s start building our database and writing a program that searches the database to see how these things work!

#### ■ Making a Database
To start making a database, first you’ll need a file to record data in. Open this file in **binary mode**<sup>★</sup>!

###### ★ What is binary mode? In text mode, writing certain sequences of characters like `\r` and `\r\n` will be treated as code for line breaks, but in binary mode they’ll be treated as those characters and nothing more. Binary mode is basically used for writing and reading raw data.

Now give your file’s name to the `btree` module’s `open()` function to create your database object.

```python=1
import btree

try:
    # Open a file in reading (writing is also possible since it’s r+) binary mode (b)
    file = open("mydb", "r+b")  
except OSError:  # If there is no such file, an OSError will occur
     # Open a file in writing (reading is also possible since it’s w+) binary mode (b).
     # If there is no such file, a new file will be made
    file = open("mydb", "w+b") 

db = btree.open(file)  # Create a database object from a file
```

#### ■ Organizing Information in a Database

You can save data in your new database using the same format you’d use to add it to a dictionary.

```
db[Key] = Value
```

However, because the keys and values in the database all need to be byte strings, you’ll have to use the `encode()` method to turn any strings you use into bytes, and the `to_bytes()` method to do the same thing for numbers. Let’s add the data below to our database so we can use it as an example.

|Key: Fruit|Value: Price (Dollars)|
|:---|:---|
|apple|1|
|orange|2|
|banana|2|

###### ★ Just like in a dictionary, you need to make sure your database doesn’t reuse any keys! Repeated values, however, are completely fine!

###### New Code (Lines 11-14)
```python=1
import btree

try:
    file = open("mydb", "r+b")  
except OSError:
    file = open("mydb", "w+b") 

db = btree.open(file)

# Adds information to the database
db["apple".encode("utf-8")] = (1).to_bytes(2, "big")
db["orange".encode("utf-8")] = (2).to_bytes(2, "big")
db["banana".encode("utf-8")] = (2).to_bytes(2, "big")
db.flush() # Writes to the file
```

Up until line 13, the data is only temporarily saved in your database. To save it permanently, you need to use the `flush()` method to write it into the file you’re using for your database.

#### ■ Searching for Information in a Database

The process of searching for data is also basically the same as a dictionary, except for the keys being byte strings. Let’s try finding the three pieces of data we just entered into the database and showing them in the terminal.

##### Example Code 2-2-1
###### New Code (Lines 16-17, Lines 20-23, Lines 26-28, Lines 31-33)
```python=1
import btree

try:
    file = open("mydb", "r+b")  
except OSError:
    file = open("mydb", "w+b") 

db = btree.open(file)

db["apple".encode("utf-8")] = (1).to_bytes(2, "big")
db["orange".encode("utf-8")] = (2).to_bytes(2, "big")
db["banana".encode("utf-8")] = (2).to_bytes(2, "big")
db.flush()

# Searches the data
price = db["apple".encode("utf-8")]
print("apple", int.from_bytes(price, "big")) # Converts from a byte string back into a number before showing

# Uses the keys method
print("Using keys()...")
for key in db.keys():
    price = db[key]
    print(key.decode("utf-8"), int.from_bytes(price, "big")) # Converts from a byte string back into a text string or number before showing

# Uses the values method
print("Using values()...")
for val in db.values():
    print(int.from_bytes(val, "big"))  # Converts from a byte string back into a number before showing

# Uses the items method
print("Using items()...")
for key, val in db.items():
    print(key.decode("utf-8"), int.from_bytes(val, "big"))  # Converts from a byte string back into a text string or number before showing

# Closes the database and the file
db.close()
file.close()
```

#####  Program Results
```
apple 1
Using keys()...
apple 1
banana 2
orange 2
Using values()...
1
2
2
Using items()...
apple 1
banana 2
orange 2
```

Like dictionaries, `btree` objects come with methods such as `keys()`, `values()`, and `items()` that you can use to retrieve data from them in order. When you’re done using a database, you should use the `close()` method to close it, and then close the file you’re using for the database too. Always be sure to close the database first before closing the file!

#### ■ Making a Range Query

The `keys()`, `values()`, and `items()` methods can also be used to search for keys and values within specific ranges! Let’s rewrite the part of Example Code 2-2-1 where we used the method `keys()` like this and see how it runs.

###### New and Changed Code (Lines 21-23)
```python=19
# Uses the keys method
print("Using keys()...")
start = "a".encode("utf-8")  # Sets the search’s start point
end = "c".encode("utf-8")  # Sets the search’s end point
for key in db.keys(start, end):  # Sets the search range in the argument
    price = db[key]
    print(key.decode("utf-8"), int.from_bytes(price, "big"))
```

#####  Program Results
<pre class="prettyprint">
Using keys()...
apple 1
banana 2
</pre>

In the new version of the code, we’re searching for keys only in the range from `"a"` to `"c"`, so we didn’t find the key orange this time.

That covers the basic usage of a database. Next, let’s talk about **regular expressions**, a technique you can use to search for precise strings and check the data you've input for mistakes, as well as the concept of **matching**, which is used with regular expressions.

## Regular Expressions and Matching

All of you have probably at some point signed up for an online service that required you to provide certain personal information in order to make an account with them. That personal information might include your name, email address, phone number, address, or other information you need to use for this service. If a user registers incorrect information when signing up, this can cause problems when they try to use the service later!

##### Examples Problems Caused by Incorrect Registration Information
* User won’t receive important notifications if their registered email address is incorrect.
* User won’t receive things they purchase by mail if their registered address is incorrect.
* Service providers can’t contact the user if their registered phone number is incorrect.

To prevent problems like these, user registration forms ask users to double check the information they’ve entered, and to enter particularly important information like email addresses twice.

In this section we’re going to introduce you to regular expressions and matching, which are common techniques for preventing these kinds of problems by checking whether the information the user types matches certain predefined patterns.

For example, email addresses and phone numbers usually come in certain set formats and only use certain kinds of characters in them, which means you can define a template pattern for them to some extent.

* Email Address: XXX@XX.XX.XX
* Cellphone Number: XXX-XXXX-XXXX

A statement or expression that defines a pattern like this is called a **regular expression**, and the process of searching strings to check whether they fit the pattern in the regular expression is called **matching**.

Let’s look into regular expressions first!

### Regular Expressions

Regular expressions define patterns by using normal alphanumeric characters as well as special symbols called **metacharacters** and **special sequences** to make strings.

Let’s say we want to make the cellphone number pattern we wrote above into a regular expression. It would look like this:

```
"^\d\d\d-\d\d\d\d-\d\d\d\d$"
```

In Python, regular expression patterns are made as text string objects. In the pattern above, the `^` at the beginning and the `$` at the end are metacharacters that mark the start and end of a line. The `\d` you see repeated over and over is a special sequence that stands in for any numeric character from 0 to 9. Basically, this pattern describes a series of numbers separated by hyphens, like 012-3456-7890.

There are a lot more metacharacters and special sequences you can use to write patterns in MicroPython than just these three. There are too many of them to easily memorize all of them at once, so let’s try writing some practice patterns and remembering the characters as we go!

##### Metacharacters in MicroPython

|Metacharacter|What does it do?|Example|What does it match?|
|:---|:---|:---|:---|
|`.`|Matches any character other than line breaks. |a.c|abc, acc, aac|
|`^`|Matches the beginning of a string. If there are line breaks, it matches the beginning of every line.|^abc|abcdef|
|`$`|Matches the end of a line.|abc$|defabc|
|`*`|Matches zero or more repetitions of the regular expression it comes after.|ab*|a, ab, abb, abbb|
|`+`|Matches one or more repetitions of the regular expression it comes after.|ab+|ab, abb, abbb|
|`?`|Matches zero or one repetition of the regular expression it comes after.|ab?|a, ab|
|`[◎〇△]`|A collection of characters. Matches one of the characters inside the `[]`.|`[abc]`|a, b, c|
|`[◎-〇]`|Setting a `-` between two characters like this describes a range of characters between the two.|`[a-e]`|a, b, c, d, e|
|`[^◎〇△]`|Putting a `^` after a `[` matches characters that aren’t in your strings.|`[^a-z]`|0, 1, 2, A, B, C|
|`|`|Make a regular expression that matches a regular expression between the `|`s.|`ab|cd`|abd, acd|
|`(...)`|Make a group from a regular expression.|`(a[bc]?)+`|a, ab, aba, abac|
|`\`|An escape character. The character after the backslash will be treated as a normal character instead of a special one.|`\.\?\+`|.?+|

##### Special Sequences in MicroPython

|Special Sequence|What does it do?|Equivalent Regular Expression|
|:---|:---|:---|
|`\d`|Matches a number.|[0-9]|
|`\D`|Matches any non-numeric character.|[^0-9]|
|`\s`|Matches whitespace.|[\t-\r]|
|`\S`|Matches any non-whitespace character.|[^\t\n\r\f\v]|
|`.`|Matches any alphanumeric character.|[a-zA-Z0-9_]|
|`.`|Matches any non-alphanumeric character.|[^a-zA-Z0-9_]|


#### ■ Practicing Regular Expressions

Let’s practice by writing regular expressions that match the patterns below!

* **Python (A specific word)**
```
"^Python$"
```
* **A 5-letter word that starts with “a” and ends with “e”**
```
"^a[a-z][a-z][a-z]e$"
```
* **A 3-or-more letter word that starts with “a” or “b”**
```
"^[ab][a-z][a-z]+$"
```
* **A 7-digit postal code (in XXX-XXXX or XXXXXXX format)**
```
"^\d\d\d-\d\d\d\d|\d\d\d\d\d\d\d$"
```
* **An email address (XXXXXX@XXXX.XX.XX)**
###### ★ This regular expression works with email addresses written with only alphanumeric characters, underscores, and hyphens.
```
"^(\w|-)+@(\w|-)+(\.(\w|-)+)+$"
```

The email address pattern is a little complicated, so let’s break it down into three parts and look at what parts of the pattern below they might match.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-3/10-3_6_E.png"/>

Now let’s use this email address regular expression to write an actual matching program!

### Matching

The `ure` module is used to do matching in MicroPython. Here are the functions that come with the `ure` module:

##### ure Module Functions

|Function|What does it do?|
|:---|:---|
|`compile(Regular Expression)`|Compile<sup>★</sup> a regular expression and return a `regex`<sup>★</sup> object.|
|`match(Regular Expression, String to Search)`|Compile a regular expression, then compare with a string to see if it matches. If it matches, return a `match` object, if not, return a `None` object. The comparison will start at the beginning of the string, so if the match is only in the middle of the expression you’ll still get a `None` object.|
|`search(Regular Expression, String to Search)`|Compile a regular expression, then compare with a string to see if it matches. If it matches, return a `match` object, if not, return a `None` object. Unlike the `match` function, this function returns a `match` object even if the matching portion is in the middle of the string.|

###### ★ ”Compiling” is the act of turning text written in a programming language into a format computers can run.
###### ★ “regex” is an abbreviation of “regular expression.”

The `regex` objects you get from the `compile()` function have the same methods as the `ure` module’s `match()` and `search()` functions. You’ll get the same results regardless of which ones you use, but if you’re going to be doing matching many times in row, it’s more efficient for your program if you compile the regular expression into a `regex` object first.

Additionally, the `match()` and `search()` functions are both used to find matches with regular expressions, but there are some other differences between them.

##### `match` vs. `search` Function Results

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-3/10-3_7_E.png"/>

#### ■ An Email Validity Checking Program

In the example code below, we use the `ure` module to make a program that checks whether an email address you type into the terminal matches a regular expression for email addresses. Copy this code and run it, then call the `main()` function in the terminal and test if your program can check emails correctly!

##### Example Code 3-2-1

```python=1
import ure

emailaddress = "^(\\w|-)+@(\\w|-)+(\\.(\\w|-)+)+$"  # A regular expression for email addresses
regex = ure.compile(emailaddress)  # Compiles the regular expression to make a regex object

def main():
    # Enter an email address into the terminal
    user_emailaddress = input("Type your email address >>> ")
    matchobj = regex.match(user_emailaddress)  
    if matchobj:  # If you get a match object, the email is valid
        print("'{}' is a valid email address.".format(user_emailaddress))
    else:  # If you get a None object, the email is not valid
        print("'{}' is an invalid email address.".format(user_emailaddress))
```

#####  Program Results
<pre class="prettyprint">
&gt;&gt;&gt; main()
Type your email address &gt;&gt;&gt; example@ex-ex.exam
'example@ex-ex.exam' is a valid email address.

&gt;&gt;&gt; main()
Type your email address &gt;&gt;&gt; sample@ex-ex
'example@ex-ex' is an invalid email address.
</pre>

## Making a Simple ID Database Authentication System

Many of the services we use online (online shops, social networking sites, etc.) require you make an account before you can use the service. When you want to use the service later, you go through **user authentication** using the account information you registered with so you can start shopping, chatting, and more!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-3/10-3_8_E.png"/>

Most websites which tailor services to their users like this use databases to manage and record their users’ account information.

The account information users enter on the website gets sent to the website’s server, and the server then sends a search request to the database. The server gets the results of its search back from the database and uses it to authenticate the user’s information and show them the results.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-3/10-3_9_E.png"/>

It’s important for these systems to make sure that the information they get from users is formatted correctly, especially when the user is registering a new account, since registering with an incorrect password or email address will prevent them from using the service later! To prevent this, these systems use regular expressions and matching to let the user know if there are formatting errors in their information.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-3/10-3_10_E.png"/>

To make a database like the ones real user authentication services use, you’d need to know a lot more about database operation, HTTP methods, encryption, and other specialized skills. However, you can make simplified database model to learn how they work using just the things you’ve learned about databases, regular expressions, and matching in this lesson!

Let’s see how to construct a user authentication system by writing an actual program!


### What Parts Does the System Need?

The authentication system we’re going to make needs to do two main things: **register account information** (email and password), and **user login**. Let’s lay out the sequence of things the program needs to do to make these features work.

##### Account Registration Process
1. Request new account information input from the terminal
2. Check the formatting of the received information
3. Open the database
4. Record the account information in the database
5. Close the database 

##### User Login Process
1. Request user information for a registered account from the terminal
2. Check the formatting of the received information
3. Open the database
4. Search the database for matching account information
5. Close the database 
6. Display authentication results

Now that we’ve laid it out, we can see that the two processes have a number of steps in common! We can make these shared processes into their own functions, giving us these functions for the whole program:

##### Our Program’s Functions
|Function|What does it do?|
|:---|:---|
|`open_db()`|Opens the database|
|`close_db()`|Closes the database|
|`get_account_info()`|Requests new account information input from the terminal and checks the formatting of received input|
|`register()`|Registers new account information|
|`login()`|Logs users into the system|

Now let's go and program all of these functions from top to bottom!

### Programming an Authentication System

Let’s start with the functions that are used by both parts of the system.

#### ■ Making the `open_db` Function

You can open the database in the same way we explained in the first part of the lesson. Make sure the database and the file object it uses are both treated as global variables!

```python=1
import btree

file = None  # The file object we’re going to use as a database
db = None  # The database object

def open_db():
    global db, file  # Declares a global variables
    try:
        file = open("mydb", "r+b")  # If a file already exists, open that file
    except OSError:
        file = open("mydb", "w+b")  # Makes a new file if no file exists
    db = btree.open(file)  # Opens the database
```

#### ■ Making the `close_db` Function

Write the code below, and don’t forget to always close the database before closing its file!

###### New Code (Lines 15-19)
```python=1
import btree

file = None
db = None

def open_db():
    global db, file
    try:
        file = open("mydb", "r+b")
    except OSError:
        file = open("mydb", "w+b")
    db = btree.open(file)


def close_db():
    if db:  # Closes the database
        db.close()
    if file: # Closes the file
        file.close()
```

#### ■ Making the `get_account_info` Function

Next we need to make a function that asks the user to enter an email address and password in the terminal, then checks to make sure the strings it receives are in the correct format.

You can use the same regular expression for email addresses we used in the first part of the lesson, and set passwords to be 6-8 alphanumeric characters long. Define both of these regular expressions in your program and make `regex` objects for them.

###### New Code (Line 2, Lines 6-9)
```python=1
import btree
import ure

file = None
db = None
pat_email = "^(\w|-)+@(\w|-)+(\.(\w|-)+)+$" # A regular expression for email addresses
pat_pass = "^\w\w\w\w\w\w$|^\w\w\w\w\w\w\w$|^\w\w\w\w\w\w\w\w$" # A regular expression for passwords
regex_email = ure.compile(pat_email)  # The compiled regular expression for email addresses
regex_pass = ure.compile(pat_pass) # The compiled regular expression for passwords
```

The program should take the email address first and then the password and check both to see if they match the regular expressions you have prepared. If they don’t match, ask the user to re-enter their information. If all the formatting is correct, store the received email and password together in a tuple and return it when this function is called.

###### New Code (Line 27-42)
```python=20
def close_db():
    if db:
        db.close()
    if file:
        file.close()
        

def get_account_info():
    while True:  
        email_address = input("Email address >>> ")  # Email address input
        matchobj = regex_email.match(email_address)  # Formatting check
        if matchobj:  # If input matches the regular expression
            break  # End line 28’s while loop
        else:  # If input doesn’t match the regular expression
            print("Invalid email address. Please try again.")  # s the user to re-enter the data
    while True:
        password = input("Password (6-8 characters) >>> ")　# Password input
        matchobj = regex_pass.match(password)  # Formatting check
        if matchobj:  # If input matches the regular expression
            break  # End line 35’s while loop
        else:  # If input doesn’t match the regular expression
            print("Invalid password. Please try again.")  # Prompts the user to re-enter the data
    return (email_address, password)  # Puts the received data into a tuple and returns it
```

#### ■ Making the `register` Function

This function takes the account information users enter into the system and registers it in the database. Use the the three previous functions to make this process work in the following code:

###### New Code (Lines 45-53)
```python=45
def register():
    account_info = get_account_info()  # Gets the previously entered account information
    email_address = account_info[0] # Account information (email)
    password = account_info[1]  # Account information (password)
    open_db()  # Open the database
    db[email_address.encode("utf-8")] = password.encode("utf-8")  # Temporarily records information to the database
    db.flush()  # Writes information into the file as permanent data
    close_db()  # Closes the database
    print("Your account is registered.\n", email_address, password)  # Informs the user their registration is complete
```

Let’s test the `register()` function to make sure it can register account information to the database. Add a function called `show_db()` that can display the account information that’s currently stored in the database, then test out the program using an example email (`test@sample.com`) and password (`testtest`).

###### New Code (Lines 56-60)

```python=56
def show_db():
    open_db()  # Opens the database
    for key, value in db.items():  # Display the account information recorded in the database
        print(key.decode("utf-8"), value.decode("utf-8"))  
    close_db()  # Closes the database
```

##### Program Results

<pre class="prettyprint">
&gt;&gt;&gt; register()
Email address &gt;&gt;&gt; test@sample.com
Password (6-8 characters) &gt;&gt;&gt; testtest
Your account is registered.
 test@sample.com testtest

&gt;&gt;&gt; show_db()
test@sample.com testtest
</pre>

#### ■ Making the `login` Function

Finally we need to make the function that searches the database for account information in order to authenticate user logins. Let’s make the program only check the password after it confirms that the email address entered is registered in the database.

###### New Code (Lines 63-78)
```python=63
def login():
    account_info = get_account_info()  # Receives account information entry
    email_address = account_info[0] # Account information (email)
    password = account_info[1]  # Account information (password)
    open_db()  # Opens the database
    try:  # Check if the entered email is registered in the database
        db_pass = db[email_address.encode("utf-8")]
    except KeyError:  # If the email isn’t registered
        print("Your email address is not registered.")
        close_db() # Closes the database and ends the current process
        return
    close_db() # If the email is registered, close the database and check if the password matches
    if db_pass == password.encode("utf-8"):
        print("'{}' logged in.".format(email_address))  # Sends this message if authentication is successful
    else:
        print("The password you entered is incorrect.")  # Sends this message if authentication fails
```

### Testing the Program

Now your program is finished! The account information you recorded while checking the `register()` function is still recorded in your database, so try using it to check whether you can login successfully too!

<pre class="prettyprint">
&gt;&gt;&gt; login()
Email address &gt;&gt;&gt; test@sample.com
Password (6-8 characters) &gt;&gt;&gt; testtest
'test@sample.com' logged in.
</pre>

If errors occur in your program and you can’t figure out how to fix them, compare your code to the example below to check for mistakes!

##### Example Code 4-3-1

```python=1
import btree
import ure

file = None
db = None
pat_email = "^(\w|-)+@(\w|-)+(\.(\w|-)+)+$"
pat_pass = "^\w\w\w\w\w\w$|^\w\w\w\w\w\w\w$|^\w\w\w\w\w\w\w\w$"
regex_email = ure.compile(pat_email)
regex_pass = ure.compile(pat_pass)

def open_db():
    global db, file
    try:
        file = open("mydb", "r+b")
    except OSError:
        file = open("mydb", "w+b")
    db = btree.open(file)


def close_db():
    if db:
        db.close()
    if file:
        file.close()


def get_account_info():
    while True:
        email_address = input("Email address >>> ")
        matchobj = regex_email.match(email_address)
        if matchobj:
            break
        else:
            print("Invalid email address. Please try again.")   
    while True:
        password = input("Password (6-8 characters) >>> ")
        matchobj = regex_pass.match(password)
        if matchobj:
            break
        else:
            print("Invalid password. Please try again.")            
    return (email_address, password)


def register():
    account_info = get_account_info()
    email_address = account_info[0]
    password = account_info[1]
    open_db()
    db[email_address.encode("utf-8")] = password.encode("utf-8")
    db.flush()
    close_db()
    print("Your account is registered.\n", email_address, password)


def show_db():
    open_db()
    for key, value in db.items():
        print(key.decode("utf-8"), value.decode("utf-8"))
    close_db()


def login():
    account_info = get_account_info()
    email_address = account_info[0]
    password = account_info[1]
    open_db()
    try:
        db_pass = db[email_address.encode("utf-8")]
    except KeyError:
        print("Your email address is not registered.")
        close_db()
        return
    close_db()
    if db_pass == password.encode("utf-8"):
        print("{} logged in.".format(email_address))
    else:
        print("The password you entered is incorrect.")
```

## Challenge: Expanded Account Information

The account information you registered to your database in Example Code 3-3-1 was made up of one key (the email address) and one value (the password). Databases for actual online services usually record a lot more information for each user, like their name, birthdate, or phone number.

For this lesson’s challenge, try adding usernames (`username`) to the account information your system registers!

### Recording Multiple Data Points as One Value

Regular database systems can attach multiple values to the same key, but the databases we can make with the `bTree` module can only assign one value per key.

With the steps below, however, you can use a trick that lets you record multiple values anyway!

1. Make a tuple that stores multiple values into a text string, like this: `"('password', 'nickname')"`
2. Encode the text string as a byte string and record it in the database.
3. Decode values retrieved from the database from byte strings into text strings.
4. Evaluate the text string from step 3 with the `eval()` function to turn it into a `('password', 'username')` tuple.

Now let’s modify the `get_account_info()`, `register()`, and `login()` functions to make the message below appear when a user successfully logs in to the system!

<pre class="prettyprint">
Welcome, (username)!
</pre>

### Program Modifications

Let’s make changes to the `get_account_info()`, `register()`, and `login()` functions, one at a time.

#### ■ Changing the `get_account_info` Function

Add a regular expression for usernames to the start of the program so the system can register usernames. Let’s not set any particular restrictions on usernames and make their regular expression just any string of one or more alphanumeric characters.

###### New Code (Line 8, Line 11)
```python=1
import btree
import ure

file = None
db = None
pat_email = "^(\w|-)+@(\w|-)+(\.(\w|-)+)+$"
pat_pass = "^\w\w\w\w\w\w$|^\w\w\w\w\w\w\w$|^\w\w\w\w\w\w\w\w$"
pat_username = "^\w+$"  # A regular expression for usernames
regex_email = ure.compile(pat_email)
regex_pass = ure.compile(pat_pass)
regex_username = ure.compile(pat_username)  # The compiled regular expression for usernames
```

We’re going to request a username when the user registers, but not when they login, so we’ll need to add a new argument to the `get_account_info()` function called `_from`. This argument takes the name of the function it’s being called in (`"register"` or `"login"`) and it switches the function’s processes.

If `get_account_info()`  is called in the `login()` function, it returns information as soon as it receives gets an email and password, but if it’s called in the `register()` function, it also needs to get a username.

###### New and Changed Code (Line 29, Lines 44-53)

```python=29
def get_account_info(_from):
    while True:
        email_address = input("Email address >>> ")
        matchobj = regex_email.match(email_address)
        if matchobj:
            break
        else:
            print("Invalid email address. Please try again.")   
    while True:
        password = input("Password (6-8 characters) >>> ")
        matchobj = regex_pass.match(password)
        if matchobj:
            break
        else:
            print("Invalid password. Please try again.")
    if _from == "login":  # Ends the process here if this function is called in the login function
        return (email_address, password)  # Returns only two input values
    while True:  # Run the code below if this function is called in the register function
        username = input("Username >>> ")
        matchobj = regex_username.match(username)
        if matchobj:
            break
        else:
            print("Invalid username. Please try again.")
    return (email_address, password, username)  # Returns three input values
```

#### ■ Changing the `register` Function

Store the password and nickname in a tuple, turn the tuple into a text string, then encode that text string as a byte string and record it in the database. Remember to set the name of the function `get_account_info()` is being called to in its arguments!

###### New and Changed Code (Line 57, Line 60, Lines 62-63, Line 66)
```python=56
def register():
    account_info = get_account_info("register")  # Sets the function name in the argument
    email_address = account_info[0]
    password = account_info[1]
    username = account_info[2]  # The new username
    open_db()
    value = "('{}', '{}')".format(account_info[1], account_info[2])  # Puts the two data values into a tuple-formatted text string
    db[email_address.encode("utf-8")] = value.encode("utf-8")  # Change this from password to value
    db.flush()
    close_db()
    print("Your account is registered.\n", email_address, password, username)  # Shows the username too
```

#### ■ Changing the `login` Function

The values this function retrieves from the database need to be decoded from bytes into a text string. After that you can use the `eval()` function, which will create a tuple since the text string is in a tuple format.

###### New and Changed Code (Line 77, Line 82, Lines 88-92)
```python=76
def login():
    account_info = get_account_info("login")  # Sets the function name in the argument
    email_address = account_info[0]
    password = account_info[1]
    open_db()
    try:
        db_value = db[email_address.encode("utf-8")]  # Change db_pass to db_value
    except KeyError:
        print("Your email address is not registered.")
        close_db()
        return
    close_db()
    db_value = eval(db_value.decode("utf-8"))  # Evaluates the decoded string with eval
    db_pass = db_value[0]  # Password
    db_username = db_value[1]  # Username
    if db_pass == password:  # Change the condition since you’re getting the decoded password here.
        print("Welcome, {}!".format(db_username))  # Shows a message with the username included
    else:
        print("The password you entered is incorrect.")
```

### Testing the Program

The completed program should look like the example code below. Transfer your program and see how it works!

##### Example Code 5-3-1

```python=1
import btree
import ure

file = None
db = None
pat_email = "^(\w|-)+@(\w|-)+(\.(\w|-)+)+$"
pat_pass = "^\w\w\w\w\w\w$|^\w\w\w\w\w\w\w$|^\w\w\w\w\w\w\w\w$"
pat_username = "^\w+$"
regex_email = ure.compile(pat_email)
regex_pass = ure.compile(pat_pass)
regex_username = ure.compile(pat_username)

def open_db():
    global db, file
    try:
        file = open("mydb", "r+b")
    except OSError:
        file = open("mydb", "w+b")
    db = btree.open(file)


def close_db():
    if db:
        db.close()
    if file:
        file.close()


def get_account_info(_from):
    while True:
        email_address = input("Email address >>> ")
        matchobj = regex_email.match(email_address)
        if matchobj:
            break
        else:
            print("Invalid email address. Please try again.")   
    while True:
        password = input("Password (6-8 characters) >>> ")
        matchobj = regex_pass.match(password)
        if matchobj:
            break
        else:
            print("Invalid password. Please try again.")
    if _from == "login":
        return (email_address, password)
    while True:
        username = input("Username >>> ")
        matchobj = regex_username.match(username)
        if matchobj:
            break
        else:
            print("Invalid username. Please try again.")
    return (email_address, password, username)


def register():
    account_info = get_account_info("register")
    email_address = account_info[0]
    password = account_info[1]
    username = account_info[2]
    open_db()
    value = "('{}', '{}')".format(account_info[1], account_info[2])
    db[email_address.encode("utf-8")] = value.encode("utf-8")
    db.flush()
    close_db()
    print("Your account is registered.\n", email_address, password, username)


def show_db():
    open_db()
    for key, value in db.items():
        print(key.decode("utf-8"), value.decode("utf-8"))
    close_db()


def login():
    account_info = get_account_info("login")
    email_address = account_info[0]
    password = account_info[1]
    open_db()
    try:
        db_value = db[email_address.encode("utf-8")]
    except KeyError:
        print("Your email address is not registered.")
        close_db()
        return
    close_db()
    db_value = eval(db_value.decode("utf-8"))
    db_pass = db_value[0]
    db_username = db_value[1]
    if db_pass == password:
        print("Welcome, {}!".format(db_username))
    else:
        print("The password you entered is incorrect.")
```

## Conclusion
### A Quick Review

In this lesson, you learned about **databases** , **regular expressions** and **matching**.

Databases and regular expressions are both essential tools in the development of online systems. Python has a lot of different development environments, including ones for robotics like ArtecRobo, as well as development environments for games, apps, and online services!

The internet is already an essential part of people’s lives today. If someday you have an idea for your own new kind of web service, the things you’ve learned in this lesson are sure to come in handy!

### Coming Up Next Time!

In the next lesson we’ll talk about techniques you can use to securely transmit data over a network!