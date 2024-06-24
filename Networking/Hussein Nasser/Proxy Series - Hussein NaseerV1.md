# Proxy Series - Hussein Naseer
"Proxy Series" is a book written by Hussein Naseer. It delves into the world of computer networking and covers the different types of proxies that are used in the modern digital world. The book also provides a deep understanding of how proxies work, their various types, and their applications, making it a valuable resource for anyone interested in the field of computer networking.
# **Proxy vs Reverse Proxy Server Explained**

**Proxy :** A proxy is a software that makes request on behalf of a client to the server. A network or request  that need to go as an outbound has to go through via proxy first.
**Reverse Proxy :**
A reverse proxy operates on the server-side, receiving client requests and sending them to the appropriate backend server. It can provide various benefits such as load balancing, increased security, and SSL encryption.
# **Other Proxies**
**Forward Proxy :**
Just like a standard proxy, a forward proxy acts on behalf of clients to fetch and return information from a server. However, it's typically used within an internal network to control outbound traffic and provide internet access to clients who don't have a direct connection.
**Open Proxy :**
An open proxy is a type of forward proxy that can be accessed by any internet user. It is often used to anonymize web browsing and bypass content restrictions, but it also poses significant security risks if not properly managed.
**Transparent Proxy :**
A transparent proxy intercepts and redirects client requests without modifying them. While it's commonly used for internet access control in businesses and schools, it can also improve caching efficiency and load balancing.
**SOCKS Proxy :**
A SOCKS proxy operates at a lower level than a standard proxy, capable of handling any type of traffic, not just web-based traffic. It is often used for activities like gaming or torrenting where the proxy needs to handle more than just HTTP/S requests.
**FTP Proxy:**
The FTP proxy functions as **a relay for the File Transfer Protocol to enable you control connections based upon source and destination addresses and user authentication**. It can also limit access to certain file transfer commands, such as put and get , based on source or destination addresses and user authentication.
**HTTP Proxy:**
The HTTPS proxy (also called SSL Proxy) works similarly to the HTTP proxy but differs in that it **establishes secure connections**. The HTTPS proxy works exclusively with web content and cannot be used for any other data types. HTTPS proxies encrypt all web traffic using the HTTPS protocol.

---
# **Proxy vs. Reverse Proxy (Explained by Example)**
Differences between the two and the benefits/advantages and disadvantages.
### Proxy -
A server that hides the client’s identity.
Eg:
Client → Traffic gets routed/ ~~ConnectionVia~~  > Proxy (Front facer makes request on behalf) → http://80→ Server (Destination where it doesnt know who the client is)

Server → ~~respondBack~~ → Proxy → (localRouteTable where the request has to be route to theset of clients) →  Client.
### Benefits/Advantages of a Proxy
- Anonymity - Server doesnt know the client
- Caching (for performace and SRE) - Cache contents locally and serve it for multiple clients instead of pulling all the files again to server xontent
- Block unwanted sites - ISP Proxies
- Geofencing - Specific contents to specific clients fencing content inbetween
### Reverse Proxy
  Reverse proxy is the client doesnt know which specific server that it gets connected to. Reverse proxy is a complete internal process where the client has to idea about the process of getting connected between racks of multiple servers.
  
  Reverse proxy (The IP of the reverse proxy) will diagnose the request to a server/service/ application to validate where to connect across multiple servers where the usecases generally be the availability, space of the server, traffic, even to another port and circle back the response.
Eg:
Client → http:80 → Reverse Proxy → ~~redirects to one of many servers after some validation~~ > Server (destination one good server) → Circle back to reverse proxy and to the client.

### Benefits/Advantages of a Reverse Proxy
- Load Balancing - Does Robin round algorithm to distribute the traffic inbetween the services
- Caching - store and deliver cache contents
- Isolating Internal trafic - a blockade between the open internet and the internal network where you can expose ports of the server in the internal DMZ network where reverse proxy defends the same.
- Logging - Information about the status of the servers
- Canary Deployment - Imposter servers where it serve different mode of content to a certain amount to traffic/request
---
# **VPN vs. Proxy Explained**
And its applications, pros and cons. Both make requests on behalf of the client but in different ways
### Synopsis:
How VPN works?
VPN pros and cons?
vice versa for Proxy
summary
### How Normal networks works:
before that, will see how **normal setup** works.
	Two different networks → Front facing router → each has its own **public IP** → each of the clients got distributed by DHCP with each having its own ip’s as **privateIP**’s in chronological order.

WHERE, one of a client makes a request → 128.x.x.4 -> GET/google.com  → (searches and couldnt find the request in local mesh and redirects to subnet) → Router (NAT translates Private IP to its Public IP) → Google Server .

*How NAT works?*
*Network Address Translator ⇒   Many applications 1) NAT processes mapping an IP address to an anothor IP address. Introducted to solve the problem of limited number of IP’ in the world.*

agenda 1) tcp connection, 2) Nat Works? 3) NAT’s Application such as Private to Publi IP translation, 3) L4 Load Balancing
//you can make other request such as ping internal and external IP’s.
### How VPN networks works:
a virtual private network where it connects you in the format of a tunnel to the client & server packets over a public network into the private connection. to ping flexible to the thing that gets assinged to the VPN.

![[Untitled 1.png]]
Anything includes Ping, Web Traffic, Database Connection,
### Pros of VPN:
- Encrypts traffic - encrypts whatever you visit on the internet even non encrypted website
- Redirects all the traffic at the lowest level (L2) - Wont stuck and gets blockade
- Access restricted content - as it speaks
- Access Private Network/works - eg: SNA
### Cons of VPN:
- Anonymity(meh) - Hoax, can know who you are 🙂
- Slow - hops to get connected
- Double encryption, slows again
- VPN can log data
- No cache
---
## Proxy:
What it is and how it works!?

Pros:
- Cache (pure HTTP), Helps the perfomance factor
- Works in L4 and L7,
- Hoax Anonimity
- Blocking Websites (in ISP Transperent L4 Proxy)
- Recent usecases in application networking (load balancingm service mesh, varnish http accelerators, firewall proxy security)
  
Cons:
- Applications can bypass proxies
- Not all traffic is routed (HTTP, HTTPS, SOCKS)
- No encrption by default
- HTTPS >>>> More dangerous than VPN
---
# **Load Balancer vs Reverse Proxy**
**(Explained by Example)**
difference, usecases examples and more

Reverse Proxy is a service/feature/server that receives client’s request and hides the backend infrastructure of the server’s outgoing request. Adding this reverse proxy layers that adds a lot of advantages instead of having the client connect directly to the application’s server outrightly.

This middle layer adds an advantage that client doesnt know which server it is connecting to.

## Benefits and application of reverse proxy:

What i can do:
- **Caching (web performance and acceleration)**  - *First fetch to the server, contain obselete data, serves back to re-requests instead of doing the whole fetch consuming bandwidth and bothering all the resources which cost you tons.*
- **Canary Deployment** - *Experiment different app’s version and serve some amount of content to little amount of traffic.*
- **Security against external traffic** - *Security measures to the fore-front facing resources between the proxy server and to the client plus the Internet itself. Internal and isolated network where Skipping unneccesary expensive security measures for each like SSL, TLS handshake and more. like DMZ.*
- **Isolation** - *Isolating Reverse proxy server as the front facing and internal servers as a private isolated network*
- **Single entry URL** - *where the internal servers can run and expose on different port and the reverse proxy can be the saviour of handling all the stuff as single entry URL.*
- **Load Balancing** - *Making the reverse proxy as a load balancer balancing and distributing requests equally is possible by implementing a little amount of work/research and modification in the reverse proxy server. LB is an instance of the reverse proxy.*

*Load balancer demands more than two servers must, where reverse proxies can be flexible working with or without multiple servers. Thing about Load balancing is that the middleware has to store some meta-data about the app/servers.*

**Static LB** - *like Round robin where distributes each server one by one without question*
**Smart LB** - *communication about the status of the server, load, traffic, processing this many requests, one is free and distribute the same accordingly by validating those metadata.*

---
# L4 vs L7 Load Balancers

Load balancing is a process of balancing incoming requests distributing traffic to multiple machines/servers/services/backend. LB’s are nothing but a proxy but reversed. Covering Two types of LB: Layer 4 and Layer 7, L4 nad L7 LB’s overview, Pros and Cons and the implementation of L4 and L7 LB.

Agenda:
- L4 and L7
- Load Balancer Overview
- L4 LB Pros and Cons
- L7 LB Pros and Cons

**L4: Transport Layer:** *Higher level transmission procotols such as TCP, UDP and all the PORTS lies here. L3 and L4 mostly intertwined and together working with the IP’s.*

**L7: Appliaction Layer:** *Where our application works such as HTTP, FTP, SMTP, Cookies, Content type, Headers, all the text, Content of the protocols, data transmission, GET/POST requests and assess all the converational data and stuff*. *where in L4 we only see ports and IP addresses*
## **What is a Load Balancer?**
(a.k.a Fault toleranct software) essenially it a software which you host somewhere like in a system, container, VM, machine, service or such where the client connects to the LOAD BALANCER and it decides and distributes where to forward the traffic/requests to multiple machines based on various metrics or critirea. Thats essentially what LB is. (you dont want to put all the requests to one machine to make it run out of TCP connection and socks and to let it die.)

### **L4 LOAD BALANCER:**
*is the Load Balancer where you know only the IP’s and the ports. These are the only pieces you know to play around or handle with.*

L4 Load balancer looks at the IP’s and Ports then makes decision based on that, not the data which that comes along with it. These methodologies such as round robin, least connection and such algorithms which makes decision based on just IP’s and Ports.

With such methodologies, where for an example..
Clients(ip:44.1.1.1) —> LB (44.2.2.2) < s1 (44.3.3.3) x s2 (44.4.4.4)
a very simple TCP request from IP:44.1.1.1 reaches by holding (44.1.1.1 + Data → 44.2.2.2) to the
→  LB 44.2.2.2 (*where it just ackowledge that where the request is coming from regardless of what amount/type of data that comes with it. sees just the address*) With the IP, it acknowledges the request and Makes the decision to forward the destination IP to IP using NAT. where NAT translates to destination to forward to balance to serve the request to the destination by translating the requested IP to the relevant server’s IP (also like a proxy) (NAT = 44.1.1.1 → 44.3.3.3) → receives the load from LB which is translated by NAT where the server thinks that it comes from the Client but it is from the LB.
*how this NAT work by translating request IP to an another IP.*
## **Network Address Translation - NAT Explained**
NAT is a process of mapping an an IP address to an another IP (or) another IP address PORT pair to another IP address port pair for various purposes.
1)  this concept got introduced to get rid of the IPv4 addresses limitation problem to forward its own ip’s to the internal network establishment.
2) Helps in Load balancing area too where to translates IP to IP’s

NAT:

1. Understanding TCP Connection
⇒ sys1-mac: AAA (192.168.1.2)  < - > router-mac: BBB(192.168.1.1) < - > target sys2-mac: CCC(192.168.1.4)

client makes a req ⇒ 8992:192.168.1.2 GET/192.168.1.4:8080 (arp is a protocol that maps IP address to a MAC address) → ROUTER (gets the request, does two jobs. 1) verfies subnet and if exists within network, itll forward to the destIP, 2) ifnot, pings the ISP and to internet to route to the public net). here it request get forwarded within the subnet →   internal target consist of nodeApp,

2. How NAT Works
3. Applications of NAT
- Public to Private IP Translation
- Port forwarding
- L4 Load balancing

- ARP?

    1) why do we need it? - google.com → (a GET request which is L7, an http). Every move we make derives down to the lower level to go through via cables, where in L2 to identify and address a device as MAC) (every frame tha happwning consist of a source nd destination MAC Address) **these two addresses are not really human readable/addressable but for machines it does. we dont get out hands on those **.
    2) always the IP but not the MAC, why?? → we dont/never send requests refering a mac addresses but the IP. communication happens like, this is my IP ping this IP address not the MAC.  Requests be like ⇒ GET/10.0.0.03:8080 → webServer’sIP (//here, the network translates to a frame → that frame have the IP address of the source and the deatination IP address and the port → that derives to the lower lower level we put the MAC and there is MAC.
    3) Address resolution Protocol: ARP Table is cached IP → MAC Mapping ⇒ What theyv’ve been saying is.. the structure  of a GET REQUEST is that:
    WHAT HAPPENS HERE IS ⇒ If we know the IP address but not the MAC. We need a method
> [!NOTE]
> 
>     📊 [ Client’sMACAddress ]   [ Client’sIP ]   GET/    [ ServerIP ]   [ ~~ServerMACunknown~~ ??]
> 

    here, the client does know the IP but not the mac where there can be a system with the sam. To identify such cases, they use MAC. Here in the GET request, No request does know the MAC of any servers. Where if a request made, it’ll also does an ARP request to fetch the destination’s MAC.

    How do we achieve getting the MAC - Method using ARPTable MAC Mapping -  Fetching each of these MAC address is costly, where LB’s or Proxies caches the MAC addresses in the ARP Table to the resprective IP Address and process the request a lot sooner instead of digging everything from scratch. That is ARP Table is caches IP and does the MAC Mapping

    Network Frame and ARP Table?
    eg of how it looks like.
    Machine1: Client1,IP10.0.0.2,MAC:xxx    
    Machine2: Sever2,IP10.0.0.2,MAC:xxx  
#table
   | clientMAC | clientIP | sourcePort | GET/ | destPort | ServerIP | ServerMAC |

   eg1:
    
   | ab:32:5g:xx.x0] | 10.0.0.2 | 2312 | GET/ | 8080 | 10.0.0.3 |  |

   which obviously gets derived to a compiled portion,
   [ab][2]  GET/  [3][bb] //portsIrrelevantHere
    /this is how a simple frame looks like

    Here, by making a connection. We dont know the MAC of the Server. How do we find the MAC of the server.
   
![[Untitled.png]]

    like the above given diagram, the each machine has its own IP & MAC,

   eg1 working in a local network: an IP2 machine like to ping IP5 machine → doesnt know the ARP → makes the ping but no ARP so makes an ARP Request → get it from the ARP Table which is cached from the router within subnet→ Request Made.

   eg2 working in a public outbound network:

   google.com → surfs the same across the same, one the request validates nothing is here → Outside my subnet → router changes the subnets to Gateway → sneds packet to throught the gatewaty → Does ARP request to get the gateway’s ARP and IP just like the same → Does a NAT and proxy the same → makes the request o google.

> 📢 Glimpse of **Address Resolution Protocol - ARP.** is a protocol that maps IP address to a MAC address making address the HOST. Will see why **ARP**, **Network frame** of an ARP with the **examples**. (Will see how a usual HTTP GET request funnels down all the way down to ARP.
> 
  
<toggle above. If we dig deeper about the same, there are Vitrual IP Addresses, ARP Proxying and more>
### How NAT Works?
# L4 vs L7 Reverse Proxying
Proxying layers, such as
L4 Proxying - Packet, Network or Stream level proxying
L7 proxying - Application Layer proxying

Different proxying options such as L7 as HTTP, L4 as TCP in HAProxy where it works like modes; same L7 as HTTP, L4 proxying/TCP as stream in NGinx where it works by writing a cfg/configuration file.
### L7 Proxying
Usecase: 1 client, 1reverse proxy server and 2 backend web server(s1, s2).
Client → ~~tcp connection~~ → RProxy