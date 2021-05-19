# Fundametal of Networking
This covers chapter 13 from the book, Computer Systems: An Integrated  Approach to Architecture and Operating  Systems. It can also be used for the students who are taking cs 2200 in Georgia Tech.

---
Three touch points for operating system to facilitate the box to hook up the network:
- **socket library**: it provides an API for developing network application. Defining and implementing this API is the first touch point for the operating system to facilitate network.
- **protocal stack**: Messages generated by an application program have to reach the destination and it involves a myraid of issues. The protoccal stack addresses all these issues.
- **network device driver**: The computer connects to network via Network Interface Card (NIC) and network device driver interacts with this NIC.

---

## Basic terminology
### Host and network
A **host** is a computer that hooks on to the network and represented as an edge of the network. It can be either _client_ or _server_ by the role it is playing
A **network** is a composite of several networks. This collection of networks form the Internet.
![host and network](https://user-images.githubusercontent.com/59812671/118580575-f8842100-b744-11eb-8955-276c646754a1.png)
### Inside the Network
1. A host plugs into a **cable modem** and access network
2. Cable modem connects to **Internet Service Provider (ISP)*** or **local area network (LAN)**
 - You may use ISP if you are at home and LAN if you are in school, office, or a dorm room.
 - REgional ISP knows how to connect with another regional ISP around the world.
![host and network (1)](https://user-images.githubusercontent.com/59812671/118581894-48fc7e00-b747-11eb-9eaf-b4c85a2d36a7.png)
### Router
A **router** sits in the core of network and route messages between ISP across the world. It has a series of queues that reside at input of the routers from source to destination.
Thereare 2 different packet flows on the Internet:
- **Queing delay**: In the presence of other traffic, a packet may experience delays on the packet.
- **Packet loss**: A packet that is full because of its finite-size, a packet may be dropped.
![router](https://user-images.githubusercontent.com/59812671/118582582-70077f80-b748-11eb-8948-995d847390c0.png)
---
## Protocol Stack for Networking
A **protocol stack for networking** is a the portion of the operating system that addresses the problems below:
1. Arbitrary message size and physical limitations of network packets (network protocal there is a maximum packet size and message size may not fit the size limitation.)
2. Out of order delivery of packets (There is no guarantee that the packets of the message will arrive in order by queueing delays and variety of paths they can take.)
3. Packet loss in the network 
4. Bit errors in transmission 
5. Queuing delays en route to the destination (Since the packets take several different networksbefore the destination, it is hard to predict this information.)

This requires well-defined interfaces between any two layers of the protocol stack. The modularity allows integration of a new module at a particular layer with minimal changes to the other layers but provides performance penalty.

### 5 layers Internet protocol stack
![Internet protocol stack layers](https://user-images.githubusercontent.com/59812671/118592528-ec0ac300-b75a-11eb-9b66-67c42f0387f1.png)
#### Application Layer
- Responsible for supporting network-based applications such as an instant messenger (IM), multi-player video games, P2P music/video sharing, web browser, electronic mail, and file transfer. 
- provide a common language for application level entities (clients, servers, and peers) to use for communicating with each other.
##### Eamples of application vs application layer
| | Applications  | Application layer  |
| ---- | ------------- | ------------- |
| web | **client**: FireFox, Internet Explorer, etc. **server**: host of specilized serveces (Google and Yahoo!) | HTTP ( Hyper-Text Transfer Protocol) |
| electronic mail | **client**: Microsoft Outlook and Unix Pine. **server**: mail server (Microsoft Exchange) | SMTP (Simple Mail Transfer Protocol) |

#### Transport Layer
- Takes an application layer message and conveys it between the end of communication.
- It is responsible of 5 problems addressed above.
There are 2 dominant transport protocols: TCP and UDP.

| TCP  | UDP  |
| ------------- | ------------- |
| provides end-to-end reliabilithy for in-order data delivery | deals with message that have strict boundaries and do not provide in-order delivery |
| forms a byte-stream and gives stream semantics for data transport | gives datagram semantics |
| connection oriented protocol: establish a connection between the two endpoints before the actual data transmission takes place, and the connection is closed( connection teardown) after it is finished | does not involve any connection establishment before sending the message, or any teardown after sending the message |
##### Main functionalities
1. Support arbitrary data size at the application level 
2. Support in-order delivery of data 
3. Shield the application from loss of data 
4. Shield the application from bit errors in transmission.
###### Solution #1: uses scatter/gather and sequence number to support functionality #1 and #2
1. scatter/gather
 -  scatter: source breaks up the data into packets commensurate with the hardware limitations of the network. 
 -  gather: destination assembles the packets into the original message
2. sequence number: helps in the reconstruction of the original message at the receiving end despite the order of the arrival.
###### Solution #2: uses positive and RTT to support functionality #3 and #4
Bit errors do not always make the packet completely unusable. It is possible to do error correction on packets, usually referred to as Forward Error Correction (FEC). However, when the errors are beyond the fixing capabilities of FEC algorithms, the packet is as good as lost.
The loss of data is dealt with following techniques:
1. positive acknowledge: It indicates package has been arrived to the destination.
2. RTT (round trip time): an estimation of the cumulative time delay experienced at the sender for sending a packet and receiving an acknowledgment, and is used in selecting timeout values.

![passage of a packet through the network](https://user-images.githubusercontent.com/59812671/118732296-e2329f80-b7ee-11eb-9d5c-f8f78f08a399.PNG)

#### Transport layer types of protocols
##### Stop and wait protocols
![stop and wait](https://user-images.githubusercontent.com/59812671/118756784-8254ec80-b820-11eb-8f00-3a02306f2ce4.PNG)

1. The sender sends a packet and waits for a positive acknowledgement(ACK)
2. As soon as a packet is received, the recipient generates and sends an ACK for that packet. The ACK contains the information for the sender to discern the packet which is the sequence number of the received packet. 
3. The sender waits for a period of time called timeout. If within this period, it does not 
hear an ACK, it re-transmits the packet. Similarly, the destination may re-transmit the ACK.

#### Network Layer
- It involves how to route a packet from source to destination.
- **For outgoing message**: find a wway to get packet to the destination.
- **For incoming message**: passes and collects packet into message to the transport layer.
- **IP address**: help to subsume both the formatting of the packet and determining the route of the packets.
#### Link Layer
- Ferries the IP packets between nodes on the Internet through which a packet has to be routed from source to destination. 
- Example: Ethernet, Token Ring, and IEEE 802.11
- **For outgoing message**: hands the IP packet to the appropriate link layer (if necessary breaking up the IP packet into fragments at the network layer)
- **For incoming message**: the network layer reassembles the fragments to reconstruct the original IP packet.
#### Physical Layer
- This layer is responsible for physically (electrically, optically, etc.) moving the bits of the packet from one node to the next.

