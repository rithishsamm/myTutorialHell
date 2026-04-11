For an optimist the glass is half full, for a pessimist it's half empty, and for an DRUNK ENGINEER is twice bigger than necessary explains networking in brief.

#### 1) OSI Reference Model - Best Explanation

The Open Systems Interconnection model (OSI model) is a conceptual model that characterizes and standardizes the communication functions of a telecommunication or computing system without regard to their underlying internal structure and technology. Its goal is the interoperability of diverse communication systems with standard protocols. The model partitions a communication system into abstraction layers. The original version of the model defined seven layers.
#### **Time Stamp Agenda**
[00:50](https://www.youtube.com/watch?v=eKHCH6rw0As&t=50s) WHY develop the OSI model? 
[05:02](https://www.youtube.com/watch?v=eKHCH6rw0As&t=302s) The Application Layer
[07:36](https://www.youtube.com/watch?v=eKHCH6rw0As&t=456s) The Presentation Layer (Layer 6) 
[09:17](https://www.youtube.com/watch?v=eKHCH6rw0As&t=557s) The SESSION Layer (Layer 5) 
[10:50](https://www.youtube.com/watch?v=eKHCH6rw0As&t=650s) The Transport Layer (Layer 4)
	[11:00](https://www.youtube.com/watch?v=eKHCH6rw0As&t=660s) TRANSPORT is not moving something from A to B. 
		[11:20](https://www.youtube.com/watch?v=eKHCH6rw0As&t=680s) It's responsibility is to get the SEGMENT to the right APPLICATION or SERVICE. 
		- When I say APPLICATION you need to think "client". 
		- When I say SERVICE, you need to think SERVER.
		[12:20](https://www.youtube.com/watch?v=eKHCH6rw0As&t=740s) What port is the SERVER going to be listening on? 
		PORT 80 Port 80 is the default port for web delivery.
		[13:25](https://www.youtube.com/watch?v=eKHCH6rw0As&t=805s) It's not about picking up from A to B; it's about working out which service on the server needs the message (once the message has arrived on the server). 
		- How does the server know to look for port 80? Answer: the **HEADER** 
	[16:10](https://www.youtube.com/watch?v=eKHCH6rw0As&t=970s) Introducing RELIABLE DELIVERY
	[17:27](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1047s) We gain EFFICIENCY
	[19:25](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1165s) Does all communication need to be reliable?
	[21:58](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1318s) FLOW CONTROL 
	[22:24](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1344s) Slap a header on there In that header there are going to be "sequence numbers". We call them fields.
	[23:20](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1400s) There's going to be a SOURCE port and a DESTINATION port.
	[25:47](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1547s) What if I just throw it out there? If you tried to verify every -item- communicated, it would not be effective.
	[26:40](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1600s) "I use unreliable delivery!"
	[27:27](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1647s) UDP and un-reliable delivery - If you don't get what I send, raise your hand! - If I choose to use UDP.
	[28:20](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1700s) "And that's what uTorrent does" 
	- To successfully download a file, you need all the bits 
	- But imagine if there was a check after each thing communicated! -
		[29:00](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1740s) Contrast with downloading a file on a web browser 
		[29:26](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1766s) Prime example of UDP delivery is VIDEO
	[31:30](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1890s) As a DEVELOPER, you have to consider the trade-off between RELIABILITY and PERFORMANCE 
	[31:49](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1909s) Summary 
	[32:00](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1920s) NOTE on segmentation 
	- We may have multiple applications running concurrently. 
	- We'll probably have multiple computers on our network using our connection, concurrently.
	- Segmentation isn't merely achieved by dispersing messages across several connections; 
	- It's also achieved by allowing multiple people to use each connection.
	[33:03](https://www.youtube.com/watch?v=eKHCH6rw0As&t=1983s) We multi-plex the conversation: we take segments from different conversations and interweave them together, so it looks like everyone's using the same network connection at the same time.
[33:27](https://www.youtube.com/watch?v=eKHCH6rw0As&t=2007s) The Network Layer (Layer 3)
	[34:24](https://www.youtube.com/watch?v=eKHCH6rw0As&t=2064s) By the time we get down to Layer 3, we have DEVICES 
	[35:15](https://www.youtube.com/watch?v=eKHCH6rw0As&t=2115s) What is a packet?
	[36:00](https://www.youtube.com/watch?v=eKHCH6rw0As&t=2160s) Addressing mechanism 
	[36:20](https://www.youtube.com/watch?v=eKHCH6rw0As&t=2180s) "The protocol used at Layer 3 is typically IPv4" - It could also be AppleTalk though. 
[39:53](https://www.youtube.com/watch?v=eKHCH6rw0As&t=2393s) The Data Link Layer (Layer 2) 
[41:10](https://www.youtube.com/watch?v=eKHCH6rw0As&t=2470s) The PHYSICAL Layer (Layer 1)
	[41:37](https://www.youtube.com/watch?v=eKHCH6rw0As&t=2497s) What is a frame? 
	[42:15](https://www.youtube.com/watch?v=eKHCH6rw0As&t=2535s) The Logical layer and the MAC layer

#### IN BRIEF

**WHAT ORGANIZATION ARE RESPONSIBLE FOR DEVELOPING THIS MODEL** -> IEEE, ISO (International Standards Org)
##### WHY develop the OSI model?
Why OSI? -> To Foster Competition + Compatibility + keeping the internet open //internet society (either a propriertory or open devices)

802.1q - Wifi Standard, 802.3- Ethernet => standards derived by IEEE. If you wanna do a wifi device, you can go to IEEE. Refer 802.1q paper and work with the rest. Keeping things open for society.
##### The Application Layer (acts as an interface for our network)
Responsibility? : L7 Skills? -> Programming, Web App devs, Software engineers who gives our computer a purpose. eg: Web Browser. http. ftp, dns, smtp -> all in an app layer protocols. it responsible for to get input from user to network
##### The Presentation Layer (Layer 6)
ASCII, 
SSL,() TLS, TCL, UTF-8) ,
Compression
##### The SESSION Layer (Layer 5)
=> Keeping the connections alive.
eg: email, torrent, web -> dns, email all at the same time without killing each other and maintaining connection alive.
##### The Transport Layer (Layer 4) 
Not easy as placing a thing to an another place. *TRANSPORT is not moving something from A to B. To get its right application(client) to the service(server). 

Pics the data and fragment it all as segments
It's responsibility is to get the SEGMENT to the right APPLICATION or SERVICE.** 
	 - When I say APPLICATION you need to think "client" 
	 -  When I say SERVICE, you need to think SERVER
	 - 
In linux, a service = Daemon, cause, running in background and listening, listening to what? -> port 80, default port for web server. Software delivery. 
**
****What port is the SERVER going to be listening on?** 
	PORT 80 Port 80 is the default port for web delivery. - > If, Apache tomcat. It's not about picking up from A to B; it's about working out which service on the server needs the message (once the message has arrived on the server) 
**How does the server know to look for port 80?**
Answer: the HEADER. upto session, we've generated data. -> segmentation -> Why segmenting - only one way to go out of the system and acknowledge all the deliveries. Prevention of EOF problem(end of file). 
###### Introducing RELIABLE DELIVERY
Acknowledgment + We gain EFFICIENCY + Security + Delivery to prior destination + back to default in order from pieces as segments.

How does it navigate to get the message to the prior destination. who gets who after segmenting the whole data into pieces. 
=> <u>Responsibility of transport layer. </u>

###### Does all communication need to be reliable?
cant compromise. TCP -> ensure same order delivery + reliable comms + mechanism to and fro guarantee FLOW CONTROL. 

**To do that:**
Slap a header on there In that header there are going to be "sequence numbers", => tags like thing that align the segments. Acknowledgement numbers -> what has received and expects what's going to receive next. We call them fields. like a tag that orders the data's segments in order. So, what applications that are going to use all this sets. => By calling out ports over the header.

eg: sender

| srcPort (is randomly assigned by Cilents OS) | header | data | destPort |
| -------------------------------------------- | ------ | ---- | -------- |
 **srcPort**
 Why there is the srcPort is randomly assigned by ClientSide OS. (You open some a youtube session running a video, you dont want to open the same video running on another browser block to navigate the rest over the same app simultaneously). it can listen to 80, but gets reassigned or forwarded to a random port for multi stable sessions. 
 **destPort**
 What about destination port? cannot randoly assigned. http 80.

**What if I don't want all these overheads, all these fields, adding proven delivery track of records controlling the flow. What if I just throw it out there? If you tried to verify every -item- communicated, it would not be effective.**
no overhead of such data and but to just throw stuff. **I use unreliable delivery!**
 
**UDP and un-reliable delivery - still segment it but bigger than TCP.  If you don't get what I send, raise your hand! - If I choose to use UDP.**
-- allows to send more without headers, without acknowledgement, without overhead, without data resending but deliver data at a single time for effectiveness. 
eg:
**" that's what uTorrent does"**
	- To successfully download a file, you need all the bits 
	- But imagine if there was a check after each thing communicated! -

Application of the UDP Protocols (usecases that are time sensitive):
-  audio, video streaming
- media delivery
- torrenting
- real time monitoring
- messaging

**Contrast with downloading a file on a web browser**** 
**Prime example of UDP delivery is VIDEO****

**As a DEVELOPER, you have to consider the trade-off between RELIABILITY and PERFORMANCE**
like video buffering where you forward and backward and video letting it loading + on the other side caching it all locally but might messup if there is a bits loss.

**Summary**** 
**NOTE on segmentation**** 
	- We may have multiple applications running concurrently. 
	- We'll probably have multiple computers on our network using our connection, concurrently.
	- Segmentation isn't merely achieved by dispersing messages across several connections; 
	- It's also achieved by allowing multiple people to use each connection.

Multitasking same processes concurrently over multiple connections where network doesnt do parallel connections. segments + headers and placing it all instead of distributing each for each but using the same segmented data for the devices.

**We multi-plex the conversation: we take segments from different conversations and interweave them together, so it looks like everyone's using the same network connection at the same time.****

> [!NOTE]
> Up until LAYER4 TRANSPORT all the navigation has been performed. Not only we have protocols for each layer. Probably in computer or OS. Some give outright config of Transport layer derives TCP/IP. Where you can do configs like flow control, segment size and more.


**The Network Layer (Layer 3) - **** where the data has to travel
	**By the time we get down to Layer 3, we have DEVICES**** +
Takes all -> The Segment + Header = Packet
Up until now, all we have is protocols to navigate all around the network. here in this layer, we have specialized devices to route it all along the way to do such that routers and switches. -> not just having protocols to identify each layers but devices ->

**What is a packet?**** - 
Takes all -> The Segment + Header = Packet. Packets are used to identify end devices which contains segments for data re-segmentation. 
	**Addressing mechanism**** - 
-> IP - Internet Protocol. => Most popular, why? its standard and open. +versions IPv4, IPv6

**The protocol used at Layer 3 is typically IPv4" - It could also be AppleTalk though.**** 
~~-> Apple talk~~
~~-> NetWARE~~
~~-> IPX~~
~~-> Connectionless Network Services~~ => All discontinued mechanisms

**The Data Link Layer (Layer 2)**** 
Connects two things together => Data segments that receives till now by IP -> converts to binaries and sends it all to the physical layer (cables).

**The PHYSICAL Layer (Layer 1)****
	**What is a frame?**** 
 -> A derivation that consist of 

| Header | Packet | Trailer |
| ------ | ------ | ------- |
why trailer? the last layer of encapsulation consist of two layers. Logical + MAC. when a frame begins and when it ends. End of the frame!.


**The Logical layer and the MAC layer**

---
---
---
#### 2) OSI and TCP/IP Model - Best Explanation

The Internet protocol suite is the conceptual model and set of communications protocols used on the Internet and similar computer networks. It is commonly known as TCP/IP because the original protocols in the suite are the Transmission Control Protocol (TCP) and the Internet Protocol (IP). It is occasionally known as the Department of Defense (DoD) model, because the development of the networking model was funded by DARPA, an agency of the United States Department of Defense. 

The Internet protocol suite provides end-to-end data communication specifying how data should be packetized, addressed, transmitted, routed and received. This functionality is organized into four abstraction layers which are used to sort all related protocols according to the scope of networking involved.[1][2] From lowest to highest, the layers are the link layer, containing communication methods for data that remains within a single network segment (link); the internet layer, connecting independent networks, thus providing internetworking; the transport layer handling host-to-host communication; and the application layer, which provides process-to-process data exchange for applications. 

Technical standards specifying the Internet protocol suite and many of its constituent protocols are maintained by the Internet Engineering Task Force (IETF). The Internet protocol suite model is a simpler model developed prior to the OSI model.

#### **ENCAPSULATION & DECAPSULATION**
L7 TO L1 -> Encapsulation, Reverse - Decapsulation. => Critical in communication to get the data from sender to the receiver 

L7 TO L5 can be grouped in which is the sole responsible for the L4 Transport layer to pass on the data. 

L7 + L6 + L5 -> PDU (protocol data unit) which is simply a DATA.
L4 Transport Data -> PDU -> 10101011 Binary lies here
Apps make requests, Services receive them.
src + port +Data + dest + port => Segment -> Packet + srcDestIP
>Since the devices can communicate in 10101 binary which can be supported over any type of comms in over a same set of wire -> CONVERGING NETWORK

L3 to L1 -> Network IP L3 => Frame Conversion + Physical Address Device forwarding L2 => Abstraction to Binary over cables L1





