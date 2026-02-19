---
tags: Python_English, Topic 12
---

Python Robotics Course Lesson 47

Topic 12-3
Controlling Robots Online
===

Build a robot forklift you can operation using controls in a web browser!

## In This Lesson...

Use what you’ve learned in Topics 12-1 and 12-2 to make a robot control panel page for your smartphone or tablet’s browser and program an application that lets you control a robot forklift with that panel!

<iframe src="https://player.vimeo.com/video/485744271" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

:::info

Lesson Introduction

■ Lesson 47 Contents
1. Making a Robot Forklift
2. Making a Forklift Operation Application
3. Challenge: Slider Controls


In this lesson you’ll make a control system that lets you operate a multi-functional robot from a webpage.

Start the lesson by building a robot modeled after the forklifts used to transport cargo in warehouses and other industrial environments.

After building the robot forklift, you’ll program the application that operates it. You’ll be able to make the forklift’s car drive around and lift the fork up and down by pressing on-screen buttons.

For the final challenge, modify your program to change some of the button controls in your application into a slider!

Now let's start the lesson!

:::

## Building a Robot Forklift

Let’s build a robot forklift you can control using a smartphone or tablet!

##### Building the Robot Forklift
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-3/12-3_1_E.png"/>

Parts You'll Need

Parts
* Core Unit x 1
* Robot Expansion Unit x 1
* Battery Box x 1
* DC Motor x 2
* Servomotor x 1
* Basic Cube (White) x 4
* Half A (Gray) x 3
* Half B (Gray) x 1
* Half C (White) x 19
* Half D (White) x 9
* Beam x 3
* Disk x 1
* Gear (L) x 2
* Tire ×2
* Axle x 4

The Artec Block Shapes

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-3/12-3_2_E.png"/>

Follow the instructions below to build the forklift we'll use in this lesson:

[Building a Forklift](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-3/12-3_build_E.pdf)

### How Your Robot Forklift Works
The robot forklift we’ve built raises and lowers its forks using a mechanism called a **linkage**. Linkages are made up of rod-shaped sections called **links** connected with rotating joints. When the linkage’s ground link is connected to a motor and the motor moves, it transfers that motion through connected couplers to follower links, making them rotate or oscillate (move back and forth).

There a many different kinds of linkages, but the one used in this robot is a **parallel crank**, made up of four links joined in a parallelogram shape.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-3/12-3_3_E.png"/>

Parallel cranks can move while maintaining their parallelogram shape, which means we can use one in this forklift to make sure the fork stays level as it moves up and down.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-3/12-3_4_E.png"/>

## Making a Forklift Operation Application

We want to make a control panel page like the one shown below for your smartphone or tablet. When you tap each button, the browser will send a request to the Core Unit to make it take the corresponding action. When you move your finger off of the button, then the controller will send a request to stop the corresponding action.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-3/12-3_5_E.png"/>

Just like the application you made in the last lesson, this controller application can be divided into a client-side program that runs in the browser and a server-side program that runs on the Core Unit. We have a pre-prepared client-side program to use for this, but we’ll be building the server-side program from the ground up!

### Setting Up the Client-Side Program

Copy and paste the code below into Mu Editor, then save it with the name **forklift.html**. **Save this file with the file type `Other(*.*)`!** After that, select **Files** in the Mu Editor menu to transfer your saved file to the Core Unit.

##### Example Code 3-1-1 (forklift.html)
```html=1
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Forklift controller</title>
<style>
	body{
		width: 100%;
		height: 100%;
		margin: auto;
		font-family: Tahoma, Geneva, "sans-serif";
	}
	div#main{
		position: absolute;
		width: 100%;
		height: 100%;
		
	}
	#main button{
		position: absolute;
		display: block;
		width:20%;
		height:20%;
		color: #ffffff;
		font-size:2em;
		border: none;
		border-radius: 5px;
		transform: translateY(-50%) translateX(-50%);
		-webkit- transform: translateY(-50%) translateX(-50%);
	}
	button.vehicle{
		background-color: #036EB8;
	}
	button.lift{
		background-color: #F39800;
	}
	button#moveForward{
		left:25%;
		top:33%;
	}
	button#moveBackward{
		left:25%;
		top:66%;
	}
	button#rotateLeft{
		left:50%;
		top:33%;
	}
	button#rotateRight{
		left:50%;
		top:66%;
	}
	button#liftUp{
		left:75%;
		top:33%;
	}
	button#liftDown{
		left:75%;
		top:66%;
	}
	
</style>
</head>
<body>
	<div id="main">
		<button id="moveForward" class="button vehicle" type="button">Move<br>forward</button>
		<button id="moveBackward" class="button vehicle" type="button">Move<br>backward</button>
		<button id="rotateLeft" class="button vehicle" type="button">Rotate<br>left</button>
		<button id="rotateRight" class="button vehicle" type="button">Rotate<br>right</button>
		<button id="liftUp" class="button lift" type="button">Lift<br>up</button>
		<button id="liftDown" class="button lift" type="button">Lift<br>down</button>
	</div>
</body>
<script>
	window.onload = function(){
		const req = new XMLHttpRequest();
		const buttonElms = document.getElementsByClassName("button");
		
		req.onreadystatechange = function(){
			if(this.readyState == 4){
				if(this.satus == 200){
					console.log(req.responseText);
				}
			}
		};
		
		function httpRequest(evt){
			evt.preventDefault();
	
			let elm = evt.target;
			let evtType = evt.type;		
			let id = elm.id;
			let className = elm.className;
			let operation = null;
			
			if(evtType=="mousedown" || evtType=="touchstart"){
				operation = id;
			}
			else if(className.indexOf("vehicle") != -1){
				operation = "brake";
			}
			else if(className.indexOf("lift") != -1){
				operation = "liftStop";
			}
			
			if (operation){
				req.open("GET", `forklift.py?operation='${operation}'`, true);
				req.send(null);	
			}
		};
		
		for(let i=0; i < buttonElms.length; i++){
			buttonElms[i].addEventListener("mousedown", httpRequest, false);
			buttonElms[i].addEventListener("touchstart", httpRequest, false);
			buttonElms[i].addEventListener("mouseup", httpRequest, false);
			buttonElms[i].addEventListener("touchend", httpRequest, false);
		}		
		
	};
</script>
</html>
```

This program sends requests when you tap and release the buttons shown in the browser. The requests include these request lines:

##### Request Line Sent When a Button is Tapped
```
GET /forklift.py?operation=%27moveForward%27 HTTP/1.1
```

##### Request Line Sent When a Button is Released
```
GET /forklift.py?operation=%27brake%27 HTTP/1.1
```

URL segments like `operation=%27moveForward%27` and `operation=%27brake%27` describe actions for the forklift to take. (The `%27` in these segments is actually a single quotation mark, written in percent encoding because you can’t use single quotations in URLs!)

The `operation` parameter here will be set to one of the following values, depending on which button you press.

##### operation Values and Their Forklift Commands
|`Operation` Value|Robot’s Action|User Action|
|:---|:---|:---|
|`"moveForward"`|Drives forward| Tapping the **Move forward** button|
|`"moveBackward"`|Drives backward| Tapping the **Move backward** button|
|`"rotateLeft"`|Rotates to the left|Tapping **Rotate left** button|
|`"rotateRight"`|Rotates to the right|Tapping the **Rotate right** button|
|`"brake"`|Use the brakes to stop|Releasing one of the last four buttons|
|`"liftUp"`|Raise the fork|Tapping the **Lift up** button|
|`"liftDown"`|Lower the fork|Tapping the **Lift down** button|
|`"liftStop`|Stop moving the fork|Releasing one of the last two buttons|

The server-side program will check which of these values it’s receiving and then run the code for the corresponding action.

### Making the Server-Side Program

We’ll split this program into two separate processes for us to code: one process controls two DC Motors to make the forklift’s car drive around, and the other process controls a Servomotor to move the forklift’s fork up and down. 

#### ■ Driving the Forklift Car

We can actually use the module we made for robocars in Topic 6 (`vehicle.py`) for this part of the control program! The class `VehicleRobot` that’s defined in this module already has methods that can make a robot vehicle drive forward, backward, and turn. For more details about this class, go back and look at Topic 6-1!

##### Example Code 3-2-1 (vehicle.py)
```python=1
from pyatcrobo2.parts import DCMotor
import time

class VehicleRobot:
    def __init__(self, *, pin_l, pin_r):
        self.dcm_l = DCMotor(pin_l)
        self.dcm_r = DCMotor(pin_r)
        self.dcm_l.power(100)
        self.dcm_r.power(100)
        
    def time_control(func):  # Decorator for using time controls
        def new_func(self, duration=-1):
            func(self)
            if duration > 0:
                time.sleep_ms(int(duration))
                self.brake()
        return new_func
    
    @time_control
    def move_forward(self):  # Driving forward
        self.dcm_l.ccw()
        self.dcm_r.ccw()
        
    @time_control
    def move_backward(self):  # Driving backward
        self.dcm_l.cw()
        self.dcm_r.cw()
    
    @time_control    
    def rotate_left(self):  # Rotating left
        self.dcm_l.cw()
        self.dcm_r.ccw()

    @time_control
    def rotate_right(self):  # Rotating right
        self.dcm_l.ccw()
        self.dcm_r.cw()

    @time_control
    def turn_left(self):  # Turning left
        self.dcm_l.brake()
        self.dcm_r.ccw()

    @time_control
    def turn_right(self):  # Turning right
        self.dcm_l.ccw()
        self.dcm_r.brake()

    def stop(self):  # Stopping slowly
        self.dcm_l.stop()
        self.dcm_r.stop()

    def brake(self):  # Stopping immediately with the brakes
        self.dcm_l.brake()
        self.dcm_r.brake()

    def set_speed_to_left(self, speed):  # Set the speed of the left DC Motor
        speed = int(speed)                 
        speed = 1 if speed < 1 else speed
        speed = 10 if speed > 10 else speed
        power = speed * 25
        self.dcm_l.power(power)

    def set_speed_to_right(self, speed):  # Set the speed of the right DC Motor
        speed = int(speed)
        speed = 1 if speed < 1 else speed
        speed = 10 if speed > 10 else speed
        power = speed * 25
        self.dcm_r.power(power)
```

Now you can transfer this program file to your Core Unit and import it into the new program you’ll be writing to make use of this class!

Either right click on the link below, select **Save link as...** and download the file to your **mu_code** folder, or  copy and paste the code above into Mu Editor and save it with the file name **vehicle** (and the extension **.py**). Once you have a copy of the file, go to **Files** in Mu Editor and transfer it to the Core Unit.

vehicle.py (https://www.artec-kk.co.jp//school/cl/textbooks/material_en/topic_12-3/vehicle.py)

After you’re done transferring, start writing the program that makes the forklift drive and turn in different directions based on the requests it receives. Make a new program file and start it by importing the `vehicle` module you just saved and creating an instance of the `VehicleRobot` class.

```python=1
import vehicle

robo = vehicle.VehicleRobot(pin_l="M2", pin_r="M1")  # Set the left DC Motor to pin_l and the right DC Motor to pin_r
```

After that, you need to definethe set of all the valid values of `operation` parameter in your client requests. Five of these values are commands that affect the movement of the forklift’s car. Additionally, the program should send a message to notify the user that their command is invalid if it receives a request with an invalid parameter value.

###### New Code (Lines 5-11)
```python=1
import vehicle

robo = vehicle.VehicleRobot(pin_l="M2", pin_r="M1")

OPERATIONS = {
    "moveForward",  # Drive forward
    "moveBackward",  # Drive backward
    "rotateLeft",  # Rotate to the left
    "rotateRight",  # Rotate to the right
    "brake",  # Use the brakes to stop
}
```

Let’s define the `main()` function, which will be called when the program receives a request. This function will check whether the value given to its `operation` argument is included in the  `OPERATIONS` set, and then send a message in response. Add this code to your program:

###### New Code (Lines 13-18)
```python=1
import vehicle

robo = vehicle.VehicleRobot(pin_l="M2", pin_r="M1")

OPERATIONS = {
    "moveForward",
    "moveBackward",
    "rotateLeft",
    "rotateRight",
    "brake",
}

def main(operation):
    if operation in OPERATIONS:  # If a valid value was received
        # (Robot controlling processes will be added here)
        return "'{}' succeeded.".format(operation), "text/plain"  # Send a success message
    else:  # If an invalid value was received
        return "Failed. '{}' is invalid.".format(operation), "text/plain"  # Send a failure message
```

When the `operation` argument receives a valid command value, we’ll run a method from the `VehicleRobot` class object we made on line 3. Add this code to your program:


###### New Code (Lines 15-24)
```python=1
import vehicle

robo = vehicle.VehicleRobot(pin_l="M2", pin_r="M1")

OPERATIONS = {
    "moveForward",
    "moveBackward",
    "rotateLeft",
    "rotateRight",
    "brake",
}

def main(operation):
    if operation in OPERATIONS:
        if operation == "moveForward":  # Drive forward
            robo.move_forward()
        elif operation == "moveBackward":  # Drive backward
            robo.move_backward()
        elif operation == "rotateLeft":  # Rotate left
            robo.rotate_left()
        elif operation == "rotateRight":  # Rotate right
            robo.rotate_right()
        elif operation == "brake":  # Stop
            robo.brake()
        return "'{}' succeeded.".format(operation), "text/plain"
    else:
        return "Failed. '{}' is invalid.".format(operation), "text/plain"
```

Now let’s test to make sure the code we have so far runs correctly. Save this file as **forklift(.py)**, then transfer it to the Core Unit from Mu Editor’s **File** menu.

After that, open the file for the web server program you made in the previous lesson and edit the beginning of the code as shown below. We won’t be using the application modules from the last lesson here, so change those parts of the program into comments.

###### New and Changed Code (Lined 3-7, Lines 13-17)
```python=1
import network
import usocket
# import monitor  # Turn unused applications into comments
# import control_display
# import control_servo
# import control_buzzer
import forklift  # New code

SSID = "studuinobit"
PASSWORD = "Artecrobo2"

modules = {
    # "/monitor.py": "monitor",
    # "/control_display.py": "control_display",
    # "/control_servo.py": "control_servo",
    # "/control_buzzer.py": "control_buzzer",
    "/forklift.py": "forklift",  # New code
}
```

Now run your web server program to turn your Core Unit into a wi-fi access point and connect your smartphone or tablet to that access point. When you’ve successfully connected, open a browser on your device and go to the URL below to start sending requests.

```
http://192.168.4.1/forklift.html
```

When you tap one of the blue buttons shown on the control panel page, the forklift should drive as that button commands it!

#### Moving the Fork

We have modules we’ve made in previous Topics we can use for this part of the program too! Specifically, we’ll be using the `myservo.py` module from Topic 8-1. This module lets us adjust the speed of our Servomotor’s rotation and make multiple Servomotors move in sync.

##### Example Code 3-2-1 (myservo.py)
```python=1
import time
from pyatcrobo2.parts import Servomotor

# A class for controlling multiple Servomotors simultaneously
class MyServomotor(Servomotor):  # Inherits from the existing Servomotor class
    def __init__(self, pin):
        super().__init__(pin)
        self.angle = 90  # Property that records the current Servomotor angle
        super().set_angle(self.angle)
    
    def set_angle(self, degree):  # Overwrite a method from the parent class
        super().set_angle(degree)
        self.angle = degree
    
    def get_angle(self):  # Method for retrieving the current angle
        return self.angle

# Function for syncronizing the movements of multiple Servomotors
def syncRotateServos(duration, *args):   # args takes tuples made up of MyServomotor class instances and angles after rotation
    speeds = {}
    for arg in args:  # Find the angle each Servomotor should rotate per unit of time (1 ms)
        myservo = arg[0]
        degree = arg[1]
        diff = degree - myservo.get_angle()
        speeds[id(myservo)] = diff/duration
    
    for _ in range(duration):  # Make the Servomotors rotate only the degrees per millisecond found above
        for arg in args:
            myservo = arg[0]
            degree = myservo.get_angle() + speeds[id(myservo)]
            myservo.set_angle(degree)
        time.sleep_ms(1)
```

In this program we’ll be using this module to adjust the speed at which the forklift’s fork moves up and down. Either right click on the link below, select **Save link as...** and download the file to your **mu_code** folder, or copy and paste the code above into Mu Editor and save it with the file name **myservo** (and the extension **.py**). Once you have a copy of the file, go to **Files** in Mu Editor and transfer it to the Core Unit.

myservo.py　(https://www.artec-kk.co.jp//school/cl/textbooks/material_en/topic_12-3/myservo.py)

When you’re done transferring, you can move on to adding code that controls the fork’s movements to `forklift.py`. Start by importing the `myservo` module at the beginning of the program and creating a `MyServomotor` class object.

###### New Code (Line 2, Line 5)
```python=1
import vehicle
import myservo

robo = vehicle.VehicleRobot(pin_l="M2", pin_r="M1")
servo = myservo.MyServomotor("P13")
```

Next, add the values of the request parameter `operation` that move the fork to your `OPERATIONS` set.

###### New Code (Lines 13-15)
```python=7
OPERATIONS = {
    "moveForward",
    "moveBackward",
    "rotateLeft",
    "rotateRight",
    "brake",
    "liftUp",  # Move the fork up
    "liftDown",  # Move the fork down
    "liftStop",  # Stop the fork
}
```

Make the Servomotor rotate one degree per each unit of time between when it receives a request to move up or down and when it receives a request to stop. We’ll define a function called `lift_up()` that raises the fork and another called `lift_down` that lowers it, but there are two points we need to pay attention to here.

First, the Servomotor’s range of movement. The Servomotor itself can rotate from 0 to 180 degrees, but here its range is limited by the fork running into the floor and the parts of the linkage hitting each other. To account for this, we’ll consider the Servomotor’s range to 50° to 150° on this robot.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-3/12-3_6_E.png"/>

Second, if the program just keeps moving the Servomotor over and over, that would stop the other processes in the program from working. That includes the process of accepting requests to stop, so you won’t be about to make the forklift stop where you want it to like this. This problem can be solved by used the `_thread` module so you can control the Servomotor in a separate thread from the other processes. You’ll need both threads here to share information about requests to stop, so you’ll have to make a global variable to handle that part.

With this information in hand, we’re ready to make the next three functions.

* `lift_up()`
Raises the angle of the Servomotor up to 150° maximum until the button is released (and a stop request is received).
* `lift_down()`
Lowers the angle of the Servomotor down to 50° minimum until the button is released (and a stop request is received).
* `stop_lifting()`
When a button is released and stop request is received, change the value of the global variables shared with the `lift_up()` and `lift_down()` functions that are running in a different thread.

The code for these functions looks like this:

###### New Code (Line 6, Lines 20-27, Lines 30-37, Lines 40-42)
```python=1
import vehicle
import myservo

robo = vehicle.VehicleRobot(pin_l="M2", pin_r="M1")
servo = myservo.MyServomotor("P13")
stop_signal = False  # Variable that shares the information that a stop request has been received

OPERATIONS = {
    "moveForward",
    "moveBackward",
    "rotateLeft",
    "rotateRight",
    "brake",
    "liftUp",
    "liftDown",
    "liftStop",
}


def lift_up():  # Move the fork up
    global stop_signal  # Reference the global variable
    stop_signal = False  # Set the variable to False when there is no stop request
    angle = servo.get_angle()  # Find the current angle of the Servomotor
    while not stop_signal:  # Repeat until a stop requet is received
        if angle < 150:  # Set an upper limit to the motor’s rotation
            angle += 1  # Increase the angle 1 degree at a time
            myservo.syncRotateServos(20, (servo, angle))  # Use a function from the imported module to make the motor move slowly


def lift_down():  # Move the fork down
    global stop_signal
    stop_signal = False
    angle = servo.get_angle()
    while not stop_signal:
        if angle > 50:  # Set a lower limit to the motor’s rotation
            angle-= 1  # Decrease the angle 1 degree at a time
            myservo.syncRotateServos(20, (servo, angle))


def stop_lifting():  # Stop the fork’s movement
    global stop_signal
    stop_signal = True  # Setting this value to True will make the while loops running in other threads (lines 24 and 34) end
```

Finally, add code that runs the three functions defined above to the `main()` function to complete the program!

#### Example Code 3-2-1 (forklift.py)
###### New Code (Line 3, Lines 58-63)
```python=1
import vehicle
import myservo
import _thread  # Import the multithreading module

robo = vehicle.VehicleRobot(pin_l="M2", pin_r="M1")
servo = myservo.MyServomotor("P13")
stop_signal = False

OPERATIONS = {
    "moveForward",
    "moveBackward",
    "rotateLeft",
    "rotateRight",
    "brake",
    "liftUp",
    "liftDown",
    "liftStop",
}


def lift_up():
    global stop_signal
    stop_signal = False
    angle = servo.get_angle()
    while not stop_signal:
        if angle < 150:
            angle += 1
            myservo.syncRotateServos(20, (servo, angle))
            

def lift_down():
    global stop_signal
    stop_signal = False
    angle = servo.get_angle()
    while not stop_signal:
        if angle > 50:
            angle -= 1
            myservo.syncRotateServos(20, (servo, angle))


def stop_lifting():
    global stop_signal
    stop_signal = True
    

def main(operation):
    if operation in OPERATIONS:
        if operation == "moveForward":
            robo.move_forward()
        elif operation == "moveBackward":
            robo.move_backward()
        elif operation == "rotateLeft":
            robo.rotate_left()
        elif operation == "rotateRight":
            robo.rotate_right()
        elif operation == "brake":
            robo.brake()
        elif operation == "liftUp":  # Raise the fork
            _thread.start_new_thread(lift_up, ())  # Start a new thread to run the function
        elif operation == "liftDown":  # Lower the fork
            _thread.start_new_thread(lift_down, ())  # Start a new thread to run the function
        elif operation == "liftStop":  # Stop the fork
            stop_lifting()  # Run this function as-is
        return "'{}' succeeded.".format(operation), "text/plain"
    else:
        return "Failed. '{}' is invalid.".format(operation), "text/plain"
```

Now your program is finished! Save the file and transfer it to the Core Unit from Mu Editor’s **File** menu, overwriting the file you transferred before.

**After pressing the Core Unit’s Reset button**, you can start running the web server program. The web server program will turn your Core Unit into a wi-fi access point. Connect your smartphone or tablet to that access point, then open its browser and go to the URL below to start sending request to your forklift!

```
http://192.168.4.1/forklift.html
```

When you tap one of the orange buttons shown on the control panel page, the forklift should raise and lower its fork as that button commands it!

### Using Your Robot Forklift

Once you’ve made sure all the functions of your forklift really work, try using your online control panel to complete block-moving challenges with your robot forklift!

Before you try these challenges, use Mu Editor’s **Transfer** option to transfer the web server program to the Core Unit.

###### ★ When you run the web server program from the Mu Editor’s **Run** option, communications with the PC via REPL will slow down your program, preventing your robot from running smoothly.

Put the blocks together as shown.

##### Block Shapes
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-3/12-3_7_E.png"/>

You can move these blocks with your robot forklift like this:

##### Block Moving Procedure
1. Lower the fork
2. Drive forward, pushing the block into the fork’s slot
3. Lift the fork

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-3/12-3_8_E.png"/>

Once you’ve gotten used to picking up blocks with your forklift, try picking up three scattered blocks and stacking them on top of each other!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-3/12-3_9_E.png"/>

## Challenge: Slider Controls

For this lesson’s challenge, let’s change the on-screen control panel for your robot’s fork from buttons into a slider. We have another prepared client-side program to use for this challenge, but you’ll need to write the server-side program yourself!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-3/12-3_10_E.png"/>

### Setting Up the Client-Side Program

Copy and paste the code below into Mu Editor, then save it with the name **forklift_slider.html**. **Save this file with the file type `Other(*.*)`!** After that, select **Files** in the Mu Editor menu to transfer your saved file to the Core Unit.

##### Example Code 4-1-1 (forklift_slider.html)
```html=1
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>Forklift controller</title>
<style>
	body{
		width: 100%;
		height: 100%;
		margin: auto;
		font-family: Tahoma, Geneva, "sans-serif";
	}
	div#main{
		position: absolute;
		width: 100%;
		height: 100%;
		
	}
	#main button{
		position: absolute;
		display: block;
		width:20%;
		height:20%;
		color: #ffffff;
		font-size:2em;
		border: none;
		border-radius: 5px;
		transform: translateY(-50%) translateX(-50%);
		-webkit- transform: translateY(-50%) translateX(-50%);
	}
	button.vehicle{
		background-color: #036EB8;
	}
	button.lift{
		background-color: #F39800;
	}
	button#moveForward{
		left:25%;
		top:33%;
	}
	button#moveBackward{
		left:25%;
		top:66%;
	}
	button#rotateLeft{
		left:50%;
		top:33%;
	}
	button#rotateRight{
		left:50%;
		top:66%;
	}
	input#slider{
		-webkit-appearance: none;
		appearance: none;
		outline: none;
		position: absolute;
		display: block;	
		width: calc(100vh*3/4);
		height: 3em;
		left: 80%;
		top: 50%;
		background-color: #cccccc;
  		border-radius: 1.5em;
		-webkit- transform: translateY(-50%) translateX(-50%) rotate(270deg);
		transform: translateY(-50%) translateX(-50%) rotate(270deg);
	}
	input#slider::-webkit-slider-thumb {
		-webkit-appearance: none;
		background-color: #2EA7E0;
		width: 3em;
		height: 3em;
		border: solid 2px #ffffff;
		border-radius: 50%;
		box-shadow: 0px 3px 6px 0px rgba(0, 0, 0, 0.15);
	}
	input#slider::-moz-range-thumb {
		background-color: #2EA7E0;
		width: 3em;
		height: 3em;
		border: solid 2px #ffffff;
		border-radius: 50%;
		box-shadow: 0px 3px 6px 0px rgba(0, 0, 0, 0.15);
		border: none;
	}
	
</style>
</head>
<body>
	<div id="main">
		<button id="moveForward" class="button vehicle" type="button">Move<br>forward</button>
		<button id="moveBackward" class="button vehicle" type="button">Move<br>backward</button>
		<button id="rotateLeft" class="button vehicle" type="button">Rotate<br>left</button>
		<button id="rotateRight" class="button vehicle" type="button">Rotate<br>right</button>
		<input id="slider" type="range" value="90" min="50" max="150" step="1">
	</div>
</body>
<script>
	window.onload = function(){
		const req = new XMLHttpRequest();
		const buttonElms = document.getElementsByClassName("button");
		const slider = document.getElementById("slider");
		
		req.onreadystatechange = function(){
			if(this.readyState == 4){
				if(this.satus == 200){
					console.log(req.responseText);
				}
			}
		};
		
		function httpRequest(evt){
			evt.preventDefault();
	
			let elm = evt.target;
			let evtType = evt.type;		
			let id = elm.id;
			let className = elm.className;
			let operation;
			
			if(evtType=="mousedown" || evtType=="touchstart"){
				operation = id;
			}
			else if(className.indexOf("vehicle")){
				operation = "brake";
			}
			else{
				operation = null;
			}
			
			if (operation){
				req.open("GET", `forklift_slider.py?operation='${operation}'`, true);
				req.send(null);	
			}
		};
		
		for(let i=0; i < buttonElms.length; i++){
			buttonElms[i].addEventListener("mousedown", httpRequest, false);
			buttonElms[i].addEventListener("touchstart", httpRequest, false);
			buttonElms[i].addEventListener("mouseup", httpRequest, false);
			buttonElms[i].addEventListener("touchend", httpRequest, false);
		}
		
		slider.addEventListener("input", (evt)=>{
			let elm = evt.target;
			let val = elm.value;
			req.open("GET", `forklift_slider.py?operation='lift'&angle=${val}`, true);
			req.send(null);
		});
	};
</script>
</html>
```

### Programming Tips

The program above sends requests when adjust the slider shown in the browser. These requests include this request line:

```
GET /forklift.py?operation=%27lift%27&angle=90 HTTP/1.1
```

When you look at the query parameters here (`operation=%27lift%27&angle=90`), you’ll see that `operation` is set to `lift`, and there’s a new parameter here called `angle`. This means that when the `operation` parameter is set to `"lift"` in order to move the fork, the `angle` parameter will set the Servomotor’s angle. When `operation` isn’t set to `"lift"`, there will be no `angle` parameter.

Add a section to the part of the program that controls the forklift car (shown below) that sets the Servomotor’s angle to match the `angle` parameter when `operation` is set to `"lift"`, and you’ll have a complete program!

**Make sure to save the completed program as `forklift_slider` (extension .py), then transfer it to the Core Unit from Mu Editor’s File menu.**

##### Example Code 4-2-1

```python=1
import vehicle
import myservo

robo = vehicle.VehicleRobot(pin_l="M2", pin_r="M1")
servo = myservo.MyServomotor("P13")

OPERATIONS = {
    "moveForward",
    "moveBackward",
    "rotateLeft",
    "rotateRight",
    "brake",
}


def main(operation):
    if operation in OPERATIONS:
        if operation == "moveForward":
            robo.move_forward()
        elif operation == "moveBackward":
            robo.move_backward()
        elif operation == "rotateLeft":
            robo.rotate_left()
        elif operation == "rotateRight":
            robo.rotate_right()
        elif operation == "brake":
            robo.brake()
        return "'{}' succeeded.".format(operation), "text/plain"
    else:
        return "Failed. '{}' is invalid.".format(operation), "text/plain"
```

### Example Program Changes

First you need to add `"lift"` to the `OPERATIONS` set to make it a valid value for the `operation` parameter.

###### New Code (Line 13)
```python=7
OPERATIONS = {
    "moveForward",
    "moveBackward",
    "rotateLeft",
    "rotateRight",
    "brake",
    "lift",  # New parameter value for controlling the fork
}
```

Next we need to add `angle` to the `main()` function’s arguments. We won’t give this argument a value except when the `operation` is "lift"`, so we need to give it a default value of `None` to stop it from causing errors when you run other operations.

###### Changed Code (Line 17)
```python=17
def main(operation, angle=None):
```

Last, you need to add code to the `main()` function that moves the Servomotor to the specified angle. Now your program is finished!

##### Example Code 4-3-1
###### New Code (Lines 29-30)
```python=1
import vehicle
import myservo

robo = vehicle.VehicleRobot(pin_l="M2", pin_r="M1")
servo = myservo.MyServomotor("P13")

OPERATIONS = {
    "moveForward",
    "moveBackward",
    "rotateLeft",
    "rotateRight",
    "brake",
    "lift",
}

 
def main(operation, angle=None):
    if operation in OPERATIONS:
        if operation == "moveForward":
            robo.move_forward()
        elif operation == "moveBackward":
            robo.move_backward()
        elif operation == "rotateLeft":
            robo.rotate_left()
        elif operation == "rotateRight":
            robo.rotate_right()
        elif operation == "brake":
            robo.brake()
        elif operation == "lift" and angle is not None:  # New code
            servo.set_angle(angle)  # New code
        return "'{}' succeeded.".format(operation), "text/plain"
    else:
        return "Failed. '{}' is invalid.".format(operation), "text/plain"
```

Save your finished program as **forklift_slider(.py)**, then transfer it to the Core Unit from Mu Editor’s **File** menu. When the transfer is complete, you can start running the web server program **after pressing the Core Unit’s Reset button**. The web server program will turn your Core Unit into a wi-fi access point. Connect your smartphone or tablet to that access point, then open its browser and go to the URL below to start sending requests to your forklift!

```
http://192.168.4.1/forklift_slider.html
```

## Conclusion
### A Quick Review
In this lesson you used what you learned in Topics 12-1 and 12-2 to build a robot forklift your can control from a smartphone or tablet’s browser using Wi-Fi HTTP signals. If you adjust your browser’s so control panel and the type of data it sends for requests and responses, you can make this program control any kind of robot you want!

In this project we had devices communication over a local network (LAN), but if you connect via a wide area network (WAN) instead, you can send signals to your robot from anywhere on earth and collect all kinds of information!

Using a network like this requires significant knowledge of networks and security, so for those of you interested in a future career in engineering, we recommend pursuing a programming course for adults at a college or other learning center to learn more. 

### Coming Up Next Time!
In the next lesson we’ll be finishing Topic 12! For our final lesson, we’ll teach you how to install open source MicroPython modules and packages from the internet onto your Core Unit. We’ll install a number of actual modules so you can see how it works for yourself!
