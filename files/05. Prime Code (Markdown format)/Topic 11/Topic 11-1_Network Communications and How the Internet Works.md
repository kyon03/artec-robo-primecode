---
tags: Python_English, Topic 11
---

Python Robotics Course Lesson 41

Topic 11-1
Network Communications and How the Internet Works
===

Connect your Core Unit to the World Wide Web!

## In This Lesson...

In this lesson we’re going to learn how to connect the Core Unit to the Internet and retrieve information from a web page. We’ll also take a simple look at how the Internet became so popular as well as how it works!

::: info

Lesson Introduction

■ Lesson 41 Contents
1. The World of the Internet 
2. Connecting to the Internet Through Wi-Fi
3. Retrieving Data from a Webpage
4. Using Retrieved Data to Control the Display
5. Challenge: Controlling the Buzzer


In Topic 11 we’re going to use the Core Unit to connect to the Internet.
We’ll then take on the challenge of using the data we retrieve online to control robots!

This lesson starts by giving a simple introduction to the Internet. As we explore exactly how the Internet became so popular as well as how it’s put together, we’ll touch on the concepts that you need to know as you study in Topic 11.

Next we’ll learn how to use Wi-Fi to get our Core Units connected to the Internet! Make sure to check whether you have a wireless access point available to use!

After this, we’ll learn about the communication protocols used to retrieve data from a web page as well as the MicroPython module we need to do it.

As practice, we’ll retrieve the RGB values for different colors from a web page and program the Core Unit’s LED display to light up according to those colors.

Last, we’ll take on the challenge of retrieving the note data for a song from a table on a web page and program the buzzer on the Core Unit to play each note in order!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!

:::

## The World of the Internet

The Internet is a massive network which connects devices all around the world through different communication lines. Using the Internet allows you to access the information you want anywhere at any time!

The recent debut of the **5G** communications standard has enabled faster speeds, more devices to be connected simultaneously, and improved latency. This has led to high expectations of making the **Internet of Things** a reality, bringing previously offline cars and home appliances onto the Internet!

### The Birth of the Web and the Spread of the Internet

The rapid spread of the Internet was made possible by the development of the hypertext system called the **World Wide Web** (or just the Web) by Tim Berners-Lee, a computer engineer at CERN, the European Organization for Nuclear Research. Hypertext is a sort of document which can use **hyperlinks** to reference and be referenced by other documents. You may also knows this as a website!

Tim Berners-Lee is also the developer behind the **HTML (Hypertext Markup Language)**, a descriptive language for hyper text, the **HTTP (Hypertext Transfer Protocol)** which allows us to send hypertext, and the **URL (Uniform Record Locator**, a set of rules which allows you to point and go to any website in the world. He also created and published the WWW client, also known as a **web browser**, and published it entirely for free!

In August of 1991 he published the world’s first website, and you can still see a copy of it on CERN’s own website:

[The World Wide Web (Republished)](http://info.cern.ch/hypertext/WWW/TheProject.html)

At present Tim Berners-Lee is the heart of the **W3C**, or World Wide Web Consortium, which helps to decide the standards used on the web!

### The Networks of the Internet

Now let’s take a detailed look at how the Internet is put together.

The networks you’ll find on the Internet can be put into two categories: **LAN (Local Area Network)** and **WAN (Wide Area Network)**.

A LAN connects devices in a limited area like a home, business, or a university. A WAN, on the other hand, is a network which connects geographically distant places! 

The smartphones, computers, and consoles in your average household connect to the network through a router installed by an Internet service provider. When these devices access the larger network of the Internet, they do it through the router!


<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-1/11-1_1_E.png"/>

An Internet service provider’s Internet connection joins up with the connections of other providers as well as large businesses and universities. There is also a facility known as an Internet exchanges which allows one provider to connect to providers on other exchanges, tying them all into one big worldwide network!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-1/11-1_2_E.png"/>

### The Layers of the Internet

In order to understand how data is exchanged on the Internet, we need to take a look at the **DARPA Model** proposed by the United States’ **D**efense **Advanced** **R**esearch **P**rojects **A**gency. The DARPA Model splits the communication functions the Internet needs into four distinct layers.

##### The DARPA Model
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-1/11-1_3_E.png"/>

* **The Application Layer**
This layer defines the format and the order of data exchanged by applications. This layer is also used to format text code and images as well as to encrypt data!
* **The Transport Layer**
The transport layer is where data is transmitted between applications. If needed, this is also where error checks and resend requests to the sender are performed!
* **The Internet Layer**
This layer is where data is transmitted between multiple devices connected to the Internet. The IP addresses which are used to specify and locate devices on the network are handled here!
* **The Network Interface Layer**
The network interface layer is the closest layer to the hardware, converting data into a physical electrical signal which can be transmitted through a wired or wireless LAN.

Aside from the DARPA Model, there’s the **OSI Reference Model** created by the ISO, or **I**nternational **O**rganization for **S**tandardization. The OSI Reference Model splits the communication functions the Internet needs into seven distinct layers.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-1/11-1_4_E.png"/>

* **The Application Layer**
The concrete communications services which transfer files and retrieve data from websites.
* **The Presentation Layer**
Decides the expression format and conversion methods for text code and encryption.
* **The Session Layer**
Decides the order of communications from start to end.
* **The Transport Layer**
Ensures the reliability of communications through things like error correction and data resend requests.
* **The Network Layer**
Secures the channel used to communicate with the target device on the network.
* **The Data Link Layer**
This later transfers signals between closely-connected devices like routers.
* **The Physical Layer**
Allows for physical connections like cables and electronic signal conversion.

The DARPA and OSI models are similar in that they divide critical communication functions into separate layers. What makes them different is that the OSI model uses a much more abstract concept, while the DARPA Model defines a realistic framework for the communication functions of the modern Internet.

## Connecting with the Core Unit

Now let’s trying connecting your Core Unit to the Internet through Wi-Fi!

### Using Wi-Fi with the `network` Module

In order to connect to the Internet through Wi-Fi we need to use the `network` module’s `WLAN` <sup>★</sup> class.

###### ★ **WLAN** stands for **W**ireless **L**ocal **A**rea **N**etwork!

#### ■ Making a WLAN Class Object

The `WLAN` class is constructor which takes an operation mode as an argument. Setting the argument to `network.STA_IF` makes the object you create into a station (also known as a client) Setting it to `network.AP_IF` makes this object into an access point. 

* **Station**: `network.STA_IF`
Operates as a client on the WLAN.
* **Access Point**: `network.AP_IF`
Operates as an access point on the WLAN.

Now let’s turn the Core Unit into a station which can connect to your own wireless access point!

We can use the code below to create a new `WLAN` object.

```python=1
import network

wifi = network.WLAN(network.STA_IF)
```

#### ■ Enabling and Disabling a Network Interface

Most of the methods in the `WLAN` won’t be available until we enable the network interface. We’ll need to run the `active()` method in order to enable or disable the interface:

* Enabling the Interface
Run the `active()` method with the argument `"up"` or `True`.
* Disabling the Interface
Run the `active()` method with the argument `"down"` or `False`.

Now let’s enable the interface by adding the following code to the program!

###### New Code (Line 4)
```python=1
import network

wifi = network.WLAN(network.STA_IF)
wifi.active("up")  # wifi.active(True) will also enable the interface
```

#### ■ Retrieving a List of Access Points

We can run the `WLAN` class’s `scan()` method to get a list of available access points. This method will return the access point’s information as a tuple:

```
(SSID, MAC address, # of channels, signal strength, authentication mode, visible/hidden)
```

* **SSID**
The name used to identify the Wi-Fi access point. These can be up to 32 alphanumeric characters.
* **MAC Address**
This is a unique identification assigned to a device. These numbers are assigned to devices at the point of production as a hexadecimal number like FF-FF-FF-FF-FF-FF or 00:00:00:00:00:00.
* **Number of Channels**
The number of channels tells you how many devices can be connected to the access point.
* **Signal Strength**
This is the strength of the signal being broadcast from the access point.
* **Authentication Mode**
This is the encryption method the WLAN uses to transmit data.
* **Visible / Hidden**
The SSID of a visible access point can be found by other devices, while a hidden access point can’t be found.

Now let’s add the following code to the program and run it to see a list of nearby access points. Make sure that you have an access point near you that you can use!

###### New Code (Lines 6-8)
```python=1
import network

wifi = network.WLAN(network.STA_IF)
wifi.active("up")

aps = wifi.scan()  # Retrieves a list of access points
for ap in aps:  # Shows info for each access point
    print(ap)
```

#### Program Results

<pre class="prettyprint">
(b'MyAP', b'\x00\x00\x00\x00\a', 6, -53, 3, False)
(b'xxxxxxxx', b'\x00\x00\x00\x00\b', 1, -91, 4, False)
(b'yyyyyyyy', b'\x00\x00\x00\x00\c', 1, -91, 4, False)
(b'zzzzzzzz', b'\x00\x00\x00\x00\d', 1, -92, 4, False)
(b'oooooooo', b'\x00\x00\x00\x00\e', 1, -93, 3, False)
(b'pppppppp', b'\x00\x00\x00\x00\f', 6, -94, 3, False)
(b'qqqqqqqq', b'\x00\x00\x00\x00\g', 11, -94, 3, False)
</pre>

#### ■ Connecting and Disconnecting

If you want to connect to an access point, run the `connect()` method. This method takes the SSID of the access point as the first argument and the password as its second argument.

If you’d like to disconnect instead, you just have to run the `disconnect()` method. Run the `connect()` method runs and it will continue attempting to connect to the access point until you run the `disconnect()` method or disable the network interface.

Now change the code below to connect to your own access point!

###### Changed Code (Line 5)
###### ★ We’ll delete the code which shows the list of access points!
```python=1
import network

wifi = network.WLAN(network.STA_IF)
wifi.active("up")
wifi.connect("SSID", "PASSWORD")  # Specifies SSID and password for the Wi-Fi connection
```

##### Showing a Message When Connected
<pre class="prettyprint">
I (15524) wifi: new:&lt6,0&gt, old:&lt1,0&gt, ap:&lt255,255&gt, sta:&lt6,0&gt, prof:1
I (16204) wifi: state: init -&gt auth (b0)
I (16214) wifi: state: auth -&gt assoc (0)
I (16224) wifi: state: assoc -&gt run (10)
I (16244) wifi: connected with xxxxxx, channel 6, bssid = xx:xx:xx:xx:xx:xx
I (16244) wifi: pm start, type: 1

I (16254) network: CONNECTED
I (17014) event: sta ip: 172.20.10.4, mask: 255.255.255.240, gw: 172.20.10.1
I (17014) network: GOT_IP
</pre>

##### Terminal Message on Failure to Connect
<pre class="prettyprint">
no AP found
I (41584) wifi: new:&lt6,0&gt, old:&lt6,0&gt, ap:&lt255,255&gt, sta:&lt6,0&gt, prof:1
I (41584) wifi: state: init -&gt auth (b0)
I (41584) wifi: state: auth -&gt assoc (0)
I (41594) wifi: state: assoc -&gt run (10)
I (41614) wifi: state: run -&gt init (2c0)
I (41614) wifi: new:&lt6,0&gt, old:&lt6,0&gt, ap:&lt255,255&gt, sta:&lt6,0&gt, prof:1
I (41614) wifi: STA_DISCONNECTED, reason:2
</pre>

#### ■ Checking the Connection

You can check whether you’re currently connected to an access point by using the `isconnected()` method. This method returns `True` when you’re connected and `False` if you’re disconnected.

As we explained above, the `connect()` method will repeatedly attempt to connect in the background until you’ve connected to the access point. However, if the program doesn’t need to connect to the network, this can block other processes in the program!

The code below uses the `isconnected()` method to automatically stop trying to attempt if no connection is made within 10 seconds of running the `connect()` method.


###### New and Changed Code (Line 2, Lines 6-20)
```python=1
import network
import time  # Imports the time module in order to count 10 seconds

wifi = network.WLAN(network.STA_IF)
wifi.active("up")
if not wifi.isconnected():  # Runs the following code only when disconnected from Wi-Fi
    wifi.connect("SSID", "PASSWORD")  # Attempts to connect to the access point
    timeout = 10  # Variable used to count 10 seconds. Decreases by 1 every second.
    while timeout > 0:
        print(".")
        time.sleep_ms(1000)  # Waits 1 second 
        timeout -= 1  # Decreases counter by 1 
        if wifi.isconnected():  # Ends the loop on line 9 if connection is successful
            print("Connected.")
            break
    else:  # Runs the following code if 10 seconds pass and the break statement fails to fun 
        print("Connection failed!")
        wifi.disconnect()  # Disconnects from Wi-Fi
else: # Runs the following code if already connected
    print("Already connected.")
```


#### ■ Retrieving Parameters from the Internet Layer

Your Core Unit will be assigned an IP address automatically once it connects to an access point.

IP stands for **Internet Protocol**, and these are handled by the Internet layer in the DARPA Model. Internet protocol assigns an IP address to every device, showing their location on the network and allowing a device to find the device it needs to communicate with!

Devices are assigned two IP addresses, a **local IP address** assigned on the LAN and a **global IP address** assigned on the WAN.

You can use the `ifconfig()` method to retrieve a device’s parameters on the Internet layer including it IP addresses! The return value of this method is a tuple showing the following:

```
(IP address, subnet mask, gateway, DNS server)
```

* **IP Address**
This idenfication number is assigned to a device in order to find it on the Internet layer. This is the local IP address.
* **Subnet Mask**
This is a numerical value showing the coverage of the network. You can use subnet masks to manage devices connected to the same network as a separate groups.
* **Gateway**
This is the IP address of the access point which acts as a gateway to an external network.
* **DNS Server**
This is the IP address of the server which acts as the DNS. **DNS** is short for **D**omain **N**ame **S**ystem. This system was developed to manage the relationship between an Internet domain name of a website like **artec-kk.co.jp** and the IP address of its server. While originally you would need to specify an IP address in order to access a device, we adopted domain names since they’re much easier to understand!

Now let’s add the following code to our program to make these parameters appear in the terminal!

###### New Code (Line 15, Line 22)
```python=1
import network
import time

wifi = network.WLAN(network.STA_IF)
wifi.active("up")
if not wifi.isconnected():
    wifi.connect("ssid", "password")
    timeout = 10
    while timeout > 0:
        print(".")
        time.sleep_ms(1000)
        timeout -= 1
        if wifi.isconnected():
            print("Connected.")
            print(wifi.ifconfig())  # Shows parameters
            break
    else:
        print("Connection failed!")
        wifi.disconnect()
else:
    print("Already connected.")
    print(wifi.ifconfig())  # Shows parameters
```

#### Program Results

<pre class="prettyprint">
Connected.
('172.20.10.2', '255.255.255.240', '172.20.10.1', '172.20.10.1')
</pre>

### Turning the Connection Feature into a Module

Since we’ve now used the `network` module to connect to a wireless access point, we’re going to turn this feature into a unique class called `MyWiFi`.

##### The `MyWiFi` Class Methods

|Method|What does it do?|
|:---|:---|:---|
|`__init__(self)`|Creates a `WLAN` class object and enables the network interface. |
|`connect(self, ssid, password)`|Connects to an access point with the specified SSID and password. Returns `True` if connection is successful and shows the parameters including the IP address. Returns `False` if the connection fails. |
|`disconnect(self)`|Removes connection to the access point. |
|`isconnected(self)`|Checks if you’re connected to the access point. |
|`get_aps(self)`|Shows a list of available access points in the terminal. |

#### ■ Defining the `MyWiFi` class.

Now let’s make a new program and add the following code to it:

##### `MyWiFi` Class Code

```python=1
import network
import time

class MyWiFi:
    def __init__(self):
        self.wifi = network.WLAN(network.STA_IF)  # Creates a WLAN object as a station
        self.wifi.active("up")  # Enables network interface

    def connect(self, ssid, password):
        if not self.wifi.isconnected():  # Runs the following code only when disconnected from Wi-Fi
            self.wifi.connect(ssid, password)  # Attempts to connect to the access point
            timeout = 10
            while timeout > 0:  # Waits up to 10 seconds for the connection to go through
                print(".")
                time.sleep(1)
                timeout -= 1
                if self.wifi.isconnected():  # If connection is successful
                    print("Connected.")
                    print(self.wifi.ifconfig())  # Shows parameters including IP address
                    return True  # Returns True if connection is successful
            else:  # If the connection fails
                print("Connection failed!")
                self.disconnect()  # Stops connection attempt
                return False
        else:  # Runs the following code if already connected
            print("Already connected.")
            print(self.wifi.ifconfig())
            return True

    def disconnect(self):  # Removes connection to the access point
        self.wifi.disconnect()
        print("Disconnected.")
    
    def isconnected(self): # Returns connection status to the access point
        return self.wifi.isconnected()

    def get_aps(self):  # Shows a list of access points
        aps = self.wifi.scan()
        for ap in aps:
            print(ap)
```

#### ■ Checking and Modularizing the Class

Now let’s see how this class is supposed to work! We’ll do this by running the code for the `MyWiFi` class we defined above. After we run it, we’ll create an instance of the `MyWiFi` class in the terminal, then run the `connect()` method using the SSID and password of the access point!

##### Running the Code
<pre class="prettyprint">
>>>  wifi = MyWiFi()
>>>  wifi.connect("SSID","PASSWORD")
.
.
.
Connected.
('172.20.10.2', '255.255.255.240', '172.20.10.1', '172.20.10.1')
True
</pre>

Now that we’ve confirmed that the other methods run with no issues, let’s turn our program file into a module.

First, save the file to your computer with the name **mywifi**. Now click the **Files** button and copy the file to your Core Unit!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-1/11-1_5_E.png"/>

## Retrieving Data from a Webpage

In the previous section we learned how to connect to the Internet through a wireless access point. Now we’re going to find out how to retrieve data from a web page in real life!

### The Communication Protocols of the Internet

In order to establish communications between computers on the Internet, they need a system of common rules that they can follow to send and receive data. In the world of computers, these rules are known as **communication protocols**.

These protocols exist on every layer of the DARPA Model Internet structure we introduced earlier. The protocols in the bottom layer of this structure determine which protocols are implemented at the top layer. You’ll find the following communication protocols used in the application layer at the very top of the structure:

###### ★ You’ll find out more about these layers in Topic 11-3!

##### DARPA Model Application Layer Protocols

|Protocol|Official Name|What does it do?|
|:---|:---|:---|
|HTTP|Hyper Text Transfer Protocol|Handles communication between the web browser and the server. You can use it to send and receive web page content written in HTML. |
|SMTP|Simple Mail Transfer Protocol|Allows you to send e-mails over the Internet. |
|POP3|Post Office Protocol|Allows a user to retrieve any e-mails addressed to them on an e-mail server. |
|FTP|File Transfer Protocol|Allows clients to exchange files with a server. |
|Telnet|Teletype network|Allows control of geographically distant devices like servers and routers through the Internet. All communication is done in unencrypted plain text, and many people choose to use SSH instead due to security issues. |
|SSH|Secure Shell|Uses techniques like encryption and authentication to securely transmit data between devices in remote locations. |

In this lesson we’ll use the **HTTP** protocol to retrieve the information we need from the web page!

### What is HTTP?

HTTP is a communication protocol developed in order to transfer web pages written in markup languages like HTML (Hypertext Markup Language) or XML (eXtensible Markup Language). You can also use it to work with a content embedded in the web page (also known as resources) like images and audio files.

HTTP communication starts with the the client’s web browser sending a **request** to the server. The server responds to the client’s request and returns the content of the web page as a result. This is known as a **response**!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-1/11-1_6_E.png"/>


The request sent by the client contains all sorts of data. The two pieces of data we’re going to explain here are the **URL** and **HTTP methods**.

#### ■ What’s a URL?

URL stands for Uniform Resource Location. To put it simply, this is an address which shows where the data is on the Internet. The address bar of a web browser shows URLs as text strings like the one below:

```
http://www.artec-kk.co.jp/artecrobo2/pdf/en/studuinobit_class_reference.pdf
```
###### ★ This URL links to a Python reference library for ArtecRobo 2.0!

The text strings inside of the URL have the following meanings:

* **Scheme**: `http:`,
The scheme is mainly used for the name of the communication protocol. This URL shows that we’re using HTML this time.
* **Host Name/Domain Name**: `//www.artec-kk.co.jp`
This shows the host name `www` and the domain name `artec-kk.co.jp` of the server we’re sending the request to.
* **Path**: `/artecrobo2/pdf/jp/studuinobit_class_reference.pdf`
This sees whether the data or web page with the name `studuinobit_class_reference.pdf` exists in the directory `/artecrobo2/pdf/en/` on the server. This is known as the path.

#### ■ What’s an HTTP Method?

There are nine types of HTTP methods, and you use them to determine what operation should be performed on the resource at the specified URL. You don’t need to remember them all, but let’s try to get a grasp on the following five methods:

* **GET**
**Gets**, or retrieves the resource. Running the get method won’t make any changes to the resource on the server.
* **HEAD**
This requests a response just like the get method, but it doesn’t retrieve the resource.
* **POST**
**Posts**, or sends data. Based on the data it receives, the server will overwrite the resource or create a new one before sending a response.
* **PUT**
This will **upload** a resource or **overwrite** and existing resource.
* **DELETE**
**Deletes** a resource.

#### ■ HTTP Requests and Responses

You can tell a request from a response by the data inside of it!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-1/11-1_7_E.png"/>

The example request below starts with the name of the method as well as the URL. It ends with `HTTP/1.1`, which is the HTTP version. The latest HTTP version is `HTTP/2`.

```
GET https://www.artec-kk.co.jp/school/cl/textbooks/sample/ HTTP/1.1
```

The header below it shows the client data as well as the various data which will be requested from the server.

```
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate, br
Accept-Language: ja,en-US;q=0.9,en;q=0.8
Cache-Control: max-age=0
Connection: keep-alive
Host: www.artec-kk.co.jp
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36
```
|Header Type|What is it?|
|:---|:---|
|**Accept**|The type of resource which can be received in the response|
|**Accept-Encoding**|Encoding which can be decoded by the web browser|
|**Accept-Language**|The language expected by the web browser|
|**Cache-Control**|Data used to control the cache. A web browser’s cache temporarily stores the data of any web page it visits, allowing the web page to load faster next time you access it! |
|**Connection**|Specify **close** here when you want to disconnect from the server or **keep-alive** if you want to keep the connection open. HTTP/1.1 and later sets this to keep-alive by default. |
|**Host**|The host name or domain name included in the URL. |
|**User-Agent**|The web browser as well as operating system and version sending the request. |

There are lots of different headers, and the one above is just a single example. The server loads this header data before running the necessary processes!

The body of the request is where you put the data for any resources like text data, images, and audio files that you want the server to send.

Now we’re going to make a program which sends a request to a real web page to see what sort of data is contained in a response!

### Practicing HTTP

The `urequests` module in MicroPython makes it easy to communicate in HTTP! This module has the following functions which allow you to use HTTP methods:

##### `urequests` Module Functions

|Function|What does it do?|
|:---|:---|
|`get(url, headers={})`|Sends a request using the GET method|
|`post(url, data, json, headers={})`|Sends a request using the POST method|
|`put(url, data)`|Sends a request using the PUT method|
|`delete(url)`|Sends a request using the DELETE method|
|`head(url, headers={})`|Sends a request using the HEAD method|
|`request(method, url, data=None, json=None, headers={})`|Sends a request with the specified method|

Every function listed above returns a `response` object. This object contains the data for the response sent by the server. Every `response` object has the following properties and methods.

##### response Object Properties and Methods

|Property or Method|What is it?|
|:---|:---|
|`status_code`|HTTP status code|
|`reason`|Reason phrase of the HTTP status code|
|`encoding`|Text encoding|
|`content`|Response data before decoding|
|`text`|Decoded text according to `content` and `encoding`|
|`json()`|Method which interprets `content` as JSON and returns data in a dictionary format|

This may be the the first time you’ve heard the terms status code and JSON, but we’ll explain them later in this lesson and explain them in further detail in the next one! First we’ll start by sending a GET method request to a simple web page and see what we get as a response.

#### ■ Retrieving Data with the GET Method

We’re going to use the GET method to retrieve data from the web page at the following URL:

[https://www.artec-kk.co.jp/school/cl/textbooks/sample/](https://www.artec-kk.co.jp/school/cl/textbooks/sample/)

If you open the URL in your web browser you’ll see the following simple web page.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-1/11-1_8_E.gif"/>

This web page is written in HTML, and the original HTML text is known as the **source**. The source looks very different from what you see in your web browser!

###### ★ HTML is often mistaken for a programming language, but it actually isn’t! HTML is a markup language which uses levels in a layered structure as well as hyperlinks to reference resources.

Your web browser has a feature which allows you to view the HTML source of any web page. In Chrome, just right click inside of the web page and choose **View page source** in the menu. You can follow the same steps to do this in other browsers. In Microsoft Edge, enable **Developer Tools** and choose **View source**, while in Safari you can click **Develop** in the File menu and choose **View Page Source** to view the source!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-1/11-1_9_E.png"/>

Follow the steps above and you’ll see the source code shown in this picture:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-1/11-1_10_E.png"/>

HTML uses **tags** to tell the web browser what each part of the document does or means. The `<title></title>` on line 5, for example, tells the browser that this is the **title** of the web page. When you use a search engine like Google, the results you see are the titles of different websites! The `<h1></h1>`tag on line 8 shows the main heading, while the `<p></p>` on line 9 indicates a paragraph.

In HTML you’ll find a wide variety of tags for different content like tables, images, audio, videos, and more! It would take a whole book to explain all of the tags used in HTML, which means we can’t use all of them in this course. However, if you’re interested in learning more, try learning on your own or by taking a course in HTML!

Now let’s try running the example code below to use the GET method to retrieve data from the web page:

##### Example Code 4-3-1

```python=1
import urequests
from mywifi import MyWiFi  # The Wi-Fi module we made in the first half of the lesson

url = "https://www.artec-kk.co.jp/school/cl/textbooks/sample/"  # Target URL

wifi = MyWiFi()
if wifi.connect("SSID", "PASSWORD"):  # Specifies the SSID and the password for the Wi-Fi connection
    response = urequests.get(url)  # Sends a request using the GET method
    print(response.status_code, response.reason, response.text, sep="\n")  # Specifies a separator with sep. This line specifies a line break.
```

#### Program Results

<pre class="prettyprint">
Connected.
('172.20.10.2', '255.255.255.240', '172.20.10.1', '172.20.10.1')
200
b'OK'
&lt;!doctype html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;meta charset="utf-8"&gt;
&lt;title>Python Robotics Course&lt;/title&gt;
&lt;/head&gt;
&lt;body>
	&lt;h1&gt;Python Robotics Course&lt;/h1&gt;
	&lt;p&gt;This is a sample website for learning HTTP methods.&lt;/p&gt;
&lt;/body&gt;
&lt;/html&gt;
</pre>

We can see from the results that we retrieved the same HTML source that we saw in the browser!

The `200` and the `OK` that you see above the source are what’s known as the **status code** and the **reason phrase**. These two items are the first line, known as the **status line** in the response structure we saw previously.



Status codes three-digit numbers which show whether the HTTP request has been processed correctly. 200 means that the process has finished with no problem! Every status code is pair with a reason phrase, and the reason phrase for the status code **200** is **OK**!

You can see a list of status codes and their reason phrases in the table below:

##### HTTP Status Codes and Reason Phrases

|Status Code|Reason Phrase|What does it mean?|
|:---|:---|:---|
|200|OK|The request was successful. |
|301|Moved Permanently|The requested URL has been permanently changed to another one. The new URL will be sent on the response. |
|304|Not Modified|If using the cache, this shows that the response hasn’t changed since the site was last accessed. This means that the client will use the resources in the cache to display the web page. |
|404|Not Found|The server can’t find the requested resource (the resource doesn’t exist). |
|405|Method Not Allowed|The request method isn’t allowed by the server. Keep in mind that you’ll never get this status code when using the GET or the HEAD methods! |
|501|Method Not Allowed|The request method isn’t supported by the server. The GET and HEAD methods will never return this status code, either! |

Now let’s try changing Example Code 4-3-1 to use the PUT method instead of the GET method to send the request!

##### Example Code 4-3-2
###### Changed Code (Line 8)
```python=1
import urequests
from mywifi import MyWiFi

url = "https://www.artec-kk.co.jp/school/cl/textbooks/sample/"

wifi = MyWiFi("SSID", "PASSWORD")
if wifi.connect():
    response = urequests.put(url)  # Change to PUT method
    print(response.status_code, response.reason, response.text, sep="\n")
```

#### Program Results
<pre class="prettyprint">
Connected.
('172.20.10.2', '255.255.255.240', '172.20.10.1', '172.20.10.1')
405
b'Method Not Allowed'
&lt;HTML&gt;
&lt;HEAD&gt;
&lt;TITLE&gt;405 Method Not Allowed&lt;/TITLE&gt;
&lt;/HEAD&gt;
&lt;BODY&gt;
&lt;H1&gt;Method Not Allowed&lt;/H1&gt;
The HTTP verb used to access this page is not allowed.
&lt;HR&gt;
&lt;ADDRESS&gt;
Web Server at artec-kk.co.jp
&lt;/ADDRESS&gt;
&lt;/BODY&gt;
&lt;/HTML&gt;
</pre>

The status code is now 405 with the reason phrase Method Not Allowed. This is because the server doesn’t allow you to use the PUT method to overwrite resources! If your request hasn’t been processed correctly, you can find out why by checking the status code and reason phrase like this!

## Using Retrieved Data to Control the Display

Depending on the client request, a web page on a server with either always return the same response, or it will return a response which changes depending on the data in the request.

The example code in our previous program is an example of the former, while the web page below is an example of the latter:

[https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php](https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php)

Send a client request with the name of a color to this web page and it will return the RGB value of that color. Now let’s try opening the following two URL and see how the results change!

[https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=red](https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=red)

[https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=green](https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=green)

You might have noticed that both of the above URLs have a **?**. The `color=red` and `color=green` after it are what’s known as **query parameters**, and you can use these to send data to a server using the GET method.

### Adding Query Parameters to a URL

You can add a query parameter by typing **?** at the end of the URL followed by **name=parameter**. **Name** is a variable and **parameter** is the value of that variable! If you want to add another parameter, just add an **&** as shown below!

```
http://www.****.com/***/***.php?parameter_name=value&parameter_name=value&...
```

The response of the web page above depends on the parameter or parameters you send to it. In other words, the web page is able to use the name of a color to look up it’s RGB value!

##### Supported Query Parameters

|Query Parameter|URL|Response|
|:---|:---|:---|
|color=red|[https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=red](https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=red)|(31, 0, 0)|
|color=green|[https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=green](https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=green)|(0, 31, 0)|
|color=blue|[https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=blue](https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=blue)|(0, 0, 31)|
|color=yellow|[https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=yellow](https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=yellow)|(31, 31, 0)|
|color=cyan|[https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=cyan](https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=cyan)|(0, 31, 31)|
|color=magenta|[https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=magenta](https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=magenta)|(31, 0, 31)|
|color=white|[https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=white](https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=white)|(31, 31, 31)|
|unidentified|[https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=orange](https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=orange)|Not Found|

Now let’s use this web page to make a program which takes the name of the color the user types into the terminal. It uses this color to retrieve the RGB value and make your Core Unit’s LED display light up in that color!

### Programming the LED Display to Light Up in the Retrieved Color

Let’s make a function called `light_up()` which runs the following processes in order and call it in the terminal:

1. **Connect to Wi-Fi**
2. **Ask the user to type the color**
3. **Retrieve the data from the web page**
4. **Check the security of the retrieved data**
5. **Light up the LED display**

Now let’s write our code in order!

#### ■ Connecting to Wi-Fi

If you call the `light_up()` function when disconnected from Wi-Fi, the program will attempt to connect to the access point. The next process can’t run if the connection fails, so let’s run a `return` statement to end this process if this happens.

```python=1
from mywifi import MyWiFi  # Imports a unique class of the module we made in this lesson

SSID = "SSID"  # SSID for the Wi-Fi connection
PASSWORD = "PASSWORD"  # Password for the Wi-Fi connection
wifi = MyWiFi()  # Creates a unique class object

def light_up():  # Function to run from the terminal
    if not wifi.isconnected():  # Runs connection process only if disconnected from Wi-Fi
        if not wifi.connect(SSID, PASSWORD):  # Connects to Wi-Fi.
            return  # Stops function here if connection fails
```

#### ■ Requesting User Input

Next we’ll use the `input()` to take the color name that the user types. Let’s add the following code in order to do this:

###### New Code (Line 11)
```python=7
def light_up():
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    colorname = input("Type the name of the color >>>")  # Asks the user for the name of the color
```

#### ■ Retrieving Data from the Webpage

The program will make a URL using the color name typed by the user as a parameter and send a request to the server using the GET method. It will then check the status code of the response. If the request isn’t successful, it will show the status code and reason phrase before ending the process!

###### New Code (Line 1, Line 7, Lines 14-17)
```python=1
import urequests  # Imports the HTTP module
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
wifi = MyWiFi()
url_base = "https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color="  # Base URL to send the request to

def light_up():
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    colorname = input("Enter color name. >>> ")
    response = urequests.get(url_base + colorname)  # Sends request with the GET method
    if not response.status_code != "200":  # Gives status code of 200 if request is successful
        print(response.status_code, response.reason)  # Shows status code and reason phrase if request fails
        return  # Ends function here
```

#### Checking the Security of the Data

If the request is successful, the server response will include a tuple-format text string in its body:

```
(31,31,31)
```

We can then use the `eval()` function to evaluate the string and get a tuple object.

There are cases, however, where the data you take from the Internet has malicious code embedded inside of it! (★ Of course, you won’t find any malicious code on this web page!) Since the `eval()` function interprets and evaluates the text string as a Python expression, any malicious code in there could end up causing problems with your program.

###### ★ Using the `exec()` function to interpret a text string as a program instead of an expression is even more dangerous since this data could contain viruses or even steal your personal data!

You can find cases where a safety check is performed by using a regular expression to match the data and see if it’s in the correct format. We’ll try using the same method here!

This program will ask that the response contains a tuple-formatted text string which contains three sets of numbers which are a maximum of two digits! The regular expression we need to match the data looks like this:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-1/11-1_11_E.png"/>

The parentheses `(` and `)` are special characters used to denote a group in regex, so we’ll need to put a `\` escape character in front of each one in order to interpret them as normal characters!

Now let’s use this to add the following code:

###### New Code (Line 2, Lines 9-10, Lines 21-25)

```python=1
import urequests
import ure  # Imports module in order to use regular expressions
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
wifi = MyWiFi()
url_base = "https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color="
pattern_rgb = "^\(([1-9]\d|\d),([1-9]\d|\d),([1-9]\d|\d)\)$"  # Expression looks for a tuple text string with three numbers of up to two digits each
regex_rgb = ure.compile(pattern_rgb)  # Regex object

def light_up():
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    colorname = input("Enter color name. >>> ")
    response = urequests.get(url_base + colorname)
    if not response.status_code != "200":
        print(response.status_code, response.reason)
        return
    match_obj = regex_rgb.match(response.text)  # Runs matching
    if match_obj:  # It matching
        rgb = eval(response.text)  # Uses eval() function to evaluate and convert into a tuple
    else:  # If not matching
        print(response.text)  # Shows text string in terminal to verify content
```

#### ■ Lighting Up the LED Display

As the final step, we’ll use the RGB value that we retrieved to light up the LED display. Let’s add the following code in order to do this: Now your program is finished!

##### Example Code 5-2-1
###### New Code (Line 3, Lines 11-12, Line 26)

```python=1
import urequests
import ure
from pystubit.board import display, Image  # Imports LED display object and the Image class
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
wifi = MyWiFi()
url_base = "https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color="
pattern_rgb = "^\(([1-9]\d|\d),([1-9]\d|\d),([1-9]\d|\d)\)$"
regex_rgb = ure.compile(pattern_rgb)
img = Image("11111:11111:11111:11111:11111")  # Creates an image object to be shown on the display

def light_up():
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    colorname = input("Enter color name. >>> ")
    response = urequests.get(url_base + colorname)
    if not response.status_code != "200":
        print(response.status_code, response.reason)
        return
    match_obj = regex_rgb.match(response.text)
    if match_obj:
        rgb = eval(response.text)
        display.show(img, color=rgb)  # Turns on the LED display
    else:
        print(response.text)
```

### Checking the Program

The web page we’re going to use supports the following colors:

```
red, green, blue, yellow, cyan, magenta, white
```

Now try running your finished program and typing a color into the terminal to see how it works!

##### Running the Code
<pre class="prettyprint">
>>>  light_up()
.
.
.
Connected.
('172.20.10.2', '255.255.255.240', '172.20.10.1', '172.20.10.1')
Enter color name. >>> red
</pre>

## Challenge: Using Retrieved Data to Control the Buzzer

In this challenge, we’ll use a web page which has a table which lists the first few notes from the melody of  Pomp and Circumstance by English composer Sir Edward Elgar.

##### The URL
[https://www.artec-kk.co.jp/school/cl/textbooks/sample/melody.html](https://www.artec-kk.co.jp/school/cl/textbooks/sample/melody.html)
##### In the Web Browser
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-1/11-1_12_E.png"/>

The source code for this web page looks like this:

##### The Source
```html=1
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>Pomp and Circumstance</title>
</head>

<style>
table{
	border-collapse: collapse;
	border:1px solid;
}
th,td{
	border:1px solid;
	padding:0.5em 0.3em;
}
th{
	background-color: #eee;
}
</style>
	
<body>
	<h1>Pomp and Circumstance</h1>
	<table>
		<thead>
			<tr>
				<th>tone(MIDI)</th><th>duration</th>
			</tr>
		<thead>
		<tbody>
			<tr><td>72</td><td>1200</td></tr>
			<tr><td>71</td><td>300</td></tr>
			<tr><td>72</td><td>300</td></tr>
			<tr><td>74</td><td>600</td></tr>
			<tr><td>69</td><td>1200</td></tr>
			<tr><td>67</td><td>1200</td></tr>
			<tr><td>65</td><td>1200</td></tr>
			<tr><td>64</td><td>300</td></tr>
			<tr><td>65</td><td>300</td></tr>
			<tr><td>67</td><td>600</td></tr>
			<tr><td>62</td><td>2400</td></tr>
			<tr><td>64</td><td>1200</td></tr>
			<tr><td>66</td><td>300</td></tr>
			<tr><td>67</td><td>600</td></tr>
			<tr><td>69</td><td>300</td></tr>
			<tr><td>74</td><td>1200</td></tr>
			<tr><td>67</td><td>1200</td></tr>
			<tr><td>72</td><td>1200</td></tr>
			<tr><td>72</td><td>300</td></tr>
			<tr><td>71</td><td>600</td></tr>
			<tr><td>69</td><td>300</td></tr>
			<tr><td>67</td><td>2400</td></tr>
		</tbody>
	</table>
</body>
</html>
```

The section of the source which shows the table of note data starts with the `<table>` tag on line 24 and closes the tag with `</table>` on line 54! Follow the hints below as you make a program which can retrieve the source with a GET method, then use regular expressions and string methods to retrieve out the notes and play them with the buzzer!

### Pulling Select Data from an HTML Source

If you take a closer look at the source above, you’ll see that each note is contained in a block of text like this:

```
<tr><td>72</td><td>1200</td></tr>
```

The `<tr></tr>` tags represent a row in the table, while the `<td></td>` tags represent a column.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-1/11-1_13_E.png"/>

We can use the following regular expression and the regex `search()` method to search the table cell by cell.

```
"<td>\\d+</td>"
```

We can retrieve the text strings matched by the `search()` method by using the `group()` method on the value returned by the matched object. By combining these two methods with a string object’s `replace()` method, we can retrieve any matching text strings we need from the source!

##### An Example Combination

```python=1
import ure  # Regex module

regexobj = ure.compile("<td>\\d+</td>")  # Regex object

# Example HTML source
source = """
<tr><td>72</td><td>1200</td></tr>
<tr><td>71</td><td>300</td></tr>
"""

# Shows the matched text strings in the terminal in order
while True:
    matchobj = regexobj.search(source)  # Searches for matching string
    if matchobj:
        _str = matchobj.group(0)  # Retrieves matching string
        # Deletes the matching string from the source so it doesn\’t get matched in the next search
        source = source.replace(_str, "", 1)  # Substitutes first item only
        print(_str)  # Shows matching string in the terminal
    else:
        break  # Ends loop if no matching string is found
```

##### Program Results

<pre class="prettyprint">
&lt;td&gt;72&lt;/td&gt;
&lt;td&gt;1200&lt;/td&gt;
&lt;td&gt;71&lt;/td&gt;
&lt;td&gt;300&lt;/td&gt;
</pre>

Using the hint above, try your best to program this yourself!

### Making the Example Program

The steps we need to take to retrieve resources from a web page are the same ones we follow to make Example Code 5-2-1. The program below puts these processes into a function named `play_melody()`.

```python=1
import urequests
from mywifi import MyWiFi  # Imports the module we made

SSID = "SSID"  # SSID for the Wi-Fi connection
PASSWORD = "PASSWORD"  # Password for the Wi-Fi connection
wifi = MyWiFi()  # Creates unique class object
url = "https://www.artec-kk.co.jp/school/cl/textbooks/sample/melody.html"  # URL the data will be retrieved from

def play_melody():  # Uses the melody data from the web page to play the buzzer
    if not wifi.isconnected():  # Attempts to connect only if disconnected from Wi-Fi
        if not wifi.connect(SSID, PASSWORD):
            return  # Stops function here if connection fails
    response = urequests.get(url)  # Sends a request using the GET method
    if not response.status_code != "200":  # If request fails
        print(response.status_code, response.reason)  # Shows status code and reason code
        return  # Process ends here
```

By applying the method above, we can retrieve the source and use a regular expression to extract the note data! Remember that your program needs to be able to tell the difference between the note itself and the duration!

```
<tr><td>72</td><td>1200</td></tr>
```
###### `<td>72</td>` on the left is the note, while `<td>1200</td>` on the right is the duration!

Your program won’t be able to do this by using the regular expression below:

```
"<td>\\d+</td>"
```

Your program can find the difference by counting the number of searches. An odd count means that the text string is a note, while an even count means that the string is the duration! It will then store the extracted note data in a list.

The program below puts this method into practice:

###### New Code (Line 2, Line 9, Lines 19-35)

```python=1
import urequests
import ure  # Regex module
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
wifi = MyWiFi()
url = "https://www.artec-kk.co.jp/school/cl/textbooks/sample/melody.html"
regex_td = ure.compile("<td>\\d+</td>")  # Creates a regex module to search for the target text string

def play_melody():   
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    response = urequests.get(url)
    if not response.status_code != "200":
        print(response.status_code, response.reason)
        return
    source = response.text  # Retrieves the source
    melody = []  # List which stores notes as (note, duration) tuples
    count = 0  # Counter for the number of searches
    while True:  # Repeats search until it runs out of strings to match
        count += 1  # Adds 1 to the counter
        matchobj = regex_td.search(source)  # Runs the search
        if matchobj:  # If a matching string is found
            _str = matchobj.group(0)  # Retrieves matching string
            source = source.replace(_str, "", 1)  # Deletes the matching string from the source to prevent it being found in the next search
            num = _str.replace("<td>", "").replace("</td>", "")  # Retrieves the number only
            if count % 2 == 1:  # If search count is odd
                tone = num  # This number shows the MIDI value of the note
            else:  # If search count is even
                duration = int(num)  # Number represents note length and the string is converted into an integer
                melody.append((note, duration))  # Adds note data to the list
        else:  # If no matching string is found
            break  # Ends the search loop
```

Now that we’ve extract the note data from the web page and put it into a list, we can finish the program by making the buzzer play each note. Try running it to see how it works!

##### Example Code 6-2-1
###### New Code (Line 3, Lines 37-38)
```python=1
import urequests
import ure
from pystubit.board import buzzer  # Imports object to control the buzzer
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
wifi = MyWiFi()
url = "https://www.artec-kk.co.jp/school/cl/textbooks/sample/melody.html"
regex_td = ure.compile("<td>\\d+</td>")

def play_melody():   
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    response = urequests.get(url)
    if not response.status_code != "200":
        print(response.status_code, response.reason)
        return
    source = response.text
    melody = []
    count = 0
    while True:
        count += 1
        matchobj = regex_td.search(source)
        if matchobj:
            _str = matchobj.group(0)
            source = source.replace(_str, "", 1)
            num = _str.replace("<td>", "").replace("</td>", "")
            if count % 2 == 1:
                tone = num
            else:
                duration = int(num)
                melody.append((note, duration))
        else:
            break
    for note in melody:  # Retrieves note data from the list in order
        buzzer.on(note[0], duration=note[1]) 　# Buzzer plays the note
```

## Conclusion
### A Quick Review

In this lesson we learned how the Internet works, the history of the World Wide Web, and how web browsers use HTTP to communicate!

We also learned how to use MicroPython’s `network` module to connect to a wireless hotspot before putting this feature into an original module. Once our Core Unit was online, we learned how to use the `urequests` module to retrieve data from a simple web page using the HTTP GET method.

In the second half of the lesson we applied what we learned and programmed our Core Unit to use the data it retrieved from the Internet to light up its LED or play its buzzer!

This lesson forms the basis of what you’ll learn in the following lessons, so be sure to come back and review when you have the time!

### Coming Up Next Time!

In the next lesson we’ll practice using the POST method in HTTP and learn about a new data format called JSON. We’ll then take on the challenge of making a program which can retrieve the weather forecast for any city in the world from the World Meteorological Organization’s website!

