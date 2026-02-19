---
tags: Python_English, Topic 10
---

Python Robotics Course Lesson 40

Topic 10-4
Making a Security Gate
===

Learn how to use encryption and hashing to keep data safe!

## In This Lesson...

In this lesson we’ll learn about **encrypting**, **decrypting**, and **hashing** data, which are important techniques in safely transmitting data over a network.

:::info

Lesson Introduction

■ Lesson 40 Contents
1. Data Encryption and Decryption
2. Data Hashing 
3. Making a Security Gate with Authentication
4. Challenge: Checking Accounts in Advance with Regular Expressions


In this lesson we’ll learn about the security techniques used to keep data safe as it’s transmitted over a network.

We’ll start the lesson by explaining what data encryption is. Encrypted data can’t be decoded until you use decryption to decode it, which means that you can use encryption when you don’t want other people to see the information you transmit on a network!

Next we’ll explain what data hashing is. Unlike decryption, hashing converts data into a state where it can’t be decoded. This is what you use when you want to put strong protections on your data!

In the second half of these lesson we’ll these two techniques and use them to make a gate with a high-security authentication system. We’ll imagine this gate will be used at a school. Students will have to open the gate by entering their student ID and password.

Last, we’ll use the regular expressions we’ve learned about as we take on the challenge of adding a check which verifies that our data is in the correct format before authenticating it!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!

:::

## Data Encryption and Decryption

Data encryption and decryption is one of the most important IT concepts to use in order to safely transmit information between a sender and receiver on a network.

When you use a smartphone app to send someone a message or a picture, for example, this data is send to the person on the other end through a network (in this case, through your Internet connection). The data in this message is converted into bits and bytes (which we studied in Topic 10-2) which are transmitted over the network.

This network is spread over a variety of different places, just like a spider’s web! Each place on the network has an assigned address known as an **IP<sup>★</sup> address**. Every time you send data to a specific person, both of your IP addresses are used to find the most optimal route on the network before the data is delivered from hub to hub!
###### ★ IP is short for **Internet Protocol**. This is the main telecommunications standard used on the Internet!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_1_E.png"/>

This means there’s always the risk of someone on the network intercepting your data! Just as we were able to convert encoded numbers and text back into their original format, transmitting data over a network as-is would make it really easy to decode if someone steals it! This would mean that a complete stranger would be able to see your messages and pictures, or that your stolen data could be sent all over the network!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_2_E.png"/>

This is why we use a technique called encryption and decryption in over to get around this problem! In encryption, the sender **uses a set key to encrypt** their data before sending it over the network. Encrypted data is very hard to decipher unless you have that key. This means that, even if someone steals it, they won’t be able to see the original data! The intended receiver will then **uses a similar key to decrypt the data** and view the original content sent! 

### Cryptographic Schemes

Encryption and decryption come in **shared key** and **public key** formats, or **schemes**, each with the following characteristics. 

#### ■ Shared Key Cryptography

* Uses the same **shared key** to encrypt and decrypt data.
* Requires a safe exchange method for keys to prevent them from being intercepted.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_3_E.png"/>

#### ■ Public Key Cryptography

* Uses a **public key** to encrypt data and a separate **private key** to decrypt it.
* Only the receiver has the private key required to decrypt the data encrypted by a public key. This means that even if a third part has the public key they don’t be able to decrypt the data!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_4_E.png"/>

### Encryption and Decryption in MicroPython

The `ucryptolib` module in MicroPython has the features we need to encrypt and decrypt data. `ucryptolib` uses a shared key cryptography scheme and use can use it with the following three block cipher modes of operation: 

* Electronic Codebook (ECB)
* Cipher Block Chaining (CBC)
* Counter (CTR)

To put it in simple terms, a block cipher mode of operation is a mechanism used to encode the data. These modes vary in how secure they are as well as how many resources they take to calculate. Each of these modes takes quite a bit of time to explain in depth, so we’ll give a simple overview of electronic codebook (ECB) and cipher block chaining (CBC) here.

#### ■ Electronic Codebook (ECB)
This is the simplest of the three modes, and it encrypts data in units called blocks. ECB is very weak in terms of security!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_5_E.png"/>

#### ■ Cipher Block Chaining (CBC)
CBC is a more secure mode compared to ECB. The encryption result of one block is also used to encrypt the block that follows it, and multiple blocks are connected in a structure very similar to a chain!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_6_E.png"/>

Follow the link below to Wikipedia if you’d like to learn more about block cipher modes of operation:

[Block cipher mode of operation (Wikipedia)](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation)

### Encrypting and Decrypting Data

Of the two modes we introduced above, **CBC** is offers a much stronger encryption process. That means from this point forward we’ll be using CBC as we try encrypting and decrypting data ourselves! Since we want this program to work on a real network, we’ll be writing separate code for our sender, receiver, and for the network itself!

##### Program Functions

|Function|What does it do?|
|:---|:---|
|`encrypt_process`|Sender process which encrypts and sends messages of **16 characters or less** typed in to the terminal|
|`decrypt_process`|Receiver process which decrypts and displays received messages in the terminal|
|`network_process`|Network process which carries messages|

#### ■ Making the Sender Process

In order to encrypt a message using the `ucryptolib` module’s CBC mode, we’ll need a **16 byte** shared key as well as an initialization vector. An initialization vector is applied to the first block inside of the data you want to encrypt. Unlike the block which follow it, the first block has no encrypted data in front of it. This is why we use an initialization vector instead!

The shared key and the initialization vector can be any string you wish as long as they’re both 16 bytes! Let’s any random series of 16 characters (remember, one character is one byte) for our shared key and initialization vector!

```python=1
import ucryptolib

key = "abcdefghijklmnop"  # Common key (16 byes)
init_vector = "1234567891234567"  # Initialization vector (16 bytes)
```

Next we’ll create the objects we’ll use to encrypt and decrypt our data. We’ll use the `ucryptolib` module’s `aes()` function in order to do this.

```
Aes(common key, block cipher mode of operation, initialization vector *for CBC only)
```

Depending on the block cipher mode of operation we pick, we’ll set the second argument in this function to the to the numerical value below:

|Mode of Operation|Second Argument Value|
|:---|:---|
|Electronic Code Book (ECB)|1|
|Cipher Block Chaining (CBC)|2|
|Counter (CTR)|6|

The `encrypt()` and `decrypt()` methods in the objects returned by this function are what will encrypt and decrypt the data. If you run either of these methods from an encryption or decryption object, you won’t be able to run the other one. In other words, while you can take the same steps to make an encryption or a decryption object, a single object can only encrypt **OR** decrypt data!

Now let’s take what we’ve learned so far and use it to define the `encrypt_process()` function.

###### New Code (Lines 7-13)
```python=1
import ucryptolib

key = "abcdefghijklmnop"  # Common key (16 byes)
init_vector = "1234567891234567"  # Initialization vector (16 bytes)

def encrypt_process():  # Encrypts the message you type into the terminal
    aes_en = ucryptolib.aes(key, 2, init_vector)  # Creates an encryption object
    msg = input("Type your message here >>> ")  # Input prompt
    if len(msg) > 16:  # Ends the process if the message is longer than 16 characters
        print("Message should be 16 characters or less.")
        return
    padding_msg = "{:*>16}".format(msg)  # Padding to make the message 16 charaters (16 bytes)
    encrypted_msg = aes_en.encrypt(padding_msg)  # Encrypts the message
    network_process(encrypted_msg)  # Sends the message
```

Just like the shared key and the initialization vector, the message we encrypt needs to be 16 bytes. If the message is shorter than 16 characters, the program adds `*` on line 13 until it reaches the right length! We call this process **padding**.

Now we’re going to pass the message we’ve just encrypted to the `network_process` starting on line 15.

#### ■ Making the Network Process

The process for the network is actually quite simple. It uses a `print` statement to print the content of the encrypted message and passes it to the receiver process!

###### New Code (Lines 16-18)
```python=16
def network_process(msg):  # Sends message to the receiver.
    print("Sending ... {}".format(msg))  # Shows encrypted message
    decrypt_process(msg)  # Passes message to receiver process
```

#### ■ Making the Receiver Process

We’ll make the receiver process last. Once received, it decrypts the message using the decryption object’s `decrypt()` method and shows the original message.

###### New Code (Lines 20-25)
```python=20
def decrypt_process(msg):　 # Decrypts the received message
    aes_de = ucryptolib.aes(key, 2, init_vector)  # Creates a decryption object
    decrypted_msg = aes_de.decrypt(msg)  # Decryption
    decoded_msg = decrypted_msg.decode("utf-8")  # Converts byte string into text string
    original_msg = decoded_msg.strip("*")  # Strips padding from message
    print("Receiving ... {}".format(original_msg))  # Shows original message
```

Messages decrypted using the `decrypt()` method become byte strings. We can use the `decode()`method to turn the byte string into text, then use the `strip()` method on lines 23 and 24 to remove the `*` padding from the message.

#### ■ Running the Program

Now let’s run the program and call the `encrypt_process()` function from the terminal. Type any message you like, then run the remaining processes to see the result!

##### Example Code 2-3-1

```python=1
import ucryptolib

key = "abcdefghijklmnop"  # Common key (16 byes)
init_vector = "1234567891234567"  # Initialization vector (16 bytes)

def encrypt_process():  # Encrypts the message you type into the terminal
    aes_en = ucryptolib.aes(key, 2, init_vector)  # Creates an encryption object
    msg = input("Type your message here >>> ")  # Input prompt
    if len(msg) > 16:  # Ends the process if the message is longer than 16 characters
        print("Message should be 16 characters or less.")
        return
    padding_msg = "{:*>16}".format(msg)  # Padding to make the message 16 characters (16 bytes)
    encrypted_msg = aes_en.encrypt(padding_msg)  # Encrypts the message
    network_process(encrypted_msg)  # Sends the message
    
def network_process(msg):  # Sends message to the receiver.
    print("Sending ... {}".format(msg))  # Shows encrypted message
    decrypt_process(msg)  # Passes message to receiver process

def decrypt_process(msg):　 # Decrypts the received message
    aes_de = ucryptolib.aes(key, 2, init_vector)  # Creates a decryption object
    decrypted_msg = aes_de.decrypt(msg)  # Decryption
    decoded_msg = decrypted_msg.decode("utf-8")  # Converts byte string into text string
    original_msg = decoded_msg.strip("*")  # Strips padding from message
    print("Receiving ... {}".format(original_msg))  # Shows original message
```

##### Running the Code

<pre class="prettyprint">
&gt;&gt;&gt; encrypt_process()
Input your message &gt;&gt;&gt; Hello
Sending ... b'\x02\nKQ]{\xf7E\x1d\x0f\xb9\xe9\x0b#P['

Receiving ... Hello
</pre>

The example above sends the message `Hello`. Convert this message into a byte string and it becomes `b'\x02\nKQ]{\xf7E\x1d\x0f\xb9\xe9\x0b#P['`. Compare each output in the terminal and you’ll see that the message has been encrypted and can’t be deciphered!

## Hashing to Lock Encrypted Data

We’re going to introduce hashing here. Hashing is a technique which converts data into an indecipherable format, just like encryption. A major difference between encryption and hashing is that hashed data **can’t be decrypted**!

The fact that hashed data can’t be returned to an original state means that it can’t be used to protect messages and other data that you’d like someone else to see. So when would you want to use hashing?

### The Case for Hashing

You’ll find hashing used to **safeguard passwords in a database**.

Of all of the information you can find in a database, passwords are the ones you want to be most careful about protecting! If someone uses passwords leaked from the database, they can pretend to be a member and do all sorts of bad things!

However, there’s no way to tell what a password is once it’s been hashed. This means that even if it leaks any potential damage is kept to a minimum!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_7_E.png"/>

### Hashing in MicroPython

The `uhashlib` module in MicroPython has the features we need to hash data. This module supports the following three hashing algorithms.

* SHA256
* SHA1 
* MD5

While it’s difficult to explain these algorithms in detail, what we want to remember for now is that **MD5** is weakest, **SHA1** is in the middle, and *SHA256** is strongest in terms of security! 

Now let’s think of the authentication system we made in Topic 10-3 as we try hashing the passwords in the database. We’re going to do this using the **SHA256** hashing algorithm.

#### ■ Preparing a Database with Hashed Passwords

In order to keep our program simple, we’ll use a dictionary as a data base instead of the `btree` module. This dictionary uses user IDs as keys. The passwords are first hashed by the `uhashlib` module’s `sha256()` function and then converted into byte strings using the `digest()` function before being set as values.

```python=1
import uhashlib

db = {  # Dictionary used as a virtual database
    "id1": uhashlib.sha256("123456").digest(),  # Password hashing and byte stringification
    "id2": uhashlib.sha256("654321").digest(),
    "id3": uhashlib.sha256("223344").digest(),
}
```

#### ■ Making the Login Function

Let’s define the function which uses the user ID and password typed by the user to perform login authentication. This function uses the user ID to pull the password stored on the database. It then hashes the password entered by the user and converts it into a byte string before comparing it with the database password!

###### New Code (Lines 9-19)
```python=1
import uhashlib

db = {
    "id1": uhashlib.sha256("123456").digest(),
    "id2": uhashlib.sha256("654321").digest(),
    "id3": uhashlib.sha256("223344").digest(),
}

def login(_id, _pass):  # Authenticates user ID and password
    try:  # Checks whether ID is in the database
        db_pass = db[_id]
    except KeyError:  # If ID doesn\’t exist
        print("ID doesn't exist.")
        return
    # Hashes entered password and compares it with the password in the database
    if db_pass == uhashlib.sha256(_pass).digest():  # If authentication is successful
        print("Login successful!")
    else:  # If authentication fails
        print(“The password is incorrect.")
```

#### ■ Making a Data Breach Function

Let’s imagine that our database has been leaked and make a function which shows a table of the information stored in there. Now your program is finished!

##### Example Code 3-2-1
###### New Code (Lines 20-23)
```python=1
import uhashlib

db = {
    "id1": uhashlib.sha256("123456").digest(),
    "id2": uhashlib.sha256("654321").digest(),
    "id3": uhashlib.sha256("223344").digest(),
}

def login(_id, _pass):
    try:
        db_pass = db[_id]
    except KeyError:
        print("ID doesn't exist.")
        return
    if db_pass == uhashlib.sha256(_pass).digest():
        print("Login successful!")
    else:
        print(“The password is incorrect.")

def leak(): # Simulates a data leak
    print(db)  # Prints a table of database info
```


#### ■ Running the Program

Now let’s try running our finished program. We’ll start by calling the `login()` function and use a valid, then an invalid user ID and password combination and compare the results!

##### Running the Code

<pre class="prettyprint">
&gt;&gt;&gt; login("id1", "1234")
Your password is incorrect.
&gt;&gt;&gt; login("id4", "123456")
ID doesn't exist.
&gt;&gt;&gt; login("id1", "123456")
Login successful!
</pre>

We can perform login authentication with no problems since this program compares two hashed passwords!

Next, let’s run the `leak()` function to see the passwords stored in the database!

##### Running the Code

<pre class="prettyprint">
&gt;&gt;&gt; leak()
{'id3': b'\x19\xe5\x8e\xfc\x7fq\xd3\xec\x0b\xd4mE\x1e\x84gO\x07,\xcct\xc3\x12\x8fO\x01~i\x81\xd4\xe9%C', 'id1': b'\x8d\x96\x9e\xefn\xca\xd3\xc2\x9a:b\x92\x80\xe6\x86\xcf\x0c?]Z\x86\xaf\xf3\xca\x12\x02\x0c\x92:\xdcl\x92', 'id2': b'H\x1fl\xc0Q\x11C\xcc\xdd~-\x1b\x1b\x94\xfa\xf0\xa7\x00\xa8\xb4\x9c\xd19"\xa7\x0bZ\xe2\x8a\xca\xa8\xc5'}
</pre>

Thought it’s a bit hard to tell since the data has been converted into byte strings, we can see that our simple six-digit passwords have turned into much more complex data. Even if someone were to take a peek at this data, it would be virtually impossible for them to guess the original password!

## Building a Gate

In the second half of the lesson, we’ll apply the encryption, decryption, and hashing techniques we’ve just learned to make a gate with a highly-secure authentication system. Let’s start by using blocks to build the gate!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_8_E.png"/>

### Parts You'll Need

##### Parts
* Core Unit x 1
* Robot Expansion Unit x 1
* Battery Box x 1
* Servomotor x 1
* Basic Cube (Gray) x 2
* Basic Cube (Red) x 4
* Half A (Gray) x 3
* Half B (Black) x 1
* Half C (White) x 12
* Half D (White) x 8
* Beam x 7
* Gear (S) x 1
* Rack ×1

##### The Artec Block Shapes

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_9_E.png"/>

### Building the Gate

Follow the link below to find instructions for building the gate:

[Building a Gate](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_build_E.pdf)

## Making a Security Gate

While part of what makes the Internet so convenient is that anyone can access it, bad people who get private data from virus attacks or phishing scams and use it for identity theft can make the Internet a very dangerous place! 

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_10_E.png"/>

In order to prevent this, providers for email, messaging services, blogs, online games, and e-commerce sites issue every user their own unique account with a user ID and password.

In recent years we’ve also seen the rise of incredibly secure authentication systems like **two-factor authentication using one-time passwords** through phones as well as **biometric authentication using fingerprints and faces**.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_11_E.png"/>

And of course security is an issue in more places than just the Internet. Offices and apartment buildings, for example, might have employees and residents enter a PIN or use a card to open the door to keep unwanted people out!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_12_E.png"/>

This is why we’ll be making a gate which authenticates user IDs and passwords to restrict who can enter! Let’s imagine that this gate is installed at a school. Each student is assigned an account with a **student number from 1 to 999 as well as a four-digit password**.

Our Security Gate architecture will use encryption and decryption from the `ucryptolib` module, hashing from the `uhashlib` module, and a database built using the `btree` module that we learned about in the last lesson!

##### The Security Gate Architecture

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_13_E.png"/>

We’ll need to create the following functions to make the architecture for our gate:

##### Security Gate Functions

|Function|What does it do?|
|:---|:---|
|`register_student()`|Registers account info to database|
|`show_db()`|Shows a list of accounts registered to the database|
|`gate_process()`|Runs the gate process|
|`auth_process()`|Runs the authentication process|
|`main()`|Runs the other functions in the program|

Now let’s make these five functions in the order below to complete our program!

### Making the Program

The program we’re going to make is a bit complex in structure, so we’ll have to check the results of each part as we code!

#### ■ Registering Student Accounts (register_students Function)

This function **registers the student account in the argument to the database**. Each account is has the following three parts. The student ID is registered as a key and the student’s name as well as their four-digit password are registered as values.

##### Student Account Info

* `student_ID`: The student’s ID (key)
* `name`: The student’s name (value)
* `password`: The student’s four-digit password (value)

If we want to register multiple account dictionaries we’ll put them into a list!

##### Example Account List

```
student_list = [  # Student accounts to register
    {"student_ID": 1, "name": "Tanaka", "password": "2312"},
    {"student_ID": 2, "name": "Ogura", "password": "4512"},
    {"student_ID": 3, "name": "Hatanaka", "password": "7543"},
    {"student_ID": 4 , "name": "Morita", "password": "0324"},
]
register_student(student_list)  # Sets list as an argument and registers accounts
```

Next we’ll pull each account from the list in order and register it to the database. We’ll hash the password using `SHA256` before recording it.

Let’s start by writing the process which opens the database. You can name the file you’ll use as a database anything you like!

```python=1
import btree

FILENAME = "school_db"  # Filename used for the database

def register_student(_list):
    try:
        file = open(FILENAME, 'r+b')  # Opens database file (write mode)
    except OSError:  # Throws OSError exception if file doesn’t exist
        file = open(FILENAME, 'w+b')  # Creates new file if file doesn\’t exist (read mode)
    db = btree.open(file)  # Creates database from file
```

Next, each account will be pulled from the list in the argument, converted into a byte string, and hashed in order. Since the student’s name and password will be recorded as one value, we’ll join these two pieces of data with a `,`.

Once all accounts have been temporarily stored to the database, we’ll use the `flush()` method to write it to the file. Let’s make sure to close the database and the file as the last step!

New Code (Line 2, Lines 13-21)

```python=1
import btree
import uhashlib  # Module used for hashing

FILENAME = "school_db"

def register_student(_list):
    try:
        file = open(FILENAME, 'r+b')
    except OSError:
        file = open(FILENAME, 'w+b')
    db = btree.open(file)
    
    for data in _list:
        key = data["student_ID"].to_bytes(1, "big")  # Converts integer into big-endian byte string
        name = (data["name"] + ",").encode("utf-8")  # Adds , at the end to join with the password and registers them as a single value
        password = uhashlib.sha256(data["password"]).digest() # Hashes password and converts it to a byte string
        val = name + password  # Registers joined name and password as a value
        db[key] = val  # Temporarily records value
    db.flush()  # Writes to file
    db.close()  # Closes database
    file.close()  # Closes file used for the database
```

Now that we’ve made the function which registers accounts to the database, let’s made and run some test accounts!

```python=24
student_list = [ # Accounts to register to the database
    {"student_ID": 1, "name": "Tanaka", "password": "2312"},
    {"student_ID": 2, "name": "Ogura", "password": "4512"},
    {"student_ID": 3, "name": "Hatanaka", "password": "7543"},
    {"student_ID": 4 , "name": "Morita", "password": "0324"},
]
register_student(student_list)  # Registers accounts to database
```

Our database registry is successful if there are no errors, but it will be a bit difficult to directly check which accounts have been registered. In order to do this, we’ll make a function which shows us a table of the information in the database!

#### ■ Showing the Database Information (show_db Function)

We’ll define the `show_db()` function below the `register_students()` function. The function will start with a process which opens the database. Since this function won’t need to write anything to the database, we’ll open it in `”rb”`, or read-only binary mode!

###### New Code (Lines 24-30)

```python=24
def show_db():
    try:
        file = open(FILENAME, "rb")  # Opens database file (read-only mode)
    except OSError:  # If it fails to open
        print("{} doesn't exist.".format(FILENAME))
        return  # Ends process here since the file doesn\’t exist
    db = btree.open(file)
```

The information in the database we’ve just open will appear in the terminal. Since databases have an `items()` method just like dictionaries, we can write `for key, val in db.items():` to pull the keys and values out of the database in order! The value is the student’s name and password joined by a `,`, we can use the text string `split()` method to separate them. Notice that we use `b","` for our `,` separator on line 33 to make the separated value a byte string!

###### New Code (Lines 31-38)

```python=24
def show_db():
    try:
        file = open(FILENAME, "rb")
    except OSError:
        print("{} doesn't exist.".format(FILENAME))
        return
    db = btree.open(file)
    for key, val in db.items():  # btree objects have an items() just like dictionaries
        num = int.from_bytes(key.decode(), "big")  # Returns big-endian byte string to an integer
        val = val.split(b",")  # Splits name and password joined by , into lists
        name = val[0].decode("utf-8")  # Returns byte string to a text string
        password = val[1]  # Password is hashed and can\’t be returned to original state
        print("{}, {}, {}".format(num, name, password))
    db.close()  # Don\’t forget to close the database and file!
    file.close()
```

Now let’s turn the section which registers test accounts into comments. We’ll then add the code which runs the `show_db()` function below it and see how our program runs so far!

###### New and Changed Code (Lines 40-49)

```python=40
""" Comment this part out
student_list = [ 
    {"student_ID": 1, "name": "Tanaka", "password": "2312"},
    {"student_ID": 2, "name": "Ogura", "password": "4512"},
    {"student_ID": 3, "name": "Hatanaka", "password": "7543"},
    {"student_ID": 4 , "name": "Morita", "password": "0324"},
]
register_student(student_list)
"""
show_db()  # Shows a table of information on the database
```

##### Program Results
<pre class="prettyprint">
1, Tanaka, b"\x08\xdd\x19\xeb\xe32\xae\xb6{\xca0X\x9eT\xe6'#)I\xc77=>5?\x94\xc2\x1fi\xa9\xd9\xc5"
2, Ogura, b'\x035\x81\xb0\x97\x04w\xaf\x91o\x08\xad\x93\x1cR\x1b\x97[U\xf86\x86\x04\xaeX\xa57\xb3\x87\xe7\x92\xe1'
3, Hatanaka, b'C\xd5\x99hR\xcc\x0c{\xea\x16\x14\x8b\xb2r\x1e\xb3\x17\xcd\x9e\x17&\x15\x11\x08\x96\xd0\xa6\xdbd\xc4e\x0c'
4, Morita, b'\x87R\xf2N\xc0\xa8\xacP\xefs/\xba\xa2o-\xf1\xce\xa3.G{\x8dJ\xd4\x16\x07H\x15^\xd20T'
</pre>

Your program works if your results look like this. Since we’ve done some complex hashing on the passwords, it’s nearly impossible to guess the original four-digit ones!

#### ■ Sending Input to the Authentication Process (gate_process Function)

Now that we’ve registered the test accounts we’re going to write the processes which authenticate these accounts and open or close the gate.

Let’s start by making the process which allows the gate to take input before encrypting and sending the data. Our account information will be put into a **queue** and passed from the gate to the authentication process.

###### New Code (Line 3, Lines 6-8, Lines 42-50)
###### ★ The shared key and initialization can be any combination of 16 characters!
```python=1
import btree
import uhashlib
import ucryptolib  # Imports module for encryption and decryption

FILENAME = "school_db"
crypto_key = "python1234python"  # Shared key for encryption and decryption (16 characters)
init_vector = "0123456789012345"  # Initialization vector (16 bytes)
queue_gate_to_auth = []  # This queue passes information from the gate to the authentication process
```

```python=38
    db.close()
    file.close()
    

def gate_process():
    aesobj = ucryptolib.aes(crypto_key, 2, init_vector)  # Creates object to encrypt data

    while True:
        num = input("Input your student number: >>>")  # Requests student number
        password = input("Input your password: >>>")  # Requests password
        num = aesobj.encrypt("{:*>16}".format(num))  # Pads if less than 16 characters (16 bytes) and encrypts
        password = aesobj.encrypt("{:*>16}".format(password))  # Pads if less than 16 characters (16 bytes) and encrypts
        queue_gate_to_auth.append((num, password))  # Adds encrypted student number and password to queue
```

#### ■ Receiving and Decrypting Account Info (auth_process Function)

Next we’ll make the process which has the authorization process take and decrypt the encrypted account info. The account info we want to authenticate is added to the `queue_gate_to_auth` queue, so we’ll add code to check it at regular intervals and pull out any account info for decryption.

Since the accounts sent were padded with `*` if they were less than 16 characters/bytes, we’ll use the text string `strip()` method to remove the padding! We’ll also have to turn the byte strings for the student numbers back into integers.

###### New Code (Line 4, Lines 54-66)
```python=1
import btree
import uhashlib
import ucryptolib
import time  # Imports module to manage time
```
```python=54
def auth_process():
    aesobj = ucryptolib.aes(crypto_key, 2, init_vector)  # Creates object for decryption
    
    while True:
        time.sleep_ms(100)  # Checks whether any information has been entered at the gate every 100 milliseconds
        if len(queue_gate_to_auth):  
            data = queue_gate_to_auth.pop(0)  # Receives info entered at the gate
            num = aesobj.decrypt(data[0])  # Decrypts data
            num = num.strip(b"*")  # Deletes * padding
            num = int(num)  # Converts to integer
            password = aesobj.decrypt(data[1])
            password = password.strip(b"*")
            print("Received", num, password)  # Shows in terminal for verification
```

Now let’s see how the process we’ve just added works! We’ll have to add code which makes a thread to run the `auth_process()` function.

###### New and Changed Code (Line 5, Lines 78-80)

```python=1
import btree
import uhashlib
import ucryptolib
import time
import _thread  # Imports multithreading module
```
```python=70
""" Comment this part out
student_list = [ 
    {"student_ID": 1, "name": "Tanaka", "password": "2312"},
    {"student_ID": 2, "name": "Ogura", "password": "4512"},
    {"student_ID": 3, "name": "Hatanaka", "password": "7543"},
    {"student_ID": 4 , "name": "Morita", "password": "0324"},
]
register_student(student_list)
show_db()  # Comments out the part which runs this function
"""
_thread.start_new_thread(auth_process,())
```

Now we’ll run the program and call the `gate_process()` function from the terminal before entering a student number and password. The program works correctly if you immediately see a message stating that the data has been received from the `auth_process()` function!

<pre class="prettyprint">
&gt;&gt;&gt; gate_process()
Input your student number: &gt;&gt;&gt;1
Input your password: &gt;&gt;&gt;1234
Input your student number: &gt;&gt;&gt;Received 1 b'1234'
</pre>

#### ■ Authenticating Account Info (auth_process Function

Since we can now receive accounts for authentication, we’ll make the process which authenticates accounts by comparing them with the information in the database!

We’ll start by adding code to the beginning of the `auth_process()` function which opens the database. Since we don’t need the `print` statement on line 74, feel free to turn it into a comment or delete it entirely!

###### New and Changed Code (Lines 58-63, Line 74)
```python=55
def auth_process():
    aesobj = ucryptolib.aes(crypto_key, 1)
    
    try:
        file = open(FILENAME, 'rb')  # Opens in read-only mode
    except OSError:
        print("{} doesn't exist.".format(FILENAME))
        return  # Ends process here if the file doesn\’t exist
    db = btree.open(file)
    
    while True:
        time.sleep_ms(100)
        if len(queue_gate_to_auth):
            data = queue_gate_to_auth.pop(0)
            num = aesobj.decrypt(data[0])
            num = num.strip(b"*")
            num = int(num)
            password = aesobj.decrypt(data[1])
            password = password.strip(b"*")
            # print("Receiving", num, password)  # Comment this out since we don\’t need it
```

The program will use the student number of the account to search for that account in the database. If the student number doesn’t exist, the program will tell the gate process that the authentication has failed! We’ll make a new queue which we’ll use to pass this information back to the gate. The information sent will consist of a `(boolean for authentication result, message text string)` tuple. We’ll make the gate process use `True` or `False` values to tell if the authentication was successful!

##### New Code (Line 11, Lines 78-82)
```python=7
FILENAME = 'school_db'
crypto_key = "python1234python"
init_vector = "0123456789012345"
queue_gate_to_auth = []
queue_auth_to_gate = []  # Queue used to pass info from the authentication process to the gate process
```
```python=68
    while True:
        time.sleep_ms(100)
        if len(queue_gate_to_auth):
            data = queue_gate_to_auth.pop(0)
            num = aesobj.decrypt(data[0])
            num = num.strip(b"*")
            num = int(num)
            password = aesobj.decrypt(data[1])
            password = password.strip(b"*")
            # print("Receiving", num, password)
            try:  # Searches for the student number in the database and retrieves it
                db_val = db[num.to_bytes(1, "big")]  # Just like when registering the account to the database, value is converted to a big-endian byte string
            except KeyError:  # If target student number isn\’t present in the database
                queue_auth_to_gate.append((False, "Your number doesn't exist."))  # Message informing user that the number doesn\’t exist is passed to the gate through a queue
                continue  # Moves on to next loop
```

If a hit is returned when search for the student number, the value is loaded from the database and the joined student name and password are split before being taken out. The `show_db()` function takes these same steps. The sent password is hashed in order to compare it with the password in the database. The authentication is successful if the passwords match, which sends a tuple containing a `True` value and a message with the student’s name to the gate. If they don’t match, we’ll send a tuple containing `False` and a message notifying the user the password is incorrect! 

###### New Code (Lines 83-89)

```python=68
    while True:
        time.sleep_ms(100)
        if len(queue_gate_to_auth):
            data = queue_gate_to_auth.pop(0)
            num = aesobj.decrypt(data[0])
            num = num.strip(b"*")
            num = int(num)
            password = aesobj.decrypt(data[1])
            password = password.strip(b"*")
            # print("Receiving", num, password)
            try:
                db_val = db[num.to_bytes(1, "big")]
            except KeyError:
                queue_auth_to_gate.append((False, "Your number doesn't exist."))
                continue
            db_val = db_val.split(b",")  # Splits student name and password joined by “,”
            db_name = db_val[0].decode("utf-8")  # Uses UTF-8 to decode name
            db_pass = db_val[1]  # Uses hashed byte string for password
            if uhashlib.sha256(password).digest() == db_pass:  # Hashes sent password and compares it with the password in the database
                queue_auth_to_gate.append((True, "Hi " + db_name + “!”))  # Data sent to gate if passwords match
            else:
                queue_auth_to_gate.append((False, "Your password is incorrect."))  # Data sent to gate if passwords don\’t match
```

#### ■ Opening and Closing the Gate Based on the (gate_process Function)

To continue, we7ll make the process which has the gate process take the authentication result and use it to open or close the gate.

The gate’s process uses a Servomotor to open or close as well as the Core Unit’s LED display and buzzer to let the user know the result of the authentication! We’ll need to import the objects and classes we need at the beginning of the program and create instances of each one.

###### New Code (Lines 6-7, Line 14)

```python=1
import btree
import uhashlib
import ucryptolib
import time
import _thread
from pystubit.board import display, buzzer, Image  # New import
from pyatcrobo2.parts import Servomotor  # New import

FILENAME = "school_db"
crypto_key = "python1234python"
init_vector = "0123456789012345"
queue_gate_to_auth = []
queue_auth_to_gate = []
servo = Servomotor("P16")  # Creates an instance
```

After typing the account info in the terminal and sending it to the authentication process, we’ll have the gate wait until it receives the result from the authentication process (this means that there’s data in the queue).

###### New Code (Lines 60-62)
```python=51
def gate_process():
    aesobj = ucryptolib.aes(crypto_key, 1)

    while True:
        num = input(“Input your student number: >>>")
        password = input("Input your password: >>>")
        num = aesobj.encrypt("{:*>16}".format(num))
        password = aesobj.encrypt("{:*>16}".format(password))
        queue_gate_to_auth.append((num, password))
        while not len(queue_auth_to_gate):  # Waits until result is returned from the authentication process
            time.sleep_ms(100)
            pass
```

Let’s check the content of the result once we receive it. A message will appear and the LED display and buzzer will notify the user whether the authentication is successful or not. If it was successful, the Servomotor will turn to open the gate for five seconds!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_10-4/10-4_14_E.png"/>

###### New Code (Lines 63-80)
```python=51
def gate_process():
    aesobj = ucryptolib.aes(crypto_key, 1)

    while True:
        num = input(“Input your student number: >>>")
        password = input("Input your password: >>>")
        num = aesobj.encrypt("{:*>16}".format(num))
        password = aesobj.encrypt("{:*>16}".format(password))
        queue_gate_to_auth.append((num, password))
        while not len(queue_auth_to_gate):
            time.sleep_ms(100)
            pass
        result = queue_auth_to_gate.pop(0)  # Retrieves authentication result
        print(result[1])  # Shows message received from authentication process
        if result[0]:  # If authentication is successful
            display.show(Image.YES, delay=0, color=(0, 31, 0))
            buzzer.on("72", duration=200)
            time.sleep_ms(100)
            buzzer.on("68", duration=600)
            servo.set_angle(75)  # Opens gate for 5 seconds
            time.sleep_ms(5000)
            servo.set_angle(180)
            time.sleep_ms(1000)
            display.clear()
        else:  # If authentication fails
            display.show(Image.NO, delay=0)
            buzzer.on("48", duration=200)
            time.sleep_ms(100)
            buzzer.on("48", duration=600)
            display.clear()
```


#### ■ Putting Processes in the main Function

Now we have all of the features our gate needs. We’ll finish by putting everything into the `main()` function, including the parts we’ve turned into comments!

###### New Code (Lines 117-128)
```python=111
            if uhashlib.sha256(password).digest() == db_pass:
                queue_auth_to_gate.append((True, "Hi " + db_name + "!"))
            else:
                queue_auth_to_gate.append((False, "Your password is incorrect."))


def main():   
    student_list = [ # This part doesn\’t need to be a comment anymore!
        {"student_ID": 1, "name": "Tanaka", "password": "2312"},
        {"student_ID": 2, "name": "Ogura", "password": "4512"},
        {"student_ID": 3, "name": "Hatanaka", "password": "7543"},
        {"student_ID": 4 , "name": "Morita", "password": "0324"},
    ]
    # Delete show_db() since it doesn\’t need to run any more
    register_student(student_list)  # Registers students to database
    servo.set_angle(180)  # Starts by closing the gate
    _thread.start_new_thread(auth_process, ())  # Starts a thread to run the authentication process
    gate_process()  # Runs the process for the gate
```

### Checking the Program

Now let’s run the finished program and type in a real account as well as a fake account to see how the program works!

##### Example Login
<pre class="prettyprint">
&gt;&gt;&gt; main()
Input your student number: &gt;&gt;&gt;1
Input your password: &gt;&gt;&gt;2312
Hi Tanaka!

Input your student number: &gt;&gt;&gt;2
Input your password: &gt;&gt;&gt;4513
Your password is incorrect.

Input your student number: &gt;&gt;&gt;5
Input your password: &gt;&gt;&gt;2233
Your number doesn't exist.
</pre>

If you’re having trouble getting your program to work, try comparing it line by line with the example code below to check for any mistakes!

##### Example Code 4-3-1

```python=1
import btree
import uhashlib
import ucryptolib
import time
import _thread
from pystubit.board import display, buzzer, Image
from pyatcrobo2.parts import Servomotor

FILENAME = "school_db"
crypto_key = "python1234python"
init_vector = "0123456789012345"
queue_gate_to_auth = []
queue_auth_to_gate = []
servo = Servomotor("P16")

def register_student(_list):
    try:
        file = open(FILENAME, 'r+b')
    except OSError:
        file = open(FILENAME, 'w+b')
    db = btree.open(file)
    
    for data in _list:
        key = data["student_ID"].to_bytes(1, "big")
        name = (data["name"] + ",").encode("utf-8")
        password = uhashlib.sha256(data["password"]).digest()
        val = name + password
        db[key] = val
    db.flush()
    db.close()
    file.close()


def show_db():
    try:
        file = open(FILENAME, "rb")
    except OSError:
        print("{} doesn't exist.".format(FILENAME))
        return
    db = btree.open(file)
    for key, val in db.items():
        num = int.from_bytes(key.decode(), "big")
        val = val.split(b",")
        name = val[0].decode("utf-8")
        password = val[1] 
        print("{}, {}, {}".format(num, name, password))
    db.close()
    file.close()


def gate_process():
    aesobj = ucryptolib.aes(crypto_key, 2, init_vector)

    while True:
        num = input(“Input your student number: >>>")
        password = input("Input your password: >>>")
        num = aesobj.encrypt("{:*>16}".format(num))
        password = aesobj.encrypt("{:*>16}".format(password))
        queue_gate_to_auth.append((num, password))
        while not len(queue_auth_to_gate):
            time.sleep_ms(100)
            pass
        result = queue_auth_to_gate.pop(0)
        print(result[1])
        if result[0]:
            display.show(Image.YES, delay=0, color=(0, 31, 0))
            buzzer.on("72", duration=200)
            time.sleep_ms(100)
            buzzer.on("68", duration=600)
            servo.set_angle(75)
            time.sleep_ms(5000)
            servo.set_angle(180)
            time.sleep_ms(1000)
            display.clear()
        else:
            display.show(Image.NO, delay=0)
            buzzer.on("48", duration=200)
            time.sleep_ms(100)
            buzzer.on("48", duration=600)
            display.clear()


def auth_process():
    aesobj = ucryptolib.aes(crypto_key, 2, init_vector)
    
    try:
        file = open(FILENAME, 'rb')
    except OSError:
        print("{} doesn't exist.".format(FILENAME))
        return 
    db = btree.open(file)
    
    while True:
        time.sleep_ms(100)
        if len(queue_gate_to_auth):
            data = queue_gate_to_auth.pop(0)
            num = aesobj.decrypt(data[0])
            num = num.strip(b"*")
            num = int(num)
            password = aesobj.decrypt(data[1])
            password = password.strip(b"*")
            # print("Receiving", num, password)
            try:
                db_val = db[num.to_bytes(1, "big")]
            except KeyError:
                queue_auth_to_gate.append((False, "Your number doesn't exist."))
                continue
            db_val = db_val.split(b",")
            db_name = db_val[0].decode("utf-8")
            db_pass = db_val[1]
            if uhashlib.sha256(password).digest() == db_pass:
                queue_auth_to_gate.append((True, "Hi " + db_name + "!"))
            else:
                queue_auth_to_gate.append((False, "Your password is incorrect."))


def main():
    student_list = [
        {"student_ID": 1, "name": "Tanaka", "password": "2312"},
        {"student_ID": 2, "name": "Ogura", "password": "4512"},
        {"student_ID": 3, "name": "Hatanaka", "password": "7543"},
        {"student_ID": 4 , "name": "Morita", "password": "0324"},
    ]
    register_student(student_list)
    servo.set_angle(180)
    _thread.start_new_thread(auth_process, ())
    gate_process()
```

## Challenge: Checking Accounts in Advance with Regular Expressions

In the last lesson we learned how to use matching with regular expressions, making a simple authentication system which checked the email addresses and passwords typed into the terminal for errors!

That’s why in this challenge we’re going to review this concept as we add a feature to Example Code 4-3-1 **which asks students to re-type their account if they make a mistake**!

|Account Data|Format|
|:---|:---|
|Student ID|A three-digit number from 1-999|
|Password|A four-digit combination of any numbers from 0-9|

Let’s take a look back at the last lesson as we review how to write regular expressions and perform matching!

### Changing the Program

Let’s import the `ure` module in order to use regular expressions.

###### New Code (Line 6)
```python=1
import btree
import uhashlib
import ucryptolib
import time
import _thread
import ure  # This module allows for use of simple regular expressions
from pystubit.board import display, buzzer, Image
from pyatcrobo2.parts import Servomotor
```

Now let’s define the regular expressions for the student ID and password and create regex objects to match these expressions!

###### New Code (Line 54-57)
```python=52
def gate_process():
    aesobj = ucryptolib.aes(crypto_key, 2, init_vector)
    pat_student_ID = "^[1-9]$|^[1-9]\d$|^[1-9]\d\d$"  # Student ID (1-999)
    pat_pass = "^\d\d\d\d$"  # The four-digit password
    regex_student_ID = ure.compile(pat_student_ID)  # Creates a regex object
    regex_pass = ure.compile(pat_pass)
```

We’ll match any student ID and password typed into the terminal. If either one fails to match, a `continue` statement will prompt the student to retype their account!

###### New Code (Lines 62-69)
```python=52
def gate_process():
    aesobj = ucryptolib.aes(crypto_key, 2, init_vector)
    pat_student_ID = "^[1-9]$|^[1-9]\d$|^[1-9]\d\d$"
    pat_pass = "^\d\d\d\d$"
    regex_student_ID = ure.compile(pat_student_ID)
    regex_pass = ure.compile(pat_pass)
    
    while True:
        num = input(“Input your student number: >>>")
        password = input("Input your password: >>>")
        matchobj_num = regex_student_ID.match(num)  # Match student number
        matchobj_pass = regex_pass.match(password)  # Match password
        if not matchobj_num or not matchobj_pass:  # If either number or password fails to match
            if not matchobj_num:  # Shows prompt to enter data in correct format
                print("Please enter a student number from 1 to 999.")
            if not matchobj_pass:
                print("Enter the password using a 4-digit number.")
            continue  # Returns to the beginning of the while statement on line 59
        num = aesobj.encrypt("{:*>16}".format(num))
        password = aesobj.encrypt("{:*>16}".format(password))
        queue_gate_to_auth.append((num, password))
```

### Checking the Program

Now let’s run the program and see how it works by typing in different accounts. If you’re having trouble getting your program to run correctly, try fixing it by comparing it against the example code below!

<pre class="prettyprint">
&gt;&gt;&gt; main()
Input your student number: &gt;&gt;&gt;3456
Input your password: &gt;&gt;&gt;23456
Please enter a student number from 1 to 999.
Enter the password using a 4-digit number.

Input your student number: &gt;&gt;&gt;012
Input your password: &gt;&gt;&gt;234
Please enter a student number from 1 to 999.
Enter the password using a 4-digit number.

Input your student number: &gt;&gt;&gt;2
Input your password: &gt;&gt;&gt;4512
Hi Ogura!
</pre>

##### Example Code 5-2-1

```python=1
import btree
import uhashlib
import ucryptolib
import time
import _thread
import ure
from pystubit.board import display, buzzer, Image
from pyatcrobo2.parts import Servomotor

FILENAME = "school_db"
crypto_key = "python1234python"
init_vector = "0123456789012345"
queue_gate_to_auth = []
queue_auth_to_gate = []
servo = Servomotor("P16")

def register_student(_list):
    try:
        file = open(FILENAME, 'r+b')
    except OSError:
        file = open(FILENAME, 'w+b')
    db = btree.open(file)
    
    for data in _list:
        key = data["student_ID"].to_bytes(1, "big")
        name = (data["name"] + ",").encode("utf-8")
        password = uhashlib.sha256(data["password"]).digest()
        val = name + password
        db[key] = val
    db.flush()
    db.close()
    file.close()


def show_db():
    try:
        file = open(FILENAME, "rb")
    except OSError:
        print("{} doesn't exist.".format(FILENAME))
        return
    db = btree.open(file)
    for key, val in db.items():
        num = int.from_bytes(key.decode(), "big")
        val = val.split(b",")
        name = val[0].decode("utf-8")
        password = val[1] 
        print("{}, {}, {}".format(num, name, password))
    db.close()
    file.close()


def gate_process():
    aesobj = ucryptolib.aes(crypto_key, 2, init_vector)
    pat_student_ID = "^[1-9]$|^[1-9]\d$|^[1-9]\d\d$"
    pat_pass = "^\d\d\d\d$"
    regex_student_ID = ure.compile(pat_student_ID)
    regex_pass = ure.compile(pat_pass)
    
    while True:
        num = input(“Input your student number: >>>")
        password = input("Input your password: >>>")
        matchobj_num = regex_student_ID.match(num)
        matchobj_pass = regex_pass.match(password)
        if not matchobj_num or not matchobj_pass:
            if not matchobj_num:
                print("Please enter a student number from 1 to 999.")
            if not matchobj_pass:
                print("Enter the password using a 4-digit number.")
            continue
        num = aesobj.encrypt("{:*>16}".format(num))
        password = aesobj.encrypt("{:*>16}".format(password))
        queue_gate_to_auth.append((num, password))
        while not len(queue_auth_to_gate):
            time.sleep_ms(100)
            pass
        result = queue_auth_to_gate.pop(0)
        print(result[1])
        if result[0]:
            display.show(Image.YES, delay=0, color=(0, 31, 0))
            buzzer.on("72", duration=200)
            time.sleep_ms(100)
            buzzer.on("68", duration=600)
            servo.set_angle(75)
            time.sleep_ms(5000)
            servo.set_angle(180)
            time.sleep_ms(1000)
            display.clear()
        else:
            display.show(Image.NO, delay=0)
            buzzer.on("48", duration=200)
            time.sleep_ms(100)
            buzzer.on("48", duration=600)
            display.clear()


def auth_process():
    aesobj = ucryptolib.aes(crypto_key, 2, init_vector)
    
    try:
        file = open(FILENAME, 'rb')
    except OSError:
        print("{} doesn't exist.".format(FILENAME))
        return 
    db = btree.open(file)
    
    while True:
        time.sleep_ms(100)
        if len(queue_gate_to_auth):
            data = queue_gate_to_auth.pop(0)
            num = aesobj.decrypt(data[0])
            num = num.strip(b"*")
            num = int(num)
            password = aesobj.decrypt(data[1])
            password = password.strip(b"*")
            # print("Receiving", num, password)
            try:
                db_val = db[num.to_bytes(1, "big")]
            except KeyError:
                queue_auth_to_gate.append((False, "Your number doesn't exist."))
                continue
            db_val = db_val.split(b",")
            db_name = db_val[0].decode("utf-8")
            db_pass = db_val[1]
            if uhashlib.sha256(password).digest() == db_pass:
                queue_auth_to_gate.append((True, "Hi " + db_name + "!"))
            else:
                queue_auth_to_gate.append((False, "Your password is incorrect."))


def main():
    student_list = [
        {"student_ID": 1, "name": "Tanaka", "password": "2312"},
        {"student_ID": 2, "name": "Ogura", "password": "4512"},
        {"student_ID": 3, "name": "Hatanaka", "password": "7543"},
        {"student_ID": 4 , "name": "Morita", "password": "0324"},
    ]
    register_student(student_list)
    servo.set_angle(180)
    _thread.start_new_thread(auth_process, ())
    gate_process()
```

## Conclusion
### A Quick Review
In this lesson we learned about **encrypting**, **decrypting**, and **hashing** data, which are critical techniques in safely transmitting data over a network.

In the free and open world of data that is the Internet, security is an ever-present concern. Aside from the techniques we’ve learned here, security specialists are always hard at work developing new ways to keep the Internet safe for everyone!

In Topic 10 we learned various techniques that we can use to handle data. Out of all of the topics we’ve studied so far, these may have been some of the most difficult. The world of computers, programming, and the Internet is incredibly deep, and the more you learn, the more you’ll understand about the incredibly sophisticated technology that supports it!

And without any delay, we’ll go into further detail about the world of the Internet in Topic 11. We only have two topics left, but hang in there as we take on these new challenges!

### Coming Up Next Time!
In Topic 11 we’re going to connect your Core Unit to the Internet, learning how to control it using data we retrieve on the network. Before you start the lesson make sure to ask your teacher or another adult if there’s a nearby wireless access point you can use!