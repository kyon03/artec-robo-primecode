---
tags: Python_English, Topic 9
---

Python Robotics Course Lesson 36

Topic 9-4
Locking Data
===

Make your multitasking more reliable by securing the data you share between threads!

## In This Lesson...
Digital clocks come included in all kinds of electronics, from car audio systems to rice cookers. Microcomputers also come equipped with functions that track the time and date in real time, and MicroPython has a built-in module for controlling those features. 

In the multithreaded programs we’ve written so far, there’s been a risk of data shared between threads having its references changed in different threads at the same time, causing unexpected errors. In order to prevent these problems, Python can place temporary protections on certain data to stop it from being used by other threads!

In this lesson we'll learn how to use both of these programming features!


:::info

Lesson Introduction

■ Lesson 36 Contents
1. The Real Time Clock 
2. Lock Objects
3. An Average Temperature Recording System
4. Challenge: Adding a Light Level Log


In this lesson you’ll learn how to use the **real time clock** to keep track of the date and time, and how to use **lock objects** to temporarily stop other threads from using data in a multithreaded program.

The real time clock feature is used to keep track of time internally in microcomputers so they can make digital clock displays and record the time when they acquired certain data from their sensors.

Lock objects let you “lock” data in a thread to make it temporarily inaccessible to other threads. This lets you share data between threads without causing errors!

In the second part of the lesson, you’ll use lock objects and the real time clock to program a system that records the average temperature for a set period of time when the recording was taken.

For your final challenge, you need to modify your system program to use the Light Sensor as well!

Now let's start the lesson!

:::

## The Real Time Clock

MicroPython comes with a feature called the **Real Time Clock** which can accurately keep track of the current date and time.

### Setting and Retrieving Date and Time

The real time clock is included in the `machine` module as the class `RTC`. To set a new date or time, you can use a method from this class called `datetime()`.

The `datetime()` method takes a tuple for its argument, which is made up of the information shown below, in order:

```
datetime((Year, Month, Date, Day, Hour, Minute, Second, Millisecond))
```

To set the day of the week in this tuple, you need to use a number instead of a string, like this:

|0|1|2|3|4|5|6|
|:---|:---|:---|:---|:---|:---|:---|:---|
|Monday|Tuesday|Wednesday|Thursday|Friday|Saturday|Sunday|

You can also use the same `datetime()` method to retrieve a date/time. To use the method to retrieve a date/time, don't set anything in the argument.

Let’s test it out by writing a program that sets and date and time and then checks it 10 seconds later.

```python=1
import time
from machine import RTC

rtc = RTC()  # Make an instance of the RTC class

# Set your current date and time here. For our example, we’re setting it to 17:30:30, Wednesday, April 1st, 2020.
rtc.datetime((2020, 4, 1, 2, 17, 30, 30, 0)) 
time.sleep(10)  # The sleep() method\’s argument takes units of time in seconds
print(rtc.datetime()) # Leave out the argument to retrieve the current date/time
```

#####  Program Results
<pre class="prettyprint">
(2020, 4, 1, 2, 17, 30, 40, 380)
</pre>

Python can actually determine the day of the week on its own when you give it the date, so setting the day part of the tuple to 0 will give us the same result!

###### Changed Code (Line 6)
```python=1
import time
from machine import RTC

rtc = RTC()

rtc.datetime((2020, 4, 1, 0, 17, 30, 30, 0))   # Change the 4th item from 2 to 0
time.sleep(10)
print(rtc.datetime())
```

#####  Program Results
<pre class="prettyprint">
(2020, 4, 1, 2, 17, 30, 40, 369)
</pre>

### Retrieving Date and Time with the Time Module

You can also find your current date/time using the `time` module, which has a method called `localtime()`. The date and time the `localtime()` method finds reference the real time clock, so if your real time clock isn’t set up correctly this method won’t find the right time/date!

Try running the program below to check its results **after pressing the Core Unit’s Reset button** to reset the real time clock to its default state.

```python=1
import time
from machine import RTC

rtc = RTC()

print("before:", time.localtime())  # Before setting the real time clock
rtc.datetime((2020, 4, 1, 2, 17, 30, 30, 0))
print("after:", time.localtime())  # After setting the real time clock
```

#####  Program Results
<pre class="prettyprint">
before: (2000, 1, 1, 0, 0, 2, 5, 1)
after: (2020, 4, 1, 17, 30, 30, 2, 92)
</pre>

###### ★ The real time clock’s default state on startup is set to 00:00:00, January 1st, 2000.

These results make it clear that the `localtime()` method references the real time clock. However, the return values from the `localtime()` method aren’t in exactly the same format as the ones from the `RTC` class’s `datetime()` method. Look at the format for `localtime()`’s return values and take note of the differences!

#####  `localtime()`’s Return Values
```
(Year, Month, Date, Hour, Minute, Second, Day, Day of the Current Year Numbered 1-366)
```

## Protecting Data Shared Between Threads

In the last lesson we wrote a program that used stacks and queues to pass data back and forth between multiple threads, and changed references to the same data from different threads at the same time. This didn’t cause any major problems for the program we were writing there, but depending on the program, sharing data between threads can cause unforseen errors!

### Problems with Shared Data

Let’s make an email mailing list as an example to see what kind of problems can happen when you’re sharing data between threads.

#### ■ Changes Don’t Get Made

Our hypothetical mailing list is set up to automatically send emails to members registered in the list after a set mailing time has passed.

In the example code below, we’re simulating a situation where a person using this system sets up a list before noticing that they've forgotten to add members. They then try to update the list to fix the mistake.

##### Example Code 3-1-1

```python=1
import time
import _thread 
from machine import Timer

mytimer = Timer(1)  # Use a timer interrupt to send out emails when the set time passes

# List of members and their email addresses
mailing_list = {
    "Daisuke": "daisuke@python.com",
    "Ichiro": "ichiro@python.com",
    "Taichi": "taichi@python.com",
}

# Send emails to members on the list, in order
def send_mail():
    for name, address in mailing_list.items():
        time.sleep_ms(100)
        print("sending a mail to {}: Hello, {}.".format(address, name))
        
# At the time set in the time, this process starts a new thread that runs the send mail function
def wake_up(t):
    _thread.start_new_thread(send_mail, ())
    t.deinit()  # This is a one-time interupt, so the timer gets released after use

# This part simulates a user updating the list
def update_mailing_list():
    global mailing_list  # The global variable needs to be declared in order to change its contents

    # The user adds new members at a speed of one per second
    time.sleep_ms(1000)
    mailing_list["Kenta"] = "kenta@python.com"
    time.sleep_ms(1000)
    mailing_list["Kaito"] = "kaito@python.com"
    time.sleep_ms(1000)
    mailing_list["Taro"] = "taro@python.com"

# Set up another scheduled email to the mailing list with a timer interrupt
mytimer.init(mode=Timer.ONE_SHOT, period=5000, callback=wake_up)
time.sleep_ms(3000) # Shortly after setting up the next mailing time,
update_mailing_list()  # the user updates the list
```

What our hypothetical user is trying to do here to send emails to all six members in their updated list. However, if you actually run this example code, you’ll see that’s not what happens!

#####  Program Results
<pre class="prettyprint">
sending a mail to kenta@python.com: Hello, Kenta.
sending a mail to daisuke@python.com: Hello, Daisuke.
sending a mail to taichi@python.com: Hello, Taichi.
sending a mail to kaito@python.com: Hello, Kaito.
sending a mail to ichiro@python.com: Hello, Ichiro.
</pre>

The program only sends emails to five of the six members on the list! Let’s take a look at the timeline for this code’s processes to see why this happens...

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-4/9-4_1_E.png"/>

As you can see, the program starts sending emails while the user is in the middle of updating the list, so it doesn’t end up sending an email to `Taro`, the last member added.

When you try to update data in a multithreaded program like this while another thread is using that data, you may not get the results you want! In order to prevent problems like this, you can use Python to add temporary protections to certain data to stop it from being used by other threads.

### Lock Objects

One tool Python provides for avoiding problems like those above is a kind of object called a **lock object**, which can “lock” data to temporarily stop other threads from accessing it! You can make lock objects by using the function `allocate_lock()` from the `_thread` module.



```
lock_obj = _thread.allocate_lock()
```

Lock objects are a special kind of object with two states: **locked** and **unlocked**. Running one of the methods below from a thread lets you change a lock object’s state.

##### Lock Object Methods

|Method|What It Does|
|:---|:---|
|`acquire(waitflag=1, timeout=-1)`|Requests a lock. If the data has already been locked from another thread, the method waits for it to be unlocked, according to the values set in its arguments. For example, if `waitflag` is set to 0, the method will only request a lock if the data is immediately accessible. If it isn’t, the program will move on to the next step without waiting for it to unlock. If `waitflag` is set to 1 instead, the method will wait for the number of seconds set in the `timeout` argument in case the data unlocks in that time. If it doesn’t unlock in that time, the program moves on to the next step. If you set `timeout` to -1, the method will wait as long as it takes for the data to unlock. |
|`release()`|Releases a data lock. |
|`locked()`|Checks whether data is currently locked and returns a boolean (`True` or `False`) as its return value. |

Lock objects really work just like public locker keys! When you use the `acquire()` method and the locker has its key, you can take that key and lock the locker with it. If the key isn’t there and the locker is locked, you can either wait for the person using it to come back and open the locker (using the `release()` method to unlock it), or you can give up on this locker and go look for another one.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-4/9-4_2_E.png"/>

The example code below shows this process of locking and unlocking a lock object.

```python=1
import time
import _thread

lock_obj = _thread.allocate_lock()  # Make a lock object

lock_obj.acquire()  # Request a lock
print("lock_obj is locked.")  
time.sleep_ms(1000) # The data stays locked for this time!
lock_obj.release()  # Release the lock
print("lock_obj is released.")  
```

Lock objects also work with `with` statements, so you can also write this example code like this:

```python=1
import time
import _thread

lock_obj = _thread.allocate_lock()

with lock_obj:  # Request a lock. If the data is already locked, wait for it to unlock
    print("lock_obj is locked.")
    time.sleep_ms(1000) # The data stays locked for this time!
print("lock_obj is released.")  # Automatically release the lock when the with process ends
```

Now let’s rewrite that mailing list program we had problems with before, but using a lock object this time!

##### Example Code 3-1-1 With Lock Objects
###### New and Changed Code (Line 6, Lines 16-19, Lines 30-36)

```python=1
import time
import _thread
from machine import Timer

mytimer = Timer(1)
lock_obj = _thread.allocate_lock()  # Make a lock object

mailing_list = {
    "Daisuke": "daisuke@python.com",
    "Ichiro": "ichiro@python.com",
    "Taichi": "taichi@python.com",
}


def send_mail():
    with lock_obj:  # Wait for the lock to be released before accessing the list data
        for name, address in mailing_list.items():
            time.sleep_ms(100)
            print("sending a mail to {}: Hello, {}.".format(address, name))


def wake_up(t):
    _thread.start_new_thread(send_mail, ())
    t.deinit()


def update_mailing_list():
    global mailing_list
    
    with lock_obj:  # Lock the list before updating it
        time.sleep_ms(1000)
        mailing_list["Kenta"] = "kenta@python.com"
        time.sleep_ms(1000)
        mailing_list["Kaito"] = "kaito@python.com"
        time.sleep_ms(1000)
        mailing_list["Taro"] = "taro@python.com"
        # Release the lock

mytimer.init(mode=Timer.ONE_SHOT, period=5000, callback=wake_up)
time.sleep_ms(3000)
update_mailing_list()
```

Now when you run this program, no emails will be sent out to the list members until the `update_mailing_list()` function has finished updating the list.

#####  Program Results

<pre class="prettyprint">
&gt;&gt;&gt; sending a mail to kenta@python.com: Hello, Kenta.
sending a mail to daisuke@python.com: Hello, Daisuke.
sending a mail to taichi@python.com: Hello, Taichi.
sending a mail to kaito@python.com: Hello, Kaito.
sending a mail to taro@python.com: Hello, Taro.
sending a mail to ichiro@python.com: Hello, Ichiro.
</pre>

Using lock objects to secure your data in multithreaded programs like this lets you prevent undesired program results like we had before!. Be careful though, there are other problems you can actually cause by using lock objects!

### The Deadlock Problem

Lock objects are extremely useful, but if you set up a lock and forget to unlock it later, or set up a bunch of locks that get in each other’s way, that can cause the whole rest of your program to come to a stop with a kind of error called a **deadlock**.

### What is a Deadlock?

First let’s look at an example program. In this program, we’re using two lock objects between two functions called `work1()` and `work2()`, and the objects repeatedly lock and unlock after set amounts of time pass.

##### Example Code 3-2-1

```python=1
import _thread
import time

# Make two lock objects
lock_obj1 = _thread.allocate_lock()
lock_obj2 = _thread.allocate_lock()

# Lock lock_obj1 every second, and also lock lock_obj2 while doing so
def work1():
    while True:
        time.sleep_ms(1000)
        with lock_obj1:
            print("work1: lock_obj1 is locked.")
            with lock_obj2:
                print("work1: lock_obj2 is locked.")
            print("work1: lock_obj2 is released.")
        print("work1: lock_obj1 is released.")
     
# Lock lock_obj2 every 1.5 seconds, and also lock lock_obj1 while doing so
def work2():
    while True:
        time.sleep_ms(1500)
        with lock_obj2:
            print("work2: lock_obj2 is locked.")
            with lock_obj1:
                print("work2: lock_obj1 is locked.")
            print("work2: lock_obj1 is released.")
        print("work2: lock_obj2 is released.")
       
# Start threads to run the two functions
_thread.start_new_thread(work1, ())
_thread.start_new_thread(work2, ())
```

When you run this program, it will work fine at first, but after a while, it will print the results shown below and then come to a stop.

<pre class="prettyprint">
.
.
.
.
.
work1: lock_obj1 is locked.
work2: lock_obj2 is locked.
</pre>

This happens because `lock_obj2` has to be locked in order to unlock  `lock_obj1` in `work1`, but in `work2`, `lock_obj1` has to be unlocked in order to unlock `lock_obj2`. With this being the case, if both `lock_obj1` and `lock_obj2` are locked at the same time, the program becomes unable to unlock either of them and can’t continue its processes at all.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-4/9-4_3_E.png"/>

This kind of situation is called a **deadlock**, and you need to be careful to avoid things like this when you use lock objects in a program!

## Programming an Average Temperature Recording System

Now let’s try writing a program using these concepts so you can get a clearer idea of how they work!

The program we’re going to make uses the Temperature Sensor to find the temperature in the area at regular intervals and stores that data in a queue. Once the queue has a certain amount of stored data in it, the program collects the data, finds the average temperature, and records the current time. After that, when the user presses the A Button, the system will make the most recent data (average temperature and time) scroll across the LED display. The diagram below gives an overview of the whole program for this system.

##### Full System Overview

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-4/9-4_4_E.png"/>

This runs three functions in parallel at the same time, and these functions share certain data between them.

You’ll need to use everything you’ve learned about **timer interrupts**, **multithreading**, **queues**, the **real time clock**, and **lock objects** to build this system!

### Constructing the Program

Let’s define the functions from the program above like this:

|Function|What does it do?|
|:---|:---|
|`logging_temperature()`|Finds the temperature every 500 milliseconds.|
|`calc_average()`|Gathers the temperature measurements for a 30 second period and calculates the average temperature.|
|`main()`|The main program loop.|

###### ★ The term “logging”, as used here, means writing down information about things that happen in chronological order! 



#### ■ The Temperature Detecting Function

We’ll start by making a program that retrieves the current temperature from the Temperature Sensor every 500 milliseconds, and putting it inside the `logging_temperature()` function.

```python=1
import time
from pystubit.board import temperature

def logging_temperature():
    while True:  # Find the current temperature every 500 ms
        time.sleep_ms(500)
        _temp = temperature.get_celsius()
```

We’ll store the temperatures this program finds in a queue. The data in this queue is shared with the `calc_average()` function, so let’s use a lock object to keep it secure whenever it’s being updated. That’s all for this function!

###### New Code (Line 2, Lines 5-6, Lines 13-14)
```python=1
import time
import _thread  # Import the multithreading module
from pystubit.board import temperature

queue = []  # A queue for storing data from the Temperature Sensor
lock_obj = _thread.allocate_lock()  # Make a lock object


def logging_temperature():
    while True:  # Find the current temperature every 500 ms
        time.sleep_ms(500)
        _temp = temperature.get_celsius()
        with lock_obj:  # Request a lock and wait for the data to be available
            queue.append(_temp)  # Store the retrieved data in the queue
```



#### ■ The 30 Second Average Temperature Function

This function collects temperature data from the queue every 30 seconds and then finds the average of that data. It also uses the real time clock to find the time when it takes an average, then records its recent temperature data in a global variable it shares with the `main()` function. You can write the code for this part of the program like this:

###### New Code (Line 3, Lines 8-9, Lines 20-30)
```python=1
import time
import _thread
from machine import RTC
from pystubit.board import temperature

queue = []
lock_obj = _thread.allocate_lock()
latest_data = None  # Global variable for recording the most recent data
rtc = RTC()  # Make a real time clock object. The date/time will be set in the main() function.


def logging_temperature():
    while True:
        time.sleep_ms(500)
        _temp = temperature.get_celsius()
        with lock_obj:
            queue.append(_temp)
            

def calc_average():
    global latest_data  # Declare the global variable so you can change its contents
    
    num = len(queue)  # Find the current length of the queue
    with lock_obj:  # Request a lock before changing the contents of the queue
        _sum = 0  # A variable for collecting data
        for _ in range(num):  # Repeat for exactly the current length of the queue
            _sum += queue.pop(0)  # Remove the first time in the queue and retrieve its value
        average = round(_sum/num, 2)  # Calculate the average from the sum of your collected data, rounded to two decimal places
    latest_data = (rtc.datetime(), average)  # Store the most recent temperature data in a tuple and record it
    print(latest_data)  # Show your data in the terminal for testing purposes
```

On line 28 of this code, we’re using the `round()` function to round the average temperature we get from `_sum/num` to a two decimal place number. Setting the second argument in the `round()` function lets you round numbers to a specific number of decimal places, not just into full integers!

Next let’s use a timer interrupt to make a thread that runs the `calc_average()` function every 30 seconds. Let’s make a new function for the timer interrupt to call and make this thread start in that function.

###### New and Changed Code (Line 3, Line 10, Lines 34-35)
```python=1
import time
import _thread
from machine import RTC, Timer  # Import the classes for timer interrupts
from pystubit.board import temperature

queue = []
lock_obj = _thread.allocate_lock()
latest_data = None
rtc = RTC()
mytimer = Timer(1)  # Make a timer object
```
```python=21
def calc_average():
    global latest_data
    
    num = len(queue)
    with lock_obj:
        _sum = 0
        for _ in range(num):
            _sum += queue.pop(0)
        average = round(_sum/num, 2)
    latest_data = (rtc.datetime(), average)
    print(latest_data)


def wake_up(t): # This is the function your timer interrupt calls. The t argument will be set to the timer object the function gets called to.
    _thread.start_new_thread(calc_average, ())  # Start and run a thread
```

#### ■ The Main Loop Function

Finally, we’ll make the loop function that shows the latest temperature data on the LED display when you press the A Button. At the start of this function we’ll find and set up the time/date for the real time clock, start threads for the functions we’ve made so far, and set up the timer interrupt.

###### New and Changed Code (Line 4, Lines 43-57)
```python=1
import time
import _thread
from machine import RTC, Timer
from pystubit.board import temperature, display, button_a  # Import LED display and Button A objects
```
```python=34
def wake_up(t):
    _thread.start_new_thread(calc_average, ())


def main(_datetime):  # Get the current date/time as an argument
    rtc.datetime(_datetime)  # Set the real time clock to the current date/time
    _thread.start_new_thread(logging_temperature, ())  # Start and run a thread
    mytimer.init(mode=Timer.PERIODIC, period=30000, callback=wake_up)  # Set the timer interrupt
    
    while True:  # Scroll the latest temperature data when Button A is pressed
        if button_a.is_pressed():
            if latest_data:  # If you have the latest data (*not available in the first 30 seconds after starting the program)
                hour = latest_data[0][4]  # The hour (The 4th item in the tuple from the rtc.datetime() method)
                minute = latest_data[0][5]  # The minute (The 5th item)
                minute = latest_data[0][6]  # The second (The 6th item)
                _time = "{}:{}:{}".format(hour, minute, second)  # Display the time in the h:m:s format
                _temp = str(latest_data[1])  # The most recent average temperature
                display.scroll(_time, delay=50)  # Scroll the time first
                display.scroll(_temp, delay=50)  # Scroll the average temperature after the time
            else:  # Show a message in the terminal if data isn’t available yet
                print("no data")
                while button_a.is_pressed():  # Wait until Button A has been released
                    pass
        time.sleep_ms(100)  # Check whether the A Button has been pressed only once every 100 milliseconds so you don’t strain the CPU
```

#### ■ Testing the System

Now your program is finished! Start the program and then call the `main()` function in the terminal with the current date/time set as its argument. 30 seconds later, your new temperature data will be shown in the terminal, and you can press the A Button to make it scroll across the LED display too!

###### ★ Remember to put the date/time in a tuple format to set it in the main function’s argument!
###### ★ Line 4 below shows newly recorded data. Press button A after this line appears.
<pre class="prettyprint">
MicroPython v1.10-229-gd579623-dirty on 2019-12-04; ESP32 module with ESP32
Type "help()" for more information.
&gt;&gt;&gt; main((2020, 4, 1, 0, 12, 0, 0, 0))
((2020, 4, 1, 2, 12, 0, 30, 3354), 31.66)
</pre>

If errors occur in your program and you can’t figure out how to fix them, compare your code to the example below to check for mistakes!

##### Example Code 4-1-1
```python=1
import time
import _thread
from machine import RTC, Timer
from pystubit.board import temperature, display, button_a

queue = []
lock_obj = _thread.allocate_lock()
latest_data = None
rtc = RTC()
mytimer = Timer(1)


def logging_temperature():
    while True:
        time.sleep_ms(500)
        _temp = temperature.get_celsius()
        with lock_obj:
            queue.append(_temp)


def calc_average():
    global latest_data
    
    num = len(queue)
    with lock_obj:
        _sum = 0
        for _ in range(num):
            _sum += queue.pop(0)
        average = round(_sum/num, 2)
    latest_data = (rtc.datetime(), average)
    print(latest_data)


def wake_up(t):
    _thread.start_new_thread(calc_average, ())


def main(_datetime):
    rtc.datetime(_datetime)
    _thread.start_new_thread(logging_temperature, ())
    mytimer.init(mode=Timer.PERIODIC, period=30000, callback=wake_up)
    
    while True:
        if button_a.is_pressed():
            if latest_data:
                hour = latest_data[0][4]
                minute = latest_data[0][5]
                second = latest_data[0][6]
                _time = "{}:{}:{}".format(hour, minute, second)
                _temp = str(latest_data[1])
                display.scroll(_time, delay=50)
                display.scroll(_temp, delay=50)
            else:
                print("no data")
                while button_a.is_pressed():
                    pass
        time.sleep_ms(100)
```

## Challenge: Adding a Light Level Log

Finish this lesson by adding a new feature to your program from the last chapter that records average values from the Light Sensor! Then you can make that information scroll across the LED display when you press the B Button.

##### New Functions and Shared Data

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-4/9-4_5_E.png"/>

As shown in the diagram, the structure of the functions and shared data your new feature needs is the same as the system for the Temperature Sensor. This means you can program the new feature by just copying your old program and changing the variable/functions names and a few key points!

### Example Program

Copy the functions and variables from your Temperature Sensor program, then change the names as shown below.

|Function/Variable for Temperature|Function/Variable for Light|
|:---|:---|
|`queue`|`queue_light`|
|`lock_obj`|`lock_obj_light`|
|`latest_data`|`latest_data_light`|
|`logging_temperature()`|`logging_lightsensor()`|
|`calc_average()`|`calc_average_light()`|

Now change parts of the code in the functions so they use the Light Sensor.

##### New and Changed Code (Line 5, Line 8, Line 10, Line 12, Lines 25-30, Lines 46-56)
```python=1
import time
import _thread
from machine import RTC, Timer
from pystubit.board import temperature, display, button_a, button_b
from pystubit.board import lightsensor  # Import a Light Sensor object 

queue = []
queue_light = []  # For the Light Sensor
lock_obj = _thread.allocate_lock()
lock_obj_light = _thread.allocate_lock()  # For the Light Sensor
latest_data = None
latest_data_light = None  # For the Light Sensor
rtc = RTC()
mytimer = Timer(1)


def logging_temperature():
    while True:
        time.sleep_ms(500)
        _temp = temperature.get_celsius()
        with lock_obj:
            queue.append(_temp)

# Make a copy and edit it to work with the Light Light Sensor
def logging_lightsensor():
    while True:
        time.sleep_ms(500)
        _light = lightsensor.get_value()  # Retrieve values from the Light Sensor
        with lock_obj_light:  # The lock object for the Light Sensor
            queue_light.append(_light)  # The queue for the Light Sensor


def calc_average():
    global latest_data
    
    num = len(queue)
    with lock_obj:
        _sum = 0
        for _ in range(num):
            _sum += queue.pop(0)
        average = round(_sum/num, 2)
    latest_data = (rtc.datetime(), average)
    print(latest_data)
    
# Make a copy and edit it to work with the Light Light Sensor
def calc_average_light():
    global latest_data_light  # Global variable for the Light Sensor
    
    num = len(queue_light)  # Find the length of the Light Sensor data queue
    with lock_obj_light:  # The lock object for the Light Sensor
        _sum = 0
        for _ in range(num):
            _sum += queue_light.pop(0)  # Remove the first item in the Light Sensor queue and retrieve its value
        average = round(_sum/num)  # Light Sensor values are always integers from 0 to 4095, so round the average to an integer as well
    latest_data_light = (rtc.datetime(), average)  # Record the most recent data
    print(latest_data_light)  # Show your data in the terminal for testing purposes
```

Now that you’ve copied the functions and variables, you can add a new thread, a timer interrupt, and code that displays the latest Light Sensor data when you press Button B.

##### New Code (Line 62, Line 69, Lines 87-100)
```python=59
def wake_up(t):
    _thread.start_new_thread(calc_average, ())
    # Start a thread that calculates the average Light Sensor value
    _thread.start_new_thread(calc_average_light, ())


def main(_datetime):
    rtc.datetime(_datetime)
    _thread.start_new_thread(logging_temperature, ())
    # Start a thread that records Light Sensor values
    _thread.start_new_thread(logging_lightsensor, ())
    mytimer.init(mode=Timer.PERIODIC, period=30000, callback=wake_up)
    
    while True:
        if button_a.is_pressed():
            if latest_data:
                hour = latest_data[0][4]
                minute = latest_data[0][5]
                second = latest_data[0][6]
                _time = "{}:{}:{}".format(hour, minute, second)
                _temp = str(latest_data[1])
                display.scroll(_time, delay=50)
                display.scroll(_temp, delay=50)
            else:
                print("no data")
                while button_a.is_pressed():
                    pass
        # Scroll the latest light data when Button B is pressed
        if button_b.is_pressed():
            if latest_data_light:
                hour = latest_data_light[0][4]
                minute = latest_data_light[0][5]
                second = latest_data_light[0][6]
                _time = "{}:{}:{}".format(hour, minute, second)
                _light = str(latest_data_light[1])
                # In this example we’ve set the scroll color to white to distinguish it from the temperature scroll
                display.scroll(_time, delay=50, color=(31, 31, 31))
                display.scroll(_light, delay=50, color=(31, 31, 31))
            else:
                print("no data")
                while button_b.is_pressed():
                    pass
        time.sleep_ms(100)
```

Now your program is finished! Run the program, then call the `main()` function in the terminal so you can test out the system! If errors occur in your program and you can’t figure out how to fix them, compare your code to the example below to check for mistakes!

##### Example Code 5-1-1
```python=1
import time
import _thread
from machine import RTC, Timer
from pystubit.board import temperature, display, button_a, button_b
from pystubit.board import lightsensor

queue = []
queue_light = []
lock_obj = _thread.allocate_lock()
lock_obj_light = _thread.allocate_lock()
latest_data = None
latest_data_light = None
rtc = RTC()
mytimer = Timer(1)


def logging_temperature():
    while True:
        time.sleep_ms(500)
        _temp = temperature.get_celsius()
        with lock_obj:
            queue.append(_temp)


def logging_lightsensor():
    while True:
        time.sleep_ms(500)
        _light = lightsensor.get_value()
        with lock_obj_light:
            que_light.append(_light)


def calc_average():
    global latest_data
    
    num = len(queue)
    with lock_obj:
        _sum = 0
        for _ in range(num):
            _sum += queue.pop(0)
        average = round(_sum/num, 2)
    latest_data = (rtc.datetime(), average)
    print(latest_data)
    

def calc_average_light():
    global latest_data_light
    
    num = len(queue_light)
    with lock_obj_light:
        _sum = 0
        for _ in range(num):
            _sum += queue_light.pop(0)
        average = round(_sum/num)
    latest_data_light = (rtc.datetime(), average)
    print(latest_data_light)


def wake_up(t):
    _thread.start_new_thread(calc_average, ())
    _thread.start_new_thread(calc_average_light, ())


def main(_datetime):
    rtc.datetime(_datetime)
    _thread.start_new_thread(logging_temperature, ())
    _thread.start_new_thread(logging_lightsensor, ())
    mytimer.init(mode=Timer.PERIODIC, period=30000, callback=wake_up)
    
    while True:
        if button_a.is_pressed():
            if latest_data:
                hour = latest_data[0][4]
                minute = latest_data[0][5]
                second = latest_data[0][6]
                _time = "{}:{}:{}".format(hour, minute, second)
                _temp = str(latest_data[1])
                display.scroll(_time, delay=50)
                display.scroll(_temp, delay=50)
            else:
                print("no data")
                while button_a.is_pressed():
                    pass
        if button_b.is_pressed():
            if latest_data_light:
                hour = latest_data_light[0][4]
                minute = latest_data_light[0][5]
                second = latest_data_light[0][6]
                _time = "{}:{}:{}".format(hour, minute, second)
                _light = str(latest_data_light[1])
                display.scroll(_time, delay=50, color=(31, 31, 31))
                display.scroll(_light, delay=50, color=(31, 31, 31))
            else:
                print("no data")
                while button_b.is_pressed():
                    pass
        time.sleep_ms(100)
```

## Conclusion
### A Quick Review
In this lesson we introduced how to use the **real time clock** to keep track of the date and time, and how to use **lock objects** to temporarily stop other threads from using data in a multithreaded program.  

Then, to conclude Topic 9, we wrote an actual program using **timer interrupts**, **multithreading**, **queues**, the **real time clock**, and **lock objects** all together!

Some of the features you’ve learned about in Topic 9 aren’t usable with certain hardware. Remember this in the future whenever you’re using Python with a microcomputer or other hardware with limited features so you remember to check what features your hardware comes with and what data its developers are providing.

###### ★ For the ESP32 microcomputer in your Core Unit, you can find out more about its features in the official MicroPython documentation at the URL below.

[Official MicroPython Documentation for ESP32](https://docs.micropython.org/en/latest/esp32/quickref.html)

### Coming Up Next Time!

In the next lesson we’ll be starting Topic 10! In Topic 10 we’ll be taking a closer look at how computers process data. For Topic 10-1, we’ll start out by learning about different systems computers use to express numbers, like **binary**, **octal**, and **hexadecimal**.