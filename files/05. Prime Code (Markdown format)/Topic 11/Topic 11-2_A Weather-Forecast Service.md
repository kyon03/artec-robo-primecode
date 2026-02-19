---
tags: Python_English, Topic 11
---

Python Robotics Course Lesson 42

Topic 11-2
A Weather-Forecast Service
===

Pull data from an actual website on the Internet!

## In This Lesson...

In this lesson we’re going program our Core Unit to retrieve weather forecast data published by the World Meteorological Organization and show it on the LED display!



::: info

Lesson Introduction

■ Lesson 42 Contents
1. The HTTP POST Method 
2. The JSON Data Format
3. Retrieving Forecast Data
4. Challenge: Showing Icons for Weather Patterns 


In the previous lesson we learned about HTTP, a communication protocol used on the Internet. We then made a program which used an HTTP GET method request to retrieve data.

We’ll start this lesson by learning about the POST method, another very popular HTTP method. In order to learn what makes this method different from GET, we’ll write actual code to see the difference for ourselves!

We’ll also study a new file format in this lesson known as JSON. Data that you retrieve from the Internet comes in a variety of formats aside from HTML, and JSON is one of those formats. What makes JSON special is that data written in this format can be easily converted into a Python dictionary object!

We’ll then take on the practical challenge of making a program which can retrieve weather forecasts from the World Meteorological Organization’s website. The data we retrieve from this website will be in the JSON format.

At the ends of the lesson we’ll take on the challenge of making icons for each weather pattern and showing those on the LED instead of text!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!


:::

## The HTTP POST Method

In the previous lesson we got acquainted with how HTTP works as well as the methods you can use in HTTP. We then practiced using the GET method for ourselves to retrieve data from the Internet. In this lesson we’re going to try out the **POST method**, another popular HTTP method!

### GET vs. POST

We’ve already learned that the GET and POST methods are used for the following different purposes:

* **GET**
**Gets**, or retrieves the resource. Running the get method won’t make any changes to the resource on the server.
* **POST**
**Posts**, or sends data. Based on the data it receives, the server will overwrite the resource or create a new one before sending a response.

Generally, you can use the GET method to retrieve a resource from a server and the POST method to send data to a server. However, we also used the GET method to send data to a server by including query parameters in a URL!

##### URL with a Query Parameter
###### The query parameter is the `color=red` following the question mark at the end

```
https://www.artec-kk.co.jp/school/cl/textbooks/sample/getrgb.php?color=red
```

One of the best examples of the POST method is sending data entered into a form on a website.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-2/11-2_1_E.png"/>


The data in the form will be sent in the body of a request.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-2/11-2_2_E.png"/>

A server will check the body of any request sent using the POST method and use the content to update resources or even make new web pages before sending a response to the client.

### Practicing the POST Method

Now let’s try using the POST method for ourselves to send data to a server. In order to practice, we’ve made a sample page which looks like a login form.

[https://www.artec-kk.co.jp/school/cl/textbooks/sample/post/](https://www.artec-kk.co.jp/school/cl/textbooks/sample/post/)

Try opening this page in your web browser and you’ll see the following message.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-2/11-2_3_E.png"/>

This is because the browser used the GET method to send the request. In order to access this page we need to use the POST method to send an `id` and a `password` parameter.

Let’s try making a program which uses the `urequests` module and the `mywifi` connection module we made in the previous lesson to send the request and then shows us the response!

#### ■ Making the Program

We’ll start by connecting the Core Unit to the Wi-Fi access point. Try remembering what you learned in the previous lesson as you code this!

```python=1
import urequests
from mywifi import MyWiFi

SSID = "SSID"  # SSID for your class\’s Wi-Fi access point
PASSWORD = "PASSWORD"  # Password for your class\’s Wi-Fi access point
URL = "https://www.artec-kk.co.jp/school/cl/textbooks/sample/post/"  # URL to send request to
wifi = MyWiFi()  # Creates an object from the unique class we made in the last lesson

def login(_id, _pass):  # Takes login ID and password as arguments
    if not wifi.isconnected():  # Attempts to connect only if disconnected from Wi-Fi
        if not wifi.connect(SSID, PASSWORD):
            return  # Stops process here if connection fails
```

We’ll need to use the `urequests` module’s `post()` function in order to send a request using the POST method.

```
urequests.post(url, data=None, header={})
```

While we didn’t need to specify the `data` or the `header` when we sent a request with GET, we’ll need to specify both this time!

We’ll start by putting the data that we want to send into the body with the following format:

```
id=....&password=....
```

Just like a query parameter, this data is formatted as **variable=value**, and you can used an & symbol to join multiple parameters together!

The format of the data inside the body is indicated by the `Content-Type` in the header. Without this, the server which receives the request would have no idea how to interpret the data you send to it!

```
Content-Type: application/x-www-form-urlencoded
```

`Application/x-www-form-urlencoded` indicates data which includes a variable and value with an `=` in between and multiple pieces of data separated by `&`. This is usually the header specified when you send form data!

Now let’s take what we’ve learned and add the following code to show the content of the response in the terminal.

##### Example Code 2-2-1
###### New Code (Lines 13-16)
```python=1
import urequests
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
URL = "https://www.artec-kk.co.jp/school/cl/textbooks/sample/post/"
wifi = MyWiFi()

def login(_id, _pass):
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    _data = "id={}&password={}".format(_id, _pass)  # Request body data
    _headers = {"Content-Type": "application/x-www-form-urlencoded"}  # Request header data
    res = urequests.post(URL, data=_data, headers=_headers)  # Sends request using POST method
    print(res.text)  # Shows response details
```

We’ll need to include the following username and password in the request we send to the web page:

* **ID**: python
* **Password**: robotics

Now let’s run the finished program and call the `login()` function from the terminal.

##### Sending the Correct Login Info
<pre class="prettyprint">
>>> login("python", "robotics")
Login succeeded.
Welcome to the Python Robotics Course!
</pre>

##### Sending the Wrong Login Info
<pre class="prettyprint">
>>> login("ruby", "robotics")
Login failed.
The ID or password is incorrect.
</pre>

As you can see here, the server will send different responses depending on the data it receives. While the web page we used here sends a simple message as a response, actual forms can send registration confirmations to the e-mail address you’ve entered and register the data in the form to a database! 

## The JSON Data Format

The data in a response sent by a server can be more than just plain text. It can be also be an HTML document, a program file written in JavaScript (this is a programming language made for web browsers), images, audio files, videos, and more!

This section introduces a type of data called **JSON**, which we can use to retrieve weather forecast data!

### What is JSON?

JSON stands for JavaScript Object Notation. It’s a type of data format developed to show objects handled by the JavaScript programming language in text format! At present, JSON is one of the standard formats used to exchange data between different web applications.

#### ■ The JSON Format
The JSON format is shown below. In Python this would be structured as a series of nested dictionaries. This means that we can easily use JSON data in Python!

```
{
    "data1": "string1",
    "data2": 2,
    "data3": {
        "data3_1": 3,
        "data3_2": "string3",
    }
}
```

### Practicing with JSON

Now let’s try to retrieve JSON data from a web page for ourselves!

[https://www.artec-kk.co.jp/school/cl/textbooks/sample/json/](https://www.artec-kk.co.jp/school/cl/textbooks/sample/json/)

Visit this page in your browser and you’ll see the following JSON data:

```
{"method":"show","args":{"string":"JSON","delay":400,"wait":false,"loop":false,"clear":true,"color":2031616}}
```

This structure is a bit hard to understand, so let’s trying breaking it up!

```
{
    "method": "show",
    "args": {
        "string": "JSON",
        "delay": 400,
        "wait": false,
        "loop": false,
        "clear": true,
        "color": 2031616
    }
}
```

This data actually shows a series of commands for the Core Unit’s LED display, and the data for each parameter is as follows:

|Parameter|What does it do?|
|:---|:---|
|`"method"`|The method used to control the LED display. This chooses `"show"` or `”scroll”` at random every time you send a request. |
|`"args"`|The argument data for `"method"`. |

##### `“args”` Parameters when `“method”` is `”scroll”`

|Parameter|What does it do?|
|:---|:---|
|`"string"`|The first argument of the `scroll()` method. This is fixed to JSON. |
|`"delay"`|The `delay` argument of the `scroll()` method. This is fixed to 10. |
|`"wait"`|The `wait` argument of the `scroll()` method. This is fixed to false. |
|`"loop"`|The `loop` argument of the `scroll()` method. This is fixed to false. |
|`"color"`|The `color` argument of the `scroll()` method. This picks one of seven colors at random. |

##### `“args”` Parameters when `“method”` is `”show”`

|Parameter|What does it do?|
|:---|:---|
|`"string"`|The first argument of the `show()` method. This is fixed to JSON. |
|`"delay"`|The `delay` argument of the `show()` method. This is fixed to 400. |
|`"wait"`|The `wait` argument of the `show()` method. This is fixed to false. |
|`"loop"`|The `loop` argument of the `show()` method. This is fixed to false. |
|`"clear"`|The `clear` argument of the `show()` method. This is fixed to true |
|`"color"`|The `color` argument of the `show()` method. This picks one of seven colors at random. |

These parameters are set to be picked at random when they reach the server.

Now let’s program our LED display to light up based on the content of the JSON data that we retrieve!

#### ■ Making the Program

We’ll start by writing code which connects our Core Unit to the wireless access point.

```python=1
import urequests
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
wifi = MyWiFi()

def control_display():
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return 
```

We’ll use the GET method to make the request since we don’t need to send any data here. We’ll store the data sent by the server inside of the `content` property of the `response` object returned by the `urequests` module’s `get()` method. Next, we’ll run the `response` object’s `json()` method to interpret `content` as JSON data and make it into a dictionary!

Let’s add the new code below and see how the program works so far!

###### New Code (Line 6, Lines 13-16)
```python=1
import urequests
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
URL = "https://www.artec-kk.co.jp/school/cl/textbooks/sample/json/"  # URL to send request to
wifi = MyWiFi()

def control_display():
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    res = urequests.get(URL)  # Sends the request and receives the response
    data = res.json()  # Converts JSON data into Python dictionary format
    print(data)  # Shows retrieved data
    print(type(data))  # Checks whether data has been converted from JSON to dictionary format
```

#### Program Results
<pre class="prettyprint">
&gt;&gt;&gt; control_display()
.
.
Connected.
('172.20.10.4', '255.255.255.240', '172.20.10.1', '172.20.10.1')
{'method': 'scroll', 'args': {'delay': 10, 'color': 2031647, 'string': 'JSON', 'wait': False, 'loop': False}}
&lt;class 'dict'&gt;
</pre>

You’ll see that `<class 'dict'>` has been printed on the last line, which will tell us whether the JSON data has been converted into a dictionary.

Now let’s use this data as we write the code to control our LED display!

##### Example Code 3-2-1
###### New Code (Line 18-32)

```python=1
import urequests
from pystubit.board import display  # Imports object to control the LED display
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
URL = "https://www.artec-kk.co.jp/school/cl/textbooks/sample/json/"
wifi = MyWiFi()

def control_display():
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return 
    res = urequests.get(URL)
    data = res.json()
    # print(data)  # Comment this out if you don\’t need it
    # print(type(data))
    if data["method"] == "scroll":  # If the data scrolls
        args = data["args"]
        display.scroll(args["string"],
                       delay=args["delay"],
                       wait=args["wait"],
                       loop=args["loop"],
                       color=args["color"])
    elif data["method"] == "show":  # If one character is shown at a time
        args = data["args"]
        display.show(args["string"],
                     delay=args["delay"],
                     wait=args["wait"],
                     loop=args["loop"],
                     clear=args["clear"],
                     color=args["color"]) 
```

Now your program is finished. This will make JSON appear on the display in a random color every time you call the `control_display()` function from the terminal!

## Retrieving Forecast Data

The websites of government agencies or public interest organizations publish a wealth of data free of charge. This includes changes in population, age demographics, traffic data, disaster alerts, postal codes, weather forecasts, and more.

In the second of the lesson we’re going to make a program which retrieves the weather forecast data published on the United Nation’s World Meteorological Organization website, then  shows the forecast on the LED display!

##### What’s the World Meteorological Organization?

```
The World Meteorological Organization is a specialized agency of the United Nations. It aims to improve and regulate international standards in weather forecasting
as well as promote effective exchange of climate data and documentation between member nations and regions.
The WMO is part of the United Nations Development Group and is headquartered in Geneva, Switzerland.
```
###### Source: [World Meteorological Organization (Wikipedia)](https://en.wikipedia.org/wiki/World_Meteorological_Organization)


### How to Retrieve Forecast Data

You can find a guide on how to retrieve climate data on WMO member nations at the page below:

[World Weather Information Service](https://worldweather.wmo.int/en/dataguide.html)

If we follow the instructions here, the first step is to use a GET method to send a request to this page:

```
https://worldweather.wmo.int/en/json/[City ID]_en.json
```

The `[City ID]` in this URL is where we put a unique ID number assigned to cities in every country. You can check the ID of any city by visiting the following page:

[World Meteorological Organization City List](https://worldweather.wmo.int/en/json/full_city_list.txt)

If we were to look at Japan we would find that seven cities have been registered with the following `[City ID]`:

##### City IDs for Japanese Cities

|City|`[City ID]`|
|:---|:---|
|Sapporo (`"Sapporo"`)|181|
|Sendai (`"Sendai"`)|182|
|Tokyo (`"Tokyo"`)|183|
|Nagoya (`"Nagoya"`)|355|
|Osaka (`"Osaka"`)|184|
|Fukuoka (`"Fukuoka"`)|185|
|Naha (`"Naha"`)|186|

Send a request and we’ll get the forecast data for that city in JSON format. This data is a nested dictionary which uses the `city` entry in the top layer as the key.

```
{
    "city":{}
}
```

|Key|Content|Data Structure|
|:---|:---|:---|
|`"city"`|Forecast data for target region|Dictionary object|

The value for `"city"` is also a dictionary object with multiple keys.

##### An Example of `”city”`

```
    "city":{
        "lang":"en",
        "cityName":"Tokyo",
        "cityLatitude":"35.680000000",
        "cityLongitude":"139.770000000",
        "cityId":183,
        "isCapital":true,
        "stationName":"Tokyo",
        "tourismURL":"www.jnto.go.jp",
        "tourismBoardName":"Japan National Tourist Organization",
        "isDep":false,
        "timeZone":"+0900",
        "isDST":"N",
        "member":{},
        "forecast":{},
        "climate":{}
    }
```

|Key|Content|Data Structure|
|:---|:---|:---|
|`"lang"`|Language|String|
|`"cityName"`|City Name|String|
|`"cityLatitude"`|Latitude|String|
|`"citiLongitude"`|Longitude|String|
|`"cityId"`|City ID|Integer|
|`"isCapital"`|Capital?|Boolean|
|`"station Name"`|Station/Airport Name|String|
|`"tourismURL"`|Tourism Board URL|String|
|`"tourismBoardName"`|Tourism Board Name|String|
|`"isDep"`|Dependent Nation?|Boolean|
|`"timeZone"`|Time Zone|String|
|`"isDST"`|Observing Daylight Savings Time?|String|
|`"member"`|WMO Member Data|Dictionary object|
|`"forecast"`|Weather Forecast Data|Dictionary object|
|`"climate"`|Climate Data|Dictionary object|

The values of entries like `"member"`, `"forecast"`, and `"climate"` are also dictionary objects! The `”forecast”` entry, for example, has the following keys:

##### An Example of `”forecast”`
```
        "forecast":{
            "issueDate":"2020-07-28 11:00:00",
            "timeZone":"Local",
            "forecastDay":[]
        }
```

|Key|Content|Data Structure|
|:---|:---|:---|
|`"issueDate"`|The date of issue|Text string|
|`"timeZone"`|The timezone of the date|Text string|
|`"forecastDay"`|All forecast data|List object|

`"forecastDay"` is a list object which stores seven days of forecast data (starting after the current date) as `[0]-[6]`<sup>★</sup>. Each item in this list is included in the following keys:

###### ★ Retrieve this data early and the current date will be included in the forecast data.

##### An Example of `”forecastDay”`

```
            "forecastDay":[
                {
                    "forecastDate":"2020-07-29",
                    "wxdesc":"",
                    "weather":"Rain",
                    "minTemp":"24",
                    "maxTemp":"27",
                    "minTempF":"75",
                    "maxTempF":"81",
                    "weatherIcon":1401
                },
                .
                .
                .
            ]
```

|Key|Content|Data Structure|
|:---|:---|:---|
|`"forecastDate"`|The date of the forecast|Text string|
|`"wxdesc"`|Description of the weather data|Text string|
|`"weather"`|The weather|Text string|
|`"minTemp"`|Lowest temperature (in Celsius)|Numerical value (decimals included)|
|`"maxTemp"`|Highest temperature (in Celsius)|Numerical value (decimals included)|
|`"minTempF"`|Lowest temperature (in Fahrenheit)|Numerical value (decimals included)|
|`"maxTempF"`|Highest temperature (in Fahrenheit)|Numerical value (decimals included)|
|`"weatherIcon"`|Icon number for the weather pattern/[List of Icons](http://worldweather.wmo.int/schema/WWISWeatherIconTable.pdf)|Integer value|

While we won’t be using them here, if you’d like to learn about attributes like `"member"` and `"climate"`, visit the page below which explains the meaning of each item in the JSON format.

[http://worldweather.wmo.int/en/json/WWIS_json_schema_v2.json](http://worldweather.wmo.int/en/json/WWIS_json_schema_v2.json)

From the above, you can tell that looking up tomorrow’s forecast would involve following the keys in the order of `"city"`, `"forecast"`, `"forecastDay"`, `[0]`, and `"weather"`.

While you may understand that the `"weather"` key will show you the weather forecast, there are quite a few words to describe the different kinds of weather! You can find a list of these below:

##### A List of Weather Phenomena

|Weather|Description|
|:---|:---|
|Sandstorm|A strong wind carrying clouds of sand with it, especially in a desert|
|Duststorm|A strong, turbulent wind which carries clouds of fine dust, soil, and sand over a large area|
|Sand|Sand in the atmosphere|
|Dust|Dust and other particles finer than sand in the atmosphere|
|Thunderstorms|A storm with thunder and lightning and typically also heavy rain or hail|
|Thundershowers|A shower accompanied by lightning and thunder|
|Storm|A violent disturbance of the atmosphere with strong winds and usually rain, thunder, lightning, or snow|
|Lightning|A flash of light produced by a discharge of atmospheric electricity|
|Hail|Pellets of frozen rain which fall in showers from cumulonimbus clouds|
|Blowing Snow|Snow lifted from the surface by the wind|
|Blizzard|A severe snowstorm with high winds|
|Snowdrift|A bank of deep snow heaped up by the wind|
|Snowstorm|A heavy fall of snow, especially with a high wind|
|Snow Showers|A short period of light-to-moderate snowfall|
|Flurries|A brief light snowfall|
|Snow|Atmospheric water vapor frozen into ice crystals and falling in light white flakes or lying on the ground as a white layer|
|Heavy Snow|Considerable amounts of snow falling which may cause damage|
|Snowfall|A fall of snow|
|Light Snow|Light, fluffy snow which melts easily|
|Showers|A brief and usually light fall of rain|
|Heavy Showers|A brief and heavy fall of rain|
|Rainshower|A brief rainfall, usually of variable intensity|
|Occasional Showers|Not frequent but recurrent rain|
|Scattered Showers|Any of a series of showers of rain, whose number, location and timing are impossible to predict|
|Isolated Showers|A brief, localized shower|
|Light Showers|A brief and usually light fall of rain|
|Freezing Rain|Rain that freezes on impact with the ground or solid objects|
|Rain|The condensed moisture of the atmosphere falling visibly in separate drops|
|Drizzle|Light rain falling in very fine drops|
|Light Rain|A light fall of rain|
|Fog|Vapor condensed to fine particles of water suspended in the lower atmosphere|
|Mist|A cloud of tiny water droplets suspended in the atmosphere at or near the earth's surface|
|Smoke|Smoke present in the atmosphere due to forest or other fires|
|Haze|A slight obscuration of the lower atmosphere, typically caused by fine suspended particles|
|Overcast|Clouds covering a large part of the sky|
|Sunny Intervals|Rain and clouds with occasional sunny periods|
|Clearing|Rain and clouds with occasional sunny periods|
|Sunny Periods|Rain and clouds with occasional sunny periods|
|Partly Cloudy|More clouds than sunshine (usually 2 to 1)|
|Partly Bright|More sunshine than clouds (usually 2 to 1)|
|Mild|Mild, comfortable weather|
|Cloudy|The sky is entirely covered by clouds|
|Mostly Cloudy|Most of the sky is covered by clouds|
|Bright|Fairly strong sunlight|
|Sunny|Strong sunlight|
|Fine|Comfortable weather|
|Fair|Clear weather with few to no clouds|
|Clear|A cloudless, sunny day|
|Windy|Strong gusts of wind|
|Squall|A sudden violent gust of wind or localized storm, especially one bringing rain, snow, or sleet|
|Stormy|Characterized by strong winds and usually rain, thunder, lightning, or snow|
|Freezing|Freezing cold temperatures|
|Volcanic Ash|A fall of volcanic ash|

You can imagine that most regions of the Earth won’t need to use that many of these! If your program retrieves a word you’re not familiar with, try looking it up in this list!

### Making the Program

Now let’s make a program which retrieves the weather data we need and shows tomorrow’s (or today’s, depending on the time) weather, low temperature, and high temperature in the terminal!

#### ■ Connecting to Wi-Fi

We’ll put the process which retrieves the forecast data inside of the `get_forecast()` function. In order to use this function for multiple cities, we’ll have it take the city ID as an argument. If your Core Unit is disconnected from Wi-Fi, the program will start by connecting to the access point!

```python=1
import urequests
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
wifi = MyWiFi()

def get_forecast(city_id):  # Takes a unique city ID number as an argument 
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return 
```

#### ■ Retrieving the Forecast Data

The URL we send the request to will depend on the city. While we can’t predict every pattern that might appear, we’ll make one common format as shown here:

```
"https://worldweather.wmo.int/en/json/[City ID]_en.json"
```

This format will create the URL by replacing the `[City ID]` portion with the city ID we set in the argument of the `get_forecast()` function.

###### New Code (Line 6, Lines 13-14)
```python=1
import urequests
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
url_format = "https://worldweather.wmo.int/en/json/[City ID]_en.json"  # Shared URL format for all cities
wifi = MyWiFi()

def get_forecast(city_id):
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return 
    url = url_format.replace("[City ID]", str(city_id))  # City IDs are integers, so make sure to convert them into text strings
    res = urequests.get(url)
```

#### ■ Showing the Result in the Terminal

The `response` object’s `json()` method will convert our JSON data into a dictionary object and extract the city name, date (tomorrow or today), weather, and low as well as high temperatures. These five pieces of information will then appear as a message in the terminal!

##### Example Code 4-2-1
###### New Code (Lines 15-23)
```python=1
import urequests
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
url_format = "https://worldweather.wmo.int/en/json/[City ID]_en.json"
wifi = MyWiFi()

def get_forecast(city_id):
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return 
    url = url_format.replace("[City ID]", str(city_id))
    res = urequests.get(url)
    data = res.json()
    name = data["city"]["cityName"]  # The name of the city
    date = data["city"]["forecast"]["forecastDay"][0]["forecastDate"]  # The forecast date
    weather = data["city"]["forecast"]["forecastDay"][0]["weather"]  # The weather
    hi_temp = data["city"]["forecast"]["forecastDay"][0]["maxTemp"] # The high temperature
    lo_temp = data["city"]["forecast"]["forecastDay"][0]["minTemp"] # The low temperature
    message = "City: {}\nDate: {}\nWeather: {}\nHigh: {}\nLow: {}"  # The message to show in the terminal
    message = message.format(name, date, weather, hi_temp, lo_temp)
    print(message)  # Shows the message
```

#### Testing the Program

Before running our finished program, let’s pick a city to get the forecast data for (this could be your own city or a city close to you) and look up its city ID on the page below:

[https://worldweather.wmo.int/en/json/full_city_list.txt](https://worldweather.wmo.int/en/json/full_city_list.txt)

Now run the program and call the `get_forecast()` function with the city ID as an argument!


##### Running the Code
###### ★ 183 is the city ID for Tokyo, Japan.
<pre class="prettyprint">
&gt;&gt;&gt; get_forecast(183)
City: Tokyo
Date: 2020-07-29
Weather: Rain
High: 27
Low: 24
</pre>


## Challenge: Showing Icons for Weather Patterns

Example Code 4-2-1 shows the forecast data we retrieve in the terminal. In this challenge we’re going to make icons which show different weather patterns and modify our program to show these icons on the LED display!

### Hint: Defining Icons

In the previous section you saw that there are quite a few words to describe different weather phenomena. It would be a lot of work to make icons for all of them, so let’s try making five patterns of weather instead!

|Pattern|Phenomena|
|:---|:---|
|Fine|No Rain, Fine, Sunny, Clear, Clearing, Bright, Fair, Mild|
|Rainy|Thunderstorms, Thundershowers, Storm, Showers, Heavy Showers, Rainshower, Occasional Showers, Scattered Showers, Isolated Showers, Light Showers, Freezing Rain, Rain, Drizzle, Light Rain, Squall|
|Cloudy|Cloudy, Partly Cloudy, Sunny Intervals, Mostly Cloudy, Partly Bright|
|Snowy|Snow, Flurries, Heavy Snow, Snowfall, Light Snow,　Blizzard, Blowing Snow, Hail, Snow Showers, Snowstorm, Snowdrift|
|Other|Sandstorm, Volcanic Ash, etc.|

Now let’s set an icon for each of these five patterns.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-2/11-2_4_E.png"/>

### Making the Program

We’ll take the following steps to modify our program:

#### ■ Defining Each Weather Pattern

We’ll start by defining each pattern of weather. Let’s do this by dividing each collection of weather phenomena into groups. We won’t need to define our Other group since it’s phenomena don’t fit into the four main patterns.

###### New Code (Lines 10-20)
```python=1
import urequests
from mywifi import MyWiFi           

SSID = "SSID"
PASSWORD = "PASSWORD"
url_format = "https://worldweather.wmo.int/en/json/[City ID]_en.json"
wifi = MyWiFi()

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
```

#### Defining the Icons

Next, let’s define the icons which represent each weather pattern.

New Code (Line 2, Lines 23-27)

```python=1
import urequests
from pystubit.board import Image  # Imports Image class to make images
from mywifi import MyWiFi           

SSID = "SSID"
PASSWORD = "PASSWORD"
url_format = "https://worldweather.wmo.int/en/json/[City ID]_en.json"
wifi = MyWiFi()

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

# The pattern icons
IMG_FINE = Image("01110:11111:11111:11111:01110:", color=(30, 10, 0))  # Fine image
IMG_RAINY = Image("00100:01110:11111:00100:01100:", color=(0, 10, 30))  # Cloudy image
IMG_CLOUDY = Image("00000:01110:11111:01110:00000:", color=(15, 15, 15))  # Cloudy image
IMG_SNOWY = Image("10101:01110:11011:01110:10101:", color=(30, 30, 30))  # Snowy image
IMG_OTHER = Image("00000:00000:01110:00000:00000:", color=(10, 5, 15))  # Other image
```

#### ■ Returning Icons Based on Patterns

Now let’s make a function which returns an icon based on the pattern set in its argument.

###### New Code (Lines 29-40)

```python=23
IMG_FINE = Image("01110:11111:11111:11111:01110:", color=(30, 10, 0))
IMG_RAINY = Image("00100:01110:11111:00100:01100:", color=(0, 10, 30))
IMG_CLOUDY = Image("00000:01110:11111:01110:00000:", color=(15, 15, 15))
IMG_SNOWY = Image("10101:01110:11011:01110:10101:", color=(30, 30, 30))
IMG_OTHER = Image("00000:00000:01110:00000:00000:", color=(10, 5, 15)) 

def get_icon(weather):
    if weather in WORDS_FINE:  # Uses an in operator to check if the weather word is included in the group
        img = IMG_FINE  # Returns an image of the corresponding icon
    elif weather in WORDS_RAINY:
        img = IMG_RAINY
    elif weather in WORDS_CLOUDY:
        img = IMG_CLOUDY
    elif weather in WORDS_SNOWY:
        img = IMG_SNOWY
    else:
        img = IMG_OTHER  # Weather is other if it doesn\`t fit into any other group
    return img
```

#### Showing the Icons
Last, let’s show the icon for the weather pattern on the Led display. Now your program is finished. Now run the program and see how it works!

##### Example Code 5-2-1
###### New Code (Line 55, Line 57)
```python=1
import urequests
from pystubit.board import Image, display
from mywifi import MyWiFi           

SSID = "SSID"
PASSWORD = "PASSWORD"
url_format = "https://worldweather.wmo.int/en/json/[City ID]_en.json"
wifi = MyWiFi()

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

def get_forecast(city_id):
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return 
    url = url_format.replace("[City ID]", str(city_id))
    res = urequests.get(url)
    data = res.json()
    name = data["city"]["cityName"]
    date = data["city"]["forecast"]["forecastDay"][0]["forecastDate"]
    weather = data["city"]["forecast"]["forecastDay"][0]["weather"]
    max_temp = data["city"]["forecast"]["forecastDay"][0]["maxTemp"]
    min_temp = data["city"]["forecast"]["forecastDay"][0]["minTemp"]
    message = "City: {}\nDate: {}\nWeather: {}\nHigh: {}\nLow: {}"
    message = message.format(name, date, weather, hi_temp, lo_temp)
    img = get_icon(weather)  # Retrieves the icon
    print(message)
    display.show(img, delay=5000, clear=True)  # Shows the icon for 5 seconds
```

## Conclusion
### A Quick Review
In this lesson we practiced using the **POST method in HTTP** and learned about the **JSON** data format, one of the most widely-adopted formats for sharing data between web applications.

We then applied what we learned to make a program which retrieves weather forecasts for a designated city from the WMO website!

Aside from the WMO, there are plenty of public and private institutions which store data on their websites in the JSON format. If you’re interested, try searching these websites to see what kind of data you can find!

### Coming Up Next Time!

In the next lesson we’re going to learn about **NTP**, or the **N**etwork **T**ime **P**rotocol. This communication protocol uses a network to keep your computer’s clock up to date!