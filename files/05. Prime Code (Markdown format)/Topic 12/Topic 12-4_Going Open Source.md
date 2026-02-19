---
tags: Python_English, Topic 12
---

Python Robotics Course Lesson 48

Topic 12-4
Going Open Source
===

Use open source code to develop your own programs more efficiently!

## In This Lesson...

Learn how to install open source MicroPython modules and packages available on the Internet onto your Core Unit. We’ll then use the module’s we’ve installed to make a simple program. At the same time, learn what it means for code to be **open source** and about different kinds of open source licenses.


:::info

Lesson Introduction

■ Lesson 48 Contents
1. Defining Open Source and Open Source Licenses
2. Installing Open Source Packages on the Core Unit 
3. Using Saved Files and Directories on the Core Unit
4. Challenge: Ever More Practical Open Source Packages


In this lesson we’re going to introduce you to the concept of program development using open source material freely available on the Internet.

We’ll start the lesson by learning what open source means as well as the licensing you should keep in mind while using open source material!

Next, we’ll learn how to connect the Core Unit to the Internet in order to install open source packages for MicroPython. Well then try out installing a simple package which lets us search for reserved keywords in MicroPython!

To continue, we’ll introduce you to a module which lets you use a program to work with the files and directories on your Core Unit. If you plan on installing multiple packages, the module we introduce here will be a big help in keeping your data organized!

In the final challenge we’ll install and use an even more practical open source package which lets you schedule programs to run any time you want!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

This is everything we’ll be learning in this topic. Now let's start the lesson!

:::

## Open Source Code

The text used to write a program in a programming language is often called the program’s **source**, its **code**, or its **source code.** In order to turn source code it something a computer can actually run, first it has to be converted into a binary-encoded machine language file. This conversion process is called **compiling** or **assembling** code.

The simplest definition of the term **open source** is source code that someone has published with a license that lets other people freely use that code. The Open Source Initiative, an organization that promotes open source software, defines open source like this:

##### The Definition of Open Source
1. **Free Redistribution**
The license shall not restrict any party from selling or giving away the software as a component of an aggregate software distribution containing programs from several different sources. The license shall not require a royalty or other fee for such sale.
2. **Source Code**
The program must include source code, and must allow distribution in source code as well as compiled form. The source code must be the preferred form in which a programmer would modify the program. Deliberately obfuscated source code is not allowed.
3. **Allows Derived Works**
The license must allow modifications and derived works, and must allow them to be distributed under the same terms as the license of the original software.
4. **Integrity of the Author’s Source Code**
The license may restrict source-code from being distributed in modified form only if the license allows the distribution of "patch files" with the source code for the purpose of modifying the program at build time. The license must explicitly permit distribution of software built from modified source code. The license may require derived works to carry a different name or version number from the original software.
5. **No Discrimination Against Persons or Groups**
The license must not discriminate against any person or group of persons.
6. **No Discrimination Against Fields of Endeavor**
The license must not restrict anyone from making use of the program in a specific field of endeavor.  For example, it may not restrict the program from being used in a business, or from being used for genetic research.
7. **Distribution of License**
The rights attached to the program must apply to all to whom the program is redistributed without the need for execution of an additional license by those parties.
8. **License Must Not Be Specific to a Product**
The rights attached to the program must not depend on the program's being part of a particular software distribution.
9. **License Must Not Restrict Other Software**
The license must not place restrictions on other software that is distributed along with the licensed software. For example, the license must not insist that all other programs distributed on the same medium must be open-source software.
10. **License Must Be Technology-Neutral**
No provision of the license may be predicated on any individual technology or style of interface.
###### The Open Source Definition (Open Source Initiative): https://opensource.org/osd (accessed 2020-09-24)

Searching for open source code online will lead you to all kinds of high-quality open source programs developed by programmers from across the globe. These open source programs can be incredibly useful for efficiently developing your own software, but you do need to pay attention to the license of any open source code you decide to use. If you violate the code’s license, you may get in legal trouble with the copyright holder!

There are many different kinds of open source license. Here are a few major ones:

##### Some Open Source Licenses
* **GPL (General Public License)**
Source code published with this license can be freely obtained, used, edited, and redistributed. However, if you use code with a GPL license to make a derivative work by editing it or using it as part of your own program, the derivative work you create must also be open source and compliant with GPL.
* **BSD (Berkeley Software Distribution) License**
Like GPL, source code published with this license can be freely obtained, used, edited, and redistributed, but unlike GPL, derivative works don’t have to be compliant with the same license. This license makes clear that the developers and distributors of source code using it give no guarantees and take no responsibility for any problems that occur when using that code. It also requires the license and copyright be displayed on redistributions of the source code and forbids use of the developer or distributor’s name in advertising for derivative works without express permission.
* **MIT License**
This license is similar to the BSD license, but simpler. Source code with this license can be used freely, redistributions must display the copyright and license, and copyright holders take no legal responsibility for its use.
* **MPL (Mozilla Public License)**
This license is similar to GPL. Code with this license can be used with files protected by other licenses and unpublicized code you’ve made yourself, but any source code protected by MPL will always remain under the MPL license and remain as accessible source code.

Not all open source code lets you do whatever you want with it - you have to follow the rules of the license that code uses.

## Installing Modules and Packages with the upip Module

In this chapter we’ll explain how you can install MicroPython packages from the open source Python package repository<sup>★</sup> **PyPI (Python Package Index)**.

###### “Repository” is a term used in the IT field to describe data storage/management locations.

First go to the PyPI website at the URL below.

[PyPI: The Python Package Index](https://pypi.org/)

You’ll see there’s a search bar in the middle of this page. You can put keywords into this search bar to find a selection of packages related to that keyword. Most of the packages on PyPl are made for regular Python, so unfortunately you’ll find a lot of useful packages that won’t work on MicroPython. Some of the packages you can use with MicroPython are ports of existing packages for regular Python, so they’ll have **micropython** attached to the beginning of their names to distinguish them from the original version. Try typing micropython into the search bar and see what you find!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-4/12-4_1_E.png"/>

With the default settings, your search results will show you a selection of packages in order of relevant to your keyword. You’ll find over 1000 packages (but still a small number compared to how many packages are in the repository). It would take a lot of time to look through all of these, so let’s start small by picking out a simple package and installing it.

### Installing the `micropython-keyword` Package

The package we’re going to install is called **micropython-keyword**. This package includes a version of Python’s `keyword` module that’s been ported to MicroPython. The `keyword` module has a function it in you can use to check whether a text string is a reserved keyword in Python.

Type **micropython-keyword** into the search bar in the upper-lefthand corner of your search results page to do another search! The package we’re looking for should be the first result.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-4/12-4_2_E.png"/>

The numbers after **micropython-keyword** in the name of the file indicate what version of the package this is. This number gets updated whenever the developer expands the contents of the package or fixes errors they find in the source code. As of September 2020 (when this lesson was written), the latest version of this package is **3.3.3**. Now click the top link to go on to the next page.

On this page you can see more information about the package. For example, under the package name you’ll see the text `pip install micropython-keyword`, which is the command you need to run to install the package. You can actually install packages from PyPI just by running a one-line command starting with `pip` like this one from your terminal/command prompt. These commands can’t be used in MicroPython, but the program you use to install them is just as simple!

Clicking **Homepage** in the navigation bar on the left side of the page will take you to the page for the project that develops this package, where you can find more detailed information.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-4/12-4_3_E.png"/>

Now let’s go ahead and install this package. First, connect your Core Unit to the Internet. You can do this using the `mywifi` module from Topic 11-1 to connect to your classroom’s wireless access point.

```python=1
from mywifi import MyWiFi

SSID = "ssid name"  # Your Wi-Fi’s SSID name
PASSWORD = "password"  # Your Wi-Fi password
wifi = MyWiFi()  # Creates unique class object

if wifi.isconnected() or wifi.connect(SSID, PASSWORD):  # If you successfully connect to the wi-fi or are already connected
    pass # Write the package install code here
else:  # If you fail to connect to the access point
    print("Failed to connect.")
```

Once you’ve connected to Wi-Fi, use the **`upip` module** to install the package. The `upip` module has a function in it called `install()` which will automatically install the package you want if you set its name and the pass for the place you want to install it in the function’s arguments.

```
upip.install(Package Name, Pass)
```

Now, import the `upip` module and add some code to install `micropython-keyword`.

###### New Code (Lines 9-11)
```python=1
from mywifi import MyWiFi
import upip

SSID = "ssid name"  # Your Wi-Fi’s SSID name
PASSWORD = "password"  # Your Wi-Fi password
wifi = MyWiFi()  # Creates unique class object

if wifi.isconnected() or wifi.connect(SSID, PASSWORD):
    package = "micropython-keyword"  # Name of the package you want to install
    path = "/"  # Pass for the location where you want to install the package Set it to your current directory for now.
    upip.install(package, path)  # Install the package
else:
    print("Failed to connect.")
```

Run this program to start the installation! While the installation is happening, the following message will appear on screen.

<pre class="prettyprint">
Installing to: /
Warning: pypi.org SSL certificate is not validated
Installing micropython-keyword 3.3.3.post1 from https://files.pythonhosted.org/packages/e5/c3/fe4afa66833f988a68b393988175b6a0ededfea546bdd6c0438b4cad41b2/micropython-keyword-3.3.3.post1.tar.gz
</pre>

Next let’s check to make sure the package was installed correctly. Select **Files** in the Mu Editor **after pressing the Core Unit’s Reset button**. The item **keyword.py** on the list in the left side of the window you’ve opened is the file/package you just installed.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-4/12-4_4_E.png"/>

Let’s take a look at the contents of this file. Click on the file’s name and drag it from the left side of the window to the right side. This will transfer it from the Core Unit to the **mu_code** folder on your computer.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-4/12-4_5_E.png"/>

Once you’ve moved the file above on to your computer, try opening it in Mu Editor, using the **Open** option from the Mu menu. The contents of keyword.py look like this:

##### The keyword.py Program
```python=1
#! /usr/bin/env python3

"""Keywords (from "graminit.c")

This file is automatically generated; please don't muck it up!

To update the symbols in this file, 'cd' to the top directory of
the python source tree after building the interpreter and run:

    ./python Lib/keyword.py
"""

__all__ = ["iskeyword", "kwlist"]

kwlist = [
#--start keywords--
        'False',
        'None',
        'True',
        'and',
        'as',
        'assert',
        'break',
        'class',
        'continue',
        'def',
        'del',
        'elif',
        'else',
        'except',
        'finally',
        'for',
        'from',
        'global',
        'if',
        'import',
        'in',
        'is',
        'lambda',
        'nonlocal',
        'not',
        'or',
        'pass',
        'raise',
        'return',
        'try',
        'while',
        'with',
        'yield',
#--end keywords--
        ]

frozenset = set
iskeyword = frozenset(kwlist).__contains__

def main():
    import sys, re

    args = sys.argv[1:]
    iptfile = args and args[0] or "Python/graminit.c"
    if len(args) > 1: optfile = args[1]
    else: optfile = "Lib/keyword.py"

    # scan the source file for keywords
    with open(iptfile) as fp:
        strprog = re.compile('"([^"]+)"')
        lines = []
        for line in fp:
            if '{1, "' in line:
                match = strprog.search(line)
                if match:
                    lines.append("        '" + match.group(1) + "',\n")
    lines.sort()

    # load the output skeleton from the target
    with open(optfile) as fp:
        format = fp.readlines()

    # insert the lines of keywords
    try:
        start = format.index("#--start keywords--\n") + 1
        end = format.index("#--end keywords--\n")
        format[start:end] = lines
    except ValueError:
        sys.stderr.write("target does not contain format markers\n")
        sys.exit(1)

    # write the output file
    fp = open(optfile, 'w')
    fp.write(''.join(format))
    fp.close()

if __name__ == "__main__":
    main()
```

Lines 15-51 define a list of all the reserved keywords in MicroPython, and lines 53-54 define a function called `iskeyword`. This part is a little hard to follow, so we’ll walk you through it.

```python=53
frozenset = set
iskeyword = frozenset(kwlist).__contains__
```

First take a look at line 53. In Python, there are mutable **sets** and immutable **frozensets**. Mutable refers to an object which can be changed, while immutable means an object which can’t be changed! In the regular Python version of the `keyword` module, the `iskeyword` function’s definition uses the frozenset data type, but MicroPython has no frozenset data type! The code `frozenset = set` replaces that data type with the set type.

Next, on line 54, the list of reserved keywords is creates as a set-type object, and the special method `__contains__` for set-type objects is defined as the `iskeyword` function.

The `__contains__` method is works with the operator `in`, which is used to check whether a specified piece of data exists inside a specified set. This means that running the code `iskeyword("string")` will work the same way as running `"string" in frozenset(kwlist)`, and give you a result of either `True` or `False`.

The code from line 56 on can read another file that defines reserved keywords and use it to overwrite the list of reserved keywords.

Now start a new program file and try importing and using the `keyword` module! Let’s try using the `iskeyword` function to check whether the strings  `"python"` and `"class"` are reserved keywords in MicroPython.

##### Example Code 3-1-1
```python=1
import keyword

print(keyword.iskeyword("python"))
print(keyword.iskeyword("class"))
```

##### Program Results
<pre class="prettyprint">
False
True
</pre>

The only thing this package we’ve installed does is let you check whether something is a reserved keyword, but when you think of all the work you’d need to do to write your own program that does the same thing, it should be clear how useful packages like this can be. The `"micropython-keyword"` package we’ve installed here uses an **MIT license**. This means that as long as you remember to include the license and the name of the copyright holder with it, you can freely edit and redistribute it! Make good use of open source code like this to improve your programming efficiency!

### Directories and File Management

The term “directory” might sound strange and technical, but they do basically the same thing as the folders you use on your computer! Directories can store all kinds of files (text, image, sound, etc.) and even other directories.

The more packages you install, the more files you’ll have lying around, so it’s a good idea to keep them organized by sorting related packages into directories and deleting unneeded files from your directories.

There’s a module called `uos` you can use to do this! The `uos` module has functions you can use to create and delete directories, move and rename files, and access the file system that lets you delete things. Have a look at these functions in the tables below!

##### Directory and File Management Functions in the `uos` Module

|Function|What does it do?|
|:---|:---|
|`chdir(path)`|Change the current directory to the directory with the specified path |
|`getcwd()`|Find the current directory’s path |
|`listdir()`|Get a list of files and directories inside of the current directory |
|`mkdir(path)`|Make a new directory at the specified path |
|`remove(path)`|Delete the file at the specified path |
|`rmdir(path)`|Delete the directory at the specified path |
|`rename(old_path, new_path)`|Change a file’s name |

###### [Source: `uos` - basic “operating system” services] (https://docs.micropython.org/en/latest/library/uos.html)

Now let’s try opganizing some files and directories ourselves using these functions.

#### ■ Checking and Changing Your Current Directory

Let’s say you want to use a command in the Mu Editor’s terminal to import a particular Python file/module. You’d use a `from` statement that specifies the path from your current directory to the location of that file. For example if you wanted to import `body.py` from the `pyatcrobo2 directory inside of the Core Unit’s `lib` directory, you’d write the code like this:
<pre class="prettyprint">
&gt;&gt;&gt; from lib.pyatcrobo2 import body
&gt;&gt;&gt;
</pre>

This will import the file you want, no problem! Now, what if you leave out the `from` statement?

<pre class="prettyprint">
&gt;&gt;&gt; import body
Traceback (most recent call last):
  File "&lt;stdin&gt;", line 1, in &lt;module&gt; 
ImportError: no module named 'body'
</pre>

This time you got an error! This is because the program specified a directory other than the directory of the file!

This is because the `from` statement specifies the **path to the file’s directory from the current directory**. This is what’s known as a **relative path**. Since the terminal starts in the top, or **root**, directory by default, leaving out the `from` statement makes the program look for `body.py` in the top directory!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-4/12-4_6_E.png"/>

You can check which directory you’re currently in using the `getcwd()` command. Type the code below into your terminal and try it out for your self:

<pre class="prettyprint">
&gt;&gt;&gt; import uos
&gt;&gt;&gt; uos.getcwd()
'/'
</pre>

`’/’` indicates the root directory. Now let’s use the `chdir()` function to go to the `pyatcrobo2` directory and run the `getcwd()` function again! While we added `.` to the `from` statement in order to show a lower directory, these are shown in the `chdir()` function by putting a `/` in its argument.

<pre class="prettyprint">
&gt;&gt;&gt; uos.chdir("/lib/pyatcrobo2")
&gt;&gt;&gt; uos.getcwd()
'/lib/pyatcrobo2'
</pre>

Since we’ve moved to the `pyatcrobo2` directory, you can now import any file in this directory without having to use a `from` statement! Now let’s try importing `const.py` ourselves:

<pre class="prettyprint">
&gt;&gt;&gt; import const
&gt;&gt;&gt; 
</pre>

Now we can import the file with no problems at all! If you want to return to the root directory, just run the `chdir()` function again:

<pre class="prettyprint">
&gt;&gt;&gt; uos.chdir("/")
&gt;&gt;&gt; uos.getcwd()
'/'
</pre>

You may have noticed already, but we had to specify the relative path to the current directory in our `from` statements, while we had to specify the path starting with the root directory in the `chdir()` function. A path which starts with the root directory is known as an **absolute path**. You can use any function in the `uos` module, including `chdir()` to specify both absolute and relative paths. Absolute paths always start with a `/`, while paths that don’t are relative paths:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-4/12-4_7_E.png"/>


#### ■ Showing Files and Directories in the Current Path

Run the `listdir()` function and you can retrieve a list of all of the files and directories inside of the current path.

<pre class="prettyprint">
&gt;&gt;&gt; uos.listdir()
['boot.py', 'lib', 'config.json', 'usr', 'select.py', 'main.py', 'upip_helper.py', 'util.py', 'radio.py', 'button.py', 'tilt.py', 'util_atrobo2.py', 'mywifi.py', 'keyword.py']
</pre>

You can also set the directory whose contents you want to see in the argument of the `listdir()` function. The code below specifies an absolute path:

<pre class="prettyprint">
&gt;&gt;&gt; uos.listdir("/lib/pyatcrobo2")
['body.py', 'const.py', 'parts.py', 'wire.py', '__init__.py']
</pre>

#### ■ Making a Directory

We can use the `mkdir()` function in order to make a new directory. Running the code below will make a directory named `lib` and another directory named `mylib` inside of it!

<pre class="prettyprint">
&gt;&gt;&gt; uos.mkdir("/lib/mylib")
</pre>

If you click the **Files** button in Mu Editor, empty directories will be hidden. Let’s run the `listdir()` to see if we actually managed to create the `mylib` directory.

<pre class="prettyprint">
&gt;&gt;&gt; uos.listdir("/lib")
['pyatcrobo2', 'pystubit_iot', 'mylib']
</pre>



#### ■ Moving a File
We can use the `rename()` function in order to move files. You can also use this function to change the name of a file! Let’s try changing the name of the `keyword.py` package we installed to `mykeyword.py` and moving it to the `mylib` directory:

<pre class="prettyprint">
&gt;&gt;&gt; uos.rename("keyword.py", "/lib/mylib/mykeyword.py")
</pre>

Now click the **Files** button in Mu Editor and see if you can find `mykeyword.py` inside of the `mylib` directory!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-4/12-4_8_E.png"/>

#### ■ Deleting Files
If we want to delete a file, we can use the `remove()` function. Now let’s use this function to delete the `mykeyword.py` file that we just renamed and moved!
<pre class="prettyprint">
&gt;&gt;&gt; uos.remove("/lib/mylib/mykeyword.py")
&gt;&gt;&gt; uos.listdir("/lib/mylib")
[]
</pre>

#### ■ Deleting a Directory

You can delete an empty directory by using the `rmdir()` function. Let’s try using this function to delete the `mylib` directory:

<pre class="prettyprint">
&gt;&gt;&gt; uos.rmdir("/lib/mylib")
&gt;&gt;&gt; uos.listdir("/lib")
['pyatcrobo2', 'pystubit_iot']
</pre>

Your Core Unit has a lot less storage space than your PC, and regularly cleaning up files and directories like this is a great way to keep your data organized!

## Challenge: Even More Practical Packages

In this challenge we’re going to install a new package on PyPi called **micropython-scron**.

[micropython-scron on PyPI](https://pypi.org/project/micropython-scron/)

### The `micropython-scron` Package

If you have jobs you want performed regularly, the `micropython-scron` package contains functions and methods which let you specify multiple days and times (including the hour, minute, and second) you want these jobs to run!

This package was inspired by **CRON**. CRON is the name of a process used on UNIX-like systems like Mac OS and Linux to run jobs automatically. `micropython-scron` is published under a GNU Affero General Public License.

Installing this package will automatically create a directory called `scron` with a file called `week.py` inside of it.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_12-4/12-4_9_E.png"/>

The `simple_cron` object defined in this file contains an `add()` method which you can use to schedule the functions and methods you wish to run, and a `run()` method which you can use to start the job!

##### An Example of Simple CRON
```python=1
from scron.week import simple_cron

# This is the function you want to schedule
def callback_func(scron_obj, callback_name, pointer, memory):
    pass
    # The simple_cron object will be passed to scron_obj
    # The "callback_name” set in the function of the add argument will be passed to callback_name
    # A (day, hour, minute, second) tuple containing the called time will be passed to pointer
    # memory is a dictionary which gets passed the same object every time it`s called
    # meaning that it can be used to show the runtime history and similar items

simple_cron.add(
    “callback_name",  # The name you want to set for this process
    callback_func,  # The function or method you wish to run
    weekdays=range(0, 7),  # Set the day (Monday is 1, Sunday is 7) as an integer or sequence data
    hours=range(0, 24),  # Set the hour from 0-23 as an integer or sequence data
    minutes=range(0, 60),  # Set the minute from 0-59 as an integer or sequence data
    seconds=range(0, 60)  # Set the second from 0-59 as an integer or sequence data
)
simple_cron.run()  # Starts the job
```

The function or method set here will run based on the time of the Core Unit’s real time clock. The code we would use to check the value of the light sensor every minute of the day and show it on the LED display would look like this:


##### Example Code 4-1-1

```python=1
from pystubit.board import display, lightsensor 
from scron.week import simple_cron

def scroll_lightval(scron_obj, callback_name, pointer, memory):
    val = lightsensor.get_value()  # Get values from the Light Sensor
    display.scroll(str(val), delay=25, wait=False, color=(31, 31, 31))  # Show retrieved values on the display
    print(pointer)  # Shows the current day and time
    memory[pointer] = val  # Logs the value of the light sensor in a tuple which uses the current day and time as a key
    print(memory)  # Shows the log

simple_cron.add(
    "scroll lightsensor value on display",
    scroll_lightval,
    weekdays=range(0, 7),  # All days of the week (1-7)
    hours=range(0, 24), # All hours of the day (0-23)
    minutes=range(0, 60), # All minutes in an hour (0-59)
    seconds=0  # 0 seconds
)
simple_cron.run()
```

#### Program Results
<pre class="prettyprint">
>>> (5, 0, 1, 0)
{(5, 0, 1, 0): 3885}
(5, 0, 2, 0)
{(5, 0, 2, 0): 3882, (5, 0, 1, 0): 3885}
(5, 0, 3, 0)
{(5, 0, 3, 0): 3885, (5, 0, 2, 0): 3882, (5, 0, 1, 0): 3882}
</pre>

While we did something similar with the `Timer` class we learned about in Topic 9-1, this package allows us to schedule multiple days and times and easily log our data. This makes it incredibly convenient! Now try installing the `micropython-scron` package we introduced in the last section for yourself. Once you've installed the package, try referencing Example Code 4-1-1 to make a simple program of your own!

## Conclusion
### A Quick Review
In this lesson we learned about the definition and purpose of open source code as well as the different sorts of licenses this code is published under. We then learned how to install open source MicroPython packages available on PyPI, or the Python Package Index by installing two packages of our own!

Using open source code effectively can help you develop your own programs much more efficiently, and getting the chance to look at the source code of more experienced programmers is a great way to improve your own programming skills! You can also upload the packages you’ve developed yourself to PyPI, so why not give it a try when you’re more comfortable with programming in Python?

This lesson marks the end of the Prime Code Python Robotics Course. Congratulations on sticking with it all the way to the end! It’s been a long road, starting with basic Python syntax, moving onto building robots and programming games, and ending with you taking on the challenge of communicating over a network, but through it all you’ve gathered the essential knowledge that any future engineer needs to have. Technology is constantly evolving which means there are a lot of things left for you to learn, but one of the best ways to learn is picking something you want to make and acquiring the knowledge you need to make it as you go along!

And that does it for our final lesson. The creators of the Prime Code Python Robotics Course wish you all the best in achieving your dreams as an engineer!
