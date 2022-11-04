# (CSC 361) - Computer Networks - Complete Notes



[TOC]



# Class Intro

>Office hours: Monday 10am - 12pm on zoom
>
>pan@uvic.ca
>
>https://web.uvic.ca/~pan
>
>tutorial starts sept 9th 1:30PM

## Content

#### First month

- IPS
- http and DNS

#### Second month

- TCP and UDP

#### Third month

- Network layers: IP addressing and routing
- link layers: logical link, media access

## Course evaluation

- one written assignment (5%)
- Three in class midterms (45%)
- Three programming assignments (35%) 
- Weekly homework (10%) + participation (5%)

# Written Assignments

## Worksheet 1 Answers

Internet access over telephone lines has been driven by our desire for higher speed. Given the following fact: voice bandwidth: 3000 Hz, signal-to-noise ratio between two phone sets: 30dB, modem sampling rate: 2400 baud, please explain concisely:

- **how your old modem was improved from 2.4 to 9.6 and 28.8kbps in speed.**
  - ANS: By packing more bits in a denser symbol from 1 to 4 bits/symbol at 2400 samples per second for 2.4 to 9.6 kbps
- **Why does the improvement stop around 33.6 Kbps?**
  - ANS: due to limited S/N (30dB = 3B = 10^3) and bandwidth (3kHz) [3000* log_2 (1+10^3) = about 30kbps]
- **How was 56 Kbps achieved otherwise?**
  - ANS: By removing the modem at ISP so higher S/N and asymetric bandwidth allocation so 56kbps downlink
- How does your modem or ONT achieve higher speeds?
  - ANS: By removing the filter at the switch so higher bandwidth over shorter distances, or much better bandwidth with fiber.

 (for the lecture next week on HTTP) A simplified HTML document is at http://a.b.com/index.html

```html
<html>
<a href="fig/tiny.gif"><img src="fig/small.gif"></a>
<img src="http://x.y.com/fig/big.gif">
<img src="http://a.b.com/fig/huge.gif">
</html>
```

Please show your work. For a web browser that loads images (GIF files) automatically, how long (in round-trip
time) does it take, excluding the DNS overhead, to display the entire Web page if all files are small enough to
be accommodated in one packet and:

**(a) non-persistent, non-parallel HTTP connections are used [0.5]**

ANS: 2RTTs for index, 2 for small, 2 for big and 2 for huge (2+2+2+2 = 8RTTs with 4 TCP connections) 

**(b) non-persistent HTTP connections with at most two parallel connections are used [0.5]**

ANS: 2RTTs for index, 2 for small and big in 2 parallel connections and 2 for huge. (2+2+2=6 RTTs with 2 TCPs)

**(c) persistent, non-parallel HTTP connections without pipelining are used [0.5]**

ANS: 2 RTTs for index , 1 for small, 1 for huge and 2 RTTs for big on x.y.com (2+1+1+2 = 6 RTTS with 2TCPs)

**(d) persistent, non-parallel HTTP connections with pipelining are used. [0.5]**

ANS:2 RTTs for index, 1 for both small and huge, and 2 RTTs for big on x.y.com (2+1+2 = 5RTTs with 2TCPs)

# MIDTERM 2 

## Midterm Content

> 3.1 - 5.2 
>
> - **Chapter 3: Transport Layer (TCP // UDP)**
>
>   - **<u>3.1 - Intro</u>**
>
>     - 3.1.1 Relationship Between Transport and Network Layers
>     - 3.1.2 Overview of the Transport Layer in the Internet
>
>   - <u>**3.2 - Multiplexing and Demultiplexing**</u>
>
>   - <u>**3.3 - Connectionless Transport: UDP**</u>
>
>     - 3.3.1 UDP Segment Structure
>     - 3.3.2 UDP Checksum
>
>   - **<u>3.4 - Principles of Reliable Data Transfer</u>**
>
>     - 3.4.1 Building a Reliable Data Transfer Protocol 
>     - 3.4.2 Pipelined Reliable Data Transfer Protocols 
>     - 3.4.3 Go-Back-N (GBN) 
>     - 3.4.4 Selective Repeat (SR)
>
>   - **<u>3.5 - Connection-Oriented Transport: TCP</u>**
>
>     - 3.5.1 The TCP Connection
>     - 3.5.2 TCP Segment Structure
>     - 3.5.3 Round-Trip Time Estimation and Timeout 
>     - 3.5.4 Reliable Data Transfer
>     - 3.5.5 Flow Control 
>     - 3.5.6 TCP Connection Management
>
>   - **<u>3.6 - Principles of Congestion Control</u>**
>
>     - 3.6.1 The Causes and the Costs of Congestion 
>     - 3.6.2 Approaches to Congestion Control
>
>   - **<u>3.7 - TCP Congestion Control</u>**
>
>     - 3.7.1 Fairness
>     - 3.7.2 Explicit Congestion Notification (ECN): Network-assisted Congestion Control
>
>     
>
> - **Chapter 4: The Network Layer  (Data Plane)** 
>
>   - **<u>4.1 - Overview of Network Layer</u>**
>
>     - 4.1.1 Forwarding and Routing: The Network Data and Control Planes
>     - 4.1.2 Network Service Models
>
>   - **<u>4.3 - The Internet Protocol (IP): IPv4, Addressing, IPv6, and More</u>**
>
>     - 4.3.1 IPv4 Datagram Format
>     - 4.3.2 IPv4 Datagram Fragmentation 
>     - 4.3.3 IPv4 Addressing
>     - 4.3.4 Network Address Translation (NAT) 
>     - 4.3.5 IPv6
>
>     
>
> - **Chapter 5: The Network Layer (Control Plane)**
>
>   - <u>**5.1 - Introduction**</u>
>   - <u>**5.2 - Routing Algorithms**</u>
>     - 5.2.1 The Link-State (LS) Routing Algorithm 
>     - 5.2.2 The Distance-Vector (DV) Routing Algorithm

> <u>**Note:**</u> 
>
> - **pros and cons of:**
>   - The Link-State (LS) Routing Algorithm
>   - The Distance-Vector (DV) Routing Algorithm
> - **Calculate shortest path of:**
>   - The Link-State (LS) Routing Algorithm
>   - The Distance-Vector (DV) Routing Algorithm

# Chapter 1 - Computer Networks and the Internet

![image-20221006171245818](image-20221006171245818.png)

End systems are made up of: 

- Communication links
- packet switches

These can consist of variable speeds depending on the link used, each one can effect the **transmission rate** (Bps). 

>  **Packet switch** 
>
> - takes an incoming packet and forwards it to the necessary location. (ex. Routers and link layered switches)

## 1.1.2 - A services Description

- The applications are said to be distributed applications, since they involve multiple end systems that exchange data with each other. Although packet switches facilitate the exchange of data among end systems, they are not concerned with the application that is the source or sink of data.

> **Socket Interface**
>
> - Specifies how a program running on one end system asks the Internet infrastructure to deliver data to a specific destination program running on another end system.
> - Sets rules that an application must have in order to access the internet.
> - Sets standardization.

## 1.1.3 - What is a protocol

![image-20221006172253314](image-20221006172253314.png)

> **Protocol**
>
> - protocol defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.

Note: Hosts and End-systems are interchangeable so they mean the same thing.

## 1.2.1 Access networks

There are 3 popular types of broadband:

- DSL (Digital subscriber line)
- Cable 
- Fiber
- Broadband Over Powerline (BPL)

#### DSL 

DSL uses phone line and the residential telephone line carries both data and traditional telephone signals simultaneously, which are encoded at different frequencies:

- A high-speed downstream channel, in the 50 kHz to 1 MHz band
- A medium-speed upstream channel, in the 4 kHz to 50 kHz band
- An ordinary two-way telephone channel, in the 0 to 4 kHz band

This approach makes the single DSL link appear as if there were three separate links, so that a telephone call and an Internet connection can share the DSL link at the same time.

As with DSL, access is typically asymmetric, with the downstream channel typically allocated a higher transmission rate than the upstream channel.

#### Cable Internet Access

makes use of the cable television company’s existing cable television infrastructure. A residence obtains cable Internet access from the same company that provides its cable television.

Traditionally, cable TV is 1-way broadcast

- One-way amplifier 
- Shared coaxial cable

#### Fiber 

HFC (Hybrid Fiber Connection)  or Data over cable service interface specification(DOCSIS).

- two-way communication channels
  - small upstream bandwidth
  - larger downstream bandwidth
- smaller (shared) cable segment
  - Security

#### Broadband Over Powerline (BPL)

- **high voltage lines**
  - very noisy
- **medium voltage lines**
  - coupler or repeater
    - to bypass transformer
- **low voltage lines**
  - bridge
    - wired/wireless access point
  - customer
    - plug-and-play

##### Analog Dial-up

- to computer w/ ISA, PCI, serial, USB
- to modem w/ RJ11
- to telephone line
  - unshielded twisted pair (UTP) or flat
- up to 56 Kbps downstream

##### Nyquist, Shannon Limit and Bandwidth, sample, symbol, bit, data rate

Some factors can limit the maximum transmission rate of a transmission system. 

- Nyquist's theorem specifies the maximum data rate for noiseless condition
- Shannon theorem specifies the maximum data rate under a noise condition.

> Legend:
>
> H: channel bandwidth 
>
> V: # of different kinds of symbols 
>
> S: signal 
>
> N: noise
>
> S/N: signal-to-noise ratio (SNR)

###### Nyquist Limit (idealistic, noiseless channel)

$$
2H\log_{2}{V}
$$

###### Shannon Limit (Noisy Channel)

- analog local loop: H=3000~4000Hz; S/N=30dB=10^3

$$
H\log_{2}{(1+\frac{S}{N})}
$$

###### Bandwidth, sample, symbol, bit, data rate

- (bits / symbol) * (symbols / second) = bps
- baud rate: 2400; data rate: 9.6, 19.2, 28.8, 33.6Kbps

#### Internet Access: LAN

Types of Connections:

- RJ-45 (AUI, BNC w/ coaxal)
- UTP Cat 3 (10Mbps)
- UTP Cat 5: (100 Mbps) 
  - More twists per inch

#### Internet access: WLAN

Type of connections:

- 802.11b: 2.4GHz, 100ft@11Mbps
- 802.11a: 5GHz, 54Mbps
- 802.11g: 2.4GHz, 54Mbps

#### Internet access: Wireless MAN

Wireless Cable form building to building

- MMDS: 198MHz@2.5GHz - (range: 25~50km; 3Mbps downstream 200Kbps up)
- LMDS: 1.3GHz@28~31GHz - (range: 2~5km, line-of-sight! / wireless DSL: 36Gbps downstream 1Mbps up/sector)
- IEEE 802.16: WiMax - (10~66GHz (802.16a: 2-11GHz NLOS), OFDM)

![image-20221006190208964](image-20221006190208964.png)

#### Internet access: Wireless *AN

- Personal area network (IEEE 802.15) - bluetooth
  - range: 10 meters
- Local area network (802.11)
  - Range: 100 meters
- Metropolitan area network (802.16)
  - range: up to 50km

#### Internet access: cellular networks

- 1st generation (80’s): analog voice (9.6Kbps)
- 2G (90’s): digital voice (14.4Kbps)

- 3G: digital voice and data (384Kbps, 2Mbps)

## 1.3 Packet Switching 

> **Store and foreword transmission**
>
> the packet switch must receive the entire packet before it can begin to transmit the first bit of the packet onto the outbound link.

if the switch instead forwarded bits as soon as they arrive (without first receiving the entire packet), then the total delay would be L/R since bits are not held up at the router. But, as we will discuss in Section 1.4, routers need to receive, store, and process the entire packet before forwarding.

one packet from source to destination over a path consisting of N links each of rate R (thus, there are N-1 routers between source and destination).
$$
d(end-to-end) = N\frac{L}{R}
$$
Where: 

- L = Bits 
- N = Links 
- R = rate 

#### Sending One packet

The source begins to transmit at time 0; at time L/R seconds, the source has transmitted the entire packet, and the entire packet has been received and stored at the router (since there is no propagation delay). At time L/R seconds, since the router has just received the entire packet, it can begin to transmit the packet onto the outbound link towards the destination; at time 2L/R, the router has transmitted the entire packet, and the entire packet has been received by the destination. Thus, the total delay is 2L/R.

TL;DR

1. Time: 0 (Packet is send from source to router/switch)
2. Time: L/R (Packet received in full at router/switch - Starts to now transmit)
3. Time: 2(L/R) (Router transmitted the entire packet to the destination)
4. Done.

#### Sending 3 packets

Now let’s calculate the amount of time that elapses from when the source begins to send the first packet until the destination has received all three packets. As before, at time L/R, the router begins to forward the first packet. But also at time L/R the source will begin to send the second packet, since it has just finished sending the entire first packet. Thus, at time 2L/R, the destination has received the first packet and the router has received the second packet. Similarly, at time 3L/R, the destination has received the first two packets and the router has received the third packet. Finally, at time 4L/R the destination has received all three packets!

1. Time: 0 (Packet is send from source to router/switch)
2. Time: L/R (Packet received in full at router/switch - Starts to now transmit)
3. Time: 2(L/R) (Router transmitted the entire packet to the destination) [Packet 1]
4. Time: 3(L/R) (Router transmitted the entire packet to the destination) [Packet 2]
5. Time: 4(L/R) (Router transmitted the entire packet to the destination) [Packet 3]
6. Done.

#### Delays and packet loss

Each packet switch has multiple links attached to it. For each attached link, the packet switch has an output buffer (also called an output queue). Thus, in addition to the store-and-forward delays, packets suffer output buffer queuing delays.

![image-20221006175705051](image-20221006175705051.png)

Buffer space is **finite** thus packet loss will occur—either the arriving packet or one of the already-queued packets will be dropped.

#### Circuit Switching

There are two fundamental approaches to moving data through a network of links and switches: **circuit switching** and **packet switching**

> **Circuit Switching Networks**
>
> The resources needed along a path (buffers, link
> transmission rate) to provide for communication between the end systems are reserved for the duration of the communication session between the end systems.

> **Packet-Switched Networks**
>
> The resources are not reserved; a session’s messages use the resources on demand and, as a consequence, may have to wait (that is, queue) for access to a communication link.

#### Multiplexing in Circuit-Switched Networks

> **Frequency-division multiplexing (FDM)** 
>
> The frequency spectrum of a link is divided up among the connections established across the link. Specifically, the link dedicates a frequency band to each connection for the duration of the connection.
>
> ![image-20221006180516515](image-20221006180516515.png)

> **time-division multiplexing (TDM)**
>
> Time is divided into frames of fixed duration, and each frame is
> divided into a fixed number of time slots. When the network establishes a connection across a link, the network dedicates one time slot in every frame to this connection.
>
> ![image-20221006180538853](image-20221006180538853.png)

Proponents of packet switching have always argued that circuit switching is wasteful because the dedicated circuits are idle during silent periods.

ex. *when one person in a telephone call stops talking, the idle network resources (frequency bands or time slots in the links along the connection’s route) cannot be used by other ongoing connections.*

#### Circuit switching example

**Q:**  Let us consider how long it takes to send a file of 640,000 bits from Host A to Host B over a circuit-switched network. Suppose that all links in the network use TDM with 24 slots and have a bit rate of 1.536 Mbps. Also suppose that it takes 500 msec to establish an end-to-end circuit before Host A can begin to transmit the file. How long does it take to send the file?

**A:** Each circuit has a transmission rate of (1.536 Mbps)/24 = 64 kbps, so it takes (640,000 bits)/(64 kbps) = 10 seconds to transmit the file. To this 10 seconds we add the circuit establishment time, giving 10.5 seconds to send the file.

**Note:** *Note that the transmission time is independent of the number of links: The transmission time would be 10 seconds if the end-to-end circuit passed through one link or a hundred links.* 

#### Packet switching vs Circuit switching Pros/Cons

- Packet switching
  - Con
    - Not suitable for real world, real time services because of its variable and unpredictable end-to-end delays.
  - Pros
    - It offers better sharing of transmission capacity than circuit switching
    - it is simpler, more efficient, and less costly to implement than circuit switching

## 1.3 A Network of Networks

#### Internet Service Providers

Over the years, the network of networks that forms the Internet has evolved into a very complex structure.

Hierarchy:

- Tier-1 ISPs (AT&T, sprint, TELUS, Shaw)
- Regional ISPs (explornet, Pris)
- Access ISPs (Local businesses that connect to either regional or Tier 1)

![image-20221006182302406](image-20221006182302406.png)

## 1.3.1 Multiplexing Technologies

#### Frequency division multiplexing (FDM)

Used by fiber to segment the signals into sections to be sent down the same line.

![image-20221006191225459](image-20221006191225459.png)

#### Wavelength division multiplexing

a technology which [multiplexes](https://en.wikipedia.org/wiki/Multiplexing) a number of [optical carrier](https://en.wikipedia.org/wiki/Optical_carrier) signals onto a single [optical fiber](https://en.wikipedia.org/wiki/Optical_fiber) by using different [wavelengths](https://en.wikipedia.org/wiki/Wavelength) (i.e., colors) of [laser](https://en.wikipedia.org/wiki/Laser) [light](https://en.wikipedia.org/wiki/Light).[[1\]](https://en.wikipedia.org/wiki/Wavelength-division_multiplexing#cite_note-:0-1)This technique enables [bidirectional](https://en.wikipedia.org/wiki/Duplex_(telecommunications)) communications over a single strand of fiber, also called **wavelength-division duplexing**, as well as multiplication of capacity.[[1\]](https://en.wikipedia.org/wiki/Wavelength-division_multiplexing#cite_note-:0-1)

![image-20221006191322867](image-20221006191322867.png)
$$
Frequency * Wavelength = Signal(Speed)
$$

#### Time division multiplexing

is a method of transmitting and receiving independent signals over a common signal path by means of synchronized switches at each end of the transmission line so that each signal appears on the line only a fraction of time in an alternating pattern. This method transmits two or more digital signals or analog signals over a common channel.

![image-20221006191824084](image-20221006191824084.png)

#### Communication satellite

![image-20221006192031173](image-20221006192031173.png)

## 1.4 Packet Delay

Types of delay:

- Nodal processing delay 
- Queuing delay 
- Transmission delay
- Propagation delay
- Total Nodal delay (Aggregate of above)

#### Definitions 

> **Processing Delay**
>
> The time required to examine the packet’s header and determine where to direct the packet

> **Queuing Delay**
>
> Time taken to wait to be transmitted onto the link. The length of the queuing delay of a specific packet will depend on the number of earlier-arriving packets that are queued and waiting for transmission onto the link.

> **Transmission Delay**
>
> Assuming that packets are transmitted in a first-come-first-served manner, as is common in packet-switched networks, our packet can be transmitted only after all the packets that have arrived before it have been transmitted.
>
> Note: for a 10 Mbps Ethernet link, the rate is R = 10 Mbps; for a 100 Mbps Ethernet link, the rate is R = 100 Mbps. <u>The transmission delay is L/R (calculated above)</u>

> **Propagation Delay**
>
> Once a bit is pushed into the link, it needs to propagate to router B. The time required to propagate from the beginning of the link to router B is the propagation delay.
>
> Range of Propagation delay: 
> $$
> 2* 10^8 m/s \leftarrow \rightarrow 3*10^8 m/s
> $$
> propagation delay is d/s, where d is the distance between router A and router B and s is the propagation speed of the link.

> **End-to-End Delay**
>
> The nodal delays accumulate and give an end-to-end delay.
> $$
> d(end-end) = N(d[proc] + d[trans] + d[prop])
> $$
> 

#### Traceroute

As these packets work their way toward the destination, they pass through a series of routers. When a router receives one of these special packets, it sends back to the source a short message that contains the name and address of the router.

suppose there are N - 1 routers between the source and the
destination. Then the source will send N special packets into the network, with each packet addressed to the ultimate destination.

![image-20221006184504543](image-20221006184504543.png)

Note: **These round-trip delays include all of the delays just discussed, including transmission delays, propagation delays, router processing delays, and queuing delay.**



## 1.5 Protocol Layers and Layered Architecture 

> **Protocol Stack**
>
> When taken together, the protocols of the various layers are called the protocol stack. The Internet protocol stack consists of five layers: the physical, link, network, transport, and application layers

| Priority | Name        |
| -------- | ----------- |
| 1        | Application |
| 2        | Transport   |
| 3        | Network     |
| 4        | Link        |
| 5        | Physical    |

#### Application Layer

The application layer is where network applications and their application-layer protocols reside. Includes:

- HTTP
- FTP
- SMTP

We’ll refer to this packet of information at the application layer as a message.

#### Transport Layer

The Internet’s transport layer transports application-layer messages between application endpoints. In the Internet, there are two transport protocols, TCP and UDP, either of which can transport application-layer messages.

##### **TCP (Transmission control Protocol)**

- provides a connection-oriented service to its applications. This service includes guaranteed delivery of application-layer messages to the destination and flow control (that is, sender/receiver speed matching).
- Breaks long messages into shorter segments and provides a congestion-control mechanism, so that a source throttles its transmission rate when the network is congested.

##### **UDP (User Diagram Protocol)**

- Provides a connectionless service to its applications
- This is a no-frills service that provides no reliability, no flow control, and no congestion control.

#### Network Layer

The Internet’s network layer is responsible for moving network-layer packets known as datagrams from one host to another. The Internet’s network layer includes the celebrated IP protocol, which defines
the fields in the datagram as well as how the end systems and routers act on these fields. 

This may include:

- IP addresses
- mac addresses 

> **Datagram**
>
> A datagram is an independent, self-contained message sent over the network whose arrival, arrival time, and content are not guaranteed.

#### Link Layer

The Internet’s network layer routes a datagram through a series of routers between the source and destination. To move a packet from one node (host or router) to the next node in the route, the network layer relies on the services of the link layer.

The services provided by the link layer depend on the specific link-layer protocol that is employed over the link.

This may Include:

- Ethernet
- WIFI
- Cable access networks

#### Physical Layer

the job of the physical layer is to move the individual bits within the frame from one node to the next. The protocols in this layer are again link dependent and further depend on the actual transmission medium of the link (for example, twisted-pair copper wire, single-mode fiber optics).

This may include:

- Fiber 
- copper wire
- coax / BNC

#### Encapsulation 

![image-20221006194711068](image-20221006194711068.png)

# Chapter 2 - Application Layer

## The Interface Between the Process and the Computer Network

> **Socket** 
>
> A process sends messages into, and receives messages from, the network through a software interface called a socket

![image-20221006213124652](image-20221006213124652.png)



## HTTP

#### Request methods

![image-20221006213257282](image-20221006213257282.png)

#### Response Codes

![image-20221006213320734](image-20221006213320734.png)

#### HTTP Request message level views

![image-20221006213426383](image-20221006213426383.png)

## Transport Services Available to Applications

Many networks, including the Internet, provide more than one transport-layer protocol. We can broadly classify the possible services along four dimensions: reliable data transfer, throughput, timing, and security.

#### Reliable Data Transfer

If a protocol provides such a guaranteed data delivery service, it is said to provide reliable data transfer.

When a transport-layer protocol doesn’t provide reliable data transfer, some of the data sent by the sending process may never arrive at the receiving process. This may be acceptable for loss-tolerant applications, most notably multimedia applications such as conversational audio/video that can tolerate some amount of data loss.

#### Throughput

> **Throughput**
>
> Is the rate at which the sending process can deliver bits to the receiving process.

For example, if an Internet telephony application encodes voice at 32 kbps, it needs to send data into the network and have data delivered to the receiving application at this rate. If the transport protocol cannot provide this throughput, the application would need to encode at a lower rate (and receive enough throughput to sustain this lower coding rate) or may have to give up, since receiving, say, half of the needed throughput is of little or no use to this Internet telephony application.

Applications that have throughput requirements are said to be **bandwidth-sensitive applications**. Many current multimedia applications are bandwidth sensitive

#### Timing

A transport-layer protocol can also provide timing guarantees. For non-real-time applications, lower delay is always preferable to higher delay, but no tight constraint is placed on the end-to-end delays.

## TCP services 

#### Connection-oriented service

- TCP has the client and server exchange transport layer control information with each other before the application-level messages begin to flow.
- The connection is a full-duplex connection in that the two processes can send messages to each other over the connection at the same time.
- When the application finishes sending messages, it must tear down the connection.

#### Reliable data transfer service

- The communicating processes can rely on TCP to deliver all data sent without error and in the proper order.
- No missing or duplicate bytes.
- includes a congestion-control mechanism - throttles a sending process if traffic congested

## UDP Services

- UDP is a no-frills, lightweight transport protocol, providing minimal services. UDP is connectionless, so there is no handshaking before the two processes start to communicate. UDP provides an unreliable data transfer service—that is, when a process sends a message into a UDP socket, UDP provides no guarantee that the message will ever reach the receiving process. Furthermore, messages that do arrive at the receiving process may arrive out of order.

- not include a congestion-control mechanism, so the sending side of
  UDP can pump data into the layer below (the network layer) at any rate it pleases.

## 2.1.5 Application-Layer Protocols

An application-layer protocol defines how an application’s processes, running on different end systems, pass messages to each other. In particular, an application-layer protocol defines:

- Types of messages 
- The syntax of the message type
- The Semantics 
- Rules for processing messages

## 2.2 The Web and HTTP

![image-20221006215558651](image-20221006215558651.png)

## 2.2.2 Non-Persistent and Persistent Connections

> **Persistent vs non-persistent** 
>
> Should each request/response pair be sent over a separate TCP connection, or should all of the requests and their corresponding responses be sent over the same TCP connection? In the former approach, the application is said to use **non-persistent connections**; and in the latter approach, **persistent connections.**

#### HTTP with Non-Persistent Connections

1. he HTTP client process initiates a TCP connection to the server
2. The HTTP client sends an HTTP request message to the server via its socket.
3. Server gets file, encapsulates the object in an HTTP response message, and sends the response message to the client via its socket.
4. The HTTP server process tells TCP to close the TCP connection. 
5. The HTTP client receives the response message.
6. The first four steps are then repeated for each of the referenced JPEG objects.

> 1. open connection
> 2. client sends request
> 3. server sends file back to client
> 4. server closes connection
> 5. client receives message
> 6. repeat

![image-20221006220158495](image-20221006220158495.png)

**One object per TCP connection**:

- default behavior in HTTP/1.0
- network cost: ~2*RTT per object
- end-host cost: 1 socket() for each object

> ![image-20221006221558807](image-20221006221558807.png)
>
> TCP establishes a new connection before data gets transmitted

##### HTTP with non-persistant concurrent connections

> ![image-20221006221843888](image-20221006221843888.png)
>
> TCP does the same but also parallels requests 

#### HTTP with Persistent Connections

Non-persistent connections have some shortcomings:

- a brand-new connection must be established and maintained for each requested object.
- TCP buffers must be allocated and TCP variables must be kept in both the client and server.
- each object suffers a delivery delay of two RTTs—one RTT to establish the TCP connection and one RTT to request and receive an object.

Persistent Pros:

- These requests for objects can be made back-to-back, without waiting for replies to pending requests (pipelining)
- server closes a connection when it isn’t used for a certain time (timeout)

> ![image-20221006222012254](image-20221006222012254.png)

> ![image-20221006222039387](image-20221006222039387.png)
>
> Persistent with pipelining
>
> - one tcp request 
> - open connection with parallel 

> ![image-20221006222238916](image-20221006222238916.png)

## 2.2.3 HTTP Message Format

HTTP Request Message

```html
GET /somedir/page.html HTTP/1.1 
Host: www.someschool.edu 
Connection: close 
User-agent: Mozilla/5.0 
Accept-language: fr
```

HTTP Response Message

```html
HTTP/1.1 200 OK 
Connection: close 
Date: Tue, 18 Aug 2015 15:44:04 GMT 
Server: Apache/2.2.3 (CentOS) 
Last-Modified: Tue, 18 Aug 2015 15:11:03 GMT 
Content-Length: 6821 
Content-Type: text/html 

(data data data data data ...)
```

the first line of a HTTP request message is called the **request line**; the subsequent lines are called the **header lines**.

The method field can take on several different values, including:

- GET
- POST
- HEAD
- PUT
- DELETE

![image-20221006220756072](image-20221006220756072.png)

## 2.2.4 Cookies

- HTTP itself is stateless.

- Many applications require states

![image-20221006221325207](image-20221006221325207.png)

## 2.2.5 Web caching

> **Web caching // Proxy Server**
>
> A network entity that satisfies HTTP requests on the behalf of an origin Web server.

#### Process of web caching

1. The browser establishes a TCP connection to the Web cache and sends an HTTP request for the object to the Web cache.
2. The Web cache checks to see if it has a copy of the object stored locally. If it does, the Web cache returns the object within an HTTP response message to the client browser.
3. If the Web cache does not have the object, the Web cache opens a TCP connection to the origin server, that is, to www.someschool.edu. The Web cache.
4. When the Web cache receives the object, it stores a copy in its local storage and sends a copy, within an HTTP response message, to the client browser (over the existing TCP connection between the client browser and the Web cache).

![image-20221011192459630](image-20221011192459630.png)

***Note:** a cache is both a server and a client at the same time.*

Typically a Web cache is purchased and installed by an ISP. For example, a university might install a cache on its campus network and configure all of the campus browsers to point to the cache.

#### Pros of Web caching

- Web cache can substantially reduce the response time for a client request, particularly if the bottleneck bandwidth between the client and the origin server is much less than the bottleneck bandwidth between the client and the cache.
- Web caches can substantially reduce traffic on an institution’s access link to the Internet. By reducing traffic, the institution (for example, a company or a university) does not have to upgrade bandwidth as quickly, thereby reducing costs. 
- Web caches can substantially reduce Web traffic in the Internet as a whole, thereby improving performance for all applications.

#### Calculating the bottleneck of delay 

![image-20221011192824637](image-20221011192824637.png)

##### Calculating: The traffic intensity on the LAN

$$
(15 requests/sec) * (1 Mbits/request)/(100 Mbps) = 0.15
$$

##### Calculating: Traffic intensity on the access link

$$
(15 requests/sec) * (1 Mbits/request)/(15 Mbps) = 1
$$

The 0.15 delays negligible but the 1 delay is growing without bound and very bad.

##### Conditional GET

HTTP has a mechanism that allows a cache to verify that its objects are up to date. This mechanism is called the conditional GET [RFC 7232]. An HTTP request message is a so-called conditional GET message if (1) the request message uses the GET method and (2) the request message includes an If-Modified-Since: header line.

**Normal client-server response:**

```html
GET /fruit/kiwi.gif HTTP/1.1 
Host: www.exotiquecuisine.com
```

```html
HTTP/1.1 200 OK 
Date: Sat, 3 Oct 2015 15:39:29 
Server: Apache/1.3.0 (Unix) 
Last-Modified: Wed, 9 Sep 2015 09:23:24 
Content-Type: image/gif
(data data data data data ...)
```

**Using the conditional GET request:**

```html
GET /fruit/kiwi.gif 
HTTP/1.1 Host: www.exotiquecuisine.com 
If-modified-since: Wed, 9 Sep 2015 09:23:24
```

```html
HTTP/1.1 304 Not Modified 
Date: Sat, 10 Oct 2015 15:39:29 
Server: Apache/1.3.0 (Unix)
(empty entity body)
```

## 2.4 DNS - The Internet’s Directory Service

#### Additional services provided by DNS

> **Host Aliasing** 
>
> A host with a complicated hostname can have one or more alias names. For example, a hostname such as relay1.west-coast .enterprise.com could have, say, two aliases such as enterprise.com and www.enterprise.com. In this case, the hostname relay1 .west-coast.enterprise.com is said to be a canonical hostname.

> **Mail server aliasing**
>
> ex. bob@relay1.west-coast.yahoo.com -> bob@yahoo.com

> **Load distribution**
>
> DNS is also used to perform load distribution among replicated servers, such as replicated Web servers. Busy sites, such as cnn.com, are replicated over multiple servers, with each server running on a different end system and each having a different IP address.

![image-20221011194905385](image-20221011194905385.png)

> **Root DNS Servers**
>
> more than 1000 root servers instances scattered all over the world, as shown in Figure 2.18. These root servers are copies of 13 different root servers, managed by 12 different organizations, and coordinated through the Internet Assigned Numbers Authority [IANA 2020]

> **Top-Level domain (TLD) Servers**
>
> For each of the top-level domains—top-level domains such as com, org, net, edu, and gov, and all of the country top-level domains such as uk, fr, ca, and jp—there is TLD server (or server cluster).

> **Authoritative DNS servers**
>
> Every organization with publicly accessible hosts (such as Web servers and mail servers) on the Internet must provide publicly accessible DNS records that map the names of those hosts to IP addresses. An organization’s authoritative DNS server houses these DNS records. An

#### Resource Records

(Name, Value, Type, TTL)

> **TTL - Time to live**
>
> it determines when a resource should be removed from a cache.

| Type  | Definition                                                   |
| ----- | ------------------------------------------------------------ |
| A     | Type A record provides the standard hostname-to-IP address mapping. |
| NS    | Name server hostname of an authoritative DNS server that knows how to obtain the IP addresses for hosts in the domain. |
| CNAME | A canonical hostname for the alias hostname Name.            |
| MX    | The canonical name of a mail server that has an alias hostname Name. |

#### DNS Messages 

![image-20221011202324458](image-20221011202324458.png)

- First 12 bytes is the *Header* section. The first field is a 16-bit number that identifies the query.
- The question section contains information about the query that is being made.
- The answer section contains the resource records for the name that was originally queried. 
- The authority section contains records of other authoritative servers.
- The additional section contains other helpful records.

How would you like to send a DNS query message directly from the host you’re working on to some DNS server? This can easily be done with the "nslookup" program

## 2.7 Socket Programming: Creating Network Applications

#### UDP Server-Client Example (Python)



##### UDP Client

```python
from socket import *

# Server name and port.
serverName = ’hostname’
serverPort = 12000 

# Open new client socket.
clientSocket = socket(AF_INET, SOCK_DGRAM) 
# AF_INET = Uses IPv4
# SOCK_DGRAM = Uses UDP

# Input message to send to server.
message = input(’Input lowercase sentence:’) clientSocket.sendto(message.encode(),(serverName, serverPort)) 

# Recieve the incomming message and print out.
modifiedMessage, serverAddress = clientSocket.recvfrom(2048) print(modifiedMessage.decode()) 

clientSocket.close()
```

##### UDP Server

```python
from socket import * 

serverPort = 12000 
serverSocket = socket(AF_INET, SOCK_DGRAM) 

serverSocket.bind(("", serverPort)) 
print("The server is ready to receive") 

while True: 
    message, clientAddress = serverSocket.recvfrom(2048)
    modifiedMessage = message.decode().upper()
    serverSocket.sendto(modifiedMessage.encode(),clientAddress)
```

#### TCP Server-Client Example (Python)



##### TCP Client 

```python
from socket import *

serverName = ’servername’ 
serverPort = 12000 

clientSocket = socket(AF_INET, SOCK_STREAM) 
clientSocket.connect((serverName,serverPort)) 
sentence = input("Input lowercase sentence:") 

clientSocket.send(sentence.encode()) modifiedSentence = clientSocket.recv(1024) 
print("From Server: ", modifiedSentence.decode()) 

clientSocket.close()
```

##### TCP Server

```python
from socket import * 

serverPort = 12000 
serverSocket = socket(AF_INET,SOCK_STREAM) 

serverSocket.bind(("",serverPort)) 
serverSocket.listen(1) 
print("The server is ready to receive")

while True:
    connectionSocket, addr = serverSocket.accept() sentence =
    connectionSocket.recv(1024).decode() capitalizedSentence =
    sentence.upper()
    connectionSocket.send(capitalizedSentence.encode())
    connectionSocket.close()
```

# Chapter 3 - Transport Layer

## 3.1 Relationship between Transport and Network layers

Recall that the Internet makes two distinct transport-layer protocols available to the application layer. One of these protocols is UDP (User Datagram Protocol), which provides an unreliable, connectionless service to the invoking application. The second of these protocols is TCP (Transmission Control Protocol), which provides a reliable, connection-oriented service to the invoking application.

> **Segment** 
>
> Transport layer packet. 
>
> Note:
>
> -  UDP is a datagram
> - TCP is a segment.



#### Transport-layer protocol Elements

![image-20221103143056642](assets/image-20221103143056642.png)

**Transport-layer protocol elements** 

- Services provided to application layer 
  - To support HTTP, DNS, etc.
- **Services provided by network layer**
  - ex. by IP.
- **Transport-layer protocol mechanisms**
  - ex. how to fill the gap

| Level       | Type of service Layer |
| ----------- | --------------------- |
| 5 (Highest) | Application           |
| 4           | Transport             |
| 3           | Network               |
| 2           | Data Link             |
| 1 (Lowest)  | Physical              |

#### Transport Layer Services

![image-20221103143127572](assets/image-20221103143127572.png)

**Services Provided by the transport layer**

- Endpoint to endpoint communication
  - endpoint: an application process in end-hosts
- connection-oriented vs connectionless
- data transfer: reliable vs unreliable

**Example: Internet transport-layer services**

- connection-oriented, reliable by TCP 
- connectionless, unreliable by UDP

#### Network layer services

![image-20221103143635756](assets/image-20221103143635756.png)

**Services provided by network layer**

- Move packets from one end-host to another
- Possibly through many intermediate systems (Ex. routers)

**Example: Internet network-layer services**

- IP: store-and-forward packet switching
- packets may get:
  - lost at communication link, router or receiver buffer
  - duplicated
  - corrupted
  - reordered



## 3.2 Multiplexing and Demultiplexing 

> **Sockets**
>
> doors through which data passes from the network to the process and through which data passes from the process to the network.

> **Demultiplexing** 
>
> Delivering the data in a transport-layer segment to the correct socket.

> **Multiplexing** 
>
> gathering data chunks at the source host from different sockets, encapsulating each data chunk with header information (that will later be used in demultiplexing) to create segments, and passing the segments to the network layer

![image-20221020144808247](image-20221020144808247.png)

**Multiplexing requires:**

- That sockets have unique identifiers.
- Each segment have special fields that indicate the socket to which the segment is to be delivered.

![image-20221020145040333](image-20221020145040333.png)

#### Connectionless Multiplexing and Demultiplexing 

```python
clientSocket = socket(AF_INET, SOCK_DGRAM)
```

This auto assigns an unused port unless specified by the .bind() command.

#### Connection-Oriented Multiplexing and Demultiplexing

**TCP uses all 4 values of the tuple to deliver data:**

- source IP address
- source port number
- destination IP address
- destination port number

**Process of TCP server creation:**

- Creates a "Welcome socket" that waits for clients to connect.
- creates the socket via:

```python
clientSocket = socket(AF_INET, SOCK_STREAM) clientSocket.connect((serverName,12000))
```

- Accepts the client server connection via:

```python
connectionSocket, addr = serverSocket.accept()
```

![image-20221020150439112](image-20221020150439112.png)

![image-20221103143736599](assets/image-20221103143736599.png)

#### What's under a socket?

![image-20221103143831371](assets/image-20221103143831371.png)

#### Socket container (Encapsulation)

- Frame Header (Physical Layer)
- Frame Payload:
  - Packet Header (Network Layer)
  - Packet Payload:
    - TPDU header (Transport Layer)
    - Payload:
      - DATA

- Frame Trailer



## 3.3 Connectionless Transport: UDP

![image-20221103144330889](assets/image-20221103144330889.png)

#### Why UDP over TCP?

![image-20221103144401430](assets/image-20221103144401430.png)

- Finer application-level control over what data is sent, and when.
- No connection establishment.
- No connection state.
- Small packet header overhead.
  - TCP overhead = 20 bytes
  - UDP overhead = 8 bytes

#### UDP Segment Structure

The UDP header has only **four fields**, each consisting of **two bytes** (8 bytes total).

![image-20221020151510531](image-20221020151510531.png)

![image-20221103144418768](assets/image-20221103144418768.png)

#### UDP Checksum

UDP at the sender side performs the 1s complement of the sum of all the 16-bit words in the segment, with any overflow encountered during the sum being wrapped around. This result is put in the checksum field of the UDP segment.

![image-20221020151908547](image-20221020151908547.png)

Note that this last addition had overflow, which was wrapped around. The 1s
complement is obtained by converting all the 0s to 1s and converting all the 1s to 0s. Thus, the 1s complement of the sum 0100101011000010 is 1011010100111101, which becomes the checksum. 

At the receiver, all four 16-bit words are added, including the checksum. If no errors are introduced into the packet, then clearly the sum at the receiver will be 1111111111111111. 

If one of the bits is a 0, then we know that errors have been introduced into the packet.

> ***Note:*** Some implementations of UDP simply discard the damaged segment; others pass the damaged segment to the application with a warning.

![image-20221103144446190](assets/image-20221103144446190.png)

## 3.4 Principles of Reliable Data Transfer

#### What could go wrong?

- Lost:
  - Transmission error or network congestion
- Duplicated:
  - Deleted by referring to sequence number; **done**
- corrupted:
  - Arrived but in “bad shape”
- reordered:
  - Rearranged by referring to sequence number; **done**

#### Error Detection

- Corrupted packets
  - Detected by TCP checksum
    - Action: drop!
- Lost packets
  - How do you tell if something is already lost?
  - TCP sender
    - Timer for acknowledgment
  - TCP receiver (cumulative acknowledgment)
    - duplicate acknowledgment

#### Error and Delay solutions

##### Sender Timer

- start a timer when sending out a packet
  - In reality: one timer per a window of packets
- On acknowledgment “covering” this packet • cancel the timer and setup another one.
- If timer timeouts: indicate packet may be lost

##### Timeout Value

- Too soon: unnecessary transmission
- Too late: “slow response”



> **ARQ (Automatic Repeat reQuest) protocols.**
>
> If a TCP packet received in error, ARQ will ask for a re transmit.

Three additional protocol capabilities are required in ARQ protocols to handle the presence of bit errors:

- Error detection
- Receiver feedback
- Retransmission

**ACK packets** - correct data transmitted

**NAK packets** - the protocol retransmits the last packet and waits for an ACK or NAK to be returned by the receiver in response to the retransmitted data packet

the sender will not send the next bit of data until the ACK or NAK has been received.

#### Nagle's Algorithm (Sender approach)

> **Problem**
>
> Data gets written byte-by-byte which leads to huge traffic of:
>
> - Data packets
> - ACK packets
>
> **Solution: The algorithm**
>
> Goal: Send either big packets or lower packet overhead.
>
> *Cons of the algorithm:*
>
> - Mouse movement, delays updates
> - Delayed acknowledgements

#### David Clark Algorithm (Receiver approach)

> **Problem**
>
> - silly window syndrome: application keeps reading data byte-by-byte
> - receiver keeps advertising small window (thus small packets)
>
> **Solution: The Algorithm**
>
> Goal: Try and advertise bigger windows.
>
> receiver only advertises:
>
> - at least one MSS, or
> - half window size
>
> *Cons of the algorithm:* 
>
> - Extra Delay

#### Consider three possibilities for handling corrupted ACKs or NAKs:

- If both are corrupt then you get a loop of retransmitting
- A second alternative is to add enough checksum bits to allow the sender not only to detect, but also to recover from, bit errors. 
- A third approach is for the sender simply to resend the current data packet when it receives a garbled ACK or NAK packet.
  - This can create duplicate packets

#### Duplicate packets

> **sequence number**
>
> Singular bit to tell whether the data packet is a retransmission or not. Either a 1 or a 0

#### Duplicate ACKS

After 3 consecutive duplicate ack numbers, the packet is considered LOST.

#### Reliable Data Transfer over a Lossy Channel with Bit Errors

If the sender is willing to wait long enough so that it is certain that a packet has been lost, it can simply retransmit the data packet.

This might also produce duplicate packets but the sequence number takes care of that normally.

![image-20221020154048769](image-20221020154048769.png)

![image-20221020154150691](image-20221020154150691.png)

#### Stop-and-wait vs pipelined protocol

ex. The speed-of-light round-trip propagation delay between these two end systems, RTT, is approximately 30 milliseconds. Suppose that they are connected by a channel with a transmission rate, R, of 1 Gbps (109 bits per second). With a packet size, L, of 1,000 bytes (8,000 bits) per packet, including both header fields and data, the time needed to actually transmit the packet into the 1 Gbps link is
$$
d(trans) = \frac{L}{R} = \frac{800 bits}{10^9 bit/sec} = 8  microseconds
$$
**Utilization**

With our stop-and-wait protocol, if the sender begins sending the packet at t = 0, then at t = L/R = 8 microseconds, the last bit enters the channel at the sender side. The packet then makes its 15-msec cross-country journey, with the last bit of the packet emerging at the receiver at t = RTT/2 + L/R = 15.008 msec. Assuming for simplicity that ACK packets are extremely small (so that we can ignore their transmission time) and that the receiver can send an ACK as soon as the last bit of a data packet is received, the ACK emerges back at the sender at t = RTT + L/R = 30.008 msec. At this point, the sender can now transmit the next message. Thus, in 30.008 msec, the sender was sending for only 0.008 msec. If we define the utilization of the sender (or the channel) as the fraction of time the sender is actually busy sending bits into the channel, the analysis in Figure 3.18(a) shows that the stop-and-wait protocol has a rather dismal sender utilization
$$
Usender = \frac{L/R}{RTT + L/R} = \frac{.008}{30.008} = 0.00027
$$
![image-20221020154932004](image-20221020154932004.png)

##### Pipelining 

> **Pipelining**
>
> if the sender is allowed to transmit three packets before having to wait for acknowledgments, the utilization of the sender is essentially tripled. Since the many in-transit sender-to-receiver packets can be visualized as filling a pipeline.

**Pipelining has the following consequences for reliable data transfer protocols:**

- The range of sequence numbers must be increased, since each in-transit packet (not counting retransmissions) must have a unique sequence number and there may be multiple, in-transit, unacknowledged packets.
- The sender and receiver sides of the protocols may have to buffer more than one packet.
-  The range of sequence numbers needed and the buffering requirements will depend on the manner in which a data transfer protocol responds to lost, corrupted, and overly delayed packets.

Two basic approaches toward pipelined error recovery can be identified: **Go-Back-N** and **selective repeat**.

###### Go-Back-N (GBN)

![image-20221020155643058](image-20221020155643058.png)

the sender is allowed to transmit multiple packets (when available) without waiting for an acknowledgment, but is constrained to have no more than some maximum allowable number, N, of unacknowledged packets in the pipeline.

> ***Note:*** N is often referred to as the **window size** and the GBN protocol itself as a **sliding-window protocol**.

**The GBN sender must respond to three types of events:**

- Invocation from above.
- Receipt of an ACK.
- A timeout event.

An acknowledgment for a packet with sequence number n will be taken to be a **cumulative acknowledgment**, indicating that all packets with a sequence number up to and including n have been correctly received at the receiver.

In our GBN protocol, the receiver discards out-of-order packets.

![image-20221020160039365](image-20221020160039365.png)

###### Selective Repeat (SR)

**Why GBN might not work:**

- when the window size and bandwidth-delay product are both large, many packets can be in the pipeline. A single packet error can thus cause GBN to retransmit a large number of packets, many unnecessarily. As the probability of channel errors increases, the pipeline can become filled with these unnecessary retransmissions. 

![image-20221020160750119](image-20221020160750119.png)

## 3.5 Connection-Oriented Transport: TCP

TCP is said to be **connection-oriented** because before one application process can begin to send data to another, the two processes must first “handshake” with each other—that is, they must send some preliminary segments to each other to establish the parameters of the ensuing data transfer.

A TCP connection provides a **full-duplex service**: If there is a TCP connection between Process A on one host and Process B on another host, then application-layer data can flow from Process A to Process B at the same time as application-layer data flows from Process B to Process A.

> **TCP three-way handshake** 
>
> The first two segments carry no payload, that is, no application-layer data; the third of these segments may carry a payload.

The maximum amount of data that can be grabbed and placed in a segment is limited by the **maximum segment size (MSS)**. The MSS is typically set by first determining the length of the largest link-layer frame that can be sent by the local sending host

> **MSS** 
>
> Maximum segment size (a typical value of MSS is 1460 bytes)
>
> ***Note:*** that the MSS is the maximum amount of application-layer data in the segment, not the maximum size of the TCP segment including headers.

Both Ethernet and PPP link-layer protocols have an MTU of 1,500 bytes.

> **MTU**
>
> Maximum transmission unit

![image-20221020161755945](image-20221020161755945.png)

##### TCP Connection ID

**TCP Connections:**

- Connection -> connect()
- Initiator -> listen()
- Responder -> accept()

**Connection flow:**

- send()
- recv()

#### TCP Segment Structure

When TCP sends a large file, such as an image as part of a Web page, it typically breaks the file into chunks of size MSS (except for the last chunk, which will often be less than the MSS)

![image-20221020162035212](image-20221020162035212.png)

**TCP packet/segment contains the following headers:**

- **Source / Destination Port** - well-known port numbers (0~1023, privileged) (ex. 80: http; 443: https)

- **Sequence number field (32-bit)** - used by the TCP sender and receiver in implementing a reliable data transfer service.
- **Acknowledgment number field (32-bit)** - used by the TCP sender and receiver in implementing a reliable data transfer service.
- **Receive window (16-bit)** - used for flow control. It is used to indicate the number of bytes that a receiver is willing to accept.
- **Header length field (4-bit)** - specifies the length of the TCP header in 32-bit words. The TCP header can be of variable length due to the TCP options field. (Typically, the options field is empty, so that the length of the typical TCP header is 20 bytes.)
- **Options field** - used when a sender and receiver negotiate the maximum segment size (MSS) or as a window scaling factor for use in high-speed networks.
- **Flag Field** **(6-bit)**
  - **ACK bit** - used to indicate that the value carried in the acknowledgment field is valid.
  - **RST bit** - used for connection setup and teardown (Rests connection)
  - **SYN** - used for connection setup and teardown
  - **FIN** - used for connection setup and teardown
  - **PSH** - indicates that the receiver should pass the data to the upper layer immediately.
  - **URG** - used to indicate that there is data in this segment that the sending-side upper layer entity has marked as “urgent.”
- **Urgent data pointer field** **(16-bit)** - TCP must inform the receiving-side upper-layer entity when urgent data exists and pass it a pointer to the end of the urgent data.

##### URG Pointers (Flag Fields)

![image-20221103145722973](assets/image-20221103145722973.png)

##### Sequence Numbers and Acknowledgment Numbers

###### Sequence Numbers

![image-20221020163034966](image-20221020163034966.png)

Suppose that the data stream consists of a file consisting of 500,000 bytes, that the MSS is 1,000 bytes, and that the first byte of the data stream is numbered 0. As shown in Figure 3.30, TCP constructs 500 segments out of the data stream. The first segment gets assigned sequence number 0, the second segment gets assigned sequence number 1,000, the third segment gets assigned sequence number 2,000, and so on. Each sequence number is inserted in the sequence number field in the header of the appropriate TCP segment.

###### Acknowledgement numbers

The acknowledgment number that Host A puts in its segment is the sequence number of the next byte Host A is expecting from Host B.

Because TCP only acknowledges bytes up to the first missing byte in the stream, TCP is said to provide **cumulative acknowledgments.**

![image-20221020163524264](image-20221020163524264.png)

#### Round-Trip Time Estimation and Timeout

The timeout should be larger than the connection’s round-trip time (RTT), that is, the time from when a segment is sent until it is acknowledged. Otherwise, unnecessary retransmissions would be sent. But how much larger?

##### Estimating the Round-Trip Time

TCP updates the RTT estimate via the following formula:
$$
EstimatedRTT = (1-\alpha) * EstimatedRTT + \alpha* sampleRTT
$$

>  ***Note:*** The recommended value of α is α = 0.125 (that is, 1/8) , so:


$$
EstimatedRTT = 0.875*EstimatedRTT+ 0.125*SampleRTT
$$

> ***Note:*** Note that EstimatedRTT is a weighted average of the SampleRTT values. As

RTT Variation or variability:
$$
DevRTT = (1-\beta) * DevRTT+\beta * |SampleRTT - EstimatedRTT|
$$

> ***Note:*** that DevRTT is an EWMA of the difference between SampleRTT and EstimatedRTT. If the SampleRTT values have little fluctuation, then DevRTT will be small; on the other hand, if there is a lot of fluctuation, DevRTT will be large. The recommended value of β is 0.25.

$$
TimeoutInterval = EstimatedRTT + 4* DevRTT
$$

##### Fast Retransmit

> **Fast retransmit** 
>
> retransmitting the missing segment before that segment’s timer expires.

![image-20221020171130020](image-20221020171130020.png)

##### Is TCP Selective or a GBN Protocol?

the so-called selective acknowledgment [RFC 2018], allows a TCP receiver to acknowledge out-of-order segments selectively rather than just cumulatively acknowledging the last correctly received, in-order segment. When combined with selective retransmission—skipping the retransmission of segments that have already been selectively acknowledged by the receiver— TCP looks a lot like our generic SR protocol. Thus, TCP’s error-recovery mechanism is probably best categorized as a hybrid of GBN and SR protocols.

#### Flow Control

TCP provides a flow-control service to its applications to eliminate the possibility of the sender overflowing the receiver’s buffer.

Flow control is thus a speed matching service—matching the rate at which the sender is sending against the rate at which the receiving application is reading.

##### TCP Receive window 

the receive window is used to give the sender an idea of how much free buffer space is available at the receiver. Because TCP is full-duplex, the sender at each side of the connection maintains a distinct receive window.

> LastByteRead
>
> the number of the last byte in the data stream read from the buffer by the application process in B.

> LastByteRcvd
>
> the number of the last byte in the data stream that has arrived from the network and has been placed in the receive buffer at B

Because TCP is not permitted to overflow the allocated buffer, we must have:

```bash
LastByteRcvd – LastByteRead <= RcvBuffer
```

The receive window, denoted rwnd is set to the amount of spare room in the buffer:

```bash
rwnd = RcvBuffer – [LastByteRcvd – LastByteRead]
```

![image-20221020172108816](image-20221020172108816.png)

###### Window space

- maximum window size 216-1: ~64K bytes!
- Achievable throughput limit: ~ win/rtt
- How to keep the “pipe” full?

**TCP over "Long-fat" networks**

- Long: large round-trip time

- Fat: high bandwidth

- Low utilization due to window limit

  

  ![image-20221103145536489](assets/image-20221103145536489.png)

#### TCP connection Management

![image-20221020172333938](image-20221020172333938.png)

![image-20221020172358599](image-20221020172358599.png)

This special segment has a flag bit in the segment’s header, the FIN bit (see Figure 3.29), set to 1. When the server receives this segment, it sends the client an acknowledgment segment in return. The server then sends its own shutdown segment, which has the FIN bit set to 1. Finally, the client acknowledges the server’s shutdown segment.



## 3.6 Principals of congestion control

#### Load-Gain Curve

> **Gain**
>
> Throughput / Delay

- Load gain curve
  - Low load (I)
  - Medium Load (II)
  - High Load (III)
- Congestion
  - Low Throughput (I)
  - High Delay (II)
  - Very Low gain (III)

![image-20221103153511573](assets/image-20221103153511573.png)

##### The 3 Stages (I, II and III):

1. If the load is small, the throughput keeps up with the increasing load. After the load reaches the network capacity, at the “knee” of the curve, the throughput stops increasing.
2.  Beyond the knee, as the load increases, <u>queues start to build up in the network,</u> thus increasing the end-to-end latency without adding to the throughput. The network is then said to be in a state of congestion.
3. If the traffic load increases even further and gets to the “cliff” part of the curve, then the throughput experiences a rather drastic reduction. This is because <u>queues within the network have increased to the point that packets are being dropped.</u> If the sources choose to retransmit to recover from losses, then this adds further to the total load, resulting in a positive feedback loop that sends the network into congestion collapse.

#### The Causes and the Costs of Congestion

![image-20221021093559185](image-20221021093559185.png)

Makes it so that we can be in a constant state.

##### Scenario 1: Two Senders, a Router with Infinite Buffers

![image-20221020172738281](image-20221020172738281.png)

##### Scenario 2: Two Senders and a Router with Finite Buffers

![image-20221020172820689](image-20221020172820689.png)

##### Scenario 3: Four Senders, Routers with Finite Buffers, and Multihop Paths

![image-20221020172908483](image-20221020172908483.png)

#### Approaches to Congestion Control

two broad approaches to congestion control that are taken in practice and discuss specific network architectures and congestion-control protocols embodying these approaches:

- **End-to-end congestion control** - TCP segment loss (as indicated by a timeout or the receipt of three duplicate acknowledgments) is taken as an indication of network congestion, and TCP decreases its window size accordingly.
- **Network-assisted congestion control** - With network-assisted congestion control, routers provide explicit feedback to the sender and/or receiver regarding the congestion state of the network. This feedback may be as simple as a single bit indicating congestion at a link

![image-20221020173117534](image-20221020173117534.png)

## 3.7 TCP Congestion Control

Because TCP uses acknowledgments to trigger (or clock) its increase in congestion window size, TCP is said to be **self-clocking.**

- A lost segment implies congestion, and hence, the TCP sender’s rate should be decreased when a segment is lost.
- An acknowledged segment indicates that the network is delivering the sender’s segments to the receiver, and hence, the sender’s rate can be increased when an ACK arrives for a previously unacknowledged segment.
- Bandwidth probing.

#### Why congestion control?

- Flow control
  - coordinate sender and receiver (buffer)
- Network Congestion
  - coordination between the sender and network - avoid a sender to overflow a router
  - coordination among many senders -  traffic aggregation from many senders
  - Congestion syndrome - increasing queuing delay, packet drop

#### Load-Gain Curve

![image-20221021093357788](image-20221021093357788.png)

> **Knee**
>
> Area between I and II 
>
> Note: It is not a defined place and trial and error is used 

> **Cliff**
>
> Area between II and III
>
> Note: It is not a defined place and trial and error is used 



![image-20221021093712642](image-20221021093712642.png)

- TCP sender control only because TCP has already been implemented for several years prior.

#### Slow start (In the first section of the curve)

Slow start is adaptive and is not initialized implicitly.

> **ssthresh**
>
> Slow start threshold

![image-20221020173342993](image-20221020173342993.png)

If there is a loss event (i.e., congestion) indicated by a timeout, the TCP sender sets the value of cwnd to 1 and begins the slow start process anew. 

![image-20221021093926552](image-20221021093926552.png)

##### Network Congestion

![image-20221021094609024](image-20221021094609024.png)

#### Congestion Avoidance (In the middle section of the curve)

![image-20221021093953370](image-20221021093953370.png)

> when should congestion avoidance’s linear increase (of 1 MSS per RTT)
> end?
>
> TCP’s congestion-avoidance algorithm behaves the same when a timeout occurs as in the case of slow start: The value of cwnd is set to 1 MSS, and the value of ssthresh is updated to half the value of cwnd when the loss event occurred.

![image-20221020173739437](image-20221020173739437.png)

> **additive-increase, multiplicative-decrease (AIMD)**
>
> TCP’s congestion control consists of linear (additive) increase in cwnd of 1 MSS per RTT and then a halving (multiplicative decrease) of cwnd on a triple duplicate-ACK event.

##### Congestion window

> **cwnd**
>
> Congestion Window 

![image-20221021095212976](image-20221021095212976.png)

#### Congestion control techniques

- **Slow start** (I)
- **Congestion Avoidance** (II)
- **Timeout retransmission** (III)
  - TCP timeout is quite conservative
  - pay attention to how ssthresh is adjusted

##### Timeout Retransmit (In the 3rd section of the curve)

- **Timeout**
  - RTO = d (SRTT + c RTTV)
- **Congestion control**
  - ssthresh = cwnd / 2
  - cwnd = 1 MSS
  - followed by slow-start
- **Error control**
  - retransmit packet
  - back off timer

##### Fast Retransmit

Duplicating acknowledgment 

>  Example
>
> - rcv: [0, 499], [500, 999], [1500, 1999], [2000, 2499], [2500, 2999]
> - ack: 500, 1000, 1000, 1000, 1000 (3rd dupack)

###### Steps for fast retransmit 

- on 3rd dupack: ssthresh=cwnd/2; cwnd=1 MSS
- followed by slow start

![image-20221021095818170](image-20221021095818170.png)

##### Fast Recovery 

Duplicate Acknowledgment 

> example
>
> - rcv: [0, 499], [500, 999], [1500, 1999], [2000, 2499], [2500, 2999]
> - ack: 500, 1000, 1000, 1000, 1000 (3rd dupack)

###### Steps for Fast recovery 

- On 3rd dupack: cwnd=ssthresh=cwnd/2
- Followed by congestion avoidance

![image-20221021100039713](image-20221021100039713.png)

#### Other TCP congestion controls 

- **Tahoe (packet loss as congestion signal)**
  - slow start, congestion avoid, timeout, fast retransmit
- **Reno (packet loss)**
  - slow start, congestion avoid, timeout, fast recovery
- **Vegas (packet delay): react earlier than loss**
- **NewReno (widely used): cwnd inflation**
- **SACK (selective ack): more info on holes**
- **CUBIC in the current Linux, MacOS, Win**

##### TCP Cubic

where packet loss occurred hasn’t changed much, then perhaps it’s better to more quickly ramp up the sending rate to get close to the pre-loss sending rate and only then probe cautiously for bandwidth.

![image-20221020174109772](image-20221020174109772.png)

The network drops a packet from the connection when the rate increases to W/RTT; the rate is then cut in half and then increases by MSS/RTT every RTT until it again reaches W/RTT. This process repeats itself over and over again. Because TCP’s throughput (that is, rate) increases linearly between the two extreme values, we have:
$$
throughputOfConnection(avg) = \frac{0.75 * W}{RTT}
$$


##### Explicit Congestion Notification (ECN): Network-assisted Congestion Control

Explicit Congestion Notification [RFC 3168] is the form of network-assisted congestion control performed within the Internet.

- both TCP and IP are involved.

![image-20221020174424279](image-20221020174424279.png)

#### Fairness of segments on the network 

- TCP adjusts accordingly to share throughput 
- UDP does not need to control congestion and will transmit at the same rate.
  - can lead to TCP crowding
- Paralleling connections don't help because Browsers still use multiple UDP and TCP packets mixed. 

# Chapter 4 - The Network Layer: Data Plane (Forwarding) 

## 4.1 Overview of Network Layer

#### 4.1.1 Forwarding and Routing: The Network Data and Control Planes 

> **NOTE:** 
>
> CONTROL PLANE = ROUTING = EXTERNAL ROUTER TRAFFIC
>
> DATA PLANE = FORWARDING = INTERNAL ROUTER TRAFFIC

Goal of the Network Layer: to move packets from sending host to the receiving host using two functions:

- **Forwarding** - When a packet arrives at a router’s input link, the router must move the packet to the appropriate output link.
- **Routing** - The network layer must determine the route or path taken by packets as they flow from a sender to a receiver.

> **Routing Algorithms**
>
> A routing algorithm would determine, for example, the path along which packets flow from H1 to H2
>
> ![image-20221103161256093](assets/image-20221103161256093.png)

##### Forwarding and Routing

> **Forwarding**
>
> The router-local action of transferring a packet from an input link interface to the appropriate output link interface.

> **Forwarding Table** 
>
> A router forwards a packet by examining the value of one or more fields in the arriving packet’s header, and then using these header values to index into its forwarding table.

> **Routing**
>
> The network-wide process that determines the end-to-end paths that packets take from source to destination. 

![image-20221103215420071](assets/image-20221103215420071.png)

##### Control Plane: The traditional approach 

![image-20221103162656659](assets/image-20221103162656659.png)

A routing algorithm runs in each and every router and both forwarding and routing functions are contained within a router. All networks have both a forwarding nd routing function.

- Routing done automatically via routing algorithm.
- Humans calculated and enter routing tables manually.

Pros to the routing algorithms:

- Fast, tested and automatic

##### Control Plane: The SDN Approach

![image-20221103162708596](assets/image-20221103162708596.png)

> **SDN**
>
> software-defined networking. Where the network is “software-defined” because the controller that computes forwarding tables and interacts with routers is implemented in software.

An alternative approach in which a physically separate, remote
controller computes and distributes the forwarding tables to be used by each and every router.

Usually this software is <u>open source</u> and available to everyone.

#### 4.1.2 Network Service Models

> **Network Service Models**
>
> Defines the characteristics of end-to-end delivery of packets between sending and receiving hosts.

##### List of services used by Network Layer:

- **Guaranteed delivery** - This service guarantees that a packet sent by a source host will eventually arrive at the destination host.
- **Guaranteed delivery with bounded delay** - This service not only guarantees delivery of the packet, but delivery within a specified host-to-host delay bound (for example, within 100 msec).
- **In-order packet delivery** - This service guarantees that packets arrive at the destination in the order that they were sent.
- **Guaranteed minimal bandwidth** - This network-layer service emulates the behavior of a transmission link of a specified bit rate (for example, 1 Mbps) between sending and receiving hosts. As long as the sending host transmits bits (as part of packets) at a rate below the specified bit rate, then all packets are eventually delivered to the destination host.
- **Security** - The network layer could encrypt all datagrams at the source and decrypt them at the destination, thereby providing confidentiality to all transport-layer segments.

> **Best-effort service** **(No real service at all)**
>
> The Internet service does not guarantee delivery, accuracy or even a response. usually "good enough".
>
> ex. No packet reaching the destination = "Best-effort" satisfied

Utilized application services:

- streaming video services such as <u>Netflix</u> and video-over-IP

- real-time conferencing applications such as <u>Skype</u> and <u>Facetime</u>.



## 4.3 The Internet Protocol (IP): IPv4, Addressing, IPv6, and More

#### 4.3.1 - IPv4 Datagram Format

> **Note:** A datagram is a Network-layer packet. 

![image-20221103164209648](assets/image-20221103164209648.png)

##### The key fields in the IPv4 datagram are the following:

- **Version number (4-bits) -** These 4 bits specify the IP protocol version of the datagram. By looking at the version number, the router can determine how to interpret the remainder of the IP datagram. Different versions of IP use different datagram formats.
- **Header Length (4-bits) -** to determine where in the IP datagram the payload (for example, the transport layer segment being encapsulated in this datagram) actually begins. Most IP datagrams do not contain options, so the typical IP datagram has a 20-byte header.
- **Type of service -** The type of service (TOS) bits were included in the IPv4 header to allow different types of IP datagrams to be distinguished from each other. (ex. FTP, SMTP, etc.)
- **Datagram length -**  This is the total length of the IP datagram (header plus data), measured in bytes. Since this field is 16 bits long, the theoretical maximum size of the IP datagram is 65,535 bytes. However, datagrams are rarely larger than 1,500 bytes, which allows an IP datagram to fit in the payload field of a maximally sized Ethernet frame.
- **Identifier, flags, fragmentation offset -** These three fields have to do with so-called IP fragmentation, when a large IP datagram is broken into several smaller IP datagrams which are then forwarded independently to the destination, where they are reassembled before their payload data is passed up to the transport layer at the destination host.
- **Time-to-Live (TTL) -** field is included to ensure that datagrams do not circulate forever.
- **Protocol (TCP/UDP) -** The value of this field indicates the specific transport-layer protocol to which the data portion of this IP datagram should be passed.
- **Header Checksum -** aids a router in detecting bit errors in a received IP datagram. 
- **Source IP Address -** Host that created the datagram
- **Destination IP address -** Recipient of datagram.
- **Options -** allow an IP header to be extended. Header options were meant to be used rarely—hence the decision to save overhead by not including the information in options fields in every datagram header.
- **Data (Payload) -** data to be delivered. Could be a UDP or TCP packet.

#### 4.3.2 - IPv4 Addressing 

> **Interface** 
>
> The boundary between the host and the physical link.

Each IP address is 32 bits long (equivalently, 4 bytes), and there are thus a total of 232 (or approximately 4 billion) possible IP addresses.

An IP address is technically associated with an interface, rather than with the host or router containing that interface.

> Note: 8 bits = 1 byte ex. 
>
> 192.168.0.1
>
> (8-bit) . (8-bit) . (8-bit) . (8-bit)
>
> 192.168.0.1/24 = Masking the first three numbers
>
> 2^32 (~4 billion) different types of IPs possible
>
> <u>*Power of 2 because in binary you can have either a 1 or a 0*</u>

> **dotted-decimal notation**
>
> Each byte of the address is written in its decimal form and is separated by a period (dot) from other bytes in the address.

![image-20221103184554704](assets/image-20221103184554704.png)

![image-20221103184641624](assets/image-20221103184641624.png)

###### Subnets

> **Subnet**
>
> A subnet is an island of interconnected computers isolated from the main network.

![image-20221103185258258](assets/image-20221103185258258.png)

Above has the subnets (6 in total):

- XXX.X.**1**.X
- XXX.X.**2**.X
- XXX.X.**3**.X
- XXX.X.**7**.X
- XXX.X.**8**.X
- XXX.X.**9**.X

> **Classless Interdomain Routing (CIDR—pronounced cider)**
>
> generalizes the notion of subnet addressing. As with subnet addressing, the 32-bit IP address is divided into two parts and again has the dotted-decimal form a.b.c.d/x, where x indicates the number of bits in the first part of the address.

##### Obtaining a Block of Addresses

In order to obtain a block of IP addresses for use within an organization’s subnet, a network administrator might first contact its ISP, which would provide addresses from a larger block of addresses that had already been allocated to the ISP.

##### Obtaining a Host Address: The Dynamic Host Configuration Protocol (DHCP)

> **DHCP (Dynamic Host Configuration Protocol)**
>
> DHCP allows a host to obtain (be allocated) an IP address automatically. Because of the ease of DHCP it is referred to as <u>plug-and-play</u> or <u>zeroconf</u>

![image-20221103194549576](assets/image-20221103194549576.png)

###### DHCP 4 step proccess for new clients

1. **DHCP server discovery** - using a DHCP discover message, which a client sends within a UDP packet to port 67. The UDP packet is encapsulated in an IP datagram.
2. **DHCP server offer(s)** - A DHCP server receiving a DHCP discover message responds to the client with a DHCP offer message that is broadcast to all nodes on the subnet, again using the IP broadcast address of 255.255.255.255. Each server offer message contains the transaction ID of the received discover message, the proposed IP address for the client, the network mask, and an IP address lease time—the amount of time for which the IP address will be valid.
3. **DHCP request** - The newly arriving client will choose from among one or more server offers and respond to its selected offer with a DHCP request message, echoing back the configuration parameters.
4. **DHCP ACK** - The server responds to the DHCP request message with a DHCP ACK message, confirming the requested parameters.

![image-20221103195038126](assets/image-20221103195038126.png)

#### 4.3.3 - Network Address Translation (NAT) 

##### Internal vs External Network IPs

![image-20221103195408552](assets/image-20221103195408552.png)

The NAT-enabled router does not look like a router to the outside world. Instead the NAT router behaves to the outside world as a single device with a single IP address.

> **NAT translation table**
>
> Contains all internal IPs and Ports than an external datagram can transmit to. Basically port forwarding.

##### Downsides of NAT

- Port numbers being used to address processes vs addressing hosts.
- Routers were meant to stay in network layer and NAT violates this principal.

#### 4.3.4 - IPv6

In the early 1990s, the Internet Engineering Task Force began an effort to develop a successor to the IPv4 protocol. A prime motivation for this effort was the realization that the 32-bit IPv4 address space was beginning to be used up, with new subnets and IP nodes being attached to the Internet (and being allocated unique IP addresses) at a breathtaking rate.

Upgrades to the IPv4 protocol was implemented as well.

##### IPv6 Datagram Format

![image-20221103200245283](assets/image-20221103200245283.png)

**New upgrades included the following:**

- **Expanded addressing capabilities** - IPv6 increases the size of the IP address from 32 to 128 bits. This ensures that the world won’t run out of IP addresses. Now, every grain of sand on the planet can be IP-addressable.
- **A streamlined 40-byte header** - A number of IPv4 fields have been dropped or made optional. The resulting 40-byte fixed-length header allows for faster processing of the IP datagram by a router. A new encoding of options allows for more flexible options processing.
- **Flow labeling** - This allows “labeling of packets belonging to particular flows for which the sender requests special handling, such as a non-default quality of service or real-time service.”. Designers of IPv6 foresaw the eventual need to be able to differentiate among the flows, even if the exact meaning of a flow had yet to be determined.
- **Next header** - This field identifies the protocol to which the contents (data field) of this datagram will be delivered (for example, to TCP or UDP). The field uses the same values as the protocol field in the IPv4 header.
- **Hop limit** - The contents of this field are decremented by one by each router that forwards the datagram. If the hop limit count reaches zero, a router must discard that datagram.
- **Source and destination addresses** - IPv6 addresses.
- **Data** - This is the payload portion of the IPv6 datagram. When the datagram reaches its destination, the payload will be removed from the IP datagram and passed on to the protocol specified in the next header field.

**Fields now removed from the IPv6 protocol that were in the IPv4 protocol:**

- **Fragmentation/reassembly** - IPv6 does not allow for fragmentation and reassembly at intermediate routers; these operations can be performed only by the source and destination. If an IPv6 datagram received by a router is too large to be forwarded over the outgoing link, the router simply drops the datagram and sends a “Packet Too Big” ICMP error message back to the sender. The sender can then resend the data, using a smaller IP datagram size. Fragmentation and reassembly is a time-consuming operation; removing this functionality from the routers and placing it squarely in the end systems considerably speeds up IP forwarding within the network.
- **Header checksum** - This functionality was sufficiently redundant in the network layer that it could be removed.
- **Options** - An options field is no longer a part of the standard IP header. The options field is one of the possible next headers pointed to from within the IPv6 header. That is, just as TCP or UDP protocol headers can be the next header within an IP packet, so too can an options field. The removal of the options field results in a fixed-length, 40-byte IP header.

##### Transitioning to IPv6 from IPv4

Right now the process uses tunneling. Placing the IPv4 packet within the IPv6 and sending it to its destination.

![image-20221103201417639](assets/image-20221103201417639.png)

 # Chapter 5 - The Network Layer: Control Plane (Routing)

## 5.1 - Introduction

![image-20221103215538821](assets/image-20221103215538821.png)

**Two possible approaches for routing:**

> **Per-router control** 
>
> A routing algorithm runs in each and every router; both a forwarding and a routing function are contained within each router. Each router has a routing component that communicates with the routing components in other routers to compute the values for its forwarding table. 
>
> ![image-20221103201633100](assets/image-20221103201633100.png)

> **Logically centralized control** 
>
> Computes and distributes the forwarding tables to be used by each and every router. the routing control service is accessed as if it were a single central service point, even though the service is likely to be implemented via multiple servers for fault-tolerance, and performance scalability reasons.
>
> ![image-20221103201932364](assets/image-20221103201932364.png)



## 5.2 - routing Algorithms 

goal is to determine good paths (equivalently, routes), from senders to receivers, through the network of routers. Typically, a “good” path is one that has the least cost.

we can classify routing algorithms is according to whether they are centralized or decentralized: 

> **Centralized routing algorithms**
>
> computes the least-cost path between a source and destination using complete, global knowledge about the network. This then requires that the algorithm somehow obtain this information before actually performing the calculation.
>
> Algorithms with global state information are often referred to as <u>link-state (LS) algorithms</u>, since the algorithm must be aware of the cost of each link in the network.

> **Decentralized routing algorithms**
>
> The calculation of the least-cost path is carried out in an iterative, distributed manner by the routers. No node has complete information about the costs of all network links. Each node begins with only the knowledge of the costs of its own directly attached links. 
>
> The decentralized routing algorithm is called a <u>distance-vector (DV) algorithm</u>, because each node maintains a vector of estimates of the costs (distances) to all other nodes in the network. 
>
> Such decentralized algorithms, with interactive message exchange between neighboring routers is perhaps more naturally suited to control planes where the routers interact directly with each other

##### A Comparison of LS and DV Routing Algorithms

The DV and LS algorithms take complementary approaches toward computing routing.

![image-20221103220137749](assets/image-20221103220137749.png)

###### DV Algorithm

Each node talks to only its directly connected neighbors, but it provides its neighbors with least-cost estimates from itself to all the nodes (that it knows about) in the network.

| Speed  | Robustness                                                   |
| ------ | ------------------------------------------------------------ |
| O(n^2) | a node can advertise incorrect least-cost paths to any or all destinations. (Indeed, in 1997, a malfunctioning router in a small ISP provided national backbone routers with erroneous routing information. This caused other routers to flood the malfunctioning router with traffic and caused large portions of the Internet to become disconnected for up to several hours |

###### LS Algorithm

The LS algorithm requires global information.

| Speed  | Robustness                                                   |
| ------ | ------------------------------------------------------------ |
| O(M+N) | router could broadcast an incorrect cost for one of its attached links (but no others). A node could also corrupt or drop any packets it received as part of an LS broadcast. But an LS node is computing only its own forwarding tables; other nodes are performing similar calculations for themselves. This means route calculations are somewhat separated under LS, providing a degree of robustness. |



#### 5.2.1 The Link-State (LS) Routing Algorithm (Dijkstra's Algorithm)

The network topology and all link costs are known, that is, available as input to the LS algorithm. This is accomplished by having each node broadcast link-state packets to all other nodes in the network, with each link-state packet containing the identities and costs of its attached links. This is often accomplished by a link-state broadcast algorithm.

![image-20221103220037790](assets/image-20221103220037790.png)

> Definitions for the following notation:
>
> - ***D(v):*** cost of the least-cost path from the source node to destination v as of this iteration of the algorithm.
> - ***p(v):*** previous node (neighbor of v) along the current least-cost path from the source to v.
> - ***N′:*** subset of nodes; v is in N′ if the least-cost path from the source to v is definitively known.

> Worst Case: O(n^2)

```pseudocode
1 	Initialization: 
2 	N’ = {u} 
3	for all nodes v 
4		if v is a neighbor of u
5 			then D(v) = c(u,v) 
6		else D(v) = ∞
7 
8	Loop 
9 		find w not in N’ such that D(w) is a minimum 
10 		add w to N’ 
11 		update D(v) for each neighbor v of w and not in N’: 
12			D(v) = min(D(v), D(w)+ c(w,v) )
13 		/* new cost to v is either old cost to v or*/ 
14 		/*known least path cost to w plus cost from w to v */
15	until N’= N
```

![image-20221103210859896](assets/image-20221103214154828.png)

![image-20221103220105757](assets/image-20221103220105757.png)

![image-20221103210842177](assets/image-20221103210842177.png)

![image-20221103210954616](assets/image-20221103210954616.png)

Route loads are not symmetric, thus a clockwise traversal will be different than a counter clockwise traversal. This creates congestion.

![image-20221103211245041](assets/image-20221103211245041.png)

##### Solutions to congestion caused by oscillations

to ensure that not all routers run the LS algorithm at the same time. This seems a more reasonable solution, since we would hope that even if routers ran the LS algorithm with the same periodicity, the execution instance of the algorithm would not be the same at each node.

Routers in the wild are known to "self-synchronize" amongst themselves.

#### 5.2.2 The Distance-Vector (DV) Routing Algorithm (Bellman-Ford Algorithm)

Whereas the LS algorithm is an algorithm using global information, the distance vector (DV) algorithm is iterative, asynchronous, and distributed. It is distributed in that each node receives some information from one or more of its directly attached neighbors, performs a calculation, and then distributes the results of its calculation back to its neighbors.

The algorithm is <u>asynchronous</u> , thus self terminates without a stop event.

![image-20221103215640424](assets/image-20221103215640424.png)

The shortest distance can be related via Bellman-Ford algorithm:
$$
d_x (y) = min_v\{c(x,v)+d_v(y)\}
$$

```pseudocode
1	Initialization: 
2	for all destinations y in N:
3 		Dx(y)= c(x,y)/* if y is not a neighbor then c(x,y)= ∞ */ 
4 	for each neighbor w
5 	Dw(y) = ? for all destinations y in N 
6 	for each neighbor w
7 	send distance vector Dx = [Dx(y): y in N] to w
8 
9	loop 
10 		wait (until I see a link cost change to some neighbor w or 
11		until I receive a distance vector from some neighbor w) 
12 
13	for each y in N: Dx(y) = minv{c(x,v) + Dv(y)}
14	Dx(y) = minv{c(x,v) + Dv(y)}
15 
16	if Dx(y) changed for any destination y 
17	send distance vector Dx = [Dx(y): y in N] to all neighbors
18 
19	forever
```

![image-20221103214359362](assets/image-20221103214359362.png)

![image-20221103214429805](assets/image-20221103214429805.png)

![image-20221103215713255](assets/image-20221103215713255.png)

For example, node x sends its distance vector Dx = [0, 2, 7] to both nodes y and z. After receiving the updates, each node recomputes its own distance vector. For example, node x computes:

![image-20221103212907475](assets/image-20221103212907475.png)

![image-20221103212926768](assets/image-20221103212926768.png)

##### DV Algorithm: Link-Cost changes and Link Failure

**The DV algorithm causes the following sequence of events to occur:**

- **At time t0** - y detects the link-cost change (the cost has changed from 4 to 1), updates its distance vector, and informs its neighbors of this change since its distance vector has changed.
- **At time t1** - z receives the update from y and updates its table. It computes a new least cost to x (it has decreased from a cost of 5 to a cost of 2) and sends its new distance vector to its neighbors.
- **At time t2** - y receives z’s update and updates its distance table. y’s least costs do not change and hence y does not send any message to z. The algorithm comes to a quiescent state.

##### Count-to-infinity

> **Problem**
>
> ![image-20221103215850952](assets/image-20221103215850952.png)

> **Solutions**
>
> ![image-20221103220001647](assets/image-20221103220001647.png)



## 5.3 - Intra-AS Routing in the Internet: OSPF

## 5.4 - Routing Among the ISPs: BGP

#### 5.4.1 The Role of BGP

#### 5.4.2 Advertising BGP Route Information 

#### 5.4.3 Determining the Best Routes 

#### 5.4.4 IP-Anycast

#### 5.4.5 Routing Policy 

#### 5.4.6 Putting the Pieces Together: Obtaining Internet Presence

## 5.6 - ICMP: The Internet Control Message Protocol

## 5.8 - Summary

# Chapter 6 - The Link Layer and LANs

