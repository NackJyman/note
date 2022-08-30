# Computer Networks
### Introduction
##### August 22nd, 2022
---

hw due friday
read chap 1,2

---
### Chapter 1
##### August 24th, 2022
---

#### Network Access
- Residential
	- Wired Connection
- Company
	- Wired Connection, Pays for high throughput lines (Ex: 300 GB/s)
- Wireless

| Connection | Speed |
|------------|--------|
| Dial-up    | 24 Kb/s|
| ABSL | 12 MB/s |
*SEE LECTURE PPT FOR MORE*

#### Cable
	Gig internet? What does that mean? that someone will give me a spear and I am going to hunt frogs with it!?

#### Transmission Media
- Twisted Pair
- Radio 

#### Switching
Two basic methods of transmitting data
- Packet Switching
	- Protocols will resolve any communication issues (hopefully) without operator intervention. Packets are sent out WITH a destinarion.
- Circuit Switching
	- A fixed route connection is established between the sender and receiver. All packets are sent over this connection. Packets require no destination information. When communication is finished, the connection is terminated.

TDM and FDM

---
##### August 26th, 2022

### Switching Differences
#### Circuit Switching

- The major delay is the "ends"
- Transmission time based on current load.
- Uses multiplexing

#### Packet Switching
- The delay is per packet.
- May or may not get better transmission rates.
- More difficult than circuit switching to guarantee QoS.
- Cheaper in terms of effort.

### Delay
Delay comes from a multitude of different reasons. The destination needs to process the incoming packet, could take longer due to reasons like photo processing. 

Queueing is also a reason, there is a limited amount of data that can be sent in one direction. So in result the sender queues the amount of data. 

The total amounts of loss points come from processing, queuing, transmission, propagation, loss. Loss can come from a multitude of reasons within the environment, kinks in the coax, high voltage wiring emmitting a energy field. 

To combat things like delay, we use an end-to-end computation.

### Service Models
Protocol layers on most modern computers 70% software. The other 30% would be hardware like a network interface card (NIC). Hardware implementations are fast but not easily changed. 

#### Models
The internet model happened, the ISO OSI model is an official standard.

- ISO = International Organization for Standardization.
- OSI = Open Systems Interconnection

| Internet Model | OSI Model    |
| -------------- | ------------ |
| Application    | Application  |
|                | Presentation |
|                | Session      |
| Transport      | Transport    |
| Network        | Network      |
| Link           | Link         |
| Physical       | Physical     |

*Read the text on Internet Application Layer, and the OSI Model Application, Presentation, and Session layers do.*

#### Data
The application layer sends *messages*.
The transport layer uses *segments*.
The network layer has *datagrams*.
The link layer transmits *frames*

Each one wraps (encapsulates) the data from the one above it. Each is supposed to be unaware of the content/format of other layers. This is normally the case, more like none of them care. There are exceptions which will add some confusion.

The encapsulation happens when a computer sends it and it just gets passed along until it reaches the destination. 

### Network Attacks

- DoS, Denial of Service
- Packet Snigging
- IP spoofing
- Man in the middle attacks

#### End of Chapter 1
---
## Chapter 2

### Application Layer
Application architecture is not part of the network. You build an applicationm it does whatever you want. But you use an _application programming interface_ (API) to integrate the application with the network.

Relies on specific functionality. This has been provided by an underlaying network. Basically two types:
- peer-to-peer (P2P), and 
- client-server,
- both have gray areas

#### Sockets
We have processes communicating and sending maessages. Applications use Sockets. Phone Operators are the earliest example of a socket. A socket is the endpoint of the communication. The software that connects the application to the rest of the network stack via an address. Sockets usually have an address binded to them. The most common type of address is *AF.INET* and the next most common is *AF.UNIX*. (Address Family Internet and Unix). Sockets get binded to ports but do not have numbers, ports have numbers.

#### Connections

A connection is defined by an API. They come in two different flavors:
- reliable $\Rightarrow$ TCP
- unreliable $\Rightarrow$ UDP

##### TCP
Transmission Control Protocol

##### UDP
User Datagram Protocol.

Connectionless, no guarantees on delivery. Put an address on the packet and chuck it into the void.

##### Which one?
Ig the application is loss-tolerant, use the unreliable protocol, UDP. The program decides which to use. 

Examples of UDP:
- VoIP
- Audio/Video
- Video Games (maybe)

UDP is unreliable, gives no guarantees. And there is much less overhead.

If the file NEEDS to be lossless, use TCP.

Examples of TCP:
- File Transfer
- Email
- IM
- Web Docs

## END OF WEEK
---
