---
tags: Python_English, Topic 12
---

Python Robotics Course Lesson 45

Topic 12-1
Making a Web Server
===

Turn your Core Unit into a web server!

## In This Lesson...

In this lesson we’ll be learning how to use our Core Unit as a wireless access point for a LAN as we turn it into a web server! Clients connected to the local network will send requests to our server, and we’ll see if we can get the server to send HTML files in response!

In the second half of the lesson we’ll make an application which you can use the monitor the values of your Core Unit’s onboard sensors, then run it on our web server!

:::info

Lesson Introduction

■ Lesson 45 Contents
1. Turning the Core Unit into a Wireless Access Point 
2. Programming a Web Server
3. Programming a Sensor Monitor App
4. Challenge: Making a Live Web Page


In Topic 12 we’re going to turn your Core Unit into a web server and use it to communicate in HTTP with the browser on your smartphone or tablet!

This lesson starts with learning how to make a program which turns your Core Unit into a wireless access point. Doing this lets us build a LAN which we can use a smartphone or tablet to connect to!

Next, we’ll learn how to program a simple web server. This server will use sockets to take HTTP GET requests from clients and send text and HTML data as a response! We’ll also make a web page which you can actually load on your device’s web browser!



Well then add a feature to our server which can run an application which takes client requests and retrieves the current values of your Core Unit’s onboard sensors. When the client receives these values they’ll be shown as text inside of the browser!



Last, we’ll take on the challenge of modifying our application to insert the sensor values into a live web page so you can view them in real time! 



This is everything we’ll be learning in this topic. Now let's start the lesson!

:::

## Turning the Core Unit into a Wireless Access Point

In Topic 11 we connected your Core Unit to a nearby wireless access point as a client in order to retrieve time and weather forecasts from the Internet. However, you can also turn your Core Unit into a wireless access point to make your own **LAN**, or **L**ocal **A**rea **N**etwork!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_1_E.png"/>


### Programming a Wireless Access Point

In order to use Wi-Fi with your Core Unit we need to use the `network` module’s `WLAN` class. You can use the `network.STA_IF` constructor argument to make a `WLAN` class object as a station or the `network.AP_IF` argument to make it into an access point!

```
network.WLAN(network.STA_IF)  # Creates a WLAN object as a station
network.WLAN(network.AP_IF)  # Creates a WLAN object as an access point
```

When you want to make it into an access point, you have to set an SSID and password for it just like any other wireless router. You can set these in the arguments of the `WLAN` class’s `config()` method before running it.

##### The `config()` Method Arguments
|Argument|Data Type|What is it?|
|:---|:---|:---|
|`essid`|Text string|The name of the access point (SSID)|
|`password`|Text string|The password to the acess point|
|`authmode`|Numerical value|The authentication mode|
|`channel`|Numerical value|The Wi-Fi channel you wish to use|
|`hidden`|Booleon|Shows or hides `essid`|

You can use the following numbers in `authmode` to set the authentication mode. **WEP** is the weakest authentication mode and **WPA2-PSK** is the strongest, with **WPA-PSK** in between. Aside from **0: No authentication**, all of these modes require you to set a password in `password`!

* 0：No authentication
* 1：WEP
* 2：WPA-PSK
* 3：WPA2-PSK
* 4：WPA／WPA2-PSK

You can set `channel` to any channel from **1** to **13**. In order to prevent interference between different Wi-Fi signals, the 2.4 Ghz and 5 GHz bands given reserved for Wi-Fi connections are divided up even further into channels. Your Core Unit uses the IEEE802.11g standard when it acts as a wireless access point. This standard uses **13 channels** on the 2.4 Ghz band. Each of these channels has 11 MHz of bandwidth on either side of its central frequency, making for a total bandwidth of 22 MHz!

###### ★ 2.4 GHz = 2400 MHz
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_2_E.png"/>

`hidden` controls whether the SSID of the access point is visible or hidden. Setting this to `True` means that the access point won’t show up on your device’s list of available wireless access points. 

Now let’s take what we’ve learned so far and set the following arguments as we write a program which turns the Core Unit into a wireless access point! We’ll put our code into a function called `launch_ap()`.

##### Setting the `config()` Method Arguments

###### Make sure to use different SSIDs for each Core Unit you’re using as a wireless access point.

|Argument|Setting|
|:---|:---|
|`essid`|`"coreunit"` or any other text string you like|
|`essid`|`"Artecrobo2"` or any other text string you like|
|`Authmode`|`4` (WPA2-PSK)|
|`channel`|`11`|
|`hidden`|`False`|

##### Access Point Example Code

```python=1
import network

SSID = "coreunit"  # The SSID
PASSWORD = "Artecrobo2"  # The password

def launch_ap(ssid, password):  
    wlan = network.WLAN(network.AP_IF)  # Creates a WLAN object as an access point
    wlan.config(essid=ssid, password=password, authmode=4, channel=11, hidden=False)  # Sets the arguments
    wlan.active(True)  # Enables the network interface
    return wlan  # Returns the WLAN object we created
    
        
wlan = launch_ap(SSID, PASSWORD)  # Starts the access point with the specified SSID and password
```

Run this program and you should see the SSID of the access point appear in the list of available access points in your device’s Wi-Fi menu.

###### ★ Screenshots taken on iPhone.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_3_E.png"/>

Choose the Core Unit’s SSID and type the password in order to connect.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_4_E.png"/>

Your device will let you know when you’ve connected successfully!
###### ★ If you’re getting an unstable connection while using a smartphone, it may help to turn off mobile data!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_5_E.png"/>

You’ll see the following message in the Mu Editor terminal if a device is connected.

<pre class="prettyprint">
I (6644591) wifi: new:&lt;1,0&gt;, old:&lt;1,0&gt;, ap:&lt;1,1&gt;, sta:&lt;255,255&gt;, prof:1
I (6644591) wifi: station: 34:08:bc:7b:09:53 join, AID=1, bgn, 20
I (6644611) network: event 15
I (6644771) tcpip_adapter: softAP assign IP to station,IP is: 192.168.4.2
I (6644781) network: event 17
</pre>

The `192.168.4.2` at the end of the message `I (6644771) tcpip_adapter: softAP assign IP to station,IP is: 192.168.4.2` on line 4 is the IP address assigned to the connected device! The protocol used to automatically assign information to a device connected to a LAN is known as the DHCP, or the **D**ynamic **H**ost **C**onfiguration **P**rotocol.

## Programming a Web Server

We’ve already made our Core Unit into an access point, so let’s try turning it into a web server! Let’s start by taking a look at everything this server needs to do.

##### The Processes of the Web Server

1. The Core Unit will open a socket which allows a client to access the server.
2. The client-side device will open its browser and use a URL to send an HTTP request to the Core Unit.
3. The Core Unit will respond to the request by sending HTML data to the client.
4. The client’s web browser will show the HTML data it receives.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_6_E.png"/>

We’ve already made a web page as an example of the HTML data you’ll see in the browser. The data for this page will be saved on your Core Unit!

##### The Core Unit Web Page

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_7_E.gif"/>

### What Web Servers Do

In real life web servers have all sorts of things they can do in response to requests. This includes sending applications, images, and videos in addition to performing user authentication! The program required to do all of this would be incredibly complex, so we’ll be turning your Core Unit into a simple web server with the following barebones features:

##### The Core Unit Server Features

1. Take GET requests and return text data from the file specified in the URL.
2. Return a 405 status code for any other methods which tells the user that the method is not supported on the server.
3. If the requested file doesn’t exist on the server, it will return a 404 status code to notify the user that the file can’t be found.

### Making the Program

Now let’s continue adding onto our program from the last section. We’ll need to write the code for the following in order:

1. Set the address info and open a socket
2. Accept the connection from the client
3. Receive the request from the client
4. Create a response to send to the client
5. Send the response to the client

#### ■ Setting Address Info and Opening a Socket

We’ll put the processes which launch the web server into a function called `launch_webserver()`. This program will start by running the `WLAN` class’s `ifconfig()` method with the set arguments to retrieve the address we need in order to open the socket.

###### New Code (Lines 13-15)
```python=1
import network

SSID = "studuinobit"
PASSWORD = "Artecrobo2"

def launch_ap(ssid, password):
    wlan = network.WLAN(network.AP_IF)
    wlan.config(essid=ssid, password=password, authmode=4, channel=11, hidden=False)
    wlan.active(True)
    return wlan


def launch_webserver(wlan):  # This function contains the processes which act as a web server
    ip_addr = wlan.ifconfig()[0]  # Retrieves the IP address from the WLAN object
    print(ip_addr)  # Shows the IP address
    
    
wlan = launch_ap(SSID, PASSWORD)
```

In order to use sockets, we need to import the `usocket` module. We can retrieve the address info for the socket by running this module’s `getaddrinfo()` method. We’ll also use the `socket()` method to create a socket object and use the `bind()` method to bind the address info to the socket. Last, we’ll use the `listen()` method to accept the connection from a client. This method can take the number of connections you set in its argument, but for now let’s set this to **1** to make it refuse any connections besides your own!

###### New Code (Line 2, Lines 20-23)
```python=1
import network
import usocket
```
```python=7
def launch_webserver(wlan):
    ip_addr = wlan.ifconfig()[0]
    print(ip_addr)

    sock_addr = usocket.getaddrinfo(ip_addr, 80)[0][-1]  # Retrieves the address data for the socket
    sock = usocket.socket(usocket.AF_INET, usocket.SOCK_STREAM)  # Creates a IPv4, TCP/IP socket object
    sock.bind(sock_addr)  # Binds the address data to the socket
    sock.listen(1)  # Sets the number of available connections. Refuses any new connections over this number.
```

#### ■ Accepting Client Connections

Next we’ll write the code which allows our server to accept a client’s connection.

We can do this by running the `accept()` method of the socket object we just created. When this method accepts a connection it returns a new socket object as well as the IP address used to communicate with the client. Once you’re done communicating, we’ll close connection by using the `close()` method! We’ll wrap this part of the program in a `while True:` loop to keep accepting connections.

###### New Code (Lines 23-27)
```python=14
def launch_webserver(wlan):
    ip_addr = wlan.ifconfig()[0]
    print(ip_addr)

    sock_addr = usocket.getaddrinfo(ip_addr, 80)[0][-1]
    sock = usocket.socket(usocket.AF_INET, usocket.SOCK_STREAM)
    sock.bind(sock_addr)
    sock.listen(1)

    while True:
        conn, addr = sock.accept()  # Accepts the connection. This returns a tuple which includes a new socket object as well as the IP address used to communicate with the client
        print("A client connected from {}.".format(addr))  # Shows the client IP address for debugging
        # The socket communication process will go here.
        conn.close()  # Closes the connection
```

Now let’s see how our program works! We’ll put a line at the end of the program which runs the function we’ve just made.

###### New Code (Line 30)
```python=29
wlan = launch_ap(SSID, PASSWORD)
launch_webserver(wlan)  # Runs the function we just made
```

Run this program only **after pressing the Reset button on your Core Unit**!

#### Program Results
<pre class="prettyprint">
I (5810) phy: phy_version: 4007, 9c6b43b, Jan 11 2019, 16:45:07, 0, 0
192.168.4.1
</pre>

The string of numbers you see in the terminal is your Core Unit’s IP address.

Now let’s continue by following the steps from earlier to connect to the Core Unit access point and open the browser on your device.

Once it’s open, type the following URL into the address bar:

```
http://192.168.4.1
```

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_8_E.png"/>

Doing this allows us to send an HTTP request to the server at **192.168.4.1**. Access the URL above from your browser and you’ll see the following message in your terminal notifying you of a successful socket connection:

#### Program Results
<pre class="prettyprint">
A client connected from ('192.168.4.2', 54720).
A client connected from ('192.168.4.2', 54721).
A client connected from ('192.168.4.2', 54722).
</pre>

Since we’re getting no response from the Core Unit’s server despite being continuously connected to the browser, the browser will automatically resend the same request over and over again!

#### Accepting Client Requests

Now that we’ve confirmed that the socket is connected, we’ll use that same socket to accept the request data sent by the client.

We’ll need to use the socket object’s `read()` and `recv()` methods in order to accept this data.

##### Receiving Data with Socket Object Methods

|Method|What does it do?|
|:---|:---|
|`recv(bufsize)`|Receives the maximum amount of data set in `bufsize` from the socket. This method returns a byte string t showing the received data. |
|`recv(bufsize)`|Just like `recv()`, this receives the maximum amount of data set in `bufsize` from the socket. This method returns a tuple containing a byte string showing the received data as well as the connected IP address. |
|`read(size)`|Loads only the number of bytes set in `size` from the socket. This method returns a byte string showing the received data. If `size` is not set, this method will load all data from the socket until the end. |
|`readinto(buf,[,nbytes])`|Stores the byte string loaded from the socket in `buf`. Set `nbyte` to limit the data to the number of bytes in the argument. |
|`readline()`|Loads one line ending in a line break from the socket. This method returns a byte string showing the line it received. |

We’ll use the `recv()` method here to receive 100 bytes of data at a time from the socket.

We’ll put the process which retrieves the request data from the socket inside of a function called `get_request()`. This function will take the socket object which established the connection as an argument.

When we run the `recv()` method here, the program wait until it receives the data before moving onto the next step. This means that if the connection drops in the middle of the receiving the data the program will go into standby.

At this point the program wouldn’t be able to accept the next client connection, so let’s use the `settimeout()` method to cut the process automatically after a certain amount of time! We’ll set the time limit to three seconds here.

The program will throw an `OSError` exception once this time limit is up! The program will stop this function as soon as this exception is caught.

Now let’s use what we’ve learned so far to write the following code:

###### New Code (Lines 29-42)
```python=14
def launch_webserver(wlan):
    ip_addr = wlan.ifconfig()[0]
    print(ip_addr)

    sock_addr = usocket.getaddrinfo(ip_addr, 80)[0][-1]
    sock = usocket.socket(usocket.AF_INET, usocket.SOCK_STREAM)
    sock.bind(sock_addr)
    sock.listen(1)

    while True:
        conn, addr = sock.accept()
        print("A client connected from {}.".format(addr))
        conn.close()


def get_request(conn):
    conn.settimeout(3.0)  # Sets time limit to 3 seconds
    req = bytes()  # Byte string object which stores the request
    try:
        while True: # Receives data in 100-byte packets
            data = conn.recv(100)  # Receives 100 bytes of data
            req += data  # Appends received data to the end
            if len(data) < 100:  # Determines end of the data has been reached and stops the
                break  # loop if data is less than 100 bytes
    except OSError:  # If exception is thrown
        print("Time out.")
        return False  # Returns False to show failure
    else:  # If no exception is thrown
        return req.decode("utf-8")  # Returns decoding from byte string into text string
```

Now let’s try running the program to see if we receive the client request! Add the code to run the `get_request()` function inside of the `launch_webserver()` function as shown below!

###### New Code (Line 26-27)
```python=14
def launch_webserver(wlan):
    ip_addr = wlan.ifconfig()[0]
    print(ip_addr)

    sock_addr = usocket.getaddrinfo(ip_addr, 80)[0][-1]
    sock = usocket.socket(usocket.AF_INET, usocket.SOCK_STREAM)
    sock.bind(sock_addr)
    sock.listen(1)

    while True:
        conn, addr = sock.accept()
        print("A client connected from {}.".format(addr))
        req = get_request(conn)  # Runs the defined function to retrieve the request data
        print(req)  # Shows request data for debugging        
        conn.close()
```

Now let’s **press the Reset button** on the Core Unit before we run this program again. We’ll follow the same steps before to connect to the Core Unit and type `http://192.168.4.1` into the browser’s address bar to send the request. You’ll see the message below in the terminal when the Core Unit receives the request:

#### Program Results

<pre class="prettyprint">
GET / HTTP/1.1
Host: 192.168.4.1
Upgrade-Insecure-Requests: 1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 12_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.1 Mobile/15E148 Safari/604.1
Accept-Language: ja-jp
Accept-Encoding: gzip, deflate
Connection: keep-alive
</pre>

In previous topics we explained that HTTP requests have the following structure:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_9_E.png"/>

In our example above, the request line the `GET / HTTP/1.1` part on the first line. The request line contains the name of the method, the path and/or URL, and the HTTP version, each separated by a blank space.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_10_E.png"/>

The header starts on line 2. Look at this table to see what each header in the example means!

|Header|What does it mean?|
|:---|:---|
|Host|Specifies the destination server name and port number of the request. Leaving out the port name will set the default port number for the requested service (port 80 for HTTP or port 443 for HTTPS). |
|Upgrade-Insecure-Requests|If the specified HTTP URL has a secure HTTPs alternative, the request will be sent to the server as if the client specified an HTTPS URL. |
|Accept|Tells the server the **MIME type**, which is the type of content the client can accept. `text/html`, for example, would mean an HTML file. |
|User-Agent|Tells the server the client’s browser information, including the type and version. |
|Accept-Language|Tells the server which languages the browser can accept. |
|Accept-Encoding|Tells the server which encoding methods the client can decode. |
|Connection|Controls whether the connection is kept open after the current transaction (the process from sending a request to receiving a response) finishes. `keep-alive` means that the connection should be kept open. |

GET method requests have no body. Both the POST and PUT methods have bodies.

The web server will use the data it receives to prepare the data it will send to the client in response.

#### ■ Creating a Response

Now let’s write the code which will make a response to send to the client. As an example, let’s make this code send the following simple web page.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_11_E.png"/>

We’ll start by saving the page source as an HTML file on the Core Unit. **Click the New button in Mu Editor and copy and paste the following code in order to do this! **

###### ★ The source below is written in three languages: HTML, CSS, and JavaScript. We won’t be studying these in this course, so we’ll skip the explanation for what these mean!

```html=1
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>Studuino:bit</title>
</head>
<!-- CSS -->
<style>
	body{
		width:100%;
		height:100%;
		background-color: #000000;
		font-family: Verdana, Geneva, "sans-serif";
	}
	h1#title{
		font-size: 2em;
		line-height: 3em;
		margin-top: 2em;
		text-align: center;
		color: #ffffff;
	}
</style>
<!-- end -->
<!-- HTML -->
<body>
	<h1 id="title">Welcome to Studuino:bit World!</h1>
</body>
<!-- end -->
<!-- JavaScript -->
<script>
	window.onload = function(){
		let elm_title = document.getElementById("title")
		let text = "Welcome to Studuino:bit World!";
		let count = 0;
		
		function scroll(){
			count += 1
			elm_title.innerHTML = text.slice(0, count);
			if (count != text.length){
				setTimeout(scroll, 250);
			}else{
				count = 0;
				setTimeout(scroll, 2000);
			} 
			
		}
		scroll();
	}
</script>
<!-- end -->
</html>
```

Next let’s save the code we just pasted. Click the **Save** button in Mu Editor. In the window that appears, type the name `index.html` and choose `Other(*.*)` as the file type. Keep in mind that choosing `Python(*.py)` as the file type will save this file as a Python program!

Choose the `mu_code` folder as a save location and click the `Save (S)` button to save the file.

###### ★ The HTML file extension is **.html**!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_12_E.png"/>

Now let’s transfer the file we just saved to our Core Unit. Click the **Files** button in Mu Editor and transfer `index.html` from your computer to the Core Unit.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_13_E.png"/>

The client will use the following URL in order to request the HTML file:

`http://192.168.4.1/index.html`

Now let’s write code which will include the data inside of the file in the response to requests from this URL! We’ll put this process inside of a function called `make_response()`

This function will take the request data as an argument. The first thing we’ll check in the request data is what sort of method it’s using!

The name of the method is contained in the request line, which is the first line of the request. Since every piece of data on the request line is separated by a space, we can get the method name by extracting a text string of everything up until the first space!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_10_E.png"/>

We can use slicing with the text object `find()` method in order to do this. Specifiy the text string you wish to search for in the `find()` method’s argument and it will return the first hit that it finds. This means that we can make it look for the first space by using `_str[:_str.find(" ")]`, and our program can extract everything that comes before that space!

###### New Code (Lines 47-48)
```python=31
def get_request(conn):
    conn.settimeout(3.0)
    req = bytes()
    try:
        while True:
            data = conn.recv(100)
            req += data
            if len(data) < 100:
                break
    except OSError:
        print("Time out.")
        return False
    else:
        return req.decode("utf-8")


def make_response(req):
    method_name = req[:req.find(" ")]
```

This web server will take requests sent using the GET method.

###### ★ Actual web servers have to take HEAD requests in addition to GET requests.

If a request is sent using any other method, the server will return `405 Method Not Allowed` to notify the user that this method is not supported.

Responses are comprised of a status line which includes the above status code as well as headers and a body.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_14_E.png"/>
We’ll add the information we need for the header here.

|Header|What is it?|
|:---|:---|:---|
|`Content-Encoding`|The encoding method used when sending compressed data. |
|`Content-Length`|The length in bytes of the content stored in the body. |
|`Content-Type`|The MIME type of the content as well as the character encoding. |
|`Server`|The server’s software version. |

```
Content-Encoding:identity
Content-Length: The actual length of the body
Content-Type: text/plain; charset=UTF-8
Server: MicroPython
```

Giving `Content-Encoding` a value of `identity` means that the content hasn’t been compressed. The value of `Content-Length` is set by converting the body into bytes and looking up its length inside of the program. A `Content-Type` value of `text/plain` means that the content is plain text. Since our Core Unit is running MicroPython, we’ll set the value of `Server` to `MicroPython` here.

We’ll also include a message inside of the response body which will let the user know if their request method isn’t supported!

Now let’s take the above items and write code for them. This information will be stored inside of three variables called `status`, `body`, and `header` before concatenating them in the last step. We’ll also define our header as a dictionary!

###### New Code (Lines 50-59)
```python=47
def make_response(req):
    method_name = req[:req.find(" ")]

    if method_name != "GET":  # If not using the GET method
        status = "HTTP/1.1 405 Method Not Allowed"  # Sets status line to 405
        body = "{} method is unacceptable on this server.".format(method_name)  # Shows message that this method is not supported in the body of the response
        header = {
            "Content-Encoding": "identity",  # The enconding method for the content
            "Content-Length": str(len(body.encode("utf-8"))),  # Length of the body of the response
            "Content-Type": "text/plain; charset=UTF-8",  # Content and character set of the response body
            "Server": "MicroPython",  # Server software information
    else:  # Below shows the process when using the GET method
        pass
```

Let’s continue by writing code for when our server receives a GET method request. Since GET methods are used to request the file at the URL or path in the request line, the server will send the file as long as it exists on the server. If the file doesn’t exist, it will send a `404 Not Found` status code.

We’ll start by writing the code which extracts the URL from the request data as shown below. This code starts by extracting the first line of the data, which is the request line. It then uses the `find()` and `rfind()` methods to find the first and second spaces in the line and take out the data we need. Unlike the `find()` method, `rfind()` begins its search at the end of the line!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_15_E.png"/>

##### New Code (Lines 60-67)
```python=47
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
        req_line = req[:req.find("\n")]  # Extracts the request line
        url = req_line[req_line.find(" ")+1:req_line.rfind(" ")]  # Extracts the URL
```

URLs can be shown both as an absolute path starting with `http://` or as a relative path which includes everything after the host name. Since we want to retrieve a URL written with the relative path, we’ll go ahead and remove the text string with the absolute path shown as `http://192.168.4.1`!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_16_E.png"/>

```python=59
    else:
        req_line = req[:req.find("\n")]
        url = req_line[req_line.find(" ")+1:req_line.rfind(" ")] 
        path = url.replace("http://192.168.4.1", "")
```

Next we’ll check to see if the file exists on the Core Unit in the specified path. We’ll use exception handling in order to do this. Running the code which attempts to open a file at a set path inside of a `try` statement will throw an `OSError` exception if the file doesn’t exist. We can then use an `except` statement to catch this!

###### New and Changed Code (Lines 60-67)
```python=59
    else:
        req_line = req[:req.find("\n")]
        url = req_line[req_line.find(" ")+1:req_line.rfind(" ")]
        path = url.replace("http://192.168.4.1", "")
        try:
            file = open(path, "rt")
        except OSError:
            # Put the process for when the file doesn\’t exist here
        else:
            # Put the process for when the file exists here
```

If the file doesn’t exist, the server will send a `404 Not Found` status code and a message saying that the file isn’t there. If the file is found, the text inside of the file will be stored in the body of the response and a `200 OK` status code will be sent showing that the file was successfully found! Now let’s keep the fact that we’ll have to store different content inside of the body for each case as we write the following code:

###### New Code (Lines 66-83)
```python=59
    else:
        req_line = req[:req.find("\n")]
        url = req_line[req_line.find(" ")+1:req_line.rfind(" ")]
        path = url.replace("http://192.168.4.1", "")
        try:
            file = open(path, "rt")
        except OSError:
            status = "HTTP/1.1 404 Not Found"  # Status code for if the file doesn\’t exist
            body = "The requested document was not found on this server."  # Message to show when the file doesn\’t exist
            header = {
                "Content-Encoding": "identity",
                "Content-Length": str(len(body.encode("utf-8"))),
                "Content-Type": "text/plain; charset=UTF-8",  # This shows text content
                "Server": "MicroPython"
            }
        else:
            status = "HTTP/1.1 200 OK"  # Status code when successful
            body = file.read()  # Body contains the text inside of the file
            file.close()  # Don\’t forget to close the file!
            header = {
                "Content-Encoding": "identity",
                "Content-Length": str(len(body.encode("utf-8"))),
                "Content-Type": "text/html; charset=UTF-8",  # This shows HTML content
                "Server": "MicroPython"
            }
```

Since we've prepared responses for a case where a non-GET request is sent, a case where a GET request is sent but the file doesn't exist, and a case where a GET request is sent and the file is found, the only thing left to do is concatenate the status line, headers, and body of each case! Remember that there needs to be a blank space between the header and the body of the response. Since we'll need to byte stringify this data, let's use the `encode()` method to encode it in `utf-8`!

###### New Code (Lines 85-90)
```python=47
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
            body = file.read()
            file.close()
            header = {
                "Content-Encoding": "identity",
                "Content-Length": str(len(body.encode("utf-8"))),
                "Content-Type": "text/html; charset=UTF-8",
                "Server": "MicroPython"
            }
    
    res = status + "\n"  # Concatenates status line
    for key, val in header.items():  # Concatenates header
        res += "{}: {}\n".format(key, val)  # Converts from a dictionary to text with headers
    res += "\n" + body  # Concatenates body
    res = res.encode("utf-8")  # Byte string
    return res  # Returns the generated response data
```

#### ■ Sending a Response to the Client

It’s finally time to send our client a response. We’ll can use the socket object’s `send()` and `write()` methods in order to send this data to the client.

##### Sending Data with Socket Object Methods

|Method|What does it do?|
|:---|:---|
|`send(bytes)`|Sends the byte string set in `bytes` to the socket. This method returns the number of bytes which were successfully sent. This method, however, may not be able to send a piece of data which is too large. In this case, the return value will be smaller than the actual size of the data. |
|`sendall(bytes)`|Sends the entire byte string set in `bytes` to the socket. Unlike `send()`, this method will break a larger piece of data into packets and send each one back-to-back to send all of the data! |
|`write(bytes)`|Just like `sendall()` this method attempts to send the entire byte string set in `bytes` to the socket, but this doesn’t guarantee it sends all of the data. This method returns the amount of data it was able to send to the socket. |

While there are subtle differences between each of these methods, they can all be used to send data! In our program we’re going to try sending data by using the `sendall()` method. Let’s add the following code to our `launch_webserver` function!

##### New Code (Lines 31-34)
```python=16
def launch_webserver(wlan):
    def launch_webserver(wlan):
    ip_addr = wlan.ifconfig()[0]
    print(ip_addr)

    sock_addr = usocket.getaddrinfo(ip_addr, 80)[0][-1]
    sock = usocket.socket(usocket.AF_INET, usocket.SOCK_STREAM)
    sock.bind(sock_addr)
    sock.listen(1)

    while True:
        conn, addr = sock.accept()
        print("A client connected from {}.".format(addr))
        req = get_request(conn)
        print(req)
        if req:  # If request was successfully sent
            res = make_response(req)  # Creates the response data
            print(res)  # Shows response in the terminal for debugging
            conn.sendall(res)  # Uses socket to send the response
        conn.close()
```

Now let’s see how our program works!

**Press the Reset button** on the Core Unit before we run this program. Connect to the Core Unit access point and type `http://192.168.4.1/index.html` into the browser’s address bar to send the request. The browser should load the page you see here:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_7_E.gif"/>

Now try sending a request with an incorrect URL like `http://192.168.4.1/sample.html` and you should see the following page appear in the browser:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_17_E.png"/>

## Programming a Sensor Monitor App

In the previous section we programmed a web server which could take client requests and send them a file as a response. We created this file in advance and stored it on the server!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_18_E.png"/>

Actual servers can not only send files, they can also run requested programs and return the files created by that program as a response!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_19_E.png"/>

In this section we’re going to modify our program to make it retrieve the values of our Core Unit’s onboard A button, B button, light sensor, and temperature sensor and send those values as a response!

### Retrieving the Onboard Sensor Values

Aside from the program which runs the web server, we’ll also have to make a program with functions that retrieve the values of the four onboard sensors, then returns them as a text list with the list’s MIME type.

Click the **New** button in Mu Editor and write the code below into your new file!

##### Example Code 4-1-1
```python=1
from pystubit.board import button_a, button_b, lightsensor, temperature

def main():  # Name this function “main”
    text = "Button A: {}, Button B: {}, LightSensor: {}, Temperature: {}"  # This text shows a table of values for the onboard sensors
    but_a_val = button_a.get_value()  # Gets button A value
    but_b_val = button_b.get_value()  # Gets button B value
    light_val = lightsensor.get_value()  # Gets Light Sensor value
    temp_val = temperature.get_celsius()  # Gets Temperature Sensor value
    text = text.format(but_a_val, but_b_val, light_val, temp_val)  # Adds the values for each sensor and button to the text string we defined on line 4
    return text, "text/plain"  # Returns generated text and MIME type to the caller
```

Let’s save this program with the name **monitor.py**. Now click the **Files** button in Mu Editor and transfer the file to your Core Unit.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_20_E.png"/>

We’ll import this program file into our web server’s program in advance and run the `main()` function when the client requests it!

### Changing the Web Server Program
We’ll need to add the following code to allow a client to ask the server to run the program!

Let’s start by importing the **monitor.py** module on the Core Unit at the beginning of the program. We’ll also need to define a dictionary which finds the file URL to the name of the module which we’ve just imported!

###### New Code (Line 3, Lines 8-10)
```python=1
import network
import usocket
import monitor  # Imports the program file we transferred

SSID = "studuinobit"
PASSWORD = "Artecrobo2"
# Binds the filename to the name of the imported module
modules = {
    "/monitor.py": "monitor"  # URL: Module Name
}
```

Now let’s change our `make_response()` function!

If the file exists, our program will run different processes depending on the type of the file. If the file is a Python program file with a .py extension, we’ll retrieve the name of the associated module from the dictionary we just made and use the `eval()` function to run the program’s `main()` function. We’ll then put the result in the body of the response!

A file with an .html extension is an HTML file. In this case, the text we load from the file will be included in the body of the response.

###### New and Changed Code (Lines 83-97)
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
            status = "HTTP/1.1 200 OK"  # Status line is share regardless of file type
            if ".py" in path:  # If the file extension is .py
                file.close()  # Closes file if it doesn\’t need to be read
                module = modules[path]  # Retrieves associated file name
                body, mimetype = eval("{}.main()".format(module))  # Interprets the text string as code with the eval() function to run it and returns result
            else:  # If the file extension is anything other than .py \(.html here\)
                body = file.read()  # Puts the file\’s text data into the body
                file.close()  # Closes the file
                mimetype = "text/html"  # MIME type of the HTML file
            header = {  # MIME type depends on the content of the response body
                "Content-Encoding": "identity",
                "Content-Length": str(len(body.encode("utf-8"))),
                "Content-Type": "{}; charset=UTF-8".format(mimetype),  # Inserts MIME type
                "Server": "MicroPython"
            }
            
    res = status + "\n"
    for key, val in header.items():
        res += "{}: {}\n".format(key, val)
    res += "\n" + body
    res = res.encode("utf-8")
    return res
```

And now we’ve finished changing our program! Let’s run it to see how it works.

**Press the Reset button** on the Core Unit before we run this program. Connect to the Core Unit access point and type `http://192.168.4.1/monitor.py` into the browser’s address bar to send the request. You should see the values of each sensor or button in the browser.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_21_E.png"/>

Now save the program with the name of your choice. We’ll be using this one in future lessons!

##### Example Code 4-2-1
```python=1
import network
import usocket
import monitor

SSID = "studuinobit"
PASSWORD = "Artecrobo2"

modules = {
    "/monitor.py": "monitor"
}

def launch_ap(ssid, password):
    wlan = network.WLAN(network.AP_IF)
    wlan.config(essid=ssid, password=password, authmode=4, channel=11, hidden=False)
    wlan.active(True)
    return wlan


def launch_webserver(wlan):
    ip_addr = wlan.ifconfig()[0]
    print(ip_addr)

    sock_addr = usocket.getaddrinfo(ip_addr, 80)[0][-1]
    sock = usocket.socket(usocket.AF_INET, usocket.SOCK_STREAM)
    sock.bind(sock_addr)
    sock.listen(1)

    while True:
        conn, addr = sock.accept()
        print("A client connected from {}.".format(addr))
        req = get_request(conn)
        print(req)
        if req:
            res = make_response(req)
            print(res)
            conn.sendall(res)
        conn.close()


def get_request(conn):
    conn.settimeout(3.0)
    req = bytes()
    try:
        while True:
            data = conn.recv(100)
            req += data
            if len(data) < 100:
                break
    except OSError:
        print("Time out.")
        return False
    else:
        return req.decode("utf-8")


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
                body, mimetype = eval("{}.main()".format(module))
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
            
    res = status + "\n"
    for key, val in header.items():
        res += "{}: {}\n".format(key, val)
    res += "\n" + body
    res = res.encode("utf-8")
    return res


wlan = launch_ap(SSID, PASSWORD)
launch_webserver(wlan)
```

## Challenge: Making a Live Web Page

In the last section we sent the values of your Core Unit’s sensors and buttons as plain text, but if we send these inside of an HTML file we organize them into a table which lets the client view them in real time!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_22_E.png"/>

Here we're going to take on the challenge of of modifying our code to replace the text in a template HTML file with the sensor and button values then send that file to the client!

### The HTML File Template

The source for our HTML file template is shown below. Click the **New** button in Mu Editor and paste the source into the new file. Save this file as `base.html` with a file type of `Other(*.*)`!

```html=1
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width , initial-scale=1">
<title>Monitoring sensors</title>
<style>
	body{
		width: 100%;
		height: 100%;
		margin-right: auto;
		margin-right: left;
		font-family: Tahoma, Geneva, "sans-serif";
	}
	div#main{
		position: absolute;
		width: 100%;
		height: 100%;	
	}
	#main h1{
		font-size: 2em;
		margin: 0.5em;
	}
	#main table{
		display: table;
		border: solid 2px #666666;
		border-collapse: collapse;
		margin: 1em;
	}
	#main table td{
		font-size: 1.2em;
		padding:0.5em 1em;
		border
	}
	#main table thead tr{
		background-color: #666666;
	}
	#main table thead td{
		color: #ffffff;
	}
	#main table tbody tr:nth-child(odd){
		background-color: #eeeeee;
	}
	#main table tbody tr:nth-child(even){
		background-color: #ffffff;
	}
	#main a{
		display: inline-block;
		margin: 1em;
	}
	#main a button{
		font-size:1.2em;
		padding: 0.5em 1em;
		color: #ffffff;
		background-color: #22AC38; 		
		border: none;
		border-radius: 5px;
	}
</style>
</head>

<body>
	<div id="main">
		<h1>Sensor values</h1>
		<table>
			<thead>
				<tr><td>Name</td><td>Value</td></tr>
			</thead>
<!-- The text strings wrapped in {} below will be replaced with retrieved sensor values -->
				<tr><td>Button A</td><td>{val_buttonA}</td></tr>
				<tr><td>Button B</td><td>{val_buttonB}</td></tr>
				<tr><td>Light Sensor</td><td>{val_lightsensor}</td></tr>
				<tr><td>Temperature</td><td>{val_temp}</td></tr>
<!--End-->
			<tbody>
			</tbody>
		</table>
		<a href=""><button>Update</button></a>
	</div>
</body>
</html>
```

Try opening this file in your web browser and you’ll see the following screen:


<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_23_E.png"/>

The parts of the file which look like `{val_buttonA}` or `{val_buttonB}` will be replaced by the sensor values we retrieve! If you look at the HTML source, you’ll see these parts on lines 70-73. In HTML, `<>` tags determine the layout of the document, and the section which starts with `<table>` on line 65 and ends with `</table>` on line 77 define a table which will be shown in the browser!

```html=62
<body>
	<div id="main">
		<h1>Sensor values</h1>
		<table>
			<thead>
				<tr><td>Name</td><td>Value</td></tr>
			</thead>
<!-- The text strings wrapped in {} below will be replaced with retrieved values -->
				<tr><td>Button A</td><td>{val_buttonA}</td></tr>
				<tr><td>Button B</td><td>{val_buttonB}</td></tr>
				<tr><td>Light Sensor</td><td>{val_lightsensor}</td></tr>
				<tr><td>Temperature</td><td>{val_temp}</td></tr>
<!--End-->
			<tbody>
			</tbody>
		</table>
		<a href=""><button>Update</button></a>
	</div>
</body>
```

Now that we’ve made our template HTML file, let’s save it with the name `template.html`.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_24_E.png"/>

### Modifying the Program

Now let’s modify our program to replace the parts in the HTML file we’ve just saved and send the file to the client. We’ll be modifying our `monitor.py` program here.

#### ■ Changing `monitor.py`

We made `monitor.py` in the last section, and we’ll need to make it do the following three things:

1. Open `template.html` and load the text.
2. Once the text is loaded, use the text string `replace()` method to replace {val_buttonA}`, `{val_buttonB}`, `{val_lightsensor}`, and `{val_temp}` with the retrieved values.
3. Return the replaced text as well as an `html/text` MIME type to the caller.

Use the hints here to write this code yourself! If you can’t figure out the steps, try following along with the example below!

#### ■ A Modified Program

Open up `monitor.py` on your PC and change the following places:

##### New and Changed Code (Lines 5-7, Lines 14-17, Line 19)
```python=1
from pystubit.board import button_a, button_b, lightsensor, temperature

def main():
    # Opens and loads text from base HTML file
    with open("base.html", "rt") as file:  # Opens in read-only text mode
        text = file.read()
        file.close()
    # Retrieves the current value of each sensor. This code doesn\’t need to change.
    but_a_val = button_a.get_value()
    but_b_val = button_b.get_value()
    light_val = lightsensor.get_value()
    temp_val = temperature.get_celsius()
    # Replaces a portion of of the text loaded from the HTML with the sensor values retrieved above
    text = text.replace("{val_buttonA}", str(but_a_val))  #　Note that we need to convert the numerical sensor values into text strings!
    text = text.replace("{val_buttonB}", str(but_b_val))
    text = text.replace("{val_lightsensor}", str(light_val))
    text = text.replace("{val_temp}", str(temp_val))
    # Returns the text and MIME type after being rewritten as callers
    return text, "text/html"  # Changes MIME to text/html
```

Now save your program and transfer it to your Core Unit to overwrite the old file!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_25_E.png"/>

To test out how it works, **press the Reset button on your Core Unit** to run the program for your web server. Connect to the Core Unit access point and type `http://192.168.4.1/monitor.py` into the browser’s address bar to send the request.

Click or touch the Update button in the browser to send the request to the URL one more time and refresh the values!

###### ★ If you can’t see the button, try viewing the web page in portrait mode!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-1/12-1_26_E.png"/>

## Conclusion
### A Quick Review
In this lesson we learned how to turn the Core Unit into a wireless access point and explained how to make a program which turned it into a web server!

Saving an HTML file to the Core Unit allowed us to connect to the access point and use a smartphone or tablet to send a request to open the page. We also took on the challenge of running a program in response to a client request to show the values of the Core Unit’s buttons and sensors in the browser!

In Topic 11 we learned that you can both connect your Core Unit to a network as a client to retrieve data from the Internet as well as use it as a web server to send and monitor sensor data requested by clients as we did in this lesson. Making real IoT projects requires knowledge of both of these things, and the concepts we’ll be learning in the next topic will help further your understanding even more!

### Coming Up Next Time!
In the next lesson we’ll be using the Core Unit as a web server again as we make a program which lets us control the Core Unit remotely by using buttons and sliders on a web page!