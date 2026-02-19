---
tags: Python_English, Topic 12
---

Python Robotics Course Lesson 46

Topic 12-2
Remote Data Gathering with Sensors
===

Use your web browser to control your Core Unit from a distance!

## In This Lesson...

In the previous lesson we used the Core Unit as a wireless access point and explained how to make a program which turned it into a web server! We then made programs in Python which let us see how we can send HTML files in response to client requests or retrieve sensor values and return the results!

In this lesson we'll learn how to take requests sent from a client and use them to control the Core Unit's onboard LED display and buzzer as well as Servomotors through the Robot Expansion Unit.

We'll change a portion of the web server program from the previous lesson to make the following three applications:

1. An app which scrolls any message you type in the browser across the LED display
2. An app which lets you adjust sliders in the browser to control Servomotors remotely
3. An app which lets play the buzzer by tapping a keyboard in the browser

###### ★ You’ll be programming the third application in the challenge at the end of the lesson!

<iframe src="https://player.vimeo.com/video/485332425" width="640" height="564" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

:::info

Lesson Introduction

■ Lesson 46 Contents
1. Modifying the Program to Make a Web Server
2. Creating an App to Control the LED Display
3. Creating an App to Control Servomotors
4. Challenge: Creating an App to Control the Buzzer


In this lesson we’ll be introducing a technique to control the Core Unit wirelessly via HTTP using the browser on your own tablet or smartphone!

We’ll start the lesson by modifying the program from the previous lesson which turned our Core Unit into a web server. These modifications will enable applications running on the web server to take data sent using the GET method in HTTP!

Next, we’ll make an application which takes any text string you type into the browser and scrolls it across the Core Unit’s LED display!


★ The applications we’ll be using in this lesson have both a client and server side, but we’ll only be coding the server-side of each one here!

After this, we’ll make an application which lets you move sliders in the web browser to control two Servomotors connected to the Robot Expansion Unit.



To finish, we’ll take what we’ve learned so far and take on the challenge of programming an application which lets you tap or click a piano in the browser to play different notes from your Core Unit’s buzzer!



★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

This is everything we’ll be learning in this topic. Now let's start the lesson!

:::

## Turning Your Core Unit Into a Web Server

In this topic we’ll be turning the web server program we made in the previous lesson into a server which can run web applications by adding code which can do the following two things:

1. Retrieve **query parameters** from URLs sent using the GET method by a client
2. Pass the query parameters as arguments to run in the `main()` function of the requested program file

The **query parameters** in the first part are pieces of data a client can add to a URL to send data other than the path to a web server. You can add a query parameter to any URL by adding a `?` at the end and following it with the the parameter name and value formatted as `parameter=value`. If you want to add multiple parameters, just put a `&` between them!

##### A URL with a Query Parameter
```
http://192.168.4.1/example.py?param1=10&param2=abc&...
```

The client will send the data needed to control the LED display and Servomotors as query parameters! In order to run the program using this data, we’ll need to add the second part of the code.

Now let’s take these changes and add them to our web server program in Example Code 4-2-1!

### Changing the Program

We’ll need to make changes to the `make_response()` function. This starts by splitting any URL containing query parameters into two parts containing the file path and the parameters.

We can easily determine if a URL contains query parameters by checking if the URL contains a `?`. We can slice and string which contains this character into two parts coming before and after the `?` to get the file path and the parameters.

###### New and Changed Code (Lines 72-76)
```python=56
def make_response(req):
    method_name = req[:req.find(" ")]

    if method_name != "GET":
        status = "HTTP/1.1 405 Method Not Allowed"
        body = "The {} method is not supported on this server.".format(method_name)
        header = {
            "Content-Encoding": "identity",
            "Content-Length": str(len(body.encode("utf-8"))),
            "Content-Type": "text/plain; charset=UTF-8",
            "Server": "MicroPython"
        }
    else:
        req_line = req[:req.find("\n")]
        url = req_line[req_line.find(" ")+1:req_line.rfind(" ")]
        path = url.replace("http://192.168.4.1", "")
        if "?" in path:  # URLs with ? Include query parameters
            params = path[path.find("?")+1:]  # The query parameter after ?
            path = path[:path.find("?")]  # Finds the ?. File path comes before this
        else:  # If there are no query parameters
            params = None  # Adds None for comparison with URLs including query parameters
        try:
            file = open(path, "rt")
```

Next we’ll make changes to the code which runs the requested program file. Any query parameters present will be passed to the argument in the `main()` function of the program file we imported as a module. This means that we’ll need to convert these parameters into a format which can be used in a function’s argument! We can convert these by associating the names of the parameters with the names of function’s arguments. Last, we can separate each argument by converting the `&` which separates each parameter into a `,`!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-2/12-2_1_E.png"/>

While we can use any parameter values which are numbers as-is, we'll need to do one more operation for values which are text strings. In order to make the text strings inside of the parameter fit the function's argument, we'll need to wrap the string in a `'` single quotation marks. The only problem is that you can't use single quotation marks in a URL! Due to this any requests sent with a URL use something called **percent-encoding** to convert special characters into a symbol which starts with `%`. An encoded single quotation mark is `%27`.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-2/12-2_2_E.png"/>

This means that the program for our web server will have to turn these back into single quotation marks! Let’s add the code shown below in order to do this:

###### New and Changed Code (Lines 93-97)

```python=77
        try:
            file = open(path, "rt")
        except OSError:
            status = "HTTP/1.1 404 Not Found"
            body = "The requested document was not found on this server."
            header = {
                "Content-Encoding": "identity",
                "Content-Length": str(len(body.encode("utf-8"))),
                "Content-Type": "text/plain; charset=UTF-8",
                "Server": "MicroPython"
            }
        else:
            status = "HTTP/1.1 200 OK"
            if ".py" in path:
                file.close()
                module = modules[path]
                if params:  # If there are query parameters
                    args = params.replace("&", ",").replace("%27", "'")  # Converts query parameters into argument format
                    body, mimetype = eval("{}.main({})".format(module, args))  # Passes arguments and runs the main() function
                else:  # If there are no query parameters
                    body, mimetype = eval("{}.main()".format(module))  # Passes arguments and runs the main() function
            else:
                body = file.read()
                file.close()
                mimetype = "text/html"
            header = {
                "Content-Encoding": "identity",
                "Content-Length": str(len(body.encode("utf-8"))),
                "Content-Type": "{}; charset=UTF-8".format(mimetype),
                "Server": "MicroPython"
            }
```

And now we’ve made the necessary changes to the web server program. Keep in mind that we’ll be using this program in the next lesson. Go ahead and save it with the name of your choice!

In the next section we’ll move on to making programs for applications which control the Core Unit’s LED display and Servomotors!

## Creating an App to Control the LED Display

The application we’ll make in this section takes the message you type into a form on the web page and scrolls it across the Core Unit’s LED display when you press the Send button!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-2/12-2_3_E.png"/>

In order for our application to work, we’ll need to make two separate programs: one for the client’s browser and the other for the web server.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-2/12-2_4_E.png"/>

### Making the Client Program

We’ve already made the client program in advance, which you can find below. Copy and paste this source code into Mu Editor and save it with the name **control_display.html**. **Make sure you save it with the `Other(*.*)` file type! **

Now click the **Files** button in Mu Editor and transfer the file you’ve just saved to your Core Unit.

##### Example Code 3-1-1 (`control_display.html`)
```html=1
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>Display controller</title>
</head>
<style>
	body{
		width: 100%;
		height: 100%;
		font-family: YuGothic, "Yu Gothic medium", "Hiragino Sans", Meiryo, "sans-serif";
	}
	form#myform{
		margin: 2em 1em;
	}
	input#message{
		display: inline-block;
		width: 10em;
		padding: 0.5em;
		font-size: 1.2em;
	}
	form#myform button[type="submit"]{
		display: inline-block;
		padding: 0.5em 1em;
		color: #ffffff;
		background-color: #22AC38;
		border: none;
		border-radius: 5px;
		font-size: 1.2em;
	}
</style>
<body>
	<form id="myform" method="get" action="control_display.py">
		<input id="message" type="text" value="" maxlength="10">
		<button type="submit">send</button><br>
		<output id="result"></output>
	</form>
</body>
<script>
	window.onload = function(){
		const req = new XMLHttpRequest();
		const $myform = document.getElementById("myform");
		const $message = document.getElementById("message");
		const $result = document.getElementById("result");
				
		req.onreadystatechange = function(){
			if(this.readyState == 4){
				if(this.status == 200){
					$result.innerHTML = this.responseText;
				}
			}
		};
		
		$myform.onsubmit =function(evt){
			evt.preventDefault();			
			let val = $message.value;
			let url = this.action + "?message='" + val + "'";
			req.open("GET", url, true);
			req.send(null);
		}
	};
</script>
</html>

````

This program sends the a request like the one below to the Core Unit when you click the **Update** button in the browser:

```
GET /control_display.py?message=%27Hello%27 HTTP/1.1
Host: 192.168.4.1
Connection: keep-alive
Accept: */*
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 13_3_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.5 Mobile/15E148 Safari/604.1
Accept-Language: ja-jp
Referrer: http://192.168.4.1/control_display.html
Accept-Encoding: gzip, deflate
```

Pay attention to the URL in the request line on line one: `/control_display.py?message=%27Hello%27`. This query parameter in this URL is `message=%27Hello%27`. The name of the parameter is `message`, and it's value is wrapped in two `%27` symbols, which are percent-encoded quotation marks.

The web server program we changed in the previous section will convert this into `message='Hello'` and pass it as an argument to the `main()` function.

### Making the Web Server Program

The server-side program will scroll the message it receives across the LED display and return a message showing the result. It will also make the message the client gets back from the server appear in the browser.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-2/12-2_5_E.png"/>

Now let’s make a new program in Mu and add the following code to it:

##### Example Code 3-2-1 (`control_display.py`)
```python=1
from pystubit.board import display

def main(message):  # Makes argument names match parameters
    display.scroll(message, delay=50, wait=False, color=(31, 31, 31))  # Scrolls the message in white
    return "Received your message '{}'.".format(message), "text/plain"  # Returns that the message was received
```

Let’s save the program we’ve just made with the name **control_display.py**. Make sure to type this name correctly to make sure the application works as intended!

Now click the **Files** button in Mu Editor and transfer the file you’ve just saved to your Core Unit.

### Registering and Testing the App on the Web Server

Now let’s see how our application is supposed to work! We’ll do this by adding code to the beginning of the web server’s program which registers the application:


###### New Code (Line 4, Line 11)
```python=1
import network
import usocket
import monitor
import control_display  # Imports the program file we made as a module

SSID = "studuinobit"
PASSWORD = "Artecrobo2"

modules = {
    "/monitor.py": "monitor",
    "/control_display.py" : "control_display",  # Binds the path of the program file to the module name
}
```

Now we'll run the application and use our device to connect to the Core Unit's wireless access point. Once you're connected, open the browser and use the URL below to send a request:

```
http://192.168.4.1/control_display.html
```

You'll see the same screen as above it it's working correctly. Now type a message of 10 characters or less and send it to your Core Unit!

## Creating an App to Control Servomotors

In this section we'll make an application which can control Servomotors connected to the Robot Expansion Unit by using two on-screen sliders.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-2/12-2_6_E.png"/>

### Connecting Servomotors to the Robot Expansion Unit

Start by plugging your Core Unit into the Robot Expansion Unit. Make sure the Battery Box is plugged in! Next, plug two Servomotors into **P13** and **P14** on the Expansion Unit and plug them into a beam to keep them in place!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-2/12-2_7_E.png"/>

### Making the Client Program

We’ll be using the source below for our client program. Copy and paste this source code into Mu Editor and save it with the name **control_servo.html**. **Make sure you save it with the `Other(*.*)` file type! **

Now click the **Files** button in Mu Editor and transfer the file you’ve just saved to your Core Unit.

##### Example Code 4-2-1 (`control_servo.html`)
```html=1
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Controlling Servomotors</title>
</head>
<style>
	html{
		font-family: YuGothic, "Yu Gothic medium", "Hiragino Sans", Meiryo, "sans-serif";
	}
	body{
		width:100%;
		height:100%;
		margin: auto;
	}
	div#main{
		width: 100%;
		height: 100%;
	}
	div.slider{
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		margin-top: calc(100vh*1/4);
	}
	div.slider label{
		display: inline-block;
		font-size: 1em;
		padding: 0 1em;
		font-weight: bold;
		color: #ffffff;
		background-color: #2EA7E0;
	}
	div.slider input{
		-webkit-appearance: none;
		appearance: none;
		outline: none;
		display: inline-block;	
		width: calc(100vw*3/4);
		height: 3em;
		margin: 0 0.5em;
		background-color: #cccccc;
  		border-radius: 1.5em;
	}
	div.slider input::-webkit-slider-thumb {
		-webkit-appearance: none;
		background-color: #2EA7E0;
		width: 3em;
		height: 3em;
		border: solid 2px #ffffff;
		border-radius: 50%;
		box-shadow: 0px 3px 6px 0px rgba(0, 0, 0, 0.15);
	}
	div.slider input::-moz-range-thumb {
		background-color: #2EA7E0;
		width: 3em;
		height: 3em;
		border: solid 2px #ffffff;
		border-radius: 50%;
		box-shadow: 0px 3px 6px 0px rgba(0, 0, 0, 0.15);
		border: none;
	}
	div.slider output{
		display: inline-block;
		font-size: 1em;
		width: 6em;
	}
</style>
<body>
	<div id="main">
		<div class="slider">
			<label>P13</label>
			<input id="p13in" class="slider-in" type="range" value="90" min="0" max="180" step="1">
			<output id="p13out">90deg</output>
		</div>
		<div class="slider">
			<label>P14</label>
			<input id="p14in" type="range" value="90" min="0" max="180" step="1">
			<output id="p14out">90deg</output>
		</div>
	</div>
</body>
<script>
	window.onload = function(){
		const req = new XMLHttpRequest();
		const p13In = document.getElementById("p13in");
		const p13Out = document.getElementById("p13out");
		const p14In = document.getElementById("p14in");
		const p14Out = document.getElementById("p14out");
				
		req.onreadystatechange = function(){
			if(this.readyState == 4){
				if(this.satus == 200){
					console.log(req.responseText);
				}
			}
		};
		
		p13In.oninput = function(){
			let val = this.value;
			p13Out.value = val + "deg";
			req.open("GET", "control_servo.py?pin='p13'&angle=" + val, true);
			req.send(null);
		}
		p14In.oninput = function(){
			let val = this.value;
			p14Out.value = val + "deg";
			req.open("GET", "control_servo.py?pin='p14'&angle=" + val, true);
			req.send(null);
		}		
	};
</script>
</html>

````

This program sends the a request like the one below to the Core Unit when you click the **Update** button in the browser:

```
GET /control_servo.py?pin=%27p13%27&angle=120 HTTP/1.1
Host: 192.168.4.1
Connection: keep-alive
Accept: */*
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 13_3_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.5 Mobile/15E148 Safari/604.1
Accept-Language: ja-jp
Referrer: http://192.168.4.1/control_servo.html
Accept-Encoding: gzip, deflate
```

Pay attention to the URL in the request line on line one: ``pin=%27p13%27&angle=120`. This URL has two set parameters. The first parameter, `pin`, sets the port for the Servomotor we wish to control. The second parameter, `angle`, decides which angle the Servomotor should rotate to. Let’s move on to using these two pieces of data to make the server-side program which controls our Servomotors!

### Making the Web Server Program

In our new program we're doing to define a `main()` functions which takes two arguments named `pin` and `angle` which match the parameters in the request. Let's write the code which uses `pin` to select a Servomotor before rotating it to the angle in `angle` below:


##### Example Code 4-3-1 (`control_servo.py`)
```python=1
from pyatcrobo2.parts import Servomotor

servo_p13 = Servomotor("P13")  # Creates an object to control the Servomotor
servo_p14 = Servomotor("P14")
servo_p13.set_angle(90)  # Sets starting Servomotor angle to 90 degrees
servo_p14.set_angle(90)  

def main(pin, angle):
    if pin == "p13":  # Controls the Servomotor on P13
        servo_p13.set_angle(angle)
    else:  # Controls the Servomotor on P14
        servo_p14.set_angle(angle)
    return "OK", "text/plain"  # Returns message showing success
```

Let’s save the program we’ve just made with the name **control_servo.py**. Now click the **Files** button in Mu Editor and transfer the file you’ve just saved to your Core Unit.

### Registering and Testing the App on the Web Server

Now let’s see how our application is supposed to work! We’ll do this by adding code to the beginning of the web server’s program which registers the application:


###### New Code (Line 5, Line 13)
```python=1
import network
import usocket
import monitor
import control_display
import control_servo  # Imports the program file we made as a module

SSID = "studuinobit"
PASSWORD = "Artecrobo2"

modules = {
    "/monitor.py": "monitor",
    "/control_display.py" : "control_display",
    "/control_servo.py" : "control_servo",  # Binds the path of the program mile to the module name
}
```

Run this program **after pressing the Reset button on your Core Unit**! Next, connect your device to the Core Unit’s wireless access point! Once you're connected, open the browser and use the URL below to send a request:

```
http://192.168.4.1/control_servo.html
```

Now adjust the two sliders on the screen to see if your Servomotors rotate to the set angle!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-2/12-2_8_E.png"/>

## Challenge: Creating an App to Control the Buzzer
In this final challenge, we’ll make an application which lets you tap or click a keyboard in the browser to play notes on the Core Unit’s buzzer!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-2/12-2_9_E.png"/>

### Making the Client Program

We’ll be using the source below for our client program. Copy and paste this source code into Mu Editor and save it with the name **control_buzzer.html**. **Make sure you save it with the `Other(*.*)` file type! **

Now click the **Files** button in Mu Editor and transfer the file you’ve just saved to your Core Unit.

##### Example Code 5-1-1 (`control_buzzer.html`)

```html=1
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Controlling the Buzzer</title>
</head>
<style>
	body{
		width: 100%;
		height: 100%;
	}
	div#keyboard{
		position: absolute;
		width: 100%;
		height: 100%;
	}
	div#blackKeys{
		position: absolute;
		width: 90%;
		height: 45%;
		left: 5%;
		top: 5%;
	}
	div#whiteKeys{
		position: absolute;
		width: 90%;
		height: 80%;
		left: 5%;
		top: 5%;
	}	
	div.white-key{
		position: absolute;
		display: block;
		width: 12%;
		height: 100%;
		background-color: #ffffff;
		border: 2px solid #666666;
	}
	div.black-key{
		position: absolute;
		display: block;
		width: 8%;
		height: 100%;
		background-color: #000000;
		border: 2px solid #666666;
	}
	div.white-key:active, div.black-key:active{
		background-color:darkorange;
	}
	div.white-key:nth-child(1){
		left:0;
	}
	div.white-key:nth-child(2){
		left:12%;
	}
	div.white-key:nth-child(3){
		left:24%;
	}
	div.white-key:nth-child(4){
		left:36%;
	}
	div.white-key:nth-child(5){
		left:48%;
	}
	div.white-key:nth-child(6){
		left:60%;
	}
	div.white-key:nth-child(7){
		left:72%;
	}
	div.white-key:nth-child(8){
		left:84%;
	}
	div.black-key:nth-child(1){
		left: 8%;
	}
	div.black-key:nth-child(2){
		left: 20%;
	}
	div.black-key:nth-child(3){
		left: 44%;
	}
	div.black-key:nth-child(4){
		left: 56%;
	}
	div.black-key:nth-child(5){
		left: 68%;
	}

</style>
<body>
	<div id="keyboard">
		<div id="whiteKeys">
			<div class="key white-key" my-tone="C4"></div>
			<div class="key white-key" my-tone="D4"></div>
			<div class="key white-key" my-tone="E4"></div>
			<div class="key white-key" my-tone="F4"></div>
			<div class="key white-key" my-tone="G4"></div>
			<div class="key white-key" my-tone="A4"></div>
			<div class="key white-key" my-tone="B4"></div>
			<div class="key white-key" my-tone="C5"></div>
		</div>
		<div id="blackKeys">
			<div class="key black-key" my-tone="CS4"></div>
			<div class="key black-key" my-tone="DS4"></div>
			<div class="key black-key" my-tone="FS4"></div>
			<div class="key black-key" my-tone="GS4"></div>
			<div class="key black-key" my-tone="AS4"></div>
		</div>
	</div>
</body>
<script>
	window.onload = function(){
		const req = new XMLHttpRequest();
		const $keys = document.getElementsByClassName("key");
		
		function httpRequest(evt){
			evt.preventDefault();
			let evtType = evt.type;
			let tone = this.getAttribute("my-tone")
			
			if(evtType=="mousedown" || evtType=="touchstart"){
				status = "on";
			}else{
				status = "off";
			}
			req.open("GET", `control_buzzer.py?tone='${tone}'&status='${status}'`, true);
			req.send(null);	
		}
		
		req.onreadystatechange = function(){
			if(this.readyState == 4){
				if(this.status == 200){
					console.log(this.responseText);
				}
			}
		};
		
		for(let i=0; i<$keys.length; i++){
			$keys[i].addEventListener("mousedown", httpRequest,false);
			$keys[i].addEventListener("touchstart", httpRequest, false);
			$keys[i].addEventListener("mouseup", httpRequest, false);
			$keys[i].addEventListener("touchend", httpRequest, false);
		}
	};
</script>
</html>
````

This program sends a request with the following request line when you tap or click the on-screen keyboard.


<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-2/12-2_10_E.png"/>

```
GET /control_buzzer.py?tone=%27C4%27&status=%27on%27 HTTP/1.1
```

Keep and eye on the two parameters in the URL: `tone` controls the tone of the note, while `status` tells whether the note should play or not. `tone` will take the value of notes like `C4` and `D4`, and we can set a text string of `on` or `off` to set the value of `status`!

The program will also send a request when you release the key, setting the `tone` parameter to the same note and the `status` parameter to `off`.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-2/12-2_11_E.png"/>

```
GET /control_buzzer.py?tone=%27C4%27&status=%27off%27 HTTP/1.1
```

### Making the Web Server Program and Registering the Application

Try using the hints above to make the program for the web server by yourself! Save the program with the name `control_buzzer.py` and use Mu Editor to transfer it to your Core Unit. Don't forget to register the application by adding the code below to the web server's program!

If you're not sure how to make the program, try referencing Example Code 5-2-1 below!

###### New Code (Line 6, Line 15)
```python=1
import network
import usocket
import monitor
import control_display
import control_servo
import control_buzzer  # Add this

SSID = "studuinobit"
PASSWORD = "Artecrobo2"

modules = {
    "/monitor.py": "monitor",
    "/control_display.py" : "control_display",
    "/control_servo.py" : "control_servo",
    "/control_buzzer.py" : "control_buzzer"  # Add this
}
```

##### Example Code 5-2-1 (`control_buzzer.py`)

```python=1
from pystubit.board import buzzer

def main(tone, status):
    if status == "off":
        buzzer.off()
    else:
        buzzer.on(tone)
    return "OK", "text/plain"
```


## Conclusion
### A Quick Review
We spent the first half of this lesson adding changes to the web server program we made in the previous lesson. These changes made three apps which we could use to control the Core Unit from a web browser! While we won't cover it in this course, mastering the **JavaScript** used in the client-side program as well as the **HTML** and **CSS** used to place elements on the screen will allow you to make much more impressive applications of your own! If that sounds interesting to you, try studying these once you're done with this course!

### Coming Up Next Time!
In the next lesson we’ll be making a new robot which uses a forklift as inspiration. We’ll try taking what we’ve learned here to control it wirelessly by using a tablet or smartphone!