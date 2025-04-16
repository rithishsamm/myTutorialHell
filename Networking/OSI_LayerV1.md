    # OSI LAYER
OSI - OPEN SYSTEMS INTERCONNECTION. A 7 Layered Architecture.

Agenda:
1) Basics of OSI
2) Why OSI
3) Webservers and services for components like sockets and more
---
---
Networking glimpses:
**Network Addresses (IP) and Ports**
generic cover, not in depth.
Can a computer have more than one IP Address.? **Yes**!, How? - **Through Interfaces.**

<aside>

💡 SIMPLY PUBLIC IP, PRIVATE IP

</aside>

How to check availbale interfaces in a system →
1) ifconfig - undertstanding ifconfig
→  each of the connections listed here is an INTERFACE
→ INTERFACE -xGateway, mode of communication between the computer and the rest (such as wifi, eth and more)
→ Each Interface here give an IP,
if No VPN

| INTERFACES |  |  |  |  |
| ---- | ---- | ---- | ---- | ---- |
| wifi | goes here < — |  |  |  |
| eth | or here  < — |  |  |  |
| vpn |  |  |  |  |
| docker | appData | ^ |  |  |
| … |  |  |  |  |
if VPN

| INTERFACES |  |  |  |  |
| ---- | ---- | ---- | ---- | ---- |
| wifi |  | v, down via VPN |  |  |
| eth |  | v, down via VPN |  |  |
| vpn | < - via here |  |  |  |
| docker | appData | ^ |  |  |
| … |  |  |  |  |

Each interface has its own IP’s. eg: WIFI with 5devices connected namely A, B, C, D

A has 3containers with VPN connected via WiFi. IDEA behind this is, The set of IP’s under each hosts. A set for WIFI, set for Docker containers, set for VPN.

1) lets take A device as an example which consist of 3IP’s one HOST/LOCAL LOOPBACK, GATEWAY and VPN/Docker host.
2) question: if the user pings [google.com](http://google.com) from Device A, since it has multiple gateways within, how does such host know that this gateway is the INTERNET, it has to go this way despite of having multiple interfaces within the Host.
3) question: how does it picks the gateway and how it knows the path of reaching google via this gateway.

⇒ this get possible by **the following contents.**
1. **IPTables & Addresses**
2. **Port Numbers (1+2 → Binding)**
3. **Net/Subnet Masking (important)**
4. **Gateway IP’s**
5. **Classes of IP’s**
6. **How services/components uses them all**

**IPTABLES**:    
Eg: there VM’s or Containers running in Device A. Which Gateway will the request lead me to google is the question. The role of IPTABLES here?
> iptables  --help

**PORT NUMBERS:**


**SUBNET MASKING:**
—continuing this in the above AppTable, multiple apps runs in a system, eg: chrome, if chrome has to establish a communication over the internet means, It has to communication to . Duty of ***Subnet Mask,*** if docker ip’s means 172.17.0.3, meanwhile the Subnet’s of Docker might be like 255.255.255.1 and 2, 3, 4, and it goes.

*What these subnet masks will do means:*
— research in brief about SUBNET MASK, PORT BINDING and MORE..

**GATEWAY:**

---
---
##### **Logic behind the current agenda:**
eg: Dos, Denial of service is not just about attacking stuff by sending bulk request. It is not as simple as it sounds. It has nook and corners to take care of. Things should be under consideration that which layer to attack, which component in such layer should be attacked. 

What lies in such layer, What wee gonna tap in such layers components, to understand how such dos get functioned should be under consideration for a deeper understanding. 

**For such case, Understanding OSI is crucial.**

SNA Agenda:
1. OSI Layer Connectivity
2. Application Layer in OSI
3. Presentation Layer in OSI
4. Transport Layer in OSI
5. Network Layer in OSI
6. Data Link Layer Layer in OSI
7. Physical Layer in OSI
### **ANALOGY:**
for eg: 
Props for the analogy: A ethernet cable with an internet Connection, A system, An application within the system such as YouTube.
State: All configured an connected which Youtube can be accessible via the machine with is connected to the Internet via Eth Cable

Here, The videos via Internet from Youtube's server doesn't transmitted as the exact video format. comes as 0's and 1's and pass through layers and gets viewed. The transit of data from cables 0's and 1's to the appropriate content is the construct here. Model is common for other types of data transmission too.   

### **8. Starting off by understanding the OSI Layer Connectivity:**
WHERE AND ALL the model exists? regardless of any devices embraced the model such as, eg: how osi lies in a hotspot modem? -> it can be a WiFi router or a LAN connection or it can be Wifi Dongle. No matter what.

Concept of modem is that, IT HAS TWO SIDES. 
1) INBOUND
2) OUTBOUND
	=> the name modem derives itself. Modem -> Modulator & Demodulator
Whatever the device that has can support network will have the implementation of the OSI Model 7Layers.

Internet to modem, -> The form of communication here is that the data comes in Bits, 
> once the bits get received, THE OSI of each will start to work

Modem to computer -> what and all it happens between those two from Bits to Data conversion over the network to the appropriate transit, device and the file format.


=> HERE, THE OSI MODEL COMES INTO THE PICTURE AND STARTS THE ACTION 😎

Bits to Data as derived in the below picture, fits well..

- 5 ⇒ Software, 4 ⇒ Heart of OSI, 3 - 1 ⇒ Hardware Layer. Packets that gets sent and received. Vice versa as bits to Data
 ![[osi_model_7_layers.png]]
The Open Systems Interconnection (OSI) model describes seven layers that computer systems use to communicate over a network. This model consist of Seven Layers. 

## **7. The Application Layer (a.k.a Desktop Layer)**


![[osi_model_application_layer_7 1.png]]
 Why we call this as Application layer?

> [!Application Layer's Logic]
>  Whenever a network works to interconnect systems, the data that comes through has to end up in an APPLICATION, right?

Data transmission where contents presented and delivered from network to Application Layer.
* This layer is at the very top of the OSI Model. 
* Implemented at the network applications. Applications that supports networking. Eg: Simple the software layer that front faces and makes all the pass through.
* ==Where the data is produced and acquire over the networks==. Data transmission 1st step in to get into network. 
* Paves a path (a port/window) for data to reach the application & its services for ==displaying/presenting== it to the user.

Functions of this layer: Client side apps eg: Mail Client for sending mails and more. The Apps. As a user, the area that you interact is the layer.

> The attacks that are getting performed, the target is application layer. WHY? cause there are protection in all the other layers. since they're all globally standardized.  + this is the layer that the programmer takes care of, who leaves all the vulnerabilities and resource limitations which can be flooded, Dos'ed an exploited easily. Where the L7 is all programmer's duty and responsibility plus the programmer only knows what has been happening over L7. THIS IS THE AREA THAT WE CAN EXPLOIT. 
> 
> As a hacker, we need to know all these layers, what to attack, which layer to attack and why we can't attack and nothing works attacking the rest of the layers. 

> [!IMP]
> Since L6 and the rest are all publicly assessable standardized applications or libraries. the L7 is our playground. 
****

General Explanation:
At this layer, both the end user and the application layer interact directly with the software application. This layer sees network services provided to end-user applications such as a web browser or Office 365. The application layer identifies communication partners, resource availability, and synchronizes communication.
## **6. The Presentation Layer (a.k.a Translation Layer)**
![[osi_model_presentation_layer_6 1.png]]
This is the layer where the data that we send and receive gets parsed or unparsed. eg: Browser's Inspect element is presentation layer where the browser does its GET/POST requests as its some format.

* a.k.a Translation Layer. Why it is called as translation layer? ALL THE DATA (IN MOST CASES) ARE/ WILL BE ENCRYPTED BEFORE PRESENTED IN THE APPLICATION LAYER. eg: Assessing Data via https. (data presented here are encrypted by TLS stds)
* if a query made, the data that comes via https will be encrypted right? The encrypted data gets decrypted in our computer THE ACTION OF encryption/decryption happens here in the .request library right? THE AREA THAT THE LIBRARY LIES IN IS CALLED THE PRESENTATION LAYER.
* the .request library is getting the data ready, parse it and present it to the user via the L7 App layer. 
* ==Function of L6 Presentation Layer - Sending/Receiving, 1) while sending, the data from App Layer L7 gets extracted, encrypted and manipulated as per the required encryption format to transmit data over the network
* Usecases of L6 Presentation Layer: 
1) Encoding/ Decoding (a.k.a translation) - ASCII, UTF-8 and more
2) Encryption/Decryption - https, TLS
3) Compression - inflate/deflate, gzip 
* the response from any server anywhere near, server's/program's/language's request module mustve served things decoded before sending it as a data? 
-- Making request via terminal (inspect -> network-> IP -> copy http req)
-- terminal -> telnet ip 80 -> paste request
5) **> THIS IS "THE PRESENTATION LAYER". Any library that lets you make a request. (the process of making such kind, that is presentation layer)**

> JUST THE TRANSIT OF DATA UNDER OR NO CERTAIN ACTION = PRESENTATION LAYER + DELIVERS TO L7 MAKING IT USER READABLE DATA & PRSENT THE SAME TO THE L6 COMPONENTS

General Explanation:
The presentation layer formats or translates data for the application layer based on the syntax or semantics that the application accepts. Because of this, it at times also called the syntax layer. This layer can also handle the encryption and decryption required by the application layer.
## **5. The Session Layer**
![[osi_model_session_layer_5.png]]
	SESSION LAYER: Is where the connection gets established where it either gets connected accepted or denied. Whether should i establish with a particular or a set of connection or not. + after the connection gets established, it maintains the session. 

1) This layer establishes connection where accept or deny, maintains session, does authentication and authorization + ensures security.
2) Session establishment, maintainance and termination. => allows two or more process to establish. use & terminate the connections
3) Sync/ Synchronization: Assembles and disassembles the data or transit (error handling too, 1024bits - BUFFER SIZE). the space where it dismantles(data segmentation) and sends data + also reconstructs(data reassembly) back to default in and out via the session layer.

What is reverse port?
eg: incoming connections get established in 3074.  the outgoing is going to be allocated on a random different other port for each connection. So that each gets it own session. in local experimentation, the connection will be stable. 

> [!NOTE]
> In production, IT IS ALL STATELESS TRANSFER where session get allocated on a freeport for outbound. After allocation, it won't be stable as same as it gets.
> THAT WHY we manage such session as cookies, session token in local and the same to server to keep things engaged.

-X-X-X- SOFTWARE LAYER ends where in the upcoming layers, the data will be out of our hands :)

> [!NOTE]
> also, Application +Presentation + Session => SOFTWARE SIDE LAYER (a,.k.a) APPLICATION LAYER => They deliver the DATA by layer combined

General Explanation:
The session layer controls the conversations between different computers. A session or connection between machines is set up, managed, and terminated at layer 5. Session layer services also include authentication and reconnections. 
## **4. The Transport Layer (HEART OF OSI)**
  ![[osi_model_transport_layer_4.png]]
=> HEART OF THE OSI. why?
SIMPLY THE FRAGMENTED DATA IS ABOUT TO TRANSIT, VIA WHERE IS DETERMINED/HOW THAT DATA REACHES IT DESTINATION ->  **TRANSPORT LAYER.** 

eg:  a socket connection software vs hardware transit comparision

```Python
##SESSION LAYER
s = socket.socket(AF_INET, socket.SOCK_STREAM) #declaring typeOfSock
s.setsockopt(sockt.SOL_SOCKET, socket.SO_REUSEADDR, 1)
s.bind((HOST, PORT)) #binding
s.listen() #listening
while True #accepting,  number of reqs in loop
	conn, addr = s.accept() #opensDoor
	start_new_multi_thread(conn, addr)
s.close()
```

here, the function's nature that what after a socket establishment.
simply sending and receiving data from and to the connection. The node where the data gets transmitted (the origin source of data) .a.k.a NODE. 

> [!simple]
> THE FLOW OF DATA TRANSMISSION IN AND OUT BETWEEN THE NODE AND HERE => TRANSPORT LAYER

TRANSPORT LAYER: 
1) It takes data from network layer while receiving & takes data from APPLICATION LAYER (+3layers) as whole while sending
2) Segmented Data (fragmentated data) gets referred here for the transit
3) TRANSPORT LAYER = (bridge between) Hardware Layer (physical, data link, network) + Software/App Layer (App, Presentation, Session)
4)  => The reason why fragmented data referred here, For the convenience to transmit data over network devices in ease. 
5) This layer also provides acknowledgment of successful data transmission and retransmits if any errors, fit for TCP. Ensure the data got received at each network cards/NIC. 

TLS -> Transport Layer Security (eg: SSL). When the data leave the computer, gets encrypted. IDEA HERE, is that the data has to be encrypted before it reaches the Transport layer. (TLS inbetween session and Transport)

##### **Transport Layer while SENDING:**
On the sender side, what lies before Transport Layer (a.k.a Lower Layer) => L1 Physical Layer, L6 Datalink Layer , L5 Network Layer.

1) Transport Layer receives and formatted from the upper layer(App Layer) -> Performs Segmentation + error & flow correction -> to transport layer for the network device's convenience. 
2) //HERE IN THIS LAYER, CHANGES/ADDS ALL THE DESTINATION IP's/PORT Number in the packet headers -> Forward the Segmented data to -> Network Layer (in and around the Lower/ Hardware Layer). ==//IP SPOOFING HAPPENS HERE== 
3) # Analyse all the packets in wireshark. Wireshark -> Filter an IP -> Make a query via that IP -> Sniff each packet and Analyse the to and fro. 
4) Frame -> ETH -> IP (Src, dest) -> TCP (Source reversed port, Dest Port) //all of these - Packet Headers-> HTTP (Actual Packet)
5) //SEGMENTS DATA -> PACKS -> ADDS HEADERS -> to the Network
##### **Transport Layer while RECEIVING:**
On the Receiving side, what lies after Transport Layer 
(a.k.a Upper Layer) => L7 Application Layer, L6 Presentation Layer , L5 Session Layer

1) It reads the port number from the headers and forward data to the specific destination / respective binded app
2) Also does sequencing & reassembly of segmented data
3) Functions: 
* Data Segmentation & Reassembly
* Service Point Addressing (Port redirector)
-> everything here is segments

**Two types of packets can be send over Transport Layer:** => TCP, UDP

**Services provided by Transport Layer:** 
1) Connection oriented a.k.a **TCP**
* TCP does 3way handshake, Hard to spoof
* If TCP, occurs Connection Establishment (SYN - sync ,ACK - acknowledgement, DT, Data Transfer)
* After this Data Transfer (does by default since spoofing is rare due to three-way handshake), Once it gets delivered, the connection gets terminated or disconnected. 
* TCP is more stable and reliable

2) Connection less a.k.a **UDP**
* No Acknowledgement also a one phase process. Data just gets delivered with no hassle.
* UDP is volatile.

General Explanation:
The transport layer manages the delivery and error checking of data packets. It regulates the size, sequencing, and ultimately the transfer of data between systems and hosts. One of the most common examples of the transport layer is TCP or the Transmission Control Protocol.
## **3. The Network Layer**
  ![[osi_model_network_layer_3.png]]
Data hopped out the system via transport layer.
This layer determines where it has to get laid to its destination with the help of the headers.

Purpose: 
1) It works for the transmission of data from one host to another which can also located on a different network.
2) Takes care of packet routing. (selection of shortest path between two host)   
3) Places senders and receivers IP on their headers (logical addressing)

 General Explanation:
The network layer is responsible for receiving frames from the data link layer, and delivering them to their intended destinations among based on the addresses contained inside the frame. The network layer finds the destination by using logical addresses, such as IP (internet protocol). At this layer, routers are a crucial component used to quite literally route information where it needs to go between networks.

---
## **2. The data link layer(DLL) //[refer ARP section](https://www.notion.so/Proxy-Series-Hussein-Naseer-f0eaf504a72b42f4a1da4ef48f7c2717?pvs=21)**

![[data_link_layer_osi_model.png]]
1) The main duty of a Data link layer is to ensure that packets are being delivered to the correct node in the network. Eg: I’m on a local home network. Each have their own devices where everyone uses facebook. If my frnd send me a message it’ll be recieved to me but not the other users who uses Facebook. Why the message wont get recieved to my brother or my mother. Even though have their same Public IP.

How it gets achieved is because of Data Link Layer. = Two types,  Logic Link Control + Media Access Control

2) Also, DLL is to provide an error free communication. Where to whom, the pactek properly sent or not and more lies in this layer. To achieve this, this layer uses the hosts MAC Address.

3) When a packet reaches a network node(-a modem or other network medium devices). The responsibility of the DLL is to send the packet to the right host (-clinet/computer).

Data Link layer are of two types.
1) Logical Link Control (LLC)
2) Media Access Control (MAC)
we can and will see every layer programmatically. where each data in terms of OSI Layer, has to pass through each of its layer in order to reach it’s destination also sending a data too.

To and fro of data/packets across Data link will be like:
> [!NOTE]
> 🔄 
> 
> **Sending** → NETWORK LAYER L3 → DATA LINK L2, 
>
> **Receiving**→ PHYSICAL LAYER L1 → DATA LINK L2

Packets to frames, frames to packets will happen while data transmission happens inbetween.

**Send** : when a data transmission happens, here to the context, the packets goes through Network Layer 3 → DataLink Layer L2. Such packets gets dismantled/divided into such pieces. such pieces are called frames. the breaking happens for the bottleneck of Network card’s transmission capability. when they into pieces as they move forward where the packet are eg:  10kb packet exist, (maximum transmission unit size) Transmission Unit’s max transmission limit is 1.4kb means. it has to pass through like this. Why? Packets cannot be transmitted  over the network just like that.

The frames gets divided based on the frame size of the Network Interface Card (a.k.a NIC)

> ARP plays a major role in this data link layer

***DLL & ARP (Data Link Layer L2 && ADDRESS RESOLUTION PROTOCOL)***
In this layer, which host has to receive which data is determined here.

<aside>

💡 Responsibilty of the DLL is to send the packets to the right host.

  

</aside>

Either sending or receiving the data to the right host means, the device’s MAC Address has to be known to identify to forward the data right? making such things possible under the concept of ARP. will see te roadmap of how it finds the right host.
→ DLL makes an ARP Request to the network, “Who owe this IP Address?”.
→ The network checks on it’s (if it is in a) network/host (if it an individual device) ⇒ responds the “I am with that IP. Here’s my MAC Address”. where each MAC is unique to reach its destination.

Even though devices exist (DLL in the network programmer) with OSI Layer, there are specific devices that made specifically DLL embraced. eg devices: Switches, Bridges, Hubs. They act as DLL devices. DLL is being handled by NIC. NIC’s present in devices by default but there are such devices with special cases/capabilities.

NIC has its own firmware and drivers inorder to make these DLL functionality possible. just like other components which all installed in hosts.

Roles and responsibilities of DLL:
> vv.important

**1. FRAMING**
1) Framing: A function of DLL that ensures data is being sent/received in a meaningful way. First duty → Constructs and Destructs frame to packets and vice verse.
2) eg: open a mail and show original. → shows metadata to and fro (OR) refer multipart form data frame id raw request..

[LINK](https://stackoverflow.com/questions/4238809/example-of-multipart-form-data):
```xml

#RAW
#THIS IS HOW A MULTIPART/FORM DATA LOOKS LIKE

POST / HTTP/1.1
HOST: host.example.com
Cookie: some_cookies...
Connection: Keep-Alive
Content-Type: multipart/form-data; boundary=12345
  
#BOUNDARYxFRAME
#Boundary determines where it started, where it ends
#Rips it off as frames bit by bit and sets boundaries

--12345 #eg: bit starts here
Content-Disposition: form-data; name="sometext"

some text that you wrote in your html form ...
--12345 #ends here
Content-Disposition: form-data; name="name_of_post_request" filename="filename.xyz"

content of filename.xyz that you upload in your form with input[type=file]
--12345
Content-Disposition: form-data; name="image" filename="picture_of_sunset.jpg"

content of picture_of_sunset.jpg ...
--12345--
```
 
//IDEA IS,
3) Such thing can be accomplished by adding a pattern of bits to the starting and the ending inbetween each of the frame. With the help of this pattern, it reconstructs the frames back to its original content.

**2. PHYSICAL ADDRESSING:**
2nd Duty : After framing, DLL adds the MAC Address. (a.k.a ADDRESSING AREA). At the header of the each frame. The contents above the boundary of to and fro are headers under MTU each frame (a.k.a Addressing Area). If the ARP lies here, this can be poisoned to make attacks + it can also able to affect DLL too.

**3. ERROR CONTROL:**
> DLL provides a way to resend lost frame in transit.
These are all tcp that it keeps on acknowledges that transmission gets sent or received. While transit if a frame gets lost, ERROR CONTROL comes into place.

What if a frame gets lost while transmission, there are a lot of checks to be reconciled such as CRC(cycle redundancy checks) checks, Parity checks, *digital principles and system designs ** and more. These are all ERROR Controls.

**4. FLOW CONTROL:**
The bit rate must be constant in sync on both side. A minor misalignmant can cause damage by losing frames (or) gets corrupted frames. To handle this, flow control does similar duty as bitrate control.

> Flow control controls and co-ordinates the amount of data that has been sent and received in between both nodes with respect to the acknowledgement.

If bit didnt gets sync, the data doesnt transfer properly. If an error occus in a DLL, most cause of BitSync. Such types of errors can be resolved with the use of these FLOW CONTROL.

**5. ACCESS CONTROL:**

Access controls administrates/controls one of a single for use across multiple devices in a controlled session. Even though multiple channels exist, a currently running channel cant be used by another service. Administrating such cases have been possible with the help of this Access Control. This control fashion are being occurs in a given time frame.

Simplification -> Data from cables to pass it through our appropriate network. Stuff between computer and router.

---
## **1. The physical layer (where wires takes place)**
![[osi_model_physical_layer_1.png]]
- The lowest layer of all which forms the actual physical connection between software components to the device components.
- Transfers bits (0’s and 1’s between devices
- Just 0’s and 1’s communication are being pulled from datalink layer during receiving mode where it reconstructs the frame

What is CLOCKING:
Clocking? eg: A rippling effect in a quiet pond. A device gets placed somewhere around the perimeter. Imagine that device detects how many waves of ripple that gets through them.
Equivalent to the the speed benchmarking - > 1herts PR ⇒ In the speed of one 1 hertz/ 1stone per sec.
⇒ the drop is a spectrum. it has to be processed. As same as that a data transfer. where no compromised is bearable which cause results conflicts.

This process of monitoring ripple as equal as = data transfer called ⇒ **Bit Synchronization.** A data transfer/passthrough happens only if the BIT SYNCHRONIZATION EXIST.

The data transfer/ passthrough has to happen under 1Hz of speed / 1stone per sec
 → basically balancing speed and the data transfer rate ⇒ eg: if i want to pass through a data number 13 in binary 1101 in a spectrum means 3 stone 4seconds to read it. / same can be achieved in 2 stones 1 secs. 2Hz clock. CPU CLOCKING SPEED!! based on Bit Synchronization.

Duties of a physical layer:
**1) Bit Synchronization:** (a.k.a Clock Sync) The physical layer provides a Clock based syncronization to ensure data is being sent/received at bit level without any errors.

We send a data from one place to another. In order to receive the data, the device has to be ready to receive the data AT THE SAME TIME. eg: sending a file via Bluetooth between two devices. To make a data transfer possible inbetween is that, Both the device should turn on Bluetooth, its has to be paired, both the device has to be in sync to get things in-between.

> ⇒ The ripple effect is equivaent to the electromagnetive waves around us which makes the syncronization possible. These electromagnectives can pass through via both air and wire.

> Hertz → Frequenzy. 1Hz → 1Sec, 2GHz = 2x10^6/ Bandwidth - bitrate (amount of data gets transmitted in a certain amount of time) - 1bit per sec,

**2)  Bitrate Control:** This layer provides a control on how many bits are being transmitted per second depending on the established enpoint’s least bit sync.

calculated in bandwidth. Where, Bitrate control is that ***How many bits per transition.***  (Bandwidth - bitrate (amount of data gets transmitted in a certain amount of time) - 1bit per sec,)  
Mismatched device or Configuration ⇒ assessing 4G net in a 3G Phone kind. Analogy: A powerful computer with a 1GBPS LAN → Tagret machine LAN is just 1MBPS.
Opening connection 1000Mbps < - > 10Mbps. What will be the data transfers bit rate? THE LOWEST. BIT SYNCRONIATION MISMATCHES. IF it is mismatched, the bitrate has to be toned down to the endpoint’s bitrate  //where,

> somehow our computer knows the the endpoint consist of 10mbps so the bitrate has to be 10mbps

THE SPEED AND DUPLEX CONCEPT.

**3) Network Toplogies:** The physical layer provides support for different netwoking topologies/patterns/arrangements of networked devices for star, mesh, ring etc.. This also means, it can define how devices are arranged in a network. //NETWORK TOPOLOGIES
It gets handle by the PHYSICAL LAYER of he OSI Model

**4) Transmission Mode (Sending and Receiveing Modes) :**
Three types: 1) Simplex (one way street) , 2) Half Duplex (Either Send or Receive, but one at a time like walkieTakie) , 3) Full Duplex (Simultaneously send and receive) .

Meaning: The physical layer also defines a method with which how how the data flows between two connected devices/hosts/nodes.

Simplification -> Just the cables or wireless transmission. 