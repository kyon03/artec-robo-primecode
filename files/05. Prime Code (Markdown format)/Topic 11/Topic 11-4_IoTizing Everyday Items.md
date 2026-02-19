---
tags: Python_English, Topic 11
---

Python Robotics Course Lesson 44

Topic 11-4
IoTizing Everyday Items
===

Connect an everyday umbrella stand to the Internet of Things!

## In This Lesson...

We’ll be taking the concepts we learned in Topic 11-2 and 11-3 to make a robot which connects a household item to the Internet of Things. The theme for this lesson is *IoTizing an umbrella stand**! Set the UmbrellaBot we make in this lesson by the door of your house and it will let anyone who walks by know today’s forecast. If the forecast is rain, it will use a folding mechanism to hand them an umbrella!



::: info

Lesson Introduction

■ Lesson 44 Contents
1. Using a JSON-to-Dictionary Conversion Module
2. Reviewing Topic 11-2 and 11-3
3. Making an UmbrellaBot
4. Making a Multifunction UmbrellaBot


In this lesson we’re going to take the concepts we studied in Topic 11-2 and 11-3 and use them to make a robot.

At the start of the lesson, we’ll introduce a module which we can use to convert between JSON format text strings and dictionary objects.

We’ll then take a look back at the techniques we used to retrieve weather forecast data in Topic 11-2 and 11-3.

In the second half of the lesson we’re going to make an UmbrellaBot which tells you today’s forecast. By making this robot you’ll learn all about the Internet of Things and how it’s changing the way we live!

Last, we’ll take our UmbrellaBots and add all sorts of useful features to it as we take on the challenge of giving it different functions for different times of the day!

Now let's start the lesson!

:::

## New Python Syntax

Let’s start the lesson by learning about the new MicroPython module we’ll use to make our program.

### Converting from JSON to Dictionary with the `ujson` Module

In Topic 11-2, we studied a data format called **JSON**, or **J**ava**S**cript **O**bject **N**otation. The program we made involved converting JSON text strings into a Python dictionary object.

The `json` module we’ll introduce here allows you to convert a dictionary back into JSON!

First let’s take a look at the four functions defined in this module:

##### The `ujson` Module Functions

|Function|What does it do?|
|:---|:---|
|`dump(obj, stream)`|Converts a dictionary object (`obj`) to JSON text strings and writes it to a file or other stream<sup>★</sup> (`stream`). |
|`dumps(obj)`|Converts a dictionary object (`obj`) to JSON text strings. |
|`load(stream)`|Loads a file or other stream (`stream`) and interpret its as JSON data before converting it into a dictionary. Throws a `ValueError` exception if that data isn’t in proper JSON format. |
|`loads(str)`|Interprets data as JSON data and converts it into a dictionary object. Throws a `ValueError` exception if that data isn’t in proper JSON format. |

###### ★ Think of a stream like a real stream of water! In the world of IT, a stream means a continuous flow of data. A good example would be sending and receiving our inputting and outputting data! Stream can also refer to objects which input and output data like files.

Two of the best examples of this would be the `dump` functions which convert JSON data into a dictionary, and the `load` functions which do the opposite! The difference between the `dump()` and the `dumps()` function is whether or not the results are written to the stream! Similarly, the difference between the `load()` and `loads()` functions is whether or not the function uses the stream.

Now let’s try writing some code which uses these functions for ourselves! We’ll start by using the `dumps()` function to convert a dictionary into JSON, then turn it back into a dictionary using the `loads()` function!

##### Example Code 2-1-1
```python=1
import ujson

# This is an example of a nested dictionary
_dict = {"name": "Tanaka", "age": 16, "grade": {"Mathematics": 82, "Physics": 76, "Chemistry": 96}}

# Converts the dictionary into a JSON string
data = ujson.dumps(_dict)  # Converts into JSON
print(type(data), "\n", data)  # Shows the conversion result

# Interprets the JSON data and converts it back into a dictionary
try:  # Throws a ValueError exception if the data isn\`t in proper JSON format
    data = ujson.loads(data)  # Converts back into a dictionary
    print(type(data), "\n", data)  # Shows the conversion result
except ValueError:  # Shows a message notifying the user of the error
    print("The data is not in JSON format.")
```

Run the code above and you should see the results below in your terminal. Use the `type()` function to see the type of object should show you that the data has been converted into a JSON string and back again!

##### Program Results
<pre class="prettyprint">
&lt;class 'str'&gt; 
 {"name": "Tanaka", "age": 16, "grade": {"Mathematics": 82, "Physics": 76, "Chemistry": 96}}
&lt;class 'dict'&gt;  
 {'grade': {'Mathematics': 82, 'Physics': 76, 'Chemistry': 96}, 'name': 'Tanaka', 'age': 16}
</pre>

Now let’s take the same sample dictionary and use the `dump()` and `load()` functions to load and save a file as we convert a dictionary into JSON and back!

##### Example Code 2-1-2
```python=1
import ujson

_dict = {"name": "Tanaka", "age": 16, "grade": {"Mathematics": 82, "Physics": 76, "Chemistry": 96}}

# Converts into a JSON string and saves it to the file
try:  # Opens example.json in readable text mode
    file = open("example.json", mode="r+t")
except OSError:  # Throws an OSError exception if example.json does not exist
    file = open("example.json", mode="w+t")  # Creates and opens new example.json
ujson.dump(_dict, file)  # Writes JSON string to file
file.close()  # Closes the file once

# Interprets file data as JSON and returns a dictionary
file = open("example.json", mode="rt")  # Opens same file in read-only mode
try:  # Throws a ValueError exception if the file data is not in proper JSON format
    data = ujson.load(file)  # Converts file data into a dictionary object
    print(type(data), "\n", data)  # Shows the conversion result
except ValueError:  # Shows a message notifying the user of the error
    print("The data is not in JSON format.")
file.close()  # Closes the file
```

###### ★ JSON data files have a **.json** extension!

Run this code and you’ll see that the dictionary we turned into and saved as a JSON string will be turned back into a dictionary!

##### Program Results
<pre class="prettyprint">
&lt;class 'dict'&gt;  
 {'grade': {'Mathematics': 82, 'Physics': 76, 'Chemistry': 96}, 'name': 'Tanaka', 'age': 16}
</pre>

**You can save data which normally can’t be saved to an external file by converting it into a JSON string**. Keep in mind that this might be another reason you would want to use the JSON format!


## Reviewing Weather Forecasts and Time Correction with NTP

In Topic 11-2 we sent HTTP requests to the World Meteorological Organization’s website to get the forecast data for our city (or the closest city to us) in JSON format. Then we took the opportunity in Topic 11-3 to retrieve the current time in UTC from Japan’s National Institute of Information and Communications Technology, using that time to correct our Core Unit’s RTC, or Real Time Clock! In this lesson we’re going to take what we learned in both of those topics and use it to make a new robot. Before we start building, however, let’s take a quick look back at what we studied!

### How to Retrieve Forecast Data

Send an HTTP GET method request to the URL below and you’ll be able to retrieve a weekly forecast in JSON format from the [WMO website](https://worldweather.wmo.int/en/dataguide.html).

```
https://worldweather.wmo.int/en/json/[City ID]_en.json
```

The `[City_ID]` part of the URL is where you put the identification code for that city! You can find the city ID for your city in the list below:

[https://worldweather.wmo.int/en/json/full_city_list.txt](https://worldweather.wmo.int/en/json/full_city_list.txt)

###### ★ If you can’t find your city in the list, try finding the closest city to you and using that one!

Japan has seven cities with the following city IDs:

##### City IDs in Japan

|City|`[City ID]`|
|:---|:---|
|Sapporo (`"Sapporo"`)|181|
|Sendai (`"Sendai"`)|182|
|Tokyo (`"Tokyo"`)|183|
|Nagoya (`"Nagoya"`)|355|
|Osaka (`"Osaka"`)|184|
|Fukuoka (`"Fukuoka"`)|185|
|Naha (`"Naha"`)|186|

The data for the forecast we retrieve will be in the JSON format shown below:

##### JSON Data for Tokyo

```
{
    "city":{
        "lang":"en",  # English (text string)
        "cityName":"Tokyo",  # City name (text string)
        "cityLatitude":"35.680000000",  # Latitude (text string)
        "cityLongitude":"139.770000000",  # Longitude (text string)
        "cityId":183,  # City ID (numerical value)
        "isCapital":true,  # Is this the capital? (boolean)
        "stationName":"Tokyo",  # Closest station or airport (text string)
        "tourismURL":"www.jnto.go.jp",# Tourism board URL (text string)
        "tourismBoardName":"Japan National Tourist Organization",  # Tourism board name (text string)
        "isDep":false,  # Dependent territory? (Text string)
        "timeZone":"+0900",  # Time zone (text string)
        "isDST":"N",  # Currently daylight savings time? (Text string)
        "member":{}  # WMO member info (dictionary) ★ Not shown
        "forecast":{  # Weekly forecast data (dictionary)
            "issueDate":"2020-07-28 11:00:00",  # Date issued (text string)
            "timeZone":"Local",  # Time zone shown (text string)
            "forecastDay":"forecastDay":[  # All forecast data (list)
                {  # List elements are the dictionary data shown below
                    "forecastDate":"2020-07-29",  # Date of the forecast (text string)
                    "wxdesc":"",  # Detailed forecast data (text string)
                    "weather":"Rain",  # Weather (text string)
                    "minTemp":"24",  # Low temperature in Celsius (text string)
                    "maxTemp":"27",  # High temperature in Celsius (text string)
                    "minTemp":"75",  # Low temperature in Fahrenheit (text string)
                    "maxTempF":"81", # High temperature in Fahrenheit (text string)
                    "weatherIcon":1401  # The icon number for that type of weather (numerical value)
                },
                .
                .
                .
            ]
        }
        "climate":{}  # Climate ★ Not shown
    }
}
```

Visit the page below to learn more about the data contained in this format:

[The WMO JSON Schema
](http://worldweather.wmo.int/en/json/WWIS_json_schema_v2.json)

We then converted the JSON data into a dictionary to access the following data:

```
data["city"]["forecast"]["forecastDay"][0]["weather"]  # Tomorrow (or today’s) weather
data["city"]["forecast"]["forecastDay"][0]["maxTemp"]  # Tomorrow (or today’s) high temperature in Celsius
data["city"]["forecast"]["forecastDay"][0]["minTemp"]  # Tomorrow (or today’s) low temperature in Celsius
```

In Japan, the seven day forecast starting with tomorrow’s date is updated at 11:00AM Japan Standard Time. This means that `"forecastDay"` 0 means today for a forecast retrieved between midnight and 11:00AM. If you retrieve the weekly forecast after 11:00, the data will start with tomorrow’s date!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-4/11-4_1_E.png"/>


### Correcting the Real Time Clock

We used the **NTP**, or **N**etwork **T**ime **P**rotocol to correct our Core Unit’s real time clock and fix any errors if it was slightly off. NTP is a communication protocol used to sync all of the clocks on the network to **UTC**, or **C**oordinated **U**niversal **T**ime. A client can send a request using the data format below to retrieve the time from an NTP server:

##### The NTP Data Format

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_30_E.png"/>

###### ★ Remember that LI shows a leap second, VN shows the NTP version, and MODE shows the operation mode!

You can use the receive timestamp and transmit timestamp for this data in the following formula to get your clock’s offset from the NTP server:

##### Finding the NTP Server Offset
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-4/11-4_2_E.png"/>
* `T1`: The time the client sent the request to the NTP server.
* `T2`: The receive timestamp. This is the time the NTP server received the request.
* `T3`: The transmit timestamp. This is the time the NTP server sent a response to the client.
* `T4`: The time the client received the NTP server’s response.


The time sent by the NTP server is in UTC+0. In order to get the correct time for our region, we had to find out which time zone our country is in:

##### The Time Zones of the World

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_34_E.png"/>


We then used the above information and make a program with the following formulat to correct our Core Unit’s real time clock:

```
Corrected RTC time = current RTC time + offset + hour difference for our time zone
```

###### ★ Since one NTP server’s time may not be accurate, real NTP protocol involves getting the time from multiple servers to correct the time little-by-little in a much more complicated process!


## Making an UmbrellaBot

It’s time to follow along with the instructions as we build an UmbrellaBot!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-4/11-4_3_E.png"/>

### Parts You'll Need

##### Parts
* Core Unit x 1
* Robot Expansion Unit x 1
* Battery Box x 1
* DC Motor x 1
* Servomotor x 2
* IR Photoreflector x 1
* Ultrasonic Sensor x 1
* Sensor Connecting Cable (3-wire, 15cm) x 1
* Sensor Connecting Cable (3-wire, 30 cm) x 1
* Basic Cube (White) x 6
* Basic Cube (Gray) x 6
* Basic Cube (Black) x 6
* Basic Cube (Red) x 4
* Triangle (Gray) x 4
* Triangle (Red) x 4
* Half A (Gray) x 2
* Half B (Gray) x 2
* Half C (White) x 16
* Half D (White) x 10
* Beam x 4

##### The Artec Block Shapes

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-4/11-4_4_E.png"/>

### Building an UmbrellaBot

Follow the link below to find instructions for building the UmbrellaBot:

[Building an UmbrellaBot](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-4/11-4_build_E.pdf)

## Programming the UmbrellaBot

The UmbrellaBot we’ll be building needs to do the following four things:

1. **Retrieve the UTC time from the NTP server and use it to correct its real time clock**
2. **Retrieve WMO forecast data and save it to an internal file**
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-4/11-4_5_E.png"/>
3. **Use an Ultrasonic Sensor to detect anyone who gets close and show the forecast from step two (weather icon, low temperature, and high temperature) on the LED display**
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-4/11-4_6_E.gif"/>
4. **Fold down to offer an umbrella if the forecast in step three is for rain!**
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-4/11-4_7_E.gif"/>

We can use our programs from Topic 11-2 and 11-3 as-is for features one and two. In order to make features three and four we’ll need to write new code.

Now let’s follow the steps below in order to make our program!

### Making the Program

We’ll need to prepare the following functions in order to make the features of our UmbrellaBot:

|Function|What does it do?|
|:---|:---|
|`give_umbrella()`|Rotates the two Servomotors **in sync** to offer the umbrella. |
|`get_icon(weather)`|Takes the `weather` as an argument and shows the icon image object on the LED display. ★ We can recycle our program from Topic 11-2 to do this! |
|`get_forecast(t=None)`|Retrieves WMO forecast data and saves it to a text file. We’ll use this function to set a timer interrupt which runs it at the same time every day! ★ We can recycle our program from Topic 11-2 to do this! |
|`show_forecast()`|References the data retrieved by the `get_forecast()`  function to show today’s (or tomorrow’s, depending on the time zone) weather forecast on the LED display! |
|`correct_time(t=None)`|Communicates with the NTP server to correct the real time clock. We’ll use this function to set a timer interrupt which runs it every 10 minutes. ★ We can recycle our program from Topic 11-3 to do this! |
|`main()`|Runs at startup. This function includes not only the functions which retrieve the time and forecast, the the function which detects people with the Ultrasonic Sensor! |

Now let’s follow along with this table as we define each of our functions!

#### ■ Making the `give_umbrella()` Function

If today’s forecast is for rain, our UmbrellaBot will bow to offer the person an umbrella. It will then wait a short amount of time before returning to its original position. In order to do this, it will need to rotate both of its Servomotors **in sync**!

We learned about synchronous control of Servomotors back in Topic 8-1. We’ll be reusing the module we made in that topic in this program!

##### Topic 8-1: Synchronous Control with the `myservo.py` Module

```python=1
import time
from pyatcrobo2.parts import Servomotor

class MyServomotor(Servomotor):
    def __init__(self, pin):
        super().__init__(pin)
        self.angle = 90
        super().set_angle(self.angle)
    
    def set_angle(self, degree):
        super().set_angle(degree)
        self.angle = degree
    
    def get_angle(self):
        return self.angle
        
def syncRotateServos(duration, *args):   
    speeds = {}
    for arg in args:
        myservo = arg[0]
        degree = arg[1]
        diff = degree - myservo.get_angle()
        speeds[id(myservo)] = diff/duration
    
    for _ in range(duration):
        for arg in args:
            myservo = arg[0]
            degree = myservo.get_angle() + speeds[id(myservo)]
            myservo.set_angle(degree)
        time.sleep_ms(1)
```

If you can’t find the module on your Core Unit, copy the code above into a new file and save it with the filename **myservo.py** before transferring it to your Core Unit! You can also download the same file from the link below. Right click the link and choose **Save link as...** to save it as **myservo.py**!

[myservo.py](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-4/myservo.py)

Now that we’ve done this, let’s define each function. We’ll start by importing modules and classes to make objects for each of the parts we need to control!

```python=1
import time
import myservo

myservo_p13 = myservo.MyServomotor("P13")  # Object for the Servomotor on the right side when viewed from the front
myservo_p14 = myservo.MyServomotor("P14")  # Object for the Servomotor on the left side when viewed from the front
```

Next, let’s put the actions which offer the umbrella into the `give_umbrella()` function. In order to offer the umbrella, the UmbrellaBot will have to rotate the right Servomotor to 45 degrees and the left Servomotor to 135 degrees.

We can use the `myservo` module’s `syncRotateServos()` method in order to make the Servomotors rotate in sync.

```
myservo.syncRotateServos(duration, (MyServomotor1, degrees), (MyServomotor2, degrees), ....)
```

We can write the code shown below to make the UmbrellaBot wait 30 seconds before returning to its original pose. This means making each Servomotor rotate back to 90 degrees!

###### New Code (Lines 7-10)

```python=1
import time
import myservo

myservo_p13 = myservo.MyServomotor("P13")
myservo_p14 = myservo.MyServomotor("P14")

def give_umbrella():  # Makes a new function
    myservo.syncRotateServos(500, (myservo_p13, 45), (myservo_p14, 135))  # Bows to offer the umbrella
    time.sleep_ms(30000)  # Waits 30 seconds
    myservo.syncRotateServos(500, (myservo_p13, 90), (myservo_p14, 90))  # Goes back to the original position
```

Now let’s try running the program we’ve made so far and call the `give_umbrella()` function from the terminal to see how it works!

<pre class="prettyprint">
&gt;&gt;&gt; give_umbrella()
</pre>

#### ■ Making the `get_icon()` Function

Next we’re going to define the `get_icon()` function. This function takes takes the weather pattern as an argument and returns an icon image object (this is an instance of the `StuduinoBitImage` class) to show on the LED display. This function is identical to the one you made in Topic 11-2’s challenge! We’ll need to set five images, one for each of our weather patterns:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-2/11-2_4_E.png"/>

###### New Code (Line 3, Lines 616, Lines 19-23, Lines 35-46)
```python=1
import time
import myservo
from pystubit.board import Image  # Imports Image class to use images

# The weather patterns
WORDS_FINE = {"No Rain", "Fine", "Sunny", "Clear", "Clearing", "Bright",
              "Fair", "Mild"}  # Fine weather
WORDS_RAINY = {"Thunderstorms", "Thundershowers", "Storm", "Showers",
               "Heavy Showers", "Rainshower", "Occasional Showers",
               "Scattered Showers", "Isolated Showers", "Light Showers",
               "Freezing Rain", "Rain", "Drizzle", "Light Rain", "Squall"}  # Rainy weather
WORDS_CLOUDY = {"Cloudy", "Partly Cloudy", "Sunny Intervals",
                "Mostly Cloudy", "Partly Bright"}  # Cloudy weather
WORDS_SNOWY = {"Snow", "Flurries", "Heavy Snow", "Snowfall", "Light Snow",
               "Blizzard", "Blowing Snow", "Hail", "Snow Showers",
               "Snowstorm", "Snowdrift"}  # Snowy weather

# Image objects for each weather pattern
IMG_FINE = Image("01110:11111:11111:11111:01110:", color=(30, 10, 0))  # Fine image
IMG_RAINY = Image("00100:01110:11111:00100:01100:", color=(0, 10, 30))  # Cloudy image
IMG_CLOUDY = Image("00000:01110:11111:01110:00000:", color=(15, 15, 15))  # Cloudy image
IMG_SNOWY = Image("10101:01110:11011:01110:10101:", color=(30, 30, 30))  # Snowy image
IMG_OTHER = Image("00000:00000:01110:00000:00000:", color=(10, 5, 15))  # Other image

myservo_p13 = myservo.MyServomotor("P13")
myservo_p14 = myservo.MyServomotor("P14")


def give_umbrella():
    myservo.syncRotateServos(500, (myservo_p13, 45), (myservo_p14, 135))
    time.sleep_ms(30000)
    myservo.syncRotateServos(500, (myservo_p13, 90), (myservo_p14, 90))


def get_icon(weather):  # Returns icon image for the pattern
    if weather in WORDS_FINE:
        img = IMG_FINE
    elif weather in WORDS_RAINY:
        img = IMG_RAINY
    elif weather in WORDS_CLOUDY:
        img = IMG_CLOUDY
    elif weather in WORDS_SNOWY:
        img = IMG_SNOWY
    else:
        img = IMG_OTHER
    return img
```

#### ■ Making the `get_forecast()` Function 

Now let’s make the function which retrieves the WMO forecast and saves the result to a text file inside of the Core Unit.

In order to do this, we’ll import the HTTP request module as well as the Wi-Fi class at the beginning of the program. We’ll also need to define the access point SSID, password, and the city ID of the city as constants!

###### New Code (Line 2, Line 5, Lines 7-9)
```python=1
import time
import urequests  # HTTP request module
import myservo
from pystubit.board import Image
from mywifi import MyWiFi  # The Wi-Fi module we made in Topic 11-1

SSID = "SSID"  # SSID for the Wi-Fi connection
PASSWORD = "PASSWORD"  # Password for the Wi-Fi connection
CITY_ID = 183  # City ID of the city you want to get the forecast for. 183 is Tokyo, Japan.
```

Next we’ll create an object from the Wi-Fi class that we imported earlier:

```python=31
myservo_p13 = myservo.MyServomotor("P13")
myservo_p14 = myservo.MyServomotor("P14")
wifi = MyWiFi()  # Creates an object to connect to Wi-Fi
```

Now let’s make a new `get_forecast()` function. The code below will make an HTTP request to the WMO website to retrieve the forecast data, then store that data inside of a text file:

###### New Code (Lines 56-68)
```python=56
def get_forecast(t=None):  # Makes a new function. We set a t argument here in order to call it with a timer interrupt.
    if not wifi.isconnected():  # Attempts to connect only if disconnected from Wi-Fi
        if not wifi.connect(SSID, PASSWORD):
            return  # Stops process here if connection fails
    url_format = "https://worldweather.wmo.int/en/json/[City ID]_en.json"  # URL format for the page we will retrieve the forecast from
    url = url_format.replace("[City ID]", str(CITY_ID))  # Replaces part of the URL with the city ID
    res = urequests.get(url)  # Sends a GET method request and takes the response
    try:  # Opens the file the program will write to
        file = open("weather_report.json", mode="r+t")  # Opens in readable text mode
    except OSError:  # Creates a new file if one does not exist
        file = open("weather_report.json", mode="w+t")  # Creates a new file and opens it in readable text mode
    file.write(res.text)  # Writes the decoded response body to the file (content is JSON string)
    file.close()  # Closes the file
```

In order to test how our program works, try running it and called the `get_forecast()` function from the terminal. After that, let’s trying loading the `weather_report.json` file that the program creates in with `mode="rt"`, or read-only text mode! Don’t forget to close the file once you’ve checked it!

<pre class="prettyprint">
&gt;&gt;&gt; get_forecast()
．
．
．
Connected.
('172.20.10.2', '255.255.255.240', '172.20.10.1', '172.20.10.1')
&gt;&gt;&gt; file = open("weather_report.json", mode="rt")
&gt;&gt;&gt; print(file.read())

----You’ll see the retrieved forecast data here!----

&gt;&gt;&gt; file.close()
</pre>

#### ■ Making the `show_forecast()` Function

This function will take the JSON string in the text file created by the `get_forecast()` function and convert it into a dictionary. It will then reference this to show today’s weather as well as the high and low temperatures in Celsius on the LED display.

We’ll start by importing the JSON string module as well asn LED display object at the beginning of the program.

```python=1
import time
import urequests
import ujson  # Used to convert the JSON string into a dictionary
import myservo
from pystubit.board import Image, display  # Imports object to control the LED display
from mywifi import MyWiFi
```

Next we’ll use a `with` statement to open the text file stored on the Core Unit. After this, we’ll pass the file stream to the `ujson` module’s `load()` function to convert the JSON string into a dictionary! The last step is to reference "weather"`, `"maxTemp"`, and `"minTemp"` in the dictionary and store each one to a variable!

###### New Code (Lines 72-77)
```python=72
def show_forecast():  # Shows the forecast for today
    with open("weather_report.json", mode="rt") as file:  # Opens in read-only text mode
        data = ujson.load(file)  # Interprets file stream as JSON
        weather = data["city"]["forecast"]["forecastDay"][0]["weather"]  # The weather
        hi_temp = data["city"]["forecast"]["forecastDay"][0]["maxTemp"] # The high temperature
        lo_temp = data["city"]["forecast"]["forecastDay"][0]["minTemp"] # The low temperature
```

Now let’s use the `get_icon()` function to retrieve the icon image from the weather. After that, the three pieces of data will appear on the LED display!

###### New Code (Lines 78-81)
```python=72
def show_forecast():
    with open("weather_report.json", mode="rt") as file:
        data = ujson.load(file)
        weather = data["city"]["forecast"]["forecastDay"][0]["weather"]
        max_temp = data["city"]["forecast"]["forecastDay"][0]["maxTemp"]
        min_temp = data["city"]["forecast"]["forecastDay"][0]["minTemp"]
        img = get_icon(weather)  # Retrieves icon from the weather pattern
        display.show(img, delay=3000)  # Shows the image
        display.scroll("Hi: "+hi_temp, delay=25, color=(31, 0, 0))  # Shows the high temperature in red
        display.scroll("Lo: "+lo_temp, delay=25, color=(0, 0, 0))  # Shows the high temperature in blue
```

If the **forecast is for rain**, the UmbrellaBot will offer you an umbrella. Let’s try running the `give_umbrella()` function we defined earlier!

###### New Code (Lines 82-83)
```python=72
def show_forecast():
    with open("weather_report.json", mode="rt") as file:
        data = ujson.load(file)
        weather = data["city"]["forecast"]["forecastDay"][0]["weather"]
        hi_temp = data["city"]["forecast"]["forecastDay"][0]["maxTemp"]
        lo_temp = data["city"]["forecast"]["forecastDay"][0]["minTemp"]
        img = get_icon(weather)
        display.show(img, delay=3000)
        display.scroll("Hi: "+hi_temp, delay=25, color=(31, 0, 0)) 
        display.scroll("Lo: "+lo_temp, delay=25, color=(0, 0, 31)) 
        if weather in WORDS_RAINY:  # If the forecast is for rain
            give_umbrella()  # Offers an umbrella
```

Now let’s try running the program we’ve made so far and call the `show_forecast()` function from the terminal to see how it works!

<pre class="prettyprint">
&gt;&gt;&gt; inform_forecast()
</pre>

#### ■ Making the `correct_time()` Function

This function is identical to the one we made in Topic 11-3! We'll start by importing the modules and classes we need at the beginning of the program.

###### New Code (Lines 4-6)
```python=1
import time
import urequests
import ujson
import usocket  # Used to communicate using sockets
import ubinascii  # Used to convert byte strings into numerical values
from machine import RTC  # The real time clock class
import myservo
from pystubit.board import Image, display
from mywifi import MyWiFi
```

We’ll be using the NICT’s NTP server to get the correct time. The domain for this server is **ntp.nict.jp**.

The UTC time we’ll retrieve from the NTP server shows the current time as amount of time (in seconds) that has passed since `1900-1-1 0:0:0`! The Core Unit’s real time clock, on the other hand, starts this count at `2000-1-1 0:0:0`. This means that we’ll have to get the time for the real time clock by subtracting the amount of time that has passed from `1900-1-1 0:0:0` to `2000-1-1 0:0:0` from this number!

Remember that, in order to get the correct time for our region, we had to find out which time zone our country is in. We’ll define these two offsets as constants in our program.

###### New Code (Lines 14-16)
```python=11
SSID = "SSID"
PASSWORD = "PASSWORD"
CITY_ID = 183
NTP_SERVER = "ntp.nict.jp"  # The domain of the NTP server
DELTA = 3155673600  # The number of seconds from 1900-1-1 0:0:0 to 2000-1-1 0:0:0
UTC_TIMEZONE_JP = 32400  # The difference in hours between UTC+0 and UTC+9 (Japan Time)
```

To continue, let’s make an object for our real time clock:

###### New Code (Line 40)

```python=37
myservo_p13 = myservo.MyServomotor("P13")
myservo_p14 = myservo.MyServomotor("P14")
wifi = MyWiFi()
rtc = RTC()  # The real time clock object
```

We can define the `correct_time()` function by using the same code that we used in Topic 11-3. Except for the time it first runs, this function will be called by a timer interrupt every 10 minutes.

```python=92
def correct_time(t=None):  # The timer object from the caller will be passed to the “t” argument every time this function is called by the timer interrupt
    if not wifi.isconnected():  # Attempts to connect only if disconnected from Wi-Fi
        if not wifi.connect(SSID, PASSWORD):
            return  # Stops process here if connection fails
    addrinfo = usocket.getaddrinfo(NTP_SERVER, 123)  # Retrieves address info of the NTP server
    sockaddr = addrinfo[0][-1]  # Extracts address info required to establish the connection
    s = usocket.socket(usocket.AF_INET, usocket.SOCK_DGRAM)  # Creates a socket
    s.connect(sockaddr)  # Establishes socket connection
    data = bytearray(48)  # Creates a 48-byte array
    data[0] = 0x23  # First byte sets the leap second (2 bits), the NTP version (3 bits), and the mode of operation (3 bits)
    client_transmit = time.time()  # Retrieves the RTC time the request is sent
    s.send(data)  # Sends request using the socket
    msg = s.recv(48)  # Retrieves response using the socket
    client_receive = time.time()  # Retrieves the RTC time the response was received
    s.close()  # Closes the socket
    server_receive = int(ubinascii.hexlify(msg[32:36]), 16) - DELTA  # Converts the integer part of the server receive timestamp into decimal
    server_transmit = int(ubinascii.hexlify(msg[40:44]), 16) - DELTA  # Converts the integer part of the server transmit timestamp into decimal
    offset = ((server_receive - client_transmit) + (server_transmit - client_receive)) / 2  # Calculates the offset with the RTC
    current_time = time.time() + offset + UTC_TIMEZONE_JP  # Gets current time after including the offset
    datetime = time.localtime(int(current_time))  # Converts from seconds into the date and time
    datetime = datetime[:3] + (datetime[6],) + datetime[3:6] + (0,)  # Changes order of data to fit the RTC datetime format
    rtc.init(datetime)  # Sets the RTC to the corrected time
```

Let’s try running our program so far and calling the `correct_time()` function from the terminal. We’ll check here to see whether the real time clock’s `datetime()` method retrieves the the current time and uses it to correct itself!

#### Program Results
<pre class="prettyprint">
&gt;&gt;&gt; correct_time()
&gt;&gt;&gt; print(rtc.datetime())
(2020, 8, 7, 4, 15, 13, 14, 333870)
</pre>

#### ■ Making the `main()` Function

The main function which runs at startup will do the following things in order:

1. Correct the real time clock
2. Set a timer interrupt to recalibrate the clock every 10 minutes
3. Retrieve the forecast data
4. Set a timer interrupt to retrieve the next forecast
5. Detect people using the Ultrasonic Sensor
6. Show today’s forecast on the LED display when it detects a person
7. Repeat steps five and six on an infinite loop

Aside from the initial startup, this function will get the data forecast once per day. Let’s find a good time for it to do this! As we mentioned before, the weather forecast in Japan is updated every day at 11:00AM. Setting our update time a bit later at 11:10AM would let us get the latest weather forecast every day!

We’ll start by importing the class for our timer interrupts and Ultrasonic Sensor at the beginning of the program.

###### New Code (Line 6, Line 9)
```python=1
import time
import urequests
import ujson
import usocket
import ubinascii
from machine import RTC, Timer  # Imports the timer interrupt class
import myservo
from pystubit.board import Image, display
from pyatcrobo2.parts import UltrasonicSensor  # Imports the Ultrasonic Sensor class
from mywifi import MyWiFi
```

Now let’s set the time our UmbrellaBot retrieves the forecast. We’ll convert this time into seconds to make it easier to compare!

###### New Code (Line 18)
```python=12
SSID = "SSID"
PASSWORD = "PASSWORD"
CITY_ID = 183
NTP_SERVER = "ntp.nict.jp"
DELTA = 3155673600
UTC_TIMEZONE_JP = 32400
WEATHER_REPORT_REQUEST_TIME = 11*3600 + 10*60  # The forecast request time (11:10:00)
```

To continue, we’ll make timer objects for our timer interrupts as well as an object for the Ultrasonic Sensor. We’ll prepare separate timer objects to retrieve the time and the latest forecast!

###### New Code (Lines 43-45)
```python=39
myservo_p13 = myservo.MyServomotor("P13")
myservo_p14 = myservo.MyServomotor("P14")
wifi = MyWiFi()
rtc = RTC()
timer_for_rtc = Timer(1)  # Timer object used for the interrupt which retrieves the time
timer_for_weather = Timer(2)  # Timer object used for the interrupt which retrieves the forecast
us = UltrasonicSensor("P0")  # Object used to control the Ultrasonic Sensor
```

Now let’s make a new `main()` function. Let’s start by setting it to retrieve the offset for the real time clock and run timer interrupt every 10 minutes.

###### New Code (Lines 121-123)
```python=121
def main():
    correct_time() # Corrects the real time clock
    timer_for_rtc.init(mode=Timer.PERIODIC, period=600000, callback=correct_time)  # Uses a timer interrupt to recalibrate the real time clock every 10 seconds
```

Next let’s set a timer interrupt to retrieve the forecast and set the next time it runs to update the forecast! The amount of time until this runs again again depends on the current time. If the current time is before the next update, we can use that time to calculate the difference. If the current time is after the update, we can calculate the difference using the next day’s update time!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-4/11-4_8_E.png"/>

###### New Code (Lines 124-131)
```python=121
def main():
    correct_time()
    timer_for_rtc.init(mode=Timer.PERIODIC, period=600000, callback=correct_time)
    get_forecast()  # Retrieves the forecast
    datetime = rtc.datetime()  # Retrieves the current time
    now = datetime[4] * 3600 + datetime[5] * 60 + datetime[6]  # Converts current time into seconds
    if now < WEATHER_REPORT_REQUEST_TIME:  # If before the update time for today`s forecast 
        diff = WEATHER_REPORT_REQUEST_TIME - now  # Calculate difference using this time
    else:  # If after the time
        diff = (WEATHER_REPORT_REQUEST_TIME + 86400)- now  # Calculate difference using tomorrow`s update time
    timer_for_weather.init(mode=Timer.ONE_SHOT, period=diff*1000, callback=get_forecast)  # Sets timer interrupt to run only once. Note that the time in period has to be set in milliseconds.
```

The timer interrupt calls after this should be set to run inside of the `get_forecast()` function. Aside from once at startup, this function’s timer interrupt will update the forecast. To do this, let’s set the timer interrupt at the same time tomorrow only when the timer object has been passed to the `t` argument!

###### New Code (Lines 69-70)
```python=68
def get_forecast(t=None):
    if t: # Sets the timer interrupt to run at the same time next day (24 hours later) when called by the timer interrupt
        t.init(mode=Timer.ONE_SHOT, period=86400*1000, callback=get_forecast)
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    url_format = "https://worldweather.wmo.int/en/json/[City ID]_en.json"
    url = url_format.replace("[City ID]", str(CITY_ID))
    res = urequests.get(url)
    try:
        file = open("weather_report.json", mode="r+t")
    except OSError:
        file = open("weather_report.json", mode="w+t")
    file.write(res.text)
    file.close()
```

Now let’s use a `while` statement to make an infinite loop which continuously checks the value of the Ultrasonic Sensor. Depending on where you use your UmbrellaBot, make sure to pick an appropriate distance to detect the person. When it detects a person, the program will run the `show_forecast()` function. We’ll add the code to run the `main()` function last in order to finish our program!

###### New Code (Lines 135-139, Line 142)
```python=123
def main():
    correct_time()
    timer_for_rtc.init(mode=Timer.PERIODIC, period=600000, callback=correct_time) 
    get_forecast()
    datetime = rtc.datetime()
    now = datetime[4] * 3600 + datetime[5] * 60 + datetime[6]
    if now < WEATHER_REPORT_REQUEST_TIME:
        diff = WEATHER_REPORT_REQUEST_TIME - now
    else:
        diff = (WEATHER_REPORT_REQUEST_TIME + 86400) - now
    timer_for_weather.init(mode=Timer.ONE_SHOT, period=diff*1000, callback=get_forecast)

    while True:  # Infinite loop
        d = us.get_distance()  # Gets the Ultrasonic Sensor value  
        if d > 0 and d < 30:  # Example where a person is 30 cm or closer
            show_forecast()  # Shows forecast on LED display 
        time.sleep_ms(20)  # When using an Ultrasonic Sensor, make the program wait for a bit before retrieving the value again


main()  # Runs the main function
```

### Checking the Finished Program

The program will look like this one you’re done. Try running it to see how it works!

##### Example Code 5-3-1
```python=1
import time
import urequests
import ujson
import usocket
import ubinascii
from machine import RTC, Timer
import myservo
from pystubit.board import Image, display
from pyatcrobo2.parts import UltrasonicSensor
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
CITY_ID = 183
NTP_SERVER = "ntp.nict.jp"
DELTA = 3155673600
UTC_TIMEZONE_JP = 32400
WEATHER_REPORT_REQUEST_TIME = 11*3600 + 10*60

WORDS_FINE = {"No Rain", "Fine", "Sunny", "Clear", "Clearing", "Bright",
              "Fair", "Mild"}
WORDS_RAINY = {"Thunderstorms", "Thundershowers", "Storm", "Showers",
               "Heavy Showers", "Rainshower", "Occasional Showers",
               "Scattered Showers", "Isolated Showers", "Light Showers",
               "Freezing Rain", "Rain", "Drizzle", "Light Rain", "Squall"}
WORDS_CLOUDY = {"Cloudy", "Partly Cloudy", "Sunny Intervals",
                "Mostly Cloudy", "Partly Bright"}
WORDS_SNOWY = {"Snow", "Flurries", "Heavy Snow", "Snowfall", "Light Snow",
               "Blizzard", "Blowing Snow", "Hail", "Snow Showers",
               "Snowstorm", "Snowdrift"} 

IMG_FINE = Image("01110:11111:11111:11111:01110:", color=(30, 10, 0)) 
IMG_RAINY = Image("00100:01110:11111:00100:01100:", color=(0, 10, 30)) 
IMG_CLOUDY = Image("00000:01110:11111:01110:00000:", color=(15, 15, 15)) 
IMG_SNOWY = Image("10101:01110:11011:01110:10101:", color=(30, 30, 30))
IMG_OTHER = Image("00000:00000:01110:00000:00000:", color=(10, 5, 15))

myservo_p13 = myservo.MyServomotor("P13")
myservo_p14 = myservo.MyServomotor("P14")
wifi = MyWiFi()
rtc = RTC()
timer_for_rtc = Timer(1)
timer_for_weather = Timer(2)
us = UltrasonicSensor("P0")


def give_umbrella():
    myservo.syncRotateServos(500, (myservo_p13, 45), (myservo_p14, 135))
    time.sleep_ms(30000)
    myservo.syncRotateServos(500, (myservo_p13, 90), (myservo_p14, 90))


def get_icon(weather):
    if weather in WORDS_FINE:
        img = IMG_FINE
    elif weather in WORDS_RAINY:
        img = IMG_RAINY
    elif weather in WORDS_CLOUDY:
        img = IMG_CLOUDY
    elif weather in WORDS_SNOWY:
        img = IMG_SNOWY
    else:
        img = IMG_OTHER
    return img
    

def get_forecast(t=None):
    if t:
        t.init(mode=Timer.ONE_SHOT, period=86400*1000, callback=get_forecast)
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    url_format = "https://worldweather.wmo.int/en/json/[City ID]_en.json"
    url = url_format.replace("[City ID]", str(CITY_ID))
    res = urequests.get(url)
    try:
        file = open("weather_report.json", mode="r+t")
    except OSError:
        file = open("weather_report.json", mode="w+t")
    file.write(res.text)
    file.close()


def show_forecast():
    with open("weather_report.json", mode="rt") as file:
        data = ujson.load(file)
        weather = data["city"]["forecast"]["forecastDay"][0]["weather"]
        hi_temp = data["city"]["forecast"]["forecastDay"][0]["maxTemp"]
        lo_temp = data["city"]["forecast"]["forecastDay"][0]["minTemp"]
        img = get_icon(weather)
        display.show(img, delay=3000)
        display.scroll("Hi: "+hi_temp, delay=25, color=(31, 0, 0))
        display.scroll("Lo: "+lo_temp, delay=25, color=(0, 0, 31))
        if weather in WORDS_RAINY:
            give_umbrella()
            

def correct_time(t=None):
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    addrinfo = usocket.getaddrinfo(NTP_SERVER, 123)
    sockaddr = addrinfo[0][-1]
    s = usocket.socket(usocket.AF_INET, usocket.SOCK_DGRAM)
    s.connect(sockaddr)
    data = bytearray(48)
    data[0] = 0x23
    client_transmit = time.time()
    s.send(data)
    msg = s.recv(48)
    client_receive = time.time()
    s.close()
    server_receive = int(ubinascii.hexlify(msg[32:36]), 16) - DELTA
    server_transmit = int(ubinascii.hexlify(msg[40:44]), 16) - DELTA
    offset = ((server_receive - client_transmit) + (server_transmit - client_receive)) / 2
    current_time = time.time() + offset + UTC_TIMEZONE_JP
    datetime = time.localtime(int(current_time))
    datetime = datetime[:3] + (datetime[6],) + datetime[3:6] + (0,)
    rtc.init(datetime)
    
    
def main():
    correct_time()
    timer_for_rtc.init(mode=Timer.PERIODIC, period=600000, callback=correct_time)
    get_forecast()
    datetime = rtc.datetime()
    now = datetime[4] * 3600 + datetime[5] * 60 + datetime[6]
    if now < WEATHER_REPORT_REQUEST_TIME: 
        diff = WEATHER_REPORT_REQUEST_TIME - now
    else:
        diff = (WEATHER_REPORT_REQUEST_TIME + 86400) - now
    timer_for_weather.init(mode=Timer.ONE_SHOT, period=diff*1000, callback=get_forecast)

    while True:
        d = us.get_distance()
        if d > 0 and d < 30:
            show_forecast()
        time.sleep_ms(20)


main()
```


## Challenge: Making a Multifunction UmbrellaBot

Example Code 5-3-1 will show anyone who leaves the house today or tomorrow’s forecast, no matter the time of day. Most people, however, want to see the weather forecast when they’re leaving home in the morning. It would be pretty convenient to add new features to our UmbrellaBot which change depending on the time!

* **Coming Home at Night**
Turns into a Sensor Light which lights up the LED display for a fixed time when it detects a person.
* **Sleeping**
Turns into a Guard Bot which uses its buzzer to sound an alarm when it detects a person.

We’ll be adding these two features to your UmbrellaBot in this challenge. Let’s trying modifying its program to changing functions depending on the day!

### Programming Hints

Start by thinking about your daily schedule: what times would be most convenient for your UmbrellaBot to run each function?

##### An Example Schedule
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-4/11-4_9_E.png"/>

Try adding each the Sensor Light and burglar alarm functions by referencing each bit of standalone code below:

##### The Sensor Light Code
```python=1
from pystubit.board import Image, display
from pyatcrobo2.parts import UltrasonicSensor

us = UltrasonicSensor("P0")
img_all = Image("11111:11111:11111:11111:11111")

def light_on():
    display.show(img_all, delay=30000, clear=True, color=(31, 31, 31))
    
while True:
    d = us.get_distance()
    if d > 0 and d < 30:
        light_on()
    time.sleep_ms(20)
```

##### Burglar Alarm Example
```python=1
from pystubit.board import buzzer
from pyatcrobo2.parts import UltrasonicSensor

def alarm():
    while True:  # Repeats until you reset the Core Unit
        buzzer.on("C7", duration=200)
        time.sleep_ms(100)

while True:
    d = us.get_distance()
    if d > 0 and d < 30:
        alarm()
    time.sleep_ms(20)
```

### Making the Program

We’re going to explain how to program each part using a schedule with the following three time periods:

* Morning (leaving the house): 7:30-8:30
* Evening (coming home): 18:00-19:30
* Sleeping: 23:00-06:00

#### ■ Defining the Time Periods

We can start by defining each of our time periods using a `(start time, end time)` tuple. We’ll have to convert each start and end time into seconds!

```python=12
SSID = "SSID"
PASSWORD = "PASSWORD"
CITY_ID = 183
NTP_SERVER = "ntp.nict.jp"
DELTA = 3155673600
UTC_TIMEZONE_JP = 32400
PERIOD_GO_OUT = (7*3600+30*60, 8*3600+30*60)  # Morning period (7:30-8:30)
PERIOD_RETURN_HOME = (18*3600, 19*3600+30*60)  # Evening period (18:00-19:30)
PERIOD_SLEEP = (23*3600, 6*3600)  # Sleeping period (23:00-06:00)
```

#### ■ Making the Sensor Light Function
We’ll put the process which makes the LED display light up in bright white for a short period of time (for example, 30 seconds) into a function called `light_on()` In order to make this work, we’ll prepare an image object which turns on all of the LEDs!

###### New Code (Line 47, Lines 125-126)
```python=40
myservo_p13 = myservo.MyServomotor("P13")
myservo_p14 = myservo.MyServomotor("P14")
wifi = MyWiFi()
rtc = RTC()
timer_for_rtc = Timer(1)
timer_for_weather = Timer(2)
us = UltrasonicSensor("P0")
img_all = Image("11111:11111:11111:11111:11111")  # Image which turns on every LED on the LED display
```
```python=125
def light_on():  # Makes the LED display periodically flash bright white
    display.show(img_all, delay=30000, clear=True, color=(31, 31, 31))
```

#### ■ Making the Guard Bot Function
We’ll put the process which uses the buzzer to repeatedly sound the alarm into a function called `alarm()`. Let’s make this alarm play until you reset the Core Unit!

Changed Code (Line 8, Lines 129-132)
```python=1
import time
import urequests
import ujson
import usocket
import ubinascii
from machine import RTC, Timer
import myservo
from pystubit.board import Image, display, buzzer  # Adds buzzer object to imports
from pyatcrobo2.parts import UltrasonicSensor
from mywifi import MyWiFi
```
```python=129
def alarm():  # Sounds alarm until you reset the Core Unit
    while True:
        buzzer.on("C7" , duration=200)
        time.sleep_ms(100)
```

#### ■ Changing the `main()` Function

Now let’s change the `main()` function! When our robot detects a person, it will retrieve the current time from the real time clock, check the time period, and then choose the right function to run!

###### New Code (Lines 150-157)
```python=135
def main():
    correct_time()
    timer_for_rtc.init(mode=Timer.PERIODIC, period=600000, callback=correct_time)
    get_forecast()
    datetime = rtc.datetime()
    now = datetime[4] * 3600 + datetime[5] * 60 + datetime[6]
    if now < WEATHER_REPORT_REQUEST_TIME: 
        diff = WEATHER_REPORT_REQUEST_TIME - now
    else:
        diff = (WEATHER_REPORT_REQUEST_TIME + 86400) - now
    timer_for_weather.init(mode=Timer.ONE_SHOT, period=diff*1000, callback=get_forecast)

    while True:
        d = us.get_distance()
        if d > 0 and d < 30:
            datetime = rtc.datetime()  # Retrieves the current time from the RTC
            now = datetime[4] * 3600 + datetime[5] * 60 + datetime[6]  # Converts current time into seconds
            if now >= PERIOD_GO_OUT[0] and now <= PERIOD_GO_OUT[1]:  # If leaving in the morning
                shows_forecast()  # Shows the forecast
            elif now >= PERIOD_RETURN_HOME[0] and now <= PERIOD_RETURN_HOME[1]:  # If coming home at night
                light_on()  # Turns on the sensor light
            elif now >= PERIOD_SLEEP[0] or now <= PERIOD_SLEEP[1]:  # If sleeping
                alarm()  # Sounds the burglar alarm
        time.sleep_ms(20)
```

#### ■ The Finished Program

And now our program is finished! In order to test how it works, try changing the time periods required to run each function and make sure they all work!

##### Example Code 6-2-1
```python=1
import time
import urequests
import ujson
import usocket
import ubinascii
from machine import RTC, Timer
import myservo
from pystubit.board import Image, display
from pyatcrobo2.parts import UltrasonicSensor
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
CITY_ID = 183
NTP_SERVER = "ntp.nict.jp"
DELTA = 3155673600
UTC_TIMEZONE_JP = 32400
WEATHER_REPORT_REQUEST_TIME = 11*3600 + 10*60
PERIOD_GO_OUT = (7*3600, 8*3600)
PERIOD_RETURN_HOME = (18*3600, 19*3600)
PERIOD_SLEEP = (11*3600+30*60, 6*3600)

WORDS_FINE = {"No Rain", "Fine", "Sunny", "Clear", "Clearing", "Bright",
              "Fair", "Mild"}
WORDS_RAINY = {"Thunderstorms", "Thundershowers", "Storm", "Showers",
               "Heavy Showers", "Rainshower", "Occasional Showers",
               "Scattered Showers", "Isolated Showers", "Light Showers",
               "Freezing Rain", "Rain", "Drizzle", "Light Rain", "Squall"}
WORDS_CLOUDY = {"Cloudy", "Partly Cloudy", "Sunny Intervals",
                "Mostly Cloudy", "Partly Bright"}
WORDS_SNOWY = {"Snow", "Flurries", "Heavy Snow", "Snowfall", "Light Snow",
               "Blizzard", "Blowing Snow", "Hail", "Snow Showers",
               "Snowstorm", "Snowdrift"} 

IMG_FINE = Image("01110:11111:11111:11111:01110:", color=(30, 10, 0)) 
IMG_RAINY = Image("00100:01110:11111:00100:01100:", color=(0, 10, 30)) 
IMG_CLOUDY = Image("00000:01110:11111:01110:00000:", color=(15, 15, 15)) 
IMG_SNOWY = Image("10101:01110:11011:01110:10101:", color=(30, 30, 30))
IMG_OTHER = Image("00000:00000:01110:00000:00000:", color=(10, 5, 15))

myservo_p13 = myservo.MyServomotor("P13")
myservo_p14 = myservo.MyServomotor("P14")
wifi = MyWiFi()
rtc = RTC()
timer_for_rtc = Timer(1)
timer_for_weather = Timer(2)
us = UltrasonicSensor("P0")
img_all = Image("11111:11111:11111:11111:11111")


def give_umbrella():
    myservo.syncRotateServos(500, (myservo_p13, 45), (myservo_p14, 135))
    time.sleep_ms(30000)
    myservo.syncRotateServos(500, (myservo_p13, 90), (myservo_p14, 90))


def get_icon(weather):
    if weather in WORDS_FINE:
        img = IMG_FINE
    elif weather in WORDS_RAINY:
        img = IMG_RAINY
    elif weather in WORDS_CLOUDY:
        img = IMG_CLOUDY
    elif weather in WORDS_SNOWY:
        img = IMG_SNOWY
    else:
        img = IMG_OTHER
    return img
    

def get_forecast(t=None):
    if t:
        t.init(mode=Timer.ONE_SHOT, period=86400*1000, callback=get_forecast)
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    url_format = "https://worldweather.wmo.int/en/json/[City ID]_en.json"
    url = url_format.replace("[City ID]", str(CITY_ID))
    res = urequests.get(url)
    try:
        file = open("weather_report.json", mode="r+t")
    except OSError:
        file = open("weather_report.json", mode="w+t")
    file.write(res.text)
    file.close()


def show_forecast():
    with open("weather_report.json", mode="rt") as file:
        data = ujson.load(file)
        weather = data["city"]["forecast"]["forecastDay"][0]["weather"]
        hi_temp = data["city"]["forecast"]["forecastDay"][0]["maxTemp"]
        lo_temp = data["city"]["forecast"]["forecastDay"][0]["minTemp"]
        img = get_icon(weather)
        display.show(img, delay=3000)
        display.scroll("Hi: "+hi_temp, delay=25, color=(31, 0, 0))
        display.scroll("Lo: "+lo_temp, delay=25, color=(0, 0, 31))
        if weather in WORDS_RAINY:
            give_umbrella()
            

def correct_time(t=None):
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    addrinfo = usocket.getaddrinfo(NTP_SERVER, 123)
    sockaddr = addrinfo[0][-1]
    s = usocket.socket(usocket.AF_INET, usocket.SOCK_DGRAM)
    s.connect(sockaddr)
    data = bytearray(48)
    data[0] = 0x23
    client_transmit = time.time()
    s.send(data)
    msg = s.recv(48)
    client_receive = time.time()
    s.close()
    server_receive = int(ubinascii.hexlify(msg[32:36]), 16) - DELTA
    server_transmit = int(ubinascii.hexlify(msg[40:44]), 16) - DELTA
    offset = ((server_receive - client_transmit) + (server_transmit - client_receive)) / 2
    current_time = time.time() + offset + UTC_TIMEZONE_JP
    datetime = time.localtime(int(current_time))
    datetime = datetime[:3] + (datetime[6],) + datetime[3:6] + (0,)
    rtc.init(datetime)
    

def light_on():
    display.show(img_all, delay=30000, clear=True, color=(31, 31, 31))


def alarm():
    while True:
        buzzer.on("C7" , duration=200)
        time.sleep_ms(100)


def main():
    correct_time()
    timer_for_rtc.init(mode=Timer.PERIODIC, period=600000, callback=correct_time)
    get_forecast()
    datetime = rtc.datetime()
    now = datetime[4] * 3600 + datetime[5] * 60 + datetime[6]
    if now < WEATHER_REPORT_REQUEST_TIME: 
        diff = WEATHER_REPORT_REQUEST_TIME - now
    else:
        diff = (WEATHER_REPORT_REQUEST_TIME + 86400) - now
    timer_for_weather.init(mode=Timer.ONE_SHOT, period=diff*1000, callback=get_forecast)

    while True:
        d = us.get_distance()
        if d > 0 and d < 30:
            datetime = rtc.datetime()
            now = datetime[4] * 3600 + datetime[5] * 60 + datetime[6]
            if now >= PERIOD_GO_OUT[0] and now <= PERIOD_GO_OUT[1]:
                show_forecast()
            elif now >= PERIOD_RETURN_HOME[0] and now <= PERIOD_RETURN_HOME[1]:
                light_on()
            elif now >= PERIOD_SLEEP[0] or now <= PERIOD_SLEEP[1]:
                alarm()
        time.sleep_ms(20)


main()
```

## Conclusion
### A Quick Review
In this lesson we took what we learned about using the Internet to retrieve a forecast in Topic 11-2 as well as correct our Core Unit's real time clock in Topic 11-3 and took on the challenge of making an UmbrellaBot using the Internet of Things as an inspiration!

By using the Internet of Things to connect everyday items to Internet-based services, we can hope to add new values to these items make our lives much richer.

While we didn't learn about these in Topic 11, there are services which allow you to send emails and used AI-based voice and image recognition for free just by sending HTTP requests. Most of these services require you to register as a user, but if you’re interested try looking some up and adding them to your future projects.

### Coming Up Next Time!

In Topic 12 we’ll learn how to **turn your Core Unit into a web server**, as well as how to **install and use open source<sup>★</sup> MicroPython packages**.

In the next lesson we’ll introduce a way to control your Core Unit remotely using a tablet or smartphone. Be sure to bring a Wi-Fi capable device from home!

###### ★ Open source refers to software or programs that anyone can use, any way they like for free!