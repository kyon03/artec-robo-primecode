---
tags: Python_English, Topic 9
---

Python Robotics Course Lesson 33

Topic 9-1
Interrupts and Parallel Processes
===

Learn how to use interrupts!

## In Topic 9...

**Parallel processing** is a term for when a computer runs multiple programs at once, or when a single program runs through multiple sequences of commands at once. Parallel processing is what lets us have multiple applications open and running on one computer at the same time! You’re actually using this feature even while you’re taking these lessons, since you’re using both your web browser and your program editor at once! In Topic 9 we’ll learn about techniques like **interrupts** and **multithreading** which make parallel processing like this possible! 

## In This Lesson...

Let’s learn about a new programming technique called an **interrupt**. While your program is processing a series of commands, you can use an interrupt to make the program put aside what it’s doing to run another, higher priority command first! To understand how interrupts work, first you’ll need to understand how computers themselves work, so that’s where we’ll start our explanations this time. It’ll take a while, but stick with us! Once you’ve understood the concepts in the lesson, you can use interrupts to program a game for your final challenge.



:::info

Lesson Introduction

■ Lesson 33 Contents
1. How Computers Work 
2. Unique Features of MicroPython 
3. Using Interrupts
4. Timer and GPIO Interrupts 
5. Challenge: Game Programming with Interrupts


In Topic 9, we’ll be learning about “parallel processing,” the technique computers use to run multiple sets of commands at the same time. In this lesson we’ll talk about a parallel processing technique called an **interrupt**.

We’ll start by discussing some information about how computers work and the special features of MicroPython, which you’ll need to know for the lessons in Topic 9.

Interrupts are a technique you can use to make a program put aside the sequence of commands it’s currently running to run another, higher priority command first! We’ll give some examples of situations where interrupts are necessary to help explain the concept.

In the second part of the lesson you can start writing programs using interrupts yourself! There are a lot of kinds of interrupts available to you, but in this lesson we’ll use timer interrupts and external hardware interrupts specifically. We’ll also show you how some of the previous lessons’ programs could be made simpler by using interrupts!

For the final challenge we’ll take a hands on approach and program a simple game using interrupts.

This lesson is a harder one so it may take extra time to complete! Just stick with it and you’ll have mastered an important skill for advanced programming soon enough! 

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!

:::

## How Computers Work

To understand how interrupts work, you need to understand not just the Python syntax, but also some things about how computer **hardware** works. We’ll go over these things in this chapter, focusing on miniature computers like your Core Unit primarily.

### Microcomputers for Robots

The small-scale computers used to control robots and home electronics are called **microcomputers**. Unlike full-scale computers which you can install a lot of different software on for many different purposes, microcomputers are limited to specific uses. Microcomputers are designed to be small and energy-efficient rather than powerful.

A microcomputer chip contains various hardware pieces, the core of which is the **CPU (Central Processing Unit)** which processes data and controls any peripherals* attached to the microcomputer. The other hardware elements include the computer’s **memory**, where programs and data are stored, and **GPIOs (General Purpose Inputs/Outputs)**, which send and receive electric signals between the microcomputer and other devices.

###### ★ “Peripherals” include parts like motors and sensors, but also common computer parts like keyboards, monitors, hard disk drives (HDD), and SD cards.

Your Core Unit contains a microcomputer called the **ESP32**, which looks like this:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-1/9-1_1_E.png"/>

The ESP32 microcomputer has the following component parts:

##### ESP32 Hardware Components

|Component|What is it?|
|:---|:---|
|CPU|A processor performs calculation processes, kind of like the computer’s brain.|
|Internal Memory (Primary Storage)|Storage space which stores data and programs.|
|Timer|An electric circuit that can measure time and and send signals when set amounts of time have passed.|
|Watchdog Timer|A special timer that checks at regular intervals to see if your computer is working properly and resets it if problems are detected.|
|Clock Generator|A circuit that creates a **clock signal**. Clock signals are created by certain wave frequencies, and computers use the “beat” of the clock signal coordinate the timing of all of the computer’s functions. |
|ADC|An **A**nalog-to-**D**igital **C**onverter. This circuit converts analog signals received from sources like sensors into digital signals the computer can use.|
|DAC|A **D**igital-to-**A**nalog **C**onverter. This circuit converts digital signals from the computer into analog signals to control actuators like motors.|
|GPIOs|Pins/ports that receive signals from external components (like sensors) and send signals to control actuators (like motors).|
|Communications Interface|Parts that let the board connect to and communicate with other devices, for example sending signals via USB or reading/writing from an SD card.|

MicroPython actually includes a module that lets you directly control all of these components! We’ll try it out for ourselves in the next chapter.

## Unique Features of MicroPython

Python’s runtime environment is designed based on the assumption that you’ll be running it on a regular computer with free memory and good processing speed. What we’ve been using in these lessons is actually the **MicroPython** runtime environment, a scaled-down version of Python designed for microcomputers. MicroPython is more than just a smaller, lighter version Python, though! It also has unique features that let you control microcomputer components like the ones introduced last chapter. Most of these features can be found in the `machine` module.

###### ★ MicroPython has a number of other unique features not found in regular Python aside from the `machine` module, including the `network` module (for network communications) and the `bluetooth` module (for Bluetooth communications).

### The `machine` Module

The `machine` module has lots of functions and classes which you can use to control the hardware components we talked about earlier. We won’t explain all of these classes and functions in this lesson! If you want to learn about more of them, feel free to check out the official documentation for MicroPython at the link below.

###### ★ The official documentation contains a lot of technical language, so you may need some extra background knowledge of computer hardware and software to understand it. You don’t need to know all of this to continue with this lesson, so feel free to skip it if it feels too difficult!

[Official MicroPython Documentation](https://docs.micropython.org/en/latest/)

##### Some Functions from the `machine` Module

|Function|What does it do?|
|:---|:---|
|`reset()`|Resets the microcontroller to its default state, just like the Reset Button. |
|`disable_irq()`|Prevents the program from accepting **I**nterrupt **R**e**q**uests. |
|`enable_irq()`|Allows the program to accept interrupt requests. |
|`freq()`|Finds the frequency of the CPU’s clock signal. |

###### ★ We’ll talk about **interrupts** in detail in the next chapter!

##### Some Classes from the machine Module

|Class|What does it do?|
|:---|:---|:---|
|`Pin`|Controls input/output pins. |
|`ADC`|Converts analog signals received from sources like sensors into digital signals the computer can understand (the **A**nalog-to-**D**igital **C**onverter class).|
|`RTC`|Provides **R**eal **T**ime **C**lock features that keep track of the current time. |
|`Timer`|Uses the timer to run certain processes at regular intervals (every second, every 10 seconds, etc.). |


The interrupt process we’ll be learning about in this lesson will use the `Pin` and `Timer` classes. In the next chapter we’ll use these classes to start writing interrupt programs for real!

## Using Interrupts

An **interrupt** process is when you make a new process start instead of another process that was already running. The original process will pause temporarily while this is happening, the start up again when the interrupt process is finished!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-1/9-1_2_E.png"/>

### Why Use Interrupts?

Think about a restaurant kitchen where you have dishes you need to wash, dry, and put away. Having a person or robot dedicated to doing each part of the job lets work on all parts proceed at the same time. Dividing up tasks this way lets the whole job get done faster!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-1/9-1_3_E.png"/>

Computers with high processing power have a lot of processors, letting the processors split tasks between them just like in our kitchen example, so they can perform multiple tasks in parallel. Computers with lower processing power, like microcomputers, can’t do this! A microcomputer would be like a kitchen with only one person (or machine) available to wash the dishes, so they can’t start drying dishes until they finish washing them, or put them away until they finish drying, making the whole task take longer.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-1/9-1_4_E.png"/>

If you’re the restaurant worker in this example, what do you do if someone needs a plate right away while you’re in the middle of washing? You’d probably stop washing dishes for a minute to dry one and pass it on to the person who needs it, right?

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-1/9-1_5_E.png"/>

This is exactly what interrupts do for microcomputer programs! Running multiple processes in parallel on a computer and switching between them is called **multitasking**. Interrupts are widely used technique for letting microcomputers multitask!

###### ★ Interrupts are used on regular computers too, not just microcomputers!

### Different Kinds of Interrupts

Computer interrupts can be broadly divided into two main categories: **software interrupts** and **hardware interrupts**.

* **Software Interrupts**
These are interrupts that are requested by the CPU itself. The CPU might request a software interrupt if an error occurs while running a program, for example.

* **Hardware Interrupts**
These are interrupts that are requested by devices external to the CPU. For example, an interrupt could be requested when the computer receives digital input from a keyboard or other peripheral device, and then the GPIO specified for interrupt requests would adjust its voltage to signal the request.

Usually when people talk about interrupts, they mean hardware interrupts. In the next part of the lesson we’ll look at some actual examples of hardware interrupts, which should make this easier to understand!

## Hardware Interrupts in Action

Now let’s start writing an actual program so we can see for ourselves how hardware interrupts work. We’ll be using two specific kinds of hardware interrupts: **timer interrupts** and **GPIO interrupts**.

### Timer Interrupts

A microcomputer’s **timer** can measure time and send signals when certain amounts of time have passed. If you use the timer to request an interrupt after a set amount of time has passed or to request one regularly at set intervals, that’s a **timer interrupt.**

In MicroPython, you can use the `Timer` class from the `machine` module to set up timer interrupts. Let’s try programming a timer interrupt that makes the Buzzer play the note C4 every second.

#### ■ Programming with Timer Interrupts

To set up a timer interrupt, make an instance of the `Timer` class and use its `init()` method. This `init()` method takes the following three arguments:

```
Timer.init(*, mode=Timer.PERIODIC, period=-1, callback=None)
```

###### ★ The arguments after the `*` need to be keyword arguments!

|Argument|Contents|
|:---|:---|:---|
|`mode`|To create an interrupt only once after time interval set in `period` has passed, set this argument to `Timer.ONE_SHOT`. To create interrupts every time the time interval from `period` passes, set this argument to `Timer.PERIODIC`. |
|`period`|Set how much time (in milliseconds) to wait before running the interrupt again. |
|`callback`|Set the name of the function you want to run during the interrupt here. |



Now let’s write some code with these. Start by importing the objects and classes you need.

```python=1
from machine import Timer  # The class for timer interrupts
from pystubit.board import buzzer  # An object for controlling the Buzzer
```

Next let’s define the function we want to run during the interrupt. This function’s job is to turn the Buzzer on and off, playing the note C4. Interrupt functions like this also share global variables, so let’s record on and off states for the Buzzer in a global variable so this function can act like an on/off switch.

###### New Code (Line 4, Lines 6-13)

```python=1
from machine import Timer
from pystubit.board import buzzer

is_playing = False  # This boolean value shows the Buzzer’s on and off states

def toggle_buzzer(t): #  When the interrupt calls this function the argument with be set to the timer object it’s being called to
    global is_playing  # You need to declare this global variable to change its contents
    
    if is_playing:  # Turn the Buzzer off it it’s currently playing
        buzzer.off() 
    else:  # Turn the Buzzer on if it’s currently off
        buzzer.on("C4")
    is_playing = not is_playing  # Switch between True and False using the not operator
```

Pay attention to how the function you call with the interrupt needs you to specify one argument for it. When you call the function from a timer object’s `init()` method, that timer object itself is used to set this argument.

Run the `init()` method at the end of the program to finish setting up your timer interrupt.

##### Example Code 6-1-1
###### New Code (Line 16, Line 18)

```python=1
from machine import Timer
from pystubit.board import buzzer

is_playing = False

def toggle_buzzer(t):
    global is_playing
    
    if is_playing:
        buzzer.off()
    else:
        buzzer.on("C4")
    is_playing = not is_playing


mytimer = Timer(1)  # Make an object and set its ID with an integer. Set the ID to anything as long as it’s different from any other IDs you have. 
# We want to request this interrupt at regular intervals, so set the mode argument to Timer.PERIODIC
mytimer.init(mode=Timer.PERIODIC, period=1000, callback=toggle_buzzer)
```

Now transfer your program and see how it works!

#### ■ Things to Look Out for When Using Interrupts

It’s important to keep the code you run during an interrupt as short and simple as possible. If you try to run too many commands during an interrupt, it may delay the other programs you’re trying to run quite a bit!

In Example Code 6-1-1, we kept the commands in the `toggle_buzzer()` function to a minimum, so even when we add a parallel process that scrolls a looping message across the LED display, the program won’t be slowed down!

##### Example Code 6-1-2
###### New Code (Line 2, Lines 18-19)

```python=1
from machine import Timer
from pystubit.board import buzzer, display  # Add a display object

is_playing = False

def toggle_buzzer(t):
    global is_playing
    
    if is_playing:
        buzzer.off()
    else:
        buzzer.on("C4")
    is_playing = not is_playing

mytimer = Timer(1)
mytimer.init(mode=Timer.PERIODIC, period=1000, callback=toggle_buzzer)

while True:  # Make the message loop scroll
    display.scroll("Hello Python!")
```

When you run this program, you’ll see that text scroll isn’t disrupted when the Buzzer turns on and off!

If we try to write this program using the `sleep_ms()` function from the `time` module instead, the text scroll on the display will stop every time the Buzzer turns on. Try running the example code below to see the difference for yourself!

##### Example Code 6-1-3
###### New and Changed Code (Line 1, Lines 6-10, Line 14)
```python=1
import time  # Import the time module
from machine import Timer
from pystubit.board import buzzer, display

# Use the time module’s sleep_ms() function to make the Buzzer play for one second
def toggle_buzzer(t):
    buzzer.on("C4")
    time.sleep_ms(1000)
    buzzer.off()


mytimer = Timer(1)
# Change the interrupt interval to to 2000 milliseconds (period=2000)
mytimer.init(mode=Timer.PERIODIC, period=2000, callback=toggle_buzzer)

while True:
    display.scroll("Hello Python!")
```

The reason this happens is that the main process in the program (the text scroll) gets suspended for the duration of the interrupt’s process, so the main program gets delayed for as long as the `sleep_ms()` function keeps things paused.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-1/9-1_6_E.png"/>

Because of this, it’s better to avoid using pausing functions like `sleep_ms()` in interrupt programs as much as possible and see if you can find a different technique to use instead!


### GPIO Interrupts

Most microcomputers are equipped with **GPIOs** (General Purpose Inputs/Outputs) that can receive electrical signals (changes in voltage) from external sensors and send out signals to control motors and other actuators.

The image below shows where all the GPIOs on your Core Unit’s microcomputer (the ESP32) are located. These ports let you control the LED display as well as any sensors or motors you have plugged into the Robot Expansion Unit!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-1/9-1_7_E.png"/>

The Core Unit’s GPIOs are used for the purposes listed in this table:

##### The Standard ESP32 GPIOs and What the Core Unit Uses Them For

|Standard ESP32 GPIO|What does it do in the Core Unit?|
|:---|:---|
|GPIO1|Serial communications via USB cable (TXD/Transmit Data)|
|GPIO2|Switch power to the LED display on/off|
|GPIO3|Serial communications via USB cable (RXD/Receive Data)|
|GPIO4|Control lighting patterns on the LED display|
|GPIO5|Motion sensors (Accelerometer, Gyroscope, Compass)|
|GPIO6|Read/write communications with the internal flash memory|
|GPIO7|Read/write communications with the internal flash memory|
|GPIO8|Read/write communications with the internal flash memory|
|GPIO9|Read/write communications with the internal flash memory|
|GPIO10|Read/write communications with the internal flash memory|
|GPIO11|Read/write communications with the internal flash memory|
|GPIO12|Wi-Fi/Bluetooth communications light (the LED below Button B)|
|GPIO13|Port P16 on the Robot Expansion Unit|
|GPIO14|Power light (the LED below Button A)|
|GPIO15|Button A|
|GPIO16|Read from/write to PSRAM|
|GPIO17|Read from/write to PSRAM|
|GPIO18|Port P13 on the Robot Expansion Unit|
|GPIO19|Port P14 on the Robot Expansion Unit|
|GPIO21|I2C communications (SDA)|
|GPIO22|I2C communications (SCL)|
|GPIO23|Port P15 on the Robot Expansion Unit|
|GPIO25|Buzzer|
|GPIO27|Button B|
|GPIO32|Port P0 on the Robot Expansion Unit|
|GPIO33|Port P1 on the Robot Expansion Unit|
|GPIO36|Port P2 on the Robot Expansion Unit|
|GPIO39|Port P3 on the Robot Expansion Unit|

**GPIO interrupts** are interrupts that happen when the computer detects a signal (i.e. a change in voltage) from one of these standard GPIOs. Changes in voltage happen when the ports receive input from a sensor or when you send commands to an LED, Buzzer, motor, or other actuator. 

Let’s say you press and release the A and B buttons, which are connected to GPIO15 and GPIO27. That makes the voltage from the GPIOs change like this:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-1/9-1_8_E.png"/>

Let’s try this out by writing a program that interrupts a certain process when there’s a change in voltage from GPIO15 (which is connected to the A button).

#### ■ Programming with GPIO Interrupts

Features that control GPIOs can be found in the `machine` module’s `Pin` class. The `Pin` class has a method called `irq()` which we can use to set up interrupts based on voltage changes from the GPIOs.

###### ★ `irq` is short for “Interrupt ReQuest”!

When you call the `irq()` method, you need to specify the two arguments shown below.

```
Pin.irq(handler=None, trigger=(Pin.IRQ_FALLING | Pin.IRQ_RISING))
```

|Argument|What does it do?|
|:---|:---|
|`handler`|Runs the function named here as an interrupt when the voltage change specified in `trigger` occurs.|
|`trigger`|Sets the change in voltage that should prompt the interrupt request. |

You can set the `trigger` argument using one of the following two constants from the `Pin` class.

* `Pin.IRQ_FALLING`
When the voltage goes down (falling).
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-1/9-1_9_E.png"/>


* `Pin.IRQ_RISING`
When the voltage goes up (rising).
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-1/9-1_10_E.png"/>

When you use either of these states to set the `trigger` argument, you’ll need to connect them with a `|` sign, which is a **bitwise operator**. (We’ll talk about bitwise operations more in Topic 10.)

```
# Set an interrupt for both rising and falling voltage
trigger=(Pin.IRQ_FALLING | Pin.IRQ_RISING)
```

Now let’s write some code with this stuff. The program we’re going to write is designed to create an interrupt when you press the A button that makes the Buzzer turn on and off (playing note C4).

We’ll start by importing the classes and objects we need for the rest of the program.

```python=1
from machine import Pin  # The GPIO class
from pystubit.board import buzzer  # An object for controlling the Buzzer
```

We can use the same function we wrote in Example Code 6-1-1 for timer interrupts here to turn the Buzzer on and off. The difference is that since we’re making GPIO interrupt this time, the function will take the `Pin` class object it’s called to as its argument instead of a `Timer` object. Add this code to your program:

###### New Code (Lines 4-13)

```python=1
from machine import Pin
from pystubit.board import buzzer

is_playing = False  # This function is mostly the same as Example Code 6-1-1

def toggle_buzzer(pin):  # Takes the Pin class object it\’s called to as its argument
    global is_playing
    
    if is_playing:
        buzzer.off()
    else:
        buzzer.on("C4")
    is_playing = not is_playing
```

Next we’ll write the code that sets up the GPIO interrupt.

Specify the GPIO connected to the A button, then make a `Pin` class instance and set the two arguments for the `Pin` class’s constructor (an `__init__()` method). The first argument takes the number of the GPIO you’re using, and the second takes its type. The type can be set using one of these constants:

* `Pin.IN`
Set this for an input-type GPIO, such as one connected to a button or sensor.
* `pin.OUT`
Set this for an output-type GPIO, such as one connected to an LED, Buzzer, or motor.

Button A is connected to your microcomputer’s GPIO15, which accepts input from the button, so the code for this part will look like this:

###### New Code (Line 14)

```python=1
from machine import Pin
from pystubit.board import buzzer

is_playing = False

def toggle_buzzer(pin):
    global is_playing
    if is_playing:
        buzzer.off()
    else:
        buzzer.on("C4")
    is_playing = not is_playing

pin_button_a = Pin(15, Pin.IN)  # Make a Pin class object
```

Now we just need to call the `irq()` method from the object we’ve created to set up the interrupt. We’ll set the interrupt to run when the button is pressed, which means when the voltage goes down, so we’ll use `Pin.IRQ_FALLING`.

##### Example Code 6-2-1
###### New Code (Line 16)

```python=1
from machine import Pin
from pystubit.board import buzzer

is_playing = False

def toggle_buzzer(pin):

    global is_playing
    if is_playing:
        buzzer.off()
    else:
        buzzer.on("C4")
    is_playing = not is_playing

pin_button_a = Pin(15, Pin.IN)
pin_button_a.irq(handler=toggle_buzzer, trigger=Pin.IRQ_FALLING)  # Sets up the interrupt
```

Now try running the program to see how it works! Once you've checked that it works correctly, try changing the `trigger` argument on line 15 to `Pin.IRQ_RISING` and running the program again. This should make the Buzzer play when you let go of the A button instead of when you press it!

###### Changed Code (Line 16)

```python=15
pin_button_a = Pin(15, Pin.IN)
pin_button_a.irq(handler=toggle_buzzer, trigger=Pin.IRQ_RISING)  # Runs the interrupt when you let go of the button
```


### How Would This Work Without Interrupts?

You can write a program that does the same thing as the example programs we’ve been writing without using interrupts. You actually already know how to do this, since you’ve done it plenty of times before!

You could write a program that makes the Buzzer play when you press the A button like this, for example:

```python=1
from pystubit.board import buzzer, button_a

is_playing = False  # Same function as lines 5-12

def toggle_buzzer():  # No argument needed
    global is_playing
    
    if is_playing:
        buzzer.off()
    else:
        buzzer.on("C4")
    is_playing = not is_playing

while True:  # Keep checking whether the A button is pressed
    if button_a.was_pressed():
        toggle_buzzer()
```

This program checks regular to see if certain events (a button being pressed, a sensor’s value changing) have occurred. Regularly checking for specific events and then running certain processes if they occur is called **polling**. Polling and interrupts are often compared to each other!

This means that most of the programs we wrote using sensors in the previous Topics used polling.

#### ■ Interrupts vs. Polling

All you need to do to set up a basic polling program is to write a `while` loop, so it’s a relatively simple program structure. Another advantage of polling programs is that you can run them through software alone without relying on specific hardware, while interrupts have to be written specifically for the hardware they run on.

However, when you use polling the program needs to spend time waiting, which prevents it from checking for other events as well. That means the polling program below won’t be able to detect when you press the A button until the text scroll on the LED display finishes, so you can’t make the Buzzer play whenever you want!

```python=1
from pystubit.board import buzzer, display, button_a

is_playing = False

def toggle_buzzer():
    global is_playing
    if is_playing:
        buzzer.off()
    else:
        buzzer.on("C4")
    is_playing = not is_playing

while True:
    # You can\’t check whether button A is pressed until the scroll ends!
    display.scroll("Hello Python!") 
    
    if button_a.was_pressed():
        toggle_buzzer()
```

Programmers use both interrupts and polling a lot when they’re coding for microcontrollers. Get a good idea of the strengths and weaknesses of both techniques here so you can tell which one is a better fit for your program when you need to!

## Challenge: Game Programming with Interrupts

For this lesson’s challenge, use both types of interrupts you’ve learned about to program a simple game for your LED display! 

### Game Introduction

Watch this video to see what the game you’re programming will look like in action!

<iframe src="https://player.vimeo.com/video/482974192" width="640" height="360" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

In this game, the player uses the Core Unit’s buttons to control a character represented by a green dot on the display. A wall (shown with red dots) will approach the player’s character, so they need to avoid it the wall by pressing the buttons to jump at the right time. To win the game, the player needs to jump over the wall twenty times, but one missed jump means game over! 

#### ■ Game Outline

The program for this game uses interrupts to run the following processes:

|Process|Interrupt Type|Interrupt When?|
|:---|:---|:---|
|Moving the Wall|Timer Interrupt|Every 200 ms|
|Character Jumps|GPIO Interrupt|When Button A is pressed|
|Character Lands|Timer Interrupt|Once, 500 ms after a jump|

Apart from these interrupts, the program also needs a main loop to run processes like refreshing the LED display and detecting whether the character has hit the wall.

### Constructing the Program

Now let’s go through and write each process for this program one at a time.

#### ■ The Wall-Moving Process

The wall should start out placed outside the right edge of the LED display. After that, it should move one column to the left every 250 milliseconds, and return to its starting position after reaching the left edge of the display.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-1/9-1_11_E.png"/>

With this movement pattern, the wall’s y-coordinates will stay the same while only the x-coordinates change. Now let’s make a program for this process in a function called `moving()`.

```python=1
wall_pos_x = 5  # Define the wall’s current position/x-coordinate as a global variable

def moving():
    global is_playing  # You need to declare this global variable to change its contents from inside the function
    
    if wall_pos_x > 0:  # When the wall is not at the left edge of the LED display
        wall_pos_x -= 1  # Move one column to the left (x-value－1)
    else:  # When the wall is at the left edge of the LED display
        wall_pos_x = 5  # Move to outside the right edge of the display
```

We’ll call this function using a timer interrupt, which we’ll make using a specialized `Timer` object. When we call our function using a timer interrupt, it needs to take the `Timer` object it’s called to as an argument, so we’ll add an argument to the `moving()` function for this.

###### New and Changed Code (Line 1, Line 5, Line 8)
```python=1
from machine import Timer  # Import the Timer class

wall_pos_x = 5

timer_moving = Timer(1)  # The wall movement timer


def moving(t):  # Add an argument that can be set with the Timer object the function is called to
    global wall_pos_x
    
    if wall_pos_x > 0:
        wall_pos_x -= 1
    else:
        wall_pos_x = 5
```

We can count how many times the player has successfully jumped the wall inside this function too! Let’s make the program add one to the wall jump counter every time the wall moves back to its starting position.

###### New and Changed Code (Line 3, Line 10, Line 16)
```python=1
from machine import Timer

count = 0  # Counter for successful wall jumps
wall_pos_x = 5

timer_moving = Timer(1)


def moving(t):
    global count, wall_pos_x # You need to make a global declaration for the count variable to change its value in this function
    
    if wall_pos_x > 0:
        wall_pos_x -= 1
    else:
        wall_pos_x = 5
        count += 1  # Increase the wall jump counter by 1
```

#### ■ The Jumping and Landing Processes

Next let’s program the process that makes the player character jump, along with one that makes the character land a set amount of time after a jump.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-1/9-1_12_E.png"/>


It’s important to note here that the jumping process and the landing process use **different types of interrupt**! The jump process is requested by a **GPIO interrupt** caused by pressing the A button, but the landing process is requested by a **timer interrupt** that happens **just once** 500 milliseconds after the jump process runs. The timer interrupt for the landing process also has to be set up inside the jump process, so don’t forget that either!

Now let’s program these processes!

Start by importing the `Pin` class you need for the GPIO interrupt, and make an object from it set with the number of the specific GPIO connected to the A button.

###### New and Changed Code (Line 1, Line 7)
```python=1
from machine import Timer, Pin  # Import the Pin class

count = 0
wall_pos_x = 5

timer_moving = Timer(1)
pin_button_a = Pin(15, Pin.IN)  # A Pin object that works with Button A
```

Next let’s make a function called `jumping()` which contains the code for the jumping process. When the `jumping()` function is called by an interrupt request from the `Pin` object, it needs to take that `Pin` as an argument, so make sure to declare an argument for it.

###### New and Changed Code (Line 5, Lines 21-24)
```python=1
from machine import Timer, Pin

count = 0
wall_pos_x = 5
character_pos_y = 4  # The character’s y-position

timer_moving = Timer(1)
pin_button_a = Pin(15, Pin.IN)


def moving(t):
    global wall_pos_x, count
    
    if wall_pos_x > 0:
        wall_pos_x -= 1
    else:
        wall_pos_x = 5
        count += 1

# The jumping process
def jumping(pin):  # The argument is needed so the function can accept the Pin object it’s called to
    global character_pos_y  # You need to declare this global variable to change its contents

    character_pos_y = 1  # After jumping, the character’s y-position is 1
```

Next let’s program the landing process in a function called `landing()`. This function is called using a timer interrupt, so it needs to have an argument set to the `Timer` object it’s called to.

###### New and Changed Code (Lines 27-30)
```python=21
def jumping(pin):
    global character_pos_y

    character_pos_y = 1

# The landing process
def landing(t):  # The argument is needed so the function can accept the Timer object it’s called to
    global character_pos_y

    character_pos_y = 4  # After landing, the character’s y-position is 4
```

Now we’ve defined the landing process with the `landing()` function, so we can set up a timer interrupt at **500 milliseconds after** the jumping process in the `jumping()` function. Make a new `Timer` object and try running its `init()` method. This interrupt should only happen once when you run it, so set its `mode` argument to `Timer.ONE_SHOT`.

###### New and Changed Code (Line 8, Line 28)
```python=1
from machine import Timer, Pin

count = 0
wall_pos_x = 5
character_pos_y = 4

timer_moving = Timer(1)
timer_landing = Timer(2)  # The landing timer. This is the second timer in the program, so set its number to 2
pin_button_a = Pin(15, Pin.IN)


def moving(t):
    global wall_pos_x, count
    
    if wall_pos_x > 0:
        wall_pos_x -= 1
    else:
        wall_pos_x = 5
        count += 1


def jumping(pin):
    global character_pos_y, is_jumping

    character_pos_y = 1
    is_jumping = True
    # Run the timer interrupt 500 ms after running the jumping process
    timer_landing.init(mode=Timer.ONE_SHOT, period=500, callback=landing)
```

Now we’ve defined both the jumping and landing processes, but there’s one problem with this code! Namely, if you press the button while the character is jumping, the `jumping()` function will run again and the timer interrupt will be reset. To fix this, we need to set up a flag to mark when the character is jumping and add code that prevents certain processes from running during a jump.

###### New and Changed Code (Line 11, Line 24, Lines 26-27, Line 30, Line 35, Line 38)
```python=1
from machine import Timer, Pin

count = 0
wall_pos_x = 5
character_pos_y = 4

timer_moving = Timer(1)
timer_landing = Timer(2)
pin_button_a = Pin(15, Pin.IN)

is_jumping = False  # This flag marks whether the player character is currently jumping

def moving(t):
    global wall_pos_x, count
    
    if wall_pos_x > 0:
        wall_pos_x -= 1
    else:
        wall_pos_x = 5
        count += 1


def jumping(pin):
    global character_pos_y, is_jumping  # Add a global declaration

    if is_jumping:  # The processes below won’t run if the character is jumping
        return  # End the current function
    
    character_pos_y = 1
    is_jumping = True  # Raise the jumping flag
    timer_landing.init(mode=Timer.ONE_SHOT, period=500, callback=landing)


def landing(t):
    global character_pos_y, is_jumping  # Add a global declaration
    
    character_pos_y = 4
    is_jumping = False  # Lower the jumping flag after landing
```


#### ■ The Collision Detection Process

If the player character hits the wall in this game, that means it’s game over! Let’s write a program that can detect if this collision happens and put it in a function called `detect_collision()`. Detecting a collision in this game is very simple, since it only happens if the wall and the player character are in these exact positions:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_9-1/9-1_13_E.png"/>

You can write this part of the program like this:

###### New and Changed Code (Lines 39-43)
```python=34
def landing(t):
    global character_pos_y, is_jumping
    
    character_pos_y = 4
    is_jumping = False

# Detect a collision
def detect_collision():
    if wall_pos_x == 1 and character_pos_y == 4:
        return True  # Return True if there’s a collision
    else:
        return False  # Return False if there’s no collision
```

#### ■ The LED Display Refresh Process

Our program so far can change the positions of the wall and the player character, and detect if they run into each other. The next step is to make the positions we’ve programmed appear on the LED display.

The positions of the wall and the character change depending on the passage of time and the player pressing the button, so we need to make the LED display refresh itself regularly as well. To do this, let’s define an `update_display()` function that refreshes the display once.

###### New and Changed Code (Line 2, Line 14, Lines 50-62)

```python=1
from machine import Timer, Pin
from pystubit.board import display, Image # Import the display object and image classes

count = 0
wall_pos_x = 5
character_pos_y = 4

timer_moving = Timer(1)
timer_landing = Timer(2)
pin_button_a = Pin(15, Pin.IN)

is_jumping = False

img = Image(5, 5)  # Make an image object of 5x5 blank spaces
```

```python=43
def detect_collision():
    if wall_pos_x == 1 and character_pos_y == 4:
        return True
    else:
        return False

# The LED display refresh process
def update_display():
    # Reset the image
    for x in range(5):
        for y in range(5):
            img.set_pixel(x, y, 0)  # Turn off each LED
    # Add the wall to the image
    x = wall_pos_x  # Assign wall_pos_x to a local variable
    if x <= 4:  # Display the wall as long as it isn’t off the right edge of the screen (x=5)
        for y in range(2, 5):  # Keep the y-axis position between 2 and 4 
            img.set_pixel_color(x, y, color=(31, 0, 0))   # Make the wall red      
    # Add the character to the image with a fixed x-position of 1
    img.set_pixel_color(1, character_pos_y, color=(0, 31, 0))  # Make the player character green
    display.show(img, delay=0)  # Display the image without any delay
```

#### ■ Ending the Game

There are two processes we need to run to end the game: turning off the interrupts and displaying the game results.

`Timer` objects come with a specialized method for turning them off called `deinit()`, which doesn’t need any arguments to run. `Pin` objects don’t have a method like this, so instead you need to run the `irq()` method and set its `handler` argument with a `None` object to turn off a GPIO interrupt.

We’ll use the value of the `count` variable to judge whether the player has won the game. If the count reaches twenty, the player wins, and if it hasn’t when the game ends, that’s a game over! Either way, when the game ends we’ll have the results scroll across the LED display.

We’ll put both these processes in a function called `quit_game()`, using this code:

###### New and Changed Code (Line 3, Lines 62-73)
```python=1
from machine import Timer, Pin
from pystubit.board import display, Image
import time  # Import the time module
```

```python=50
def update_display():
    for x in range(5):
        for y in range(5):
            img.set_pixel(x, y, 0)
    x = wall_pos_x
    if x >= 0 and x <= 4:
        for y in range(2, 5):
            img.set_pixel_color(x, y, color=(31, 0, 0))          
    img.set_pixel_color(1, character_pos_y, color=(0, 31, 0))
    display.show(img, delay=0)
    
# End the game
def quit_game():
    # Turn off the interrupts
    timer_moving.deinit()
    timer_landing.deinit()
    pin_button_a.irq(handler=None)  # You don’t need to set the trigger parameter here
    
    # Display the game results
    time.sleep_ms(1000)  # Wait before displaying the game results
    if count < 20:  # Display the game over result in red
        display.scroll("Game Over", delay=50, color=(31, 0, 0))
    else:  # Display the winning result in green
        display.scroll("You Win!", delay=50, color=(0, 31, 0))
```

#### Resetting the Game

Now we need to make the game reset to its initial state after it ends so you can play it again once you finish one round! Let’s put this process in a function called `init_game()`.

###### New and Changed Code (Lines 74-80)
```python=62
def quit_game():
    timer_moving.deinit()
    timer_landing.deinit()
    pin_button_a.irq(handler=None)
    
    time.sleep_ms(1000)
    if count < 20:
        display.scroll("Game Over", delay=50, color=(31, 0, 0))
    else:
        display.scroll("You Win!", delay=50, color=(0, 31, 0))
        
# Reset the game to the initial state
def init_game():
    global count, wall_pos_x, character_pos_y, is_jumping

    count = 0
    wall_pos_x = 5
    character_pos_y = 4
    is_jumping = False
```

#### ■ The Main Loop Process

The last thing we need to program is the main loop. We’ll put this loop in a function called `start_game()`, and make it run through the following steps: Reset the game ➡ Countdown to the start of the game ➡ Set up the interrupts ➡ Loop the display refresh and collision detection processes ➡ End the game.

###### New and Changed Code (Lines 84-101)
```python=75
def init_game():
    global count, wall_pos_x, character_pos_y, is_jumping
    
    count = 0
    wall_pos_x = 5
    character_pos_y = 4
    is_jumping = False
    
# The main loop
def start_game():
    init_game()  # Reset the game
    
    # Countdown to game start
    for num in range(3, -1, -1):  # Display “3, 2, 1, 0” in order
        display.show(str(num), delay=1000)
    
    # Set up the interrupts
    timer_moving.init(mode=Timer.PERIODIC, period=200, callback=moving)
    pin_button_a.irq(handler=jumping, trigger=Pin.IRQ_FALLING)
    
    # The display refresh/collision detection loop. This loop also ends if the player jumps over the wall 20 times successfully.
    while not detect_collision() and count < 20:
        time.sleep_ms(33)  # Refresh at the frequency of about 30 times per second
        update_display()
    
    # When the loop above ends, end the game and display the results
    quit_game()
```

Now your program is finished! Run the program, then call the `start_game()` function in the terminal so you can try playing your finished game! 

<pre class="prettyprint">
&gt;&gt;&gt; start_game()
</pre>

If errors occur in your program when you run it and you can’t figure out how to fix them, compare your code to the example below to check for mistakes!

##### Example Code 7-2-1
```python=1
from machine import Timer, Pin
from pystubit.board import display, Image
import time

count = 0
wall_pos_x = 5
character_pos_y = 4

timer_moving = Timer(1)
timer_landing = Timer(2)
pin_button_a = Pin(15, Pin.IN)

is_jumping = False

img = Image(5, 5)

def moving(t):
    global  count, wall_pos_x

    if wall_pos_x > 0:
        wall_pos_x -= 1
    else:
        wall_pos_x = 5
        count += 1


def jumping(pin):
    global character_pos_y, is_jumping
    
    if is_jumping:
        return

    character_pos_y = 1
    is_jumping = True
    timer_landing.init(mode=Timer.ONE_SHOT, period=500, callback=landing)


def landing(t):
    global character_pos_y, is_jumping
    
    character_pos_y = 4
    is_jumping = False


def detect_collision():
    if wall_pos_x == 1 and character_pos_y == 4:
        return True
    else:
        return False


def update_display():
    for x in range(5):
        for y in range(5):
            img.set_pixel(x, y, 0)
    x = wall_pos_x
    if x <= 4:
        for y in range(2, 5):
            img.set_pixel_color(x, y, color=(31, 0, 0))          
    img.set_pixel_color(1, character_pos_y, color=(0, 31, 0))
    display.show(img, delay=0)
    

def quit_game():
    timer_moving.deinit()
    timer_landing.deinit()
    pin_button_a.irq(handler=None)
    
    time.sleep_ms(1000)
    if count < 20:
        display.scroll("Game Over", delay=50, color=(31, 0, 0))
    else:
        display.scroll("You Win!", delay=50, color=(0, 31, 0))
        

def init_game():
    global count, wall_pos_x, character_pos_y, is_jumping

    count = 0
    wall_pos_x = 5
    character_pos_y = 4
    is_jumping = False
    

def start_game():
    init_game()
    
    for num in range(3, -1, -1):
        display.show(str(num), delay=1000)

    timer_moving.init(mode=Timer.PERIODIC, period=200, callback=moving)
    pin_button_a.irq(handler=jumping, trigger=Pin.IRQ_FALLING)
    
    while not detect_collision() and count < 20:
        time.sleep_ms(33)
        update_display()
    
    quit_game()
```

## Conclusion

### A Quick Review

In this lesson you learned about a new technique called **interrupts**, which are used to let computers multitask. You also learned that there are different kinds of interrupts, and practiced using **timer interrupts** and **GPIO interrupts** yourself by writing a program with them!

In the first part of the lesson we gave you a small introduction to the inner workings of microcomputers. If you think you might want to become a software engineer in the future, learning about hardware like this is very important! If you want to build new technology, hardware is just as important as software. We recommend going back over this lesson and doing some further research on any terminology you don’t understand!

### Coming Up Next Time!

In the next lesson we’ll learn more parallel processing techniques!