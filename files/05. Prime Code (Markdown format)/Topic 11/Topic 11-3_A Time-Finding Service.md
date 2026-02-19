---
tags: Python_English, Topic 11
---

Python Robotics Course Lesson 43

Topic 11-3
A Time-Finding Service
===

Get an accurate time for your Core Unit!

## In This Lesson...

In this lesson we’re going to learn about a new communication protocol called **NTP** or, the **N**etwork **T**ime **P**rotocol. We’ll then make a digital clock by setting an accurate time on your Core Unit’s **RTC**, or **R**eal **T**ime **C**lock!



::: info

Lesson Introduction

■ Lesson 43 Contents
1. Communication Protocols on the Internet
2. Syncing Time with NTP
3. Making a Self-Correcting Digital Clock
4. Challenge: Adding an Alarm Function


In Topic 11-1 and 11-2 we practiced using the HTTP communication protocol.
In this lesson we’re going to introduce a new protocol known as NTP.

At the beginning of the lesson we’ll going into a detailed explanation about the application, transport, Internet, and network interface layers which make up the Internet.

We’ll then practice using the NTP, a protocol which exists in the application layer just like HTTP! Using NTP allows you to connect to a network and set your computer’s internal clock to an accurate time.

This is what we’ll use to make a digital clock which periodically corrects itself using NTP! We’ll also make it show this accurate time, just like a smartphone.

Last, we’ll take on the challenge of adding an alarm function to the digital clock!

★ If you get an error when trying to run these programs try the methods below:

1. Close the REPL window before clicking **Run**...
2. Or wait until **from pystubit.board import display** is processed in your program before clicking **Run**!

Now let's start the lesson!

:::

## Communication Protocols on the Internet

In this lesson we’ll be studying the Network Time Protocol. This communication protocol was developed to sync the clocks of any computers on a network. Thanks to NTP, the computers and smartphones we use every day can get the most accurate measurement of time!

This protocol exists on the **application layer** of the layered DARPA Model Internet structure we introduced in Topic 11-1.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_1_E.png"/>

In this section we’re going to to take a look at the functions of each layer and the communication protocols they play host to!

### The Role of the Four Layers

Let’s start by taking a look at the picture below to get a general idea of which roles these layers play.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_2_E.png"/>

### Communication Protocols by Layer

Now let’s take a closer look at the communication protocols which exist on each layer!

#### ■ Application Layer Protocols

The communication protocols in the application layer control the format and order of data transmitted between software closest to the end-user like web browsers and e-mails. This layer is also used to format text code and images as well as to encrypt data.

##### Application Layer Protocols

|Protocol|Official Name|What does it do?|
|:---|:---|:---|
|HTTP|Hyper Text Transfer Protocol|Determines how web browsers communicate with servers. In addition to HTML documents, you can use it to send and receive images, audio, video, JavaScript programs, and other content on a website! |
|SMTP|Simple Mail Transfer Protocol|Allows you to send e-mails over the Internet. |
|POP3|Post Office Protocol|Allows a user to retrieve any e-mails addressed to them on an e-mail server. |
|FTP|File Transfer Protocol|Determines how files are transferred between a client and server. You can also use FTP to upload website data to a web server. |
|Telnet|Teletype network|Allows control of geographically distant devices like servers and routers through the Internet. All communication is done in unencrypted plain text, and many people choose to use SSH instead due to security issues. |
|SSH|Secure Shell|Uses techniques like encryption and authentication to securely transmit data between devices in remote locations. This is used in place of telnet to send unencrypted data. |
|NTP|Network Time Protocol|This communication protocol was developed to sync the clocks of any computers on a network. It defines the communication method between the NTP server broadcasting the time and the client having its time synced. |
|SSL/TLS|Secure Sockets Layer/Transport Layer Security|Used when secure communication is required on the network. This protocol encrypts data and authenticates security certificates to prevent spoofing. While TLS has replaced SSL in the current day, it’s still generally referred to as SSL. |
|DNS|Domain Name System|Communicates with the DNS server which converts domain names into IP addresses. |

While we took a close look at the specifications of HTTP in Topic 11-1 and 11-2, we’ll use the second half of this lesson to take a deep dive into the specifications of NTP.

#### ■ Transport Layer Protocols

There are only two communication protocols on the Internet’s transport layer: **TCP** and **UDP**. The difference between TCP and UDP in the **reliability** and the **speed** of communication.

* **TCP (Transmission Control Protocol)**
TCP is known as connection-oriented communication, establishing a virtual transmission route to the target device before exchanging data.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_3_E.png"/>
TCP also checks whether all data has been received from the target and performs a **resend request** as necessary to ensure that the data has been sent without any loss.

* **UDP (User Datagram Protocol)**
UDP, on the other hand, is a connectionless oriented protocol which exchanges data without securing a predetermined route.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_4_E.png"/>
As it doesn’t check data for loss or perform resend requests, you may not get all of your data. Compared to TCP, however, it can **transmit data much faster**! UDP is best used when sending large volumes of data like pictures and audio that won’t be seriously affected by a bit of loss!

And there’s one more very important thing on the transport layer: **port numbers**. When you send data to a computer using its IP address, the port number is used to tell which program on the computer the data should be delivered to!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_5_E.png"/>

A port number is a **16-bit** numerical value from 0 to 65535. These values come in the following three types split into different ranges.

|Port Number Range|Port Type|What is it for?|
|:---|:---|:---|
|0-1023|Well-Known Ports|Assigned to application-layer protocols like HTTP and SMPT. |
|1024-49151|Registered Ports|Assigned for use by designated services. These ports are managed in order to prevent conflicts with other software using the network. |
|49152-65535|Dynamic/Private Ports|No designated use. These can be used by any piece of software or communication protocol! |

Well-known and registered port numbers are managed by an agency known as the **IANA**, or
**I**nternet **A**ssigned **N**umbers **A**uthority. You can find the port numbers used by representative communication protocols in the table below.

|Protocol|Port Number|TCP/UDP|
|:---|:---|:---|
|FTP|20 (data transfer port), 21 (control port)|TCP|
|SSH|22|TCP|
|telnet|23|TCP|
|SMTP|25/587|TCP|
|DNS|53|TCP/UDP|
|HTTP|80|TCP|
|POP3|110|TCP|
|NTP|123|UDP|
|SSL/TLS|443|TCP|

When using TCP to communicate, you can use the following method to easily check which port you’re using and who you’re connected to:

1. Open the **command prompt** on Windows or the **terminal** on Mac OS.
2. Type **netstat -n** and hit enter. The **netstat** command is used to check your TCP communication status. Adding the **-n** option will show all values including IP addresses and port numbers.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_6_E.png"/>

#### ■ Internet Layer Protocols
The Internet layer is used to deliver data to a designated computer within a large network. In order to do that, you need a way to determine which computer out of all the computers connected to the Internet is the one you want to send the data to!

Postal addresses, for example, show a single location out of every possible place in the world. The world of the Internet also has a way to show the address of a computer. This is known as an **IP address*. A unique IP address is assigned to a computer in order to prevent conflict with another computer. Every piece of data on the Internet has both the IP address of the sender and the destination device attached to it!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_7_E.png"/>

**Looking Up an IP Address**

There’s also a way to look up the IP address assigned to your own computer. You can do this by opening the command prompt on Windows or the terminal on Mac OS and typing **ipconfig**.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_8_E.png"/>

The example above shows that the computer has been assigned the IP address 192.128.12.8. The protocols currently used on the Internet layer come in two flavors: **IPv4** or **IPv6**, and this is an IPv4 address!

An IPv4 address is a set of four eight-bit decimal numbers, making a total of 32 bits!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_9_E.png"/>

This system allows up to 4,294,967,296 (or 2<sup>32</32>) computers to be assigned their own unique IP address! The rapid expansion of the Internet on a global scale, however, means that this isn’t nearly enough address to cover every computer. In order to support this in the current day, we saw the debut of a new protocol called **IPv6**! IPv6 uses 128-bit addresses formatted like the one in this picture:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_10_E.png"/>

IPv6 not only solves the problem of limited addresses, it also helps to improve connection speeds! Since IPv4 and IPv6 are not directly compatible, however, websites and services have to support IPv6 in order to get the most benefit from it!

**The Types of IP Addresses**

IP addresses come in two types: **global IP addresses** and **private**, or **local IP addresses**.

* Private IP Addresses
These IP addresses are used on a LAN like those in your home or a business. While each device within the LAN needs a unique IP address, there’s no problem if two devices on different LANs share the same one!
* Global IP Addresses
These are the IP addresses used on WANs. Every address on the WAN must be absolutely unique!

When a computer on a LAN communicates with a server on an outside network, it uses a technique called NAT, or **N**etwork **A**ddress **T**ranslation to convert the local address into a global one to transmit data!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_11_E.png"/>

**Converting a Domain Name into an IP**

As we’ve just learned, communicating with a specific computer on the Internet requires knowing its IP address. However, the chain of numbers used for IP addresses can be a little hard for humans to remember. This is why **domain names** were thought of to use in place of IP addresses!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_12_E.png"/>

The used to manage which domain name belongs to which IP address is called the **DNS**, or **D**omain **N**ame **S**ystem. When you type a URL in your browser to visit a website, the first thing it does is ask the DNS for the IP of that site!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_13_E.png"/>

You can actually use the **nslookup** command to ask the DNS for the IP address of any site. You can do this by opening the command prompt on Windows or the terminal on Mac OS. Let’s try running this command on **www.artec-kk.co.jp*. This is the website of Artec, the developers of ArtecRobo 2.0!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_14_E.png"/>

#### ■ Network Interface Layer Protocols

The communication protocols in the network interface layer are used to transmit data between adjacent devices connected by a LAN cable or on a wireless LAN.

The OSI reference model splits this layer up into the physical and the data link layers, which handle the hardware which deals with electrical signals and radio waves as well as the data transmitted by that hardware!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_15_E.png"/>

* **The Data Link Layer**
These protocols send and receive data between two devices connected directly by a LAN cable.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_16_E.png"/>

* **The Physical Layer**
This is the hardware used for physical connections via LAN cable or wireless LAN, converting data into electrical signals or radio waves, and more.
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_17_E.png"/>

**MAC Addresses as a Destination for Data**

All devices including computers, mobile devices, and routers include a component known as a **network adapter** used to connect to to a network through a wired or wireless LAN.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_18_E.png"/>

During production, these network adapters are assigned an identification number called a **MAC address**. Just like a global IP address, you’ll never find two devices which share the same MAC address!

MAC addresses are a 48-bit sequence of numerical values. The first 24 bits show the vendor number, while the last 24 bits are the number assigned to the network adapter manufactured by that vendor!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_19_E.png"/>

IP addresses are used to direct data to a destination on the Internet layer. On the network interface layer, however, MAC addresses are used to send data to a neighboring device!

Think of these two kinds of addresses as sending someone a package: an IP address would be the address you’re sending the package to, while the MAC address would be the names of the places you’re routing the package through.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_20_E.png"/>

This means that the MAC address attached to a piece of data every time it’s relayed through a different device!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_21_E.png"/>

**Looking Up a MAC Address**

In addition to looking up IP addresses, you can also use the **ipconfig** command to look up the MAC address of your computer’s network adapter. To do this on Windows, just open the command prompt and run ipconfig with the option **/all**.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_22_E.png"/>

The **Physical Address** shows the MAC address! On Mac OS, open up the terminal and run the **ipconfig** command with the **en0** option to do the same thing. You can find the network adapter’s MAC address under **ether** in the list that appears!

**Requesting a MAC Addresses**

As explained above, you need a destination MAC address in order to send data over a wired or wireless LAN. However, knowing a computer’s IP address doesn’t necessarily mean that you know its MAC address! The protocol used to find a device’s MAC address is called the **ARP**, or **A**ddress **R**esolution **P**rotocol.

The ARP requests the MAC address of every computer connected to a device known as a network hub. When it sends data to every device connected to the network, this is called **broadcasting** the data. Once it gets a reply from a computer with the corresponding IP address, it knows that computer’s MAC address!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_23_E.png"/>

**Wired and Wireless LAN Standards**

Both the communication system and the physical cable used by wired LAN are defined by a standard called **Ethernet** In the OSI reference model, the protocols for Ethernet are defined on both the data link and physical layers. While the protocols in the data link layer are mostly shared between standards, the protocols in the physical layer vary depending on the type of cable used.

The names of Ethernet standards are comprised of a chain of numbers and letters as shown here:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_24_E.png"/>

* **Data Transmission Speed**
The transmission speed of data is shown as a numerical value. This speed is counted in **bps**, or **b**its **p**er **s**econd. A speed of 1,000 Mbps would mean that you can send 1,000 megabits of data every single second!
* **Modulation Methods**
**Modulation** is changing the frequency and amplitude of a base signal before sending it. **Baseband transmission** is sending a signal without any modulation!
* **Cable Types**
Twisted-pair copper cable and fiber optic cable are the two most common types of Ethernet cables. These cables are divided into categories by speed: the higher the category, the faster that cable is!

##### Ethernet Standards

|Standard|What does it do?|
|:---|:---|
|100BASE-T|Uses twisted-pair cable to transmit an unmodulated data signal at 100Mbps. |
|1000BASE-T|Uses twisted-pair cable to transmit an unmodulated data signal at 1000Mbps. |
|1000BASE-LX|Uses fiber optic cable to transmit an unmodulated data signal at 1000Mbps. |
|10GBASE-T|Uses twisted-pair cable to transmit an unmodulated data signal at 10Gbps. |


Just like Ethernet, there are a number of standards used for wireless LAN with different bandwidths and transmission speeds.

##### Wireless LAN Standards

|Standard|What does it do?|
|:---|:---|
|IEEE802.11|The base standard for wireless LAN. |
|IEEE802.11a|Transmits data at up to 54 Mpbs on a 5 GHz band. |
|IEEE802.11b|Transmits data at up to 11 Mpbs on a 2.4 GHz band. |
|IEEE802.11g|Transmits data at up to 54 Mpbs on a 2.4GHz band. |
|IEEE802.11n|Uses multiple antennas to transmit data at up to 600 Mbps on either a 2.4 Ghz or 5 Ghz band. |
|IEEE902.11ac|Uses even more antennas than IEEE802.11n to transmit data at up to 6.9 Gbps on a 5 Ghz band. |


## Building a Digital Clock

It’s time to make a digital clock which uses an Ultrasonic Sensor to detect people and shows the current time when it does!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_25_E.png"/>

### Parts You'll Need

##### Parts
* Core Unit x 1
* Robot Expansion Unit x 1
* Battery Box x 1
* Ultrasonic Sensor x 1
* Sensor Connecting Cable (3-wire, 15cm) x 1
* Basic Cube (Red) x 2
* Triangle (Gray) x 2
* Half A (Gray) x 4
* Half C (White) x 9
* Half D (White) x 2

##### The Artec Block Shapes

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_26_E.png"/>

### Building a Digital Clock

Follow the link below to find instructions for building the digital clock:

[Building a Digital Clock](https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_build_E.pdf)

## Using the Internet to Get the Correct Time

Have you ever been late to meet your friends or missed a the start of your favorite live stream because your watch was slightly off or your alarm didn’t go off at the correct time? In order to prevent accidents like this from happening, the program we’re going to make for our digital watch will have a function which **uses the Internet to automatically correct the time when it’s off**!

### Syncing Computer Clocks with the Network Time Protocol
In order to do this, we’ll use the **Network Time Protocol** to correct our digital clock’s time. Also called the NTP, this communication protocol uses a high sensitive and accurate **atomic clock**<sup>★</sup> to find the **UTC**, or **C**oordinated **U**niversal **T**ime, and it uses this time to sync the clock of every computer on the network! Computers fix errors in their local clock by comparing it with the time retrieved from the NTP server!

###### ★ Atomic clocks are incredibly accurate clocks with an error of 10<sup>-15</sup>. That’s only one second every 30 million years!

Let’s start by taking a look at how the NTP works.

#### ■ The Strata of the NTP

As shown in the picture below, the NTP has a structure comprised of up to 15 layers. Each layer is called a **stratum**. **Stratum 0** serves as the atomic clock, and the NTP servers at the bottom strata make requests to the NTP servers at the top strata to sync the time! Two NTP servers on the same stratum will exchange times to adjust each of their clocks in tandem.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_27_E.png"/>

#### ■ Correcting the Time

When a client uses NTP to send a request to a server, the server records the time it receives the request as **T2** and the time it sends a response as **T3**. The client then uses these times, the time it sends the request (**T1**), and the time it receives the response (**T4**) in the following formulas to calculate the difference in time, or **offset** from the NTP server as well as the **rtt**, or the time it took for the transmission to make the round trip!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_28_E.png"/>

Now let’s try using these formulas with a simplified example:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_29_E.png"/>

###### ★ The numbers in this picture show the hour and the minute of the time!

In this example there’s a 16-second offset from the NTP server, and it took eight seconds for the transmission to make a round trip! Assuming that the request and the response took four seconds each, the 16-second offset that we received is the correct value! If this isn’t the case, however, there may be an error in the offset of up to half of the round trip time. This would be 16±4 seconds!

Due to this and the possibility that the NTP server's clock is wrong, devices in real life make repeated requests to multiple servers to correct their time in a much more complex process.

In order to keep our program simple in this lesson, we’ll only be retrieving the time from one NTP server. This means that we’ll assume that both the request and response times are equal and that the NTP server’s time is accurate. This will let us use the following simply formula to correct the Core Unit’s **RTC**, or **R**eal **T**ime **C**lock!

```
Corrected RTC time = RTC time + offset
```

#### ■ The NTP Data Format

The data formats of a request and a response in NTP are define as shown here:
###### ★ This format in this picture shows the minimum amount of data that needs to be transmitted. There are formats out there which include more!
###### ★ Timestamps are used to show the date and time when an event occurs!
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_30_E.png"/>

This format uses 48 bytes (or 384 bits) to show multiple items. Now let’s take a good look at what each of these items are!

* **LI (Leap Indicator)**: 2 bits
This indicates a leap second. While an atomic clock is super accurate, the rotation period of the Earth (generally once per 24 hours) isn’t. This means that they aren’t in perfect sync! This means that we need to add a **leap second** once every few years to correct for this difference. A leap second indicator is used to determine whether a leap second should be added to the last minute of the day. When we retrieve the time from the server, this second should be **0**.
```
LI0(00): Time is normal
LI1(01): Last minute is 61 seconds
LI2(10): Last minute is 59 seconds
LI3(11): Broadcast time is inaccurate and shouldn’t be used
```
* **VN (Version Number)**: 3 bits
This is the NTP version. Version 4 of the NTP is called the **SNTP**, or the **S**imple **N**etwork **T**ime **P**rotocol. This is a highly simplified version of the NTP we briefly explained above, and it’s optimized for clients syncing their time with the server. **We’ll be using SNTP in this lesson**!
```
VN1(001): Version 1
VN2(010): Version 2
VN3(011): Version 3 (Normal NTP Server)
VN4(100): Version 4 (Normal SNTP Server)
```
* **Mode**: 3 bits
This number shows the mode of operation. In the symmetric active/passive mode, NTP servers on the same stratum will adjust their times by exchanging time data. In broadcast mode, only the server will send time data. **The mode should be set to 3 for client when retrieving the time from a server**.
```
MODE0(000): Reserved
MODE1(001): Symmetric active mode ★ NTP only
MODE2(010): Symmetric passive mode ★ NTP only
MODE3(011): Client
MODE4(100): Server
MODE5(101): Broadcast ★ NTP only
MODE6(110): Reserved (NTP control message)
MODE7(111): Reserved (private use)
```
* **Stratum**: 8 bits
This is the stratum level of the clock. This should be set to **0** when sending a request from an SNTP client.
```
0: Unspecified or invalid stratum
1: Stratum 1
2-15: Stratum 2-15
16-255: Reserved
```
* **Poll Interval**: 8 bits
This is the interval between requests sent to an NTP server on a higher stratum. These are in powers of two. This should be set to **0** when sending a request from an SNTP client.
```
00000000: Every 0 seconds
00000001: Every 1 seconds
00000010: Every 2 seconds
00000011: Every 4 seconds
.
.
.
```
* **Precision**: 8 bits
This is a signed (positive or negative) eight bit number showing the precision of the local clock. These are in powers of two. ** This should be set to **0** when sending a request from an SNTP client**.
* **Root Delay**: 32 bits
The total value of the round trip delay. The top 16 bits are an integer while the lower 16 bits are are fraction. ** This should be set to **0** when sending a request from an SNTP client**.
* **Root Dispersion**: 32 bits
This shows the difference in time when an NTP server transmits to an NTP server on a higher stratum. The top 16 bits are an integer while the lower 16 bits are are fraction. ** This should be set to **0** when sending a request from an SNTP client**.
* **Reference Clock Identifier**: 32 bits
Text string which which shows the NTP server used as a reference clock. ** This should be set to **0** when sending a request from an SNTP client**.
* **Reference Timestamp**: 64 bits
The time at which the local clock was last set or corrected. The top 32 bits are an integer while the lower 32 bits are are fraction. ** This should be set to **0** when sending a request from an SNTP client**.
* **Originate Timestamp**: 64 bits
The time at which the client sent the request to the server. The top 32 bits are an integer while the lower 32 bits are are fraction. ** This should be set to **0** when sending a request from an SNTP client**.
* **Receive Timestamp**: 64 bits
The time at which the server received the request. The top 32 bits are an integer while the lower 32 bits are are fraction. ** This should be set to **0** when sending a request from an SNTP client**.
* **Transmit Timestamp**: 64 bits
The time at which the server sent the response to the client. The top 32 bits are an integer while the lower 32 bits are are fraction. ** This should be set to **0** when sending a request from an SNTP client**.

In this lesson we’ll be using the SNTP, a simplified version of the NTP. As we explained above, most of the items for SNTP will be set to zero. With that in mind, we’ll only be keeping an eye on LI, VN, and MODE as we set them to the values shown here:

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_31_E.png"/>

* **LI**: 00 ⇒ 0 ⇒ This is 0 since we’re retrieving the time from the server
* **VN**: 100 ⇒ 4 ⇒ SNTP for NTP version 4
* **MODE**: 011 ⇒ 3 ⇒ For client

Since the data for these three items is one byte, we can express it as the hexadecimal value **0x23**. Make sure to remember this value since we’ll be using it in our program!

### Coordinated Universal Time and Time Zones

While we explained that NTP retrieves the Coordinated Universal Time, but what exactly does this mean? Let’s take this as an opportunity to see how the current time is determined in each region!

#### ■ Finding the Time

Humans used to measure time in their region by using **solar time**. This is the position of the Sun when it’s the highest in the sky at noon. Solar time, however, changes depending on where you’re observing the Sun from. A one-degree change in longitude equals a four-minute time difference, while 15 degrees equals a whole hour.

In Japan, for example, Naha, Okinawa is at 127 degrees east while Sapporo, Hokkaido is at 141 degrees east. That means there’s almost a one-hour time difference between them if you use solar time!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_32_E.png"/>

If you visit Japan, however, you’ll find that **the time is the same no matter where you go**! This is because it can be pretty inconvenient for distant towns and cities to run on different times. Imagine what New Year’s countdowns would look like if every city had its own time!

This is why large regions of the Earth use a shard **time standard** to coordinate time. You call these regions which share the same time standard a **time zone**! While smaller countries like Japan only have one time zone, larger countries like Russia or the United States have several time zones!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_33_E.png"/>

The time in every time zone on Earth is determined by a single clock which serves as a baseline. This baseline used to be **Greenwich Mean Time**, and it was based on the solar time of the Royal Observatory in Greenwich, London. In the present day, the standard is set using the **UTC**, or **C**oordinated **U**niversal **T**ime. This time is based on the **I**nternational **A**tomic **T**ime measured by an atomic clock!

Most of the time zones used today are all set in one-hour or half-hour increments from UTC in the following format:

```
UTC+9: This time zone is nine hours ahead of UTC
UTC-3:30: This time zone is three hours and 30 minutes behind UTC
```

##### The Time Zones of the World
<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_34_E.png"/>

The country of Japan is in the **UTC+9** time zone, or **Japan Standard Time**. This means that we can get the current time in Japan by adding nine hours to the time we retrieve from the NTP server! If you’d like to set the time for a different time zone, try finding it in the table below!

##### The Big Table of Time Zones
[Source: Time zone (Wikipedia)](https://en.wikipedia.org/wiki/Time_zone))
|Time Zone|Country|
|:---|:---|
|UTC+14|Kiribati (Line Islands)|
|UTC+13|Kiribati (Phoenix Islands), Samoa, Tonga|
|UTC +12|New Zealand, Kiribati (Gilbert Islands), Tuvalu, Nauru, Fiji, Marshall Islands, Russia (Kamchatka time)|
UTC+11|Solomon Islands, Vanuatu, Russia (Magadan Time)|
|UTC+10|Australia (Eastern), Papua New Guinea, Russia (Vladivostok Time)|
|UTC+9:30|Australia (Central)|
|UTC+9|Japan, South Korea, Indonesia (Eastern), Russia (Yatsk Time)|
|UTC+8|Australia (West), Singapore, People's Republic of China, Hong Kong, Macau, Taiwan, Philippines, Mongolia, Malaysia, Russia (Irkutsk Time)|
|UTC+7|Indonesia (West), Cambodia, Thailand, Vietnam, Laos, Russia (Krasnoyarsk Time)|
|UTC+6:30|Myanmar|
|UTC+6|Kazakhstan (Eastern), Bangladesh, Kyrgyzstan, Bhutan, Russia (Omsk Time)|
|UTC+5:45|Nepal|
|UTC+5:30|India, Sri Lanka|
|UTC+5|Uzbekistan, Kazakhstan (West), Tajikistan, Turkmenistan, Pakistan, Maldives, Russia (Yekaterinburg Time)|
|UTC+4:30|Afghanistan|
|UTC+4|Azerbaijan, United Arab Emirates, Armenia, Oman, Georgia, Seychelles, Mauritius, Russia (Samara Time)|
|UTC+3:30|Iran|
|UTC+3|Yemen, Iraq, Uganda, Ethiopia, Eritrea, Qatar, Kuwait, Kenya, Comoros, Saudi Arabia, Djibouti, Somalia, Tanzania, Turkey, Bahrain, Belarus, Madagascar, South Sudan, Russia (Moscow Time)|
|UTC+2|Israel, Ukraine, Democratic Republic of the Congo (Eastern Collection, West Kasai, East Kasai, Katanga, North Kib, Maniema, South Kib), Zambia, Syria, Zimbabwe, Sudan, Swaziland, Palestine, Eastern European Time (Estonia, Cyprus, Greece, Finland, Bulgaria, Latvia, Lithuania, Romania), Egypt, Sudan, Burundi, Botswana, Malawi, South Africa, Mozambique, Moldova, Namibia, Jordan, Libya, Rwanda, Lesotho, Lebanon, Russia (Cariningrad Time)|
|UTC+1|Angola, Gabon, Cameroon, Republic of Congo, Democratic Republic of the Congo (Kinshasa, Equator, Ba Congo, Bandundu), Equatorial Guinea, Chad, Central Africa, Central European Time (Algeria, Albania, Andra, Italy, Austria, Netherlands, Croatia, San Marino, Switzerland, Sweden, Spain, Slovakia, Slovenia, Serbia, Czech Republic, Tunisia, Denmark, Germany, Norway, Vatican, Hungary, France, Belgium, Bosnia and Herzegovina, Poland, Macedonia, Malta, Monaco, Montenegro, Liechtenstein, Luxembourg), Nigeria, West Sahara, Niger, Benin, Morocco|
|UTC|Côte d'Ivoire, Gambia, Ghana, Guinea, Guinea-Bissau, Danish Greenland (Northeast), Santome Principe, Sierra Leone, Senegal, St. Helena, Togo, Western European Time (Iceland, Ireland, United Kingdom, Spain, Faroe Islands, Portugal), Bouve Island, Burkina Faso, Mali, Mauritania, Liberia|
|UTC-1|Cape Verde, Danish Greenland (Eastern), Portugal|
|UTC-2|South Georgia and the South Sandwich Islands, Brazil (Marine Islands)|
|UTC-3|Argentina, Uruguay, Danish Greenland (South Coast, Southwest Coast), Saint-Pierre Micron, Suriname, Chile (Magajanes y de la Antartica Chilena), Brazil (Amapa, Aragoas, Espirito Santo, Goias, Santa Catarina, Sao Paulo, Ceara, Sergipe, Tocantins, Bahia, Para (Eastern), Paraiba, Parana, Piauí, State of Brasilia, Pernambuco State, Maragno, Minas Gerais, Rio Grande do Sur, Rio Grande do Norchi, Rio de Janeiro), French Guyana|
|UTC-3:30|Canada (Newfoundland and Labrador)|
|UTC-4|Dutch Alba, British Anguilla, Antigua Barbuda, British Virgin Islands, Dutch Antilles, Guyana, French Guadeloupe, Danish Greenland (Northwest), Grenada, Saint Kitts and Nevis, St. Vincent and Grenadine Islands, St. Lucia, Venezuela, USA (US Virgin Islands, Puerto Rico), Canada (Quebec, Newfoundland, Labrador, New Brunswick, Nova Scotia, Prince Edward Island), Bermuda Islands, Chile, Dominica, Republic of Dominican Republic, Trinidad Tobago, Paraguay, Barbados, British Falkland Islands, Brazil (Amazonas, Para, Mato Grosso, Loraima, Rondonia), Bolivia, French Martinique, British Montserrat|
|UTC-5|Ecuador, Cuba, British Cayman Islands, Colombia, Jamaica, British Turks Islands / Caikos Islands, United States (Indiana, West Virginia, Ohio, Kentucky, Connecticut, South Carolina, Georgia, Delaware, New Jersey, New Hampshire, New York, North Carolina, Virginia, Vermont, Florida, Pennsylvania, Massachusetts, Michigan, Maine, Maryland, Rhode Island, Washington DC), Canada (Indiana, Nunavut Territory, Quebec), Mexico (Estado de Quintana Roo), Haiti, Panama, Bahamas, Brazil (Acre, Amazonas), Peru|
|UTC-6|Ecuador (Galapagos Islands), El Salvador, Guatemala, Costa Rica, United States (Arkansas, Iowa, Alabama, Illinois, Indiana, Wisconsin, Oklahoma, Kansas, Kentucky, South Dakota, Texas, Tennessee, Nebraska, North Dakota, Florida, Michigan, Mississippi, Missouri, Minnesota, Louisiana), Canada (Ontario, Saskatchewan, Nunavut Territory, Matniva) State), Chile (Easter Island), Nicaragua, Belize, Honduras|
|UTC-7|United States (Utah, Arizona, Oklahoma, Oregon, Kansas, Colorado, South Dakota, Texas, New Mexico, Nevada, Nebraska, North Dakota, Montana, Utah, Wyoming), Canada (Alberta, British Colombia, Northwest Territory, Saskatchewan, Nunavut Territory, Yukon Territory), Mexico (Sina Loa, Sonora, Nayarit, Chihuahua), Baja California Sur|
|UTC-8|United States (Idaho, Oregon, California, Nevada, Washington), Canada (British Columbia, British Pitcairn Islands), Mexico (Baja California)|
|UTC-9|United States (Alaska), French Polynesia (Gambier Islands)|
|UTC-9:30|French Polynesia (Marquesas Islands)|
|UTC -10|Cook Islands, Johnston Island, United States (Hawaii), French Polynesia (Society Islands, Tuamotu, Tupua’i)|
|UTC-11|American Samoa, Niue, United States (Midway Islands)|
|UTC-12|United States (Baker Island, Howland Island)|


## Programming the Digital Clock

We used the `urequests` module in Topic 11-1 and 11-2 to communicate in HTTP, but we can’t use this module for NTP. To use NTP, we’ll have to use the `usocket` module instead! This module allows us to use something called **socket communication**, which we’ll explain next!

### What’s Socket Communication?

A socket, to put it simply, is like a connector a program uses to communicate with a network over TCP or UDP. Application layer protocols like HTTP and NTP all use sockets!

This is where things get a bit complicated, but you can think of a socket like a post box. Put a letter into the post box and it will travel through the post office to make its way to the receiver. The person who sends the letter and person who receive it, however, don’t need to know a single thing about how the letter gets to its destination!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_35_E.png"/>

To a piece of software, a socket is just like a post box! The only thing the software has to do is pass the data it wants to send to a socket and the socket will take care of the rest. This allows the software to easily send data over the Internet without ever thinking twice about TCP and UDP on the transport layer or the IP on the Internet layer!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_36_E.png"/>

Now let’s try this out for ourselves by writing a program which uses a socket to communicate with the NTP server.

### Programming Time Correction

The program we make here will use the NTP server provided by the National Institute of Information and Communications Technology in Japan. The time we retrieve from this server is the one we’ll use to correct our Core Unit’s real-time clock!


[Japan Standard Time (JST) Group](http://jjy.nict.go.jp/index-e.html)

This is the domain name for the NICT’s NTP server:

```
ntp.nict.jp
```

This server is on stratum **1**. In order to use this server, we have to limit our poll interval to an average of 20 times per hour, or 480 times per day.

The UTC time returned by the server is shown in seconds counted from January 1st, 1900 to the present day. Let’s make sure to keep this in mind as we make our program in the following order!

#### ■ Connecting to Wi-Fi

We’ll put the part of our program which retrieves the time from the NTP server and uses it to correct our Core Unit’s RTC into a function called `correct_time()` This function will start by connecting to our wireless access point!

```python=1
from mywifi import MyWiFi  # Imports class from the module we made in Topic 11-1

SSID = "SSID"  # SSID for your Wi-Fi connection
PASSWORD = "PASSWORD"  # Password for the Wi-Fi connection

wifi = MyWiFi()  # Creates an object to connect to Wi-Fi

def correct_time():
    if not wifi.isconnected():  # Attempts to connect only if disconnected from Wi-Fi
        if not wifi.connect(SSID, PASSWORD):
            return
```

#### ■ Retrieving an Address for Socket Communication

We’ll import the `usocket` module next, and use the module’s `getaddrinfo()` function to convert the NTP server’s domain name into the address we need in order to communicate with the socket.

```
Getaddrinfo(host name/domain name, port number)
```

The `getaddrinfo()` function will return a list containing a five-item tuple:

```
[(family, type, proto, canonname, sockaddr)]
```

* `family`: Address Family
This numerical value which shows the address family. `usocket.AF_INET` (2) shows an IPv4 address, while `usocket.AF_INET6` (10) shows an IPv6 address.
* `type`: Socket Type
This numerical value shows the socket type. `usocket.SOCK_STREAM` (1) shows a TCP stream socket, while `usocket.SOCK_DGRAM` (2) shows a UDP datagram socket.
* `proto`: Protocol Number
This number shows the top protocol using an IP to communicate. The IANA defines the number **6** for TCP and the number **17** for UDP. We generally don’t have to worry about setting this number in MicroPython!
* `canonname`: Canonical Name
The official domain name.
* `sockaddr`: Socket Address
The socket address used for communication. This is a tuple of `(IP address, port number)`.

We’ll only need to use `sockaddr` from the converted data here. Write the code below and run the function from the terminal to see how the domain name is converted into address data!

New Code (Line 1, Line 6, Lines 14-16)
```python=1
import _thread  # Imports a new usocket module
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
NTP_SERVER = "ntp.nict.jp"  # The domain name of the NTP server

wifi = MyWiFi()

def correct_time():
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    addrinfo = usocket.getaddrinfo(NTP_SERVER, 123)  # Retrieves address info from the domain name. NTP uses port 123.
    sockaddr = addrinfo[0][-1]  # Specifies -1 to get the last item
    print(sockaddr)  # Shows converted info
```

##### Program Results
<pre class="prettyprint">
&gt;&gt;&gt; correct_time("ntp.nict.jp")
.
.
.
Connected.
('61.205.120.130', 123)
</pre>

#### ■ Creating a Socket and Establishing the Connection

When using sockets to communicate, you have to establish the connection before sending the data.

We’ll start by using the `socket()` function to create a socket object.

```
Socket(address family, socket type)
```

We’ll be using the IPv4 address family here with a datagram, or UDP socket. This means that we’ll have to set this function with the arguments `usocket.AF_INET` and `usocket.SOCK_DGRAM`.

We can use the socket object’s `connect()` method to establish the connection.

###### New and Changed Code (Lines 16-18)
```python=1
import usocket
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
NTP_SERVER = "ntp.nict.jp"

wifi = MyWiFi()

def correct_time():
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    addrinfo = usocket.getaddrinfo(NTP_SERVER, 123) 
    sockaddr = addrinfo[0][-1]
    # print(sockaddr)  # Comment out code used for debugging
    s = usocket.socket(usocket.AF_INET, usocket.SOCK_DGRAM)  # Creates a socket
    s.connect(sockaddr)  # Establishes the connection
```

#### ■ Sending and Receiving Data

Once we’ve established the socket connection with the NTP server, we can send a request to retrieve the time. We’ve already gone over the format of this data in the previous section! We’ll need a 48-byte byte string which will set the three items we need to the values below and everything else **0**!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_31_E.png"/>

* **LI**: 00 ⇒ 0 ⇒ This is 0 since we’re retrieving the time from the server
* **VN**: 100 ⇒ 4 ⇒ SNTP for NTP version 4
* **MODE**: 011 ⇒ 3 ⇒ For client

Now let’s prepare the data we need to send by make a 48-byte array (`bytearray`) and storing `0x23` in the first byte!

We can use the `send()` function to pass the data we wish to send to the socket. If you want to receive data from a socket, use the `recv()` function!
```
send(data to pass to socket)
recv(maximum size of data to receive from socket)
```

The response from the NTP server will share the same data format. Since this means that we’ll receive a 48-byte array from the socket, we can set this to `recv(48)`. Once you’ve sent and received data, remember to always close the connection by using the socket object’s `close()` method!

Let’s take what we’ve learned so far and add the following code. Next, run the program and see if the data you’ve received appears in the terminal!

###### New Code (Line 19-24)
```python=1
import usocket
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
NTP_SERVER = "ntp.nict.jp"

wifi = MyWiFi()

def correct_time():
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    addrinfo = usocket.getaddrinfo(NTP_SERVER, 123) 
    sockaddr = addrinfo[0][-1]
    # print(sockaddr) 
    s = usocket.socket(usocket.AF_INET, usocket.SOCK_DGRAM)
    s.connect(sockaddr)
    data = bytearray(48)  # Creates a 48-byte array to send to the NTP server
    data[0] = 0x23  # The first byte contains the LI, VN, and MODE settings
    s.send(data)  # Passes data to the socket
    msg = s.recv(48)  # Receives data from the socket
    s.close()  # Closes the connection
    print(msg)  # Shows received data
```

##### Program Results
<pre class="prettyprint">
&gt;&gt;&gt; correct_time()
b'$\x01\x00\xec\x00\x00\x00\x00\x00\x00\x00\x00nict\xe2\xd1\xe98\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\xe2\xd1\xe98\xfeA\x88f\xe2\xd1\xe98\xfeA\x96('
</pre>

#### ■ Extracting Time Data and Converting it to Decimal

In order to correct our Core Unit’s clock we’ll need to take the **receive timestamp** and **transmit timestamp** out of the data sent by the NTP server.

* **Receive Timestamp**
The time the server received the request
* **Transmit Timestamp**
This is the time at which the server sent the response to the client.

This timestamps are both eight bytes (or 64 bits) of data. The receive timestamp can be found in bytes 33-40 of the data we receive, while the transmit timestamp can be found in byes 41-48. The first four bytes of both pieces of data have an integer in the first four bytes and a fraction in the last four. **We don’t need to be ultra-specific here, so we’ll only use the integer part of the number!

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_37_E.png"/>

The above data will be in byte arrays. This means that we’ll use the `ubinascii` module’s `hexlify()` function to convert them into hexadecimal values, then use the `int()` to convert them one more time into decimal! Now let’s add the code below:

###### New Code (Line 2, Lines 25-27)

```python=1
import usocket  
import ubinascii  # Imports a new ubinascii
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
NTP_SERVER = "ntp.nict.jp"

wifi = MyWiFi()

def correct_time():
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    addrinfo = usocket.getaddrinfo(NTP_SERVER, 123) 
    sockaddr = addrinfo[0][-1]
    # print(sockaddr) 
    s = usocket.socket(usocket.AF_INET, usocket.SOCK_DGRAM)
    s.connect(sockaddr)
    data = bytearray(48)
    data[0] = 0x23
    s.send(data)
    msg = s.recv(48)
    s.close()
    # print(msg)  # Comment out code used for debugging
    server_receive = int(ubinascii.hexlify(msg[32:36]), 16)  # Integer portion of received timestamp
    server_transmit = int(ubinascii.hexlify(msg[40:44]), 16)  # Integer portion of sent timestamp
```

###### ★ Note that we use `msg[32:36]` to extract bytes 33-36 since the array starts with the number 0.

Now let’s see how our program works so far!

#### Program Results

<pre class="prettyprint">
&gt;&gt;&gt; correct_time("ntp.nict.jp")
3805415865 3805415865
</pre>

###### ★ Depending on your connection, there may be up to a 1 second difference between the receive and transmit timestamps.

#### ■ Calculating the Offset

In order to calculate the offset, we used the time the client sent the request as well as the time it received the response. This means that we can use the `time` module’s `time()` function to record the current RTC (in seconds) both when the data is sent and received!

###### New Code (Line 3, Lines 23-26)
```python=1
import usocket  
import ubinascii
import time  # Imports the time module
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
NTP_SERVER = "ntp.nict.jp"

wifi = MyWiFi()

def correct_time():
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    addrinfo = usocket.getaddrinfo(NTP_SERVER, 123) 
    sockaddr = addrinfo[0][-1]
    # print(sockaddr) 
    s = usocket.socket(usocket.AF_INET, usocket.SOCK_DGRAM)
    s.connect(sockaddr)
    data = bytearray(48)
    data[0] = 0x23
    client_transmit = time.time()  # The time the client transmitted the request
    s.send(data)
    msg = s.recv(48)
    client_transmit = time.time()  # The time the client request was received
    s.close()
    # print(msg)
    server_receive = int(ubinascii.hexlify(msg[32:36]), 16)
    server_transmit = int(ubinascii.hexlify(msg[40:44]), 16)
```

While we have the four pieces of time data that we need to correct the clock, we still have one issue to solve. This issue is our Core Unit’s RTC referenced by the `time` module: its starting time is **January 1st, 2000 00:00:00**, while the starting time for the NTP server is **January 1st, 1900 UTC 00:00:00**. This means that we can’t compare these two times directly!

In order to match the starting time of the RTC, we’ll subtract the number of seconds from **January 1st, 1900 00:00:00** to **January 1st, 2000 00:00:00** from the time retrieved from the NTP server.

This is **3,155,673,600 seconds**! We’ll define this as a constant called `DELTA`, and subtract this number from both `server_receive` and `server_transmit`.

New and Changed Code (Line 9, Lines 30-33)

```python=1
import usocket  
import ubinascii
import time
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
NTP_SERVER = "ntp.nict.jp"
DELTA = 3155673600  # Time difference used to sync start times (1900-1-1 0:0:0 ～ 2000-1-1 0:0:0)

wifi = MyWiFi()

def correct_time():
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    addrinfo = usocket.getaddrinfo(NTP_SERVER, 123) 
    sockaddr = addrinfo[0][-1]
    # print(sockaddr) 
    s = usocket.socket(usocket.AF_INET, usocket.SOCK_DGRAM)
    s.connect(sockaddr)
    data = bytearray(48)
    data[0] = 0x23
    client_transmit = time.time()
    s.send(data)
    msg = s.recv(48)
    client_receive = time.time()
    s.close()
    # print(msg)
    server_receive = int(ubinascii.hexlify(msg[32:36]), 16) - DELTA   # Syncs the start time
    server_transmit = int(ubinascii.hexlify(msg[40:44]), 16) - DELTA   # Syncs the start time
```

And now we’re ready to go! Let’s used the formula from the previous section to calculate the offset.

<img src="https://www.artec-kk.co.jp/school/cl/textbooks/material_en/topic_11-3/11-3_38_E.png"/>

###### New Code (Line 32)
```python=30
    server_receive = int(ubinascii.hexlify(msg[32:36]), 16) - DELTA   
    server_transmit = int(ubinascii.hexlify(msg[40:44]), 16) - DELTA 
    offset = ((server_receive - client_transmit) + (server_transmit - client_receive)) / 2
```

#### ■ Correcting the RTC

Last, let’s correct the Core Unit’s RTC. We’ll start by adding code to import the `RTC` class and create an object from it.

###### New Code (Line 4, Line 12)

```python=1
import usocket  
import ubinascii
import time
from machine import RTC  # Imports the RTC class
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
NTP_SERVER = "ntp.nict.jp"
DELTA = 3155673600

rtc = RTC()  # Creates an RTC object
wifi = MyWiFi()
```

Next, we’ll use the offset we calculated in `offset` to calculate the correct current time. We’ll need to take our time zone into account as we do this. Japan’s time zone is in UTC+9, which is nine hours ahead of the time we’ll retrieve from the NTP server. That means we need to add that number to the time! Let’s define this time as a constant:

###### New Code (Line 11, Line 36)
```python=1
import usocket  
import ubinascii
import time
from machine import RTC
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
NTP_SERVER = "ntp.nict.jp"
DELTA = 3155673600
UTC_TIMEZONE_JP = 32400  # Starts 9 hours (3600*9=32400 seconds) ahead as Japan is UTC+9
```
```python=33
    server_receive = int(ubinascii.hexlify(msg[32:36]), 16) - DELTA
    server_transmit = int(ubinascii.hexlify(msg[40:44]), 16) - DELTA
    offset = ((server_receive - client_transmit) + (server_transmit - client_receive)) / 2
    current_time = time.time() + offset + UTC_TIMEZONE_JP  # Gets current time after including the offset
```

The time date and time are corrected once we pass the current date and time to the RTC’s `init()` method. This means that we’ll have to convert the time from seconds into the year, month, date, hour, minute, and second! In order to do this, we can use the `time` module’s `localtime()` function. If you pass the number of elapsed seconds to this function, it will return the current date and time in the format shown below:

**The `localtime()` Function’s Return Value**
```
(year, month, date, hour, minute, second, day, day of the year from 1-366)
```

While it would be nice to pass this to the `RTC` class’s `init()` method as-is, the tuple in the argument isn’t quite in the same order!

**The `RTC` Class `init()` Method’s Argument**
```
(year, month, date, day, hour, minute, second, millisecond)
```

This means that we’ll have to make a tuple which changes the order of this data before passing it to the `RTC` class’s `init()` method! Now let’s add the code below:

###### New Code (Lines 37-39)
```python=33
    server_receive = int(ubinascii.hexlify(msg[32:36]), 16) - DELTA
    server_transmit = int(ubinascii.hexlify(msg[40:44]), 16) - DELTA
    offset = ((server_receive - client_transmit) + (server_transmit - client_receive)) / 2
    current_time = time.time() + offset + UTC_TIMEZONE_JP
    datetime = time.localtime(int(current_time))  # Converts into date and time format. Argument only takes integers, so we use the int() function to convert it.
    datetime = datetime[:3] + (datetime[6],) + datetime[3:6] + (0,)  # Changes order to fit the init() method
    rtc.init(datetime)  # Sets new time which includes the offset
```

And now we’ve finished a program which corrects our Core Unit’s clock! Now let’s continue by making a digital clock which shows the time on the display and periodically corrects itself!

### Showing the Time and Periodic Correction

We’ll start by defining a `scroll_time()` function which shows the current time on the LED display.

#### ■ Scrolling the Current Time Across the LED Display

In order to get the RTC’s current time, we can use either the `RTC` class’s `datetime()` method or the `time` module’s `localtime()` method. Either one of these will work just fine! The code shown here uses the `datetime()` method from the `RTC` class:

###### New Code (Line 5, Lines 43-46)
```python=1
import usocket  
import ubinascii
import time
from machine import RTC
from pystubit.board import display  # Imports object to control the LED display
from mywifi import MyWiFi
```

```python=34
    server_receive = int(ubinascii.hexlify(msg[32:36]), 16) - DELTA
    server_transmit = int(ubinascii.hexlify(msg[40:44]), 16) - DELTA
    offset = ((server_receive - client_transmit) + (server_transmit - client_receive)) / 2
    current_time = time.time() + offset + UTC_TIMEZONE_JP
    datetime = time.localtime(int(current_time))
    datetime = datetime[:3] + (datetime[6],) + datetime[3:6] + (0,) 
    rtc.init(datetime)
    

def scroll_time():  # This function scrolls the current RTC time across the display
    datetime = rtc.datetime()  # Retrieves current date and time as a tuple
    msg = "{}:{}".format(datetime[4], datetime[5])  # Makes a string in hour:minute format to show
    display.scroll(msg, delay=10, color=(31,31,31))  # Scrolls the message across the LED display in white
```

While the LED display lights up white in this program, it could be a good idea to make it change colors depending on the time zone!

#### ■ Using an Ultrasonic Sensor to Show the Time

Let’s add a main loop process which uses the Ultrasonic Sensor on your digital clock to show the time when you bring your hand close! We’ll put this process into a function called `main()`:

New Code (Line 6, Line 17, Lines 51-56)
```python=1
import usocket  
import ubinascii
import time
from machine import RTC
from pystubit.board import display
from pyatcrobo2.parts import UltrasonicSensor  # Imports the Ultrasonic Sensor class
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
NTP_SERVER = "ntp.nict.jp"
DELTA = 3155673600
UTC_TIMEZONE_JP = 32400

rtc = RTC()
timer = Timer(1)
us = UltrasonicSensor("P0")  # Creates an object to control the Ultrasonic Sensor
wifi = MyWiFi()
```
```python=45
def scroll_time():
    datetime = rtc.datetime()
    msg = "{}:{}".format(datetime[4], datetime[5])
    display.scroll(msg, delay=10, color=(31,31,31))
    

def main():  # Main loop process
    while True:
        distance = us.get_distance()  # Retrieves the hand\’s distance from the Ultrasonic Sensor
        if distance > 0 and distance < 5:  # If hand is less than 5 cm away
            scroll_time()  # Scrolls current RTC time across the display
        time.sleep_ms(20)  # Waits a moment before retrieving distance again
```

#### ■ Correcting the Time Automatically

We’re going to use a timer interrupt from the `Timer` class in order to periodically correct the digital clock’s time. The NICT’s Terms of Use limits polling intervals to 20 times per hour. Let’s set our clock to correct itself six times per hour (or once every 10 minutes) here!

##### Example Code 5-3-1
###### New and Changed Code (Line 4, Line 16, Line 20, Lines 53-54, Line 62)
```python=1
import usocket  
import ubinascii
import time
from machine import RTC, Timer  # Adds Timer class to imports
from pystubit.board import display
from pyatcrobo2.parts import UltrasonicSensor
from mywifi import MyWiFi

SSID = "SSID"
PASSWORD = "PASSWORD"
NTP_SERVER = "ntp.nict.jp"
DELTA = 3155673600
UTC_TIMEZONE_JP = 32400

rtc = RTC()
timer = Timer(1)  # Creates a Timer class object
us = UltrasonicSensor("P0")
wifi = MyWiFi()

def correct_time(t=None):  # Adds an argument. The Timer object is passed to this argument when the timer interrupt is called.
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    addrinfo = usocket.getaddrinfo(NTP_SERVER, 123) 
    sockaddr = addrinfo[0][-1]
    # print(sockaddr) 
    s = usocket.socket(usocket.AF_INET, usocket.SOCK_DGRAM)
    s.connect(sockaddr)
    data = bytearray(48)
    data[0] = 0x23
    client_transmit = time.time()
    s.send(data)
    msg = s.recv(48)
    client_receive = time.time()
    s.close()
    # print(msg)
    server_receive = int(ubinascii.hexlify(msg[32:36]), 16) - DELTA
    server_transmit = int(ubinascii.hexlify(msg[40:44]), 16) - DELTA
    offset = ((server_receive - client_transmit) + (server_transmit - client_receive)) / 2
    current_time = time.time() + offset + UTC_TIMEZONE_JP
    datetime = time.localtime(int(current_time))
    datetime = datetime[:3] + (datetime[6],) + datetime[3:6] + (0,) 
    rtc.init(datetime)
    

def scroll_time():
    datetime = rtc.datetime()
    msg = "{}:{}".format(datetime[4], datetime[5])
    display.scroll(msg, delay=10, color=(31,31,31))
    

def main():
    correct_time()  # Initial time correction
    timer.init(mode=Timer.PERIODIC, period=600000, callback=correct_time)  # Sets a timer interrupt every 10 minutes (600 seconds)
    while True:
        distance = us.get_distance()
        if distance > 0 and distance < 5:
            scroll_time()
        time.sleep_ms(20)
        
        
main()  # Runs the main() function
```

By adding the `t` argument argument on line 20 and running a timer interrupt to call the function on line 54, we can pass the caller's timer object (`timer`) back to the argument! The reason we set its default argument to `None` is to prevent any problems when calling the function with no argument set, like on line 53!

Now your program is finished. Try transferring it to your Core Unit and running it to see how it works!

## Challenge: Adding an Alarm Function

In this section, we’ll take on the challenge adding a simple **alarm function** to our digital clock.

##### The Alarm Function
* Repeatedly sounds an alarm at the time set in the program.
* Stops the alarm when you press the A button on the Core Unit.
* Allows you to use the A button to toggle the alarm when the alarm isn’t sounding. ★ Its default setting is off!

### Programming Hints

You’ll need to define two functions in order to make your arm. The time setting for the alarm needs to be directly defined in the program as a `(hour,minute)` tuple, and you’ll need to start the alarm by using a timer interrupt with a newly-created timer object.

##### New Functions
|Function|What does it do?|
|:---|:---|
|`Alarm()` Function|Plays the alarm sound from the buzzer until you press the A button on the Core Unit. This function is called using a timer interrupt. |
|`Toggle_alarm()` Function|Toggles the alarm between on and off when you press the A button on the Core Unit. Turning the alarm off cancels the timer interrupt. Turning the alarm on sets the timer interrupt to run only once. |

Treat these hints as suggestions at the most. Try your best to make a program that you’ve thought of by yourself! If you’re having trouble thinking of anything, follow along as we make the example program below!

### Making the Program

We’ll defined the two functions introduced in the hints above in order.

#### ■ Making the `alarm()` Function

The `alarm()` function uses the buzzer to play the alarm sound until you press the A button. We’ll need to import objects for both the A button at the buzzer at the beginning of this program.

###### Changed Code (Line 5)
```python=1
import usocket  
import ubinascii
import time
from machine import RTC, Timer
from pystubit.board import display, button_a, buzzer  # Imports objects to control the A button and the buzzer
from pyatcrobo2.parts import UltrasonicSensor
from mywifi import MyWiFi
```

Next, we’ll prepare a flag which tells us whether the alarm is turned off or turned on. The default setting of the alarm should be off, and this flag should switch to off when we stop the alarm!

###### New Code (Line 19)
```python=15
rtc = RTC()
timer = Timer(1)
us = UltrasonicSensor("P0")
wifi = MyWiFi()
is_alarm_on = False   # True when the alarm is on and False when the alarm is off
```

The alarm will continue to play the note C7 while on. This will repeat until the `alarm()` function recognizes that you’ve pressed the A button. Since we’ll call the `alarm()` function with a timer interrupt just like the `correct_time()` function, we’ll add an argument to the `alarm()` which takes the caller’s timer object! We’ll set the default argument’s value to `None` so we can call the function independently!

###### New Code (Lines 54-61)
```python=48
def scroll_time():
    datetime = rtc.datetime()
    msg = "{}:{}".format(datetime[4], datetime[5])
    display.scroll(msg, delay=10, color=(31,31,31))
    

def alarm(t=None):  # The timer interrupt calls this function
    global is_alarm_on  # Declare this variable as global so it can be overwritten
    while not button_a.is_pressed():  # Continuously sounds the alarm until you press the A button
        buzzer.on("C7", duration=200)
        time.sleep_ms(100)
    while button_a.is_pressed():  # Waits until you release the A button  
        pass
    is_alarm_on = False  # Sets the flag to off
```

Now let’s run the program and call the `alarm()` function from the terminal. Check to see if the alarm stops when you press the A button!

#### ■ Making the `toggle_alarm()` Function

This function will contain the code which sets or cancels the `alarm()` function’s timer interrupt. We’ll start by preparing a new timer object.

###### New Code (Line 17)
```python=15
rtc = RTC()
timer = Timer(1)
timer_for_alarm = Timer(2)  # Timer object for the alarm
us = UltrasonicSensor("P0")
wifi = MyWiFi()
is_alarm_on = False
```

This section defines the time used to start the alarm as `alarm_time`. The time is shown as an `(hour, minute)` tuple.

###### New Code (Line 21)
```python=15
rtc = RTC()
timer = Timer(1)
timer_for_alarm = Timer(2)
us = UltrasonicSensor("P0")
wifi = MyWiFi()
is_alarm_on = False
alarm_time = (7, 30)  # (hour, minute)
```

Now let's make the `toggle_alarm()` function. This function will cancel the timer interrupt if the value of the `is_alarm_flag` is `True`.

###### New Code (Lines 65-71)
```python=65
def toggle_alarm():
    global is_alarm_on
    if is_alarm_on:  # If the alarm is currently turned on
        timer_for_alarm.deinit()  # Cancels the timer interrupt
        is_alarm_on = False  # Sets the flag to off
        display.scroll("OFF", delay=10, color=(31, 0, 0))  # Scrolls the word OFF across the display
    else:  # If the alarm is currently turned off
        # Write the code to set the timer interrupt below here
```

Setting `is_alarm_on` to `False` sets a new timer interrupt. In order to set this, we need to know the difference between the current time and the start time of the alarm. The formula we use to calculate this changes depending on the current time and the time we've set the alarm to start for that day.

* **If the current time is before the alarm start time...**
The time until the alarm sounds is `alarm start time - current time`.
* **If the current time is after the alarm start time...**
Since the alarm will sound at the same time the next day, the time until the alarm sounds is `(alarm start time + 24 hours) - current time`.

Let’s keep this in mind as we add the code shown below:

###### New Code (Lines 72-81)
```python=55
def alarm(t=None):
    global is_alarm_on
    while not button_a.is_pressed():
        buzzer.on("C7", duration=200)
        time.sleep_ms(100)
    while button_a.is_pressed(): 
        pass
    is_alarm_on = False
    
    
def toggle_alarm():
    global is_alarm_on
    if is_alarm_on:
        timer_for_alarm.deinit()
        is_alarm_on = False
        display.scroll("OFF", delay=10, color=(31, 0, 0))
    else:
        datetime = rtc.datetime()  # Retrieves current RTC time
        secs_now = datetime[4] * 3600 + datetime[5] * 60 + datetime[6]  # Gets the current time by converting the amount of time that has passed since 00:00:00 of the current day into seconds
        secs_alarm = alarm_time[0] * 3600 + alarm_time[1] * 60  # Sets the alarm start time with the same conversion as the previous line
        if secs_alarm > secs_now:  # If the current time is before the the start time for today\`s alarm
            secs = secs_alarm - secs_now  # Calculates the difference as-is
        else:  # If the current time is after the the start time for today\`s alarm
            secs = secs_alarm + 86400 - secs_now  # Calculates the difference in seconds to the same time tomorrow
        timer_for_alarm.init(mode=Timer.ONE_SHOT, period=secs*1000, callback=alarm)  # Uses a timer interrupt to set the alarm once. Note that the period argument is in milliseconds.
        is_alarm_on = True  # Raises the flag
        display.scroll("ON", delay=10, color=(31, 0, 0))  # Scrolls the word ON across the display
```

Now let’s call this function every time you press the A button to turn the alarm on or off. Once you’ve done that, the program is finished!

###### New Code (Lines 91-92)
```python=84
def main():
    correct_time()
    timer.init(mode=Timer.PERIODIC, period=600000, callback=correct_time)
    while True:
        distance = us.get_distance()
        if distance > 0 and distance < 5:
            scroll_time()
        if button_a.is_pressed():  # Turns the alarm on or of
            toggle_alarm()  # when you press the A button
        time.sleep_ms(20)
```

The entire program will look like this one you’re done. If you can’t fix errors in your program, try comparing this code with yours!

##### Example Code 6-2-1

```python=1
import usocket  
import ubinascii
import time
from machine import RTC, Timer
from pystubit.board import display, button_a, buzzer  # Imports objects to control the A button and the buzzer
from pyatcrobo2.parts import UltrasonicSensor
from mywifi import MyWiFi

SSID = "atatro"
PASSWORD = "dgsrfdavfjm43"
NTP_SERVER = "ntp.nict.jp"
DELTA = 3155673600
UTC_TIMEZONE_JP = 32400

rtc = RTC()
timer = Timer(1)
timer_for_alarm = Timer(2)
us = UltrasonicSensor("P0")
wifi = MyWiFi()
is_alarm_on = False
alarm_time = (18, 40)

def correct_time(t=None):
    if not wifi.isconnected():
        if not wifi.connect(SSID, PASSWORD):
            return
    addrinfo = usocket.getaddrinfo(NTP_SERVER, 123) 
    sockaddr = addrinfo[0][-1]
    # print(sockaddr) 
    s = usocket.socket(usocket.AF_INET, usocket.SOCK_DGRAM)
    s.connect(sockaddr)
    data = bytearray(48)
    data[0] = 0x23
    client_transmit = time.time()
    s.send(data)
    msg = s.recv(48)
    client_receive = time.time()
    s.close()
    # print(msg)
    server_receive = int(ubinascii.hexlify(msg[32:36]), 16) - DELTA
    server_transmit = int(ubinascii.hexlify(msg[40:44]), 16) - DELTA
    offset = ((server_receive - client_transmit) + (server_transmit - client_receive)) / 2
    current_time = time.time() + offset + UTC_TIMEZONE_JP
    datetime = time.localtime(int(current_time))
    datetime = datetime[:3] + (datetime[6],) + datetime[3:6] + (0,) 
    rtc.init(datetime)
    

def scroll_time():
    datetime = rtc.datetime()
    msg = "{}:{}".format(datetime[4], datetime[5])
    display.scroll(msg, delay=10, color=(31,31,31))


def alarm(t=None):
    global is_alarm_on
    while not button_a.is_pressed():
        buzzer.on("C7", duration=200)
        time.sleep_ms(100)
    while button_a.is_pressed():
        pass
    is_alarm_on = False


def toggle_alarm():
    global is_alarm_on
    if is_alarm_on:
        timer_for_alarm.deinit()
        is_alarm_on = False
        display.scroll("OFF", delay=10, color=(31, 0, 0))
    else:
        datetime = rtc.datetime()
        secs_now = datetime[4] * 3600 + datetime[5] * 60 + datetime[6]
        secs_alarm = alarm_time[0] * 3600 + alarm_time[1] * 60
        if secs_alarm > secs_now:
            secs = secs_alarm - secs_now
        else:
            secs = secs_alarm + 86400 - secs_now
        timer_for_alarm.init(mode=Timer.ONE_SHOT, period=secs*1000, callback=alarm)
        is_alarm_on = True
        display.scroll("ON", delay=10, color=(31, 0, 0))
        

def main():
    correct_time()
    timer.init(mode=Timer.PERIODIC, period=600000, callback=correct_time)
    while True:
        distance = us.get_distance()
        if distance > 0 and distance < 5:
            scroll_time()
        if button_a.is_pressed():
            toggle_alarm()
        time.sleep_ms(20)
        
        
main()
```

## Conclusion
### A Quick Review
In this lesson we studied the Internet protocols used on the four layers of the Internet in much more detail:

* The Application Layer
* The Transport Layer
* The Internet Layer
* The Network Interface Layer

We also introduced the **N**etwork **T**imer **P**rotocol, or **NTP**. Just like HTTP, this protocol is used on the the application layer, and every computer on the Internet can use it to sync their clocks! We then used the NTP to set the real-time clock on your Core Unit to the current time.

Just like this, a computer's ability to set its clock to the most accurate time comes in very handy when coordinating with other services which make use of the Internet! A good example would be scheduling an email. If the computer's clock isn't set correctly, it might send the email out at the wrong time. Using NTP to set the clock to the correct time helps avoid problems like these!

If you find yourself building a robot which uses the RTC in the future, try your best to apply what you've learned here!

### Coming Up Next Time!

In the next lesson, we’ll take what we learned in Topic 11-2 and 11-3 to connect an **everyday object to the Internet of Things** and make an **UmbrellaBot**!

