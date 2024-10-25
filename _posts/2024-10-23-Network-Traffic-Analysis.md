---
title: Network Traffic Analysis
date: 2024-08-20 10:18:00 +0300
categories: [HackTheBox, Network Security, Cyber Forensics, Network Traffic Analysis]
tags: [hackthebox, traffic-analysis, packet-inspection, tcpdump, wireshark, IDS, intrusion-detection, CTF]
image: /assets/img/Posts/images/NTAhero.png
---



Introduction
============

Network Traffic Analysis (NTA) involves analyzing network traffic to identify prevalent ports and protocols, establish a baseline for the network environment, monitor and address potential threats, and gain comprehensive insights into the organization's network infrastructure. By enabling security experts to promptly and accurately detect anomalies, such as security threats, NTA plays a crucial role in enhancing network security. Furthermore, NTA supports adherence to security protocols by identifying evolving attack strategies aimed at bypassing detection and leveraging authorized tools within network systems, posing challenges for cybersecurity defenders.
=========================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================

Objectives:
===========

\- Enhance understanding of TCP/IP stack & OSI model
====================================================

\-Analysis using Tcpdump & Wireshark
====================================

 Answer to questions
====================

Networking Primer – Layers 1-4
==============================

![](/assets/img/Posts/images/image2.png)
======================

How many layers does the OSI model have?

7

How many layers are there in the TCP/IP model?

4

True or False: Routers operate at layer 2 of the OSI model?

False

What addressing mechanism is used at the Link Layer of the TCP/IP model?

Mac-Address

At what layer of the OSI model is a PDU encapsulated into a packet? ( the number )

3

![](/assets/img/Posts//assets/img/Posts/images/image4.png)

What addressing mechanism utilizes a 32-bit address?

IPv4

![](/assets/img/Posts/images/image3.png)

What Transport layer protocol is connection-oriented?

TCP

What Transport Layer protocol is considered unreliable?

UDP

![](/assets/img/Posts/images/image6.png)

TCP’s three-way handshake consists of 3 packets: 1.Syn, 2.Syn & ACK, 3. \_? What is the final packet of the handshake?

ACK

Networking Primer — Layers 5–7
==============================

![](/assets/img/Posts/images/image5.png)
======================

What is the default operational mode method used by FTP?

active

FTP utilizes what two ports for command and data transfer? (separate the two numbers with a space)

20 21

Does SMB utilize TCP or UDP as its transport layer protocol?

TCP

![](/assets/img/Posts/images/image8.png)

SMB has moved to using what TCP port?

445

![](/assets/img/Posts/images/image7.png)

Hypertext Transfer Protocol uses what well-known TCP port number?

80

What HTTP method is used to request information and content from the web server?

GET

 True or False: When utilizing HTTPS, all data sent across the session will appear as TLS Application data?

True

TCPdump Fundamentals
====================

(Question-1.zip had an image , attached below)

![](/assets/img/Posts/images/image10.png)

Utilizing the output shown in question-1.png, who is the server in this communication? (IP Address)

174.143.213.184

Were absolute or relative sequence numbers used during the capture? (see question-1.zip to answer)

Relative

![](/assets/img/Posts/images/image9.png)

If I wish to start a capture without hostname resolution, verbose output, showing contents in ASCII and hex, and grab the first 100 packets; what are the switches used? please answer in the order the switches are asked for in the question.

\-nvXc 100

Given the capture file at /tmp/capture.pcap, what tcpdump command will enable you to read from the capture and show the output contents in Hex and ASCII? (Please use best practices when using switches)

sudo tcpdump -Xr /tmp/capture.pcap

What TCPDump switch will increase the verbosity of our output? ( Include the — with the proper switch )

\-v

What built-in terminal help reference can tell us more about TCPDump?

Man

What TCPDump switch will let me write my output to a file?

\-w

Fundamentals Lab
================

![](/assets/img/Posts/images/image13.png)
=======================

What TCPDump switch will allow us to pipe the contents of a pcap file out to another function such as ‘grep’?

\-l

True or False: The filter “port” looks at source and destination traffic.

True

If we wished to filter out ICMP traffic from our capture, what filter could we use? ( word only, not symbol please.)

not icmp

What command will show you where / if TCPDump is installed?

which tcpdump

How do you start a capture with TCPDump to capture on eth0?

tcpdump -i eth0

What switch will provide more verbosity in your output?

\-v

What switch will write your capture output to a .pcap file?

\-w

What switch will read a capture from a .pcap file?

\-r

What switch will show the contents of a capture in Hex and ASCII?

\-X

Tcpdump Packet Filtering
========================

What filter will allow me to see traffic coming from or destined to the host with an ip of 10.10.20.1?

host 10.10.20.1

What filter will allow me to capture based on either of two options?

or

True or False: TCPDump will resolve IPs to hostnames by default

True

Interrogating Network Traffic With Capture and Display Filters
==============================================================

(The section requires we unzip TCPDump-lab-2.zip)
=================================================

![](/assets/img/Posts/images/image11.png)
=======================

What are the client and server port numbers used in first full TCP three-way handshake? (low number first then high number)
===========================================================================================================================

80 43806
========

Based on the traffic seen in the pcap file, who is the DNS server in this network segment? (ip address)
=======================================================================================================

172.16.146.1
============

![](/assets/img/Posts/images/image12.png)
=======================

Analysis with Wireshark
=======================

![](/assets/img/Posts/images/image14.png)
=======================

True or False: Wireshark can run on both Windows and Linux.

True

Which Pane allows a user to see a summary of each packet grabbed during the capture?

Packet List

Which pane provides you insight into the traffic you captured and displays it in both ASCII and Hex?

Packet Bytes

![](/assets/img/Posts/images/image15.png)

What switch is used with TShark to list possible interfaces to capture on?

\-D

What switch allows us to apply filters in TShark?

\-f

Is a capture filter applied before the capture starts or after? (answer before or after)

Before

Wireshark Advanced Usage
========================

![](/assets/img/Posts/images/image16.png)
=======================

Which plugin tab can provide us with a way to view conversation metadata and even protocol breakdowns for the entire PCAP file?

Statistics

What plugin tab will allow me to accomplish tasks such as applying filters, following streams, and viewing expert info?

Analyze

What stream-oriented Transport protocol enables us to follow and rebuild conversations and the included data?

TCP

True or False: Wireshark can extract files from HTTP traffic.

True

True or False: The ftp-data filter will show us any data sent over TCP port 21.

False

Packet Inception, Dissecting Network Traffic With Wireshark
===========================================================

![](/assets/img/Posts/images/image17.png)What was the filename of the image that contained a certain Transformer Leader? (name.filetype)

Rise-Up.jpg

Which employee is suspected of performing potentially malicious actions in the live environment? Bob

Guided Lab: Traffic Analysis Workflow
=====================================

What was the name of the new user created on Mr. Ben’s host?

hacker

How many total packets were there in the Guided-analysis PCAP?

44

What was the suspicious port that was being used?

4444

![](/assets/img/Posts/images/image18.png)

Decrypting RDP connections
==========================

What user account was used to initiate the RDP connection?

Bucky

![](/assets/img/Posts/images/image19.png)

![](/assets/img/Posts/images/image20.png)
=======================

Glad to share this achievement:  [https://academy.hackthebox.com/achievement/1317759/81](https://www.google.com/url?q=https://academy.hackthebox.com/achievement/1317759/81&sa=D&source=editors&ust=1729842341275683&usg=AOvVaw2FFagzg6wg_HxzlZyKi7iz)
======================================================================================================================================================================================================================================================

Conclusion
==========

Completing the Network Traffic Analysis module has given me a solid foundation in analyzing and interpreting network data, which is essential in cybersecurity. By working hands-on with tools like Wireshark and tcpdump, I’ve learned to spot malicious patterns and leverage intrusion detection systems effectively. This experience has strengthened my skills in detecting network anomalies and has equipped me with practical tools and methodologies that I can apply in real-world network defense and threat intelligence scenarios.