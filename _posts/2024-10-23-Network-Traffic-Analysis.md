---
title: Network Traffic Analysis
date: 2024-10-23 10:18:00 +0300
categories: [HackTheBox, Network Security, Cyber Forensics, Network Traffic Analysis]
tags: [hackthebox, traffic-analysis, packet-inspection, tcpdump, wireshark, IDS, intrusion-detection, CTF]
image: [NTAhero.png](https://postimg.cc/N24nV21m)
---



# Introduction


Network Traffic Analysis (NTA) involves analyzing network traffic to identify prevalent ports and protocols, establish a baseline for the network environment, monitor and address potential threats, and gain comprehensive insights into the organization's network infrastructure. By enabling security experts to promptly and accurately detect anomalies, such as security threats, NTA plays a crucial role in enhancing network security. Furthermore, NTA supports adherence to security protocols by identifying evolving attack strategies aimed at bypassing detection and leveraging authorized tools within network systems, posing challenges for cybersecurity defenders.


# Objectives:


\- Enhance understanding of TCP/IP stack & OSI model


\-Analysis using Tcpdump & Wireshark


 **Knowledge Check * ?**


## Networking Primer – Layers 1-4


![](/assets/img/Posts/NTA/images/image2.png)


How many layers does the OSI model have?

_7_

How many layers are there in the TCP/IP model?

_4_

True or False: Routers operate at layer 2 of the OSI model?

_False_

What addressing mechanism is used at the Link Layer of the TCP/IP model?

_Mac-Address_

At what layer of the OSI model is a PDU encapsulated into a packet? ( the number )

_3_

![](/assets/img/Posts/NTA/images/image4.png)

What addressing mechanism utilizes a 32-bit address?

_IPv4_

![](/assets/img/Posts/NTA/images/image3.png)

What Transport layer protocol is connection-oriented?

_TCP_

What Transport Layer protocol is considered unreliable?

_UDP_

![](/assets/img/Posts/NTA/images/image6.png)

TCP’s three-way handshake consists of 3 packets: 1.Syn, 2.Syn & ACK, 3. \_? What is the final packet of the handshake?

_ACK_

Networking Primer — Layers 5–7


![](/assets/img/Posts/NTA/images/image5.png)


What is the default operational mode method used by FTP?

_active_

FTP utilizes what two ports for command and data transfer? (separate the two numbers with a space)

_20 21_

Does SMB utilize TCP or UDP as its transport layer protocol?

_TCP_

![](/assets/img/Posts/NTA/images/image8.png)

SMB has moved to using what TCP port?

_445_

![](/assets/img/Posts/NTA/images/image7.png)

Hypertext Transfer Protocol uses what well-known TCP port number?

_80_

What HTTP method is used to request information and content from the web server?

_GET_

 True or False: When utilizing HTTPS, all data sent across the session will appear as TLS Application data?

_True_

## TCPdump Fundamentals


(Question-1.zip had an image , attached below)

![](/assets/img/Posts/NTA/images/image10.png)

Utilizing the output shown in question-1.png, who is the server in this communication? (IP Address)

_174.143.213.184_

Were absolute or relative sequence numbers used during the capture? (see question-1.zip to answer)

_Relative_

![](/assets/img/Posts/NTA/images/image9.png)

If I wish to start a capture without hostname resolution, verbose output, showing contents in ASCII and hex, and grab the first 100 packets; what are the switches used? please answer in the order the switches are asked for in the question.

_\-nvXc 100_

Given the capture file at /tmp/capture.pcap, what tcpdump command will enable you to read from the capture and show the output contents in Hex and ASCII? (Please use best practices when using switches)

_sudo tcpdump -Xr /tmp/capture.pcap_

What TCPDump switch will increase the verbosity of our output? ( Include the — with the proper switch )

_\-v_

What built-in terminal help reference can tell us more about TCPDump?

_Man_

What TCPDump switch will let me write my output to a file?

_\-w_

Fundamentals Lab


![](/assets/img/Posts/NTA/images/image13.png)


What TCPDump switch will allow us to pipe the contents of a pcap file out to another function such as ‘grep’?

_\-l_

True or False: The filter “port” looks at source and destination traffic.

_True_

If we wished to filter out ICMP traffic from our capture, what filter could we use? ( word only, not symbol please.)

_not icmp_

What command will show you where / if TCPDump is installed?

_which tcpdump_

How do you start a capture with TCPDump to capture on eth0?

_tcpdump -i eth0_

What switch will provide more verbosity in your output?

_\-v_

What switch will write your capture output to a .pcap file?

_\-w_

What switch will read a capture from a .pcap file?

_\-r_

What switch will show the contents of a capture in Hex and ASCII?

_\-X_

## Tcpdump Packet Filtering


What filter will allow me to see traffic coming from or destined to the host with an ip of 10.10.20.1?

_host 10.10.20.1_

What filter will allow me to capture based on either of two options?

_or_

True or False: TCPDump will resolve IPs to hostnames by default

_True_

## Interrogating Network Traffic With Capture and Display Filters


(The section requires we unzip TCPDump-lab-2.zip)


![](/assets/img/Posts/NTA/images/image11.png)


What are the client and server port numbers used in first full TCP three-way handshake? (low number first then high number)


_80 43806_


Based on the traffic seen in the pcap file, who is the DNS server in this network segment? (ip address)


_172.16.146.1_


![](/assets/img/Posts/NTA/images/image12.png)


Analysis with Wireshark


![](/assets/img/Posts/NTA/images/image14.png)


True or False: Wireshark can run on both Windows and Linux.

_True_

Which Pane allows a user to see a summary of each packet grabbed during the capture?

_Packet List_

Which pane provides you insight into the traffic you captured and displays it in both ASCII and Hex?

_Packet Bytes_

![](/assets/img/Posts/NTA/images/image15.png)

What switch is used with TShark to list possible interfaces to capture on?

_\-D_

What switch allows us to apply filters in TShark?

_\-f_

Is a capture filter applied before the capture starts or after? (answer before or after)

_Before_

## Wireshark Advanced Usage


![](/assets/img/Posts/NTA/images/image16.png)


Which plugin tab can provide us with a way to view conversation metadata and even protocol breakdowns for the entire PCAP file?

_Statistics_

What plugin tab will allow me to accomplish tasks such as applying filters, following streams, and viewing expert info?

_Analyze_

What stream-oriented Transport protocol enables us to follow and rebuild conversations and the included data?

_TCP_

True or False: Wireshark can extract files from HTTP traffic.

_True_

True or False: The ftp-data filter will show us any data sent over TCP port 21.

_False_

## Packet Inception, Dissecting Network Traffic With Wireshark


![](/assets/img/Posts/NTA/images/image17.png)What was the filename of the image that contained a certain Transformer Leader? (name.filetype)

_Rise-Up.jpg_

Which employee is suspected of performing potentially malicious actions in the live environment? 

_Bob_

## Guided Lab: Traffic Analysis Workflow


What was the name of the new user created on Mr. Ben’s host?

_hacker_

How many total packets were there in the Guided-analysis PCAP?

_44_

What was the suspicious port that was being used?

_4444_

![](/assets/img/Posts/NTA/images/image18.png)

## Decrypting RDP connections


What user account was used to initiate the RDP connection?

_Bucky_

![](/assets/img/Posts/NTA/images/image19.png)

![](/assets/img/Posts/NTA/images/image20.png)


Glad to share this achievement:  [https://academy.hackthebox.com/achievement/1317759/81](https://www.google.com/url?qhttps://academy.hackthebox.com/achievement/1317759/81&saD&sourceeditors&ust1729842341275683&usgAOvVaw2FFagzg6wg_HxzlZyKi7iz)


# Conclusion


Completing the Network Traffic Analysis module has given me a solid foundation in analyzing and interpreting network data, which is essential in cybersecurity. By working hands-on with tools like Wireshark and tcpdump, I’ve learned to spot malicious patterns and leverage intrusion detection systems effectively. This experience has strengthened my skills in detecting network anomalies and has equipped me with practical tools and methodologies that I can apply in real-world network defense and threat intelligence scenarios.