1) Webservers and services for components like sockets and more
2) What type of networking tool that can be created
3) Capabilities of such tool

- **+SOCKETS (toggle)**

    **Orientation overview**:
    Sockets in terms of security and several purposes. Plus. we,ll create a monitoring software for status and more.

    **Objective:**
    Covering Sockets, RMQ, DevOps + Live development of such monitoring tool + Wieguard VPN development

    **Resource Monitoring tool:**
    → Transit data in and out of the resources without HTTP/AJAX
    → Such tool that we’re gonna program has to run as a system service to GET the Usage stats of resources such as CPU, RAM, NetIO, DiskIO per resource and to keep it updated in a dashboard using JS&PHP (dashboard)

    Idea behind this:
    - Run program as a system service
    - Monitoring threading, usage, stats, DDoS, Security
    - + Scenario stats, Power supply, outage monitoring, seamless service run even after outages
    - Make thins to run amidst several outage, disruption and downtime
    - Establishing such infra + protection, without external components such as configurations, tools, third party softs and managed services but by programming..
    - Not a calk walk but still it has to be precise/stable kind of a difficult task :) will see the best practices still..
    eg: IoT and Edge devices monitoring the ppsuh, usage and stats like that..

    **Understanding Services:**
    a Service is a program that runs always & provide some info for other programs eg: DB, WebServer. Interdependent + has to autorun.

    eg software architecture of the monitoring tool..

    Architecture:
    ⇒ Composition of different services that make a whole system/app possible

    > Publish/stream data via UDP with Sockets 1, 2, 3, 4  and more for each component ⇒ Message Broker/Queuer/MQ Service (RabbitMQ, Kafka etc) ⇒ Web Sockets (PubSub with alarms) ⇒ Visualize data in dashboard.

    <aside>

    💡 This project is a combination of Web Development + Python Backend + Message Queuing Setup

    </aside>

    AJAX vs Socket:
    AJAX → Client <=> Server ⇒ Request response; TCP, using ajax request in a loop duppySocket(not a real socket) /receives response only if a request been made. Action/Event driven

    Socket → Pub → subscribe → Server ⇒ pubsub Model datastream; UDP, Actual Socket. It establishes communtication and throws data.

    **SETTING UP AN OWN INSTANCE OF A MESSAGE QUEUE/BROKER SERVER**
    +its capabilities and the idea behind bare sockets, Actual Publisher Subscriber model…

    Basics - Sockets + Will continue the same further…
    …YET TO DOCUMENT…
    Webservers/Services and Components in OSI

**Sockets structure with Networking** (inbound, outbound)

   ⇒ FOUR COMMON STEPS OF OPENING A SOCKET COMMS

   *How a socket connection gets established, (socket is a combo of IP Addr+ Port)**
    1) Does a connection
    2) Binding, in brief, Functions in Programming sockets such as,
    - Bind
    - Socket
    - Listen
    - Accept
    eg: an app that can contains a small server which can listen incoming comms.

   1) introduce a **socket()** function 
   2) →  **bind()**, a process /function helps binding an interface)
   3) →  **listen()**, listens where we program it to. 
   4) → **accept(),** accepts incoming connections.
   5) → based on that, it either **Send/Receive.**
    Structure of sock: //Sokcet is a system API provided by the OS itself.

   ### INCOMING/INBOUND CONNECTIONS
    SERVER
> [! SERVER]
>     ℹ️ 
>     
>     ***socket(0.0.0.0, 9999)** //ip, port
>     
>      ***s.bind()**
> >     //wifi, 0.0.0.0, if someone tries to bind 9999, gets kicked out throws error
> >     //talks to the adapter, obtains and retricts future processes with 9999. throw error.
> >     //bind, establishing a way, like a tunnel.
> >     //if an incoming request comes via 9999, knows that it has to go here.
> >      //if no listen is configured, will throw error for the request/user gets pissed.
> 
>     ***s.listen()**
> >     //listens received requests/also like a doorway where i can able to either accept or refuse
> >  ***c=s.accept() / s.deny()**  
//accept or deny connection
***c.send() / s.receive()**
 //send or receive.

   //related to OS regardless of Programming langauges
   //applies for both inbound and outbound

### OUTGOING/OUTBOUND CONNECTIONS
CLIENT
> [! CLIENT]
>     ℹ️ 
>     
>     ***s.socket(domain/ip, port)**    
>     
>         ***s.connect**
>         
    ***send/receive()**


//COMMON TO ALL OPERATING SYSTEMS
⇒ TO GET ACCEPTED BY SUCH REQUEST, GOES TO SERVER’S STRUCTURE.
//KINDS OF SOCKES CAN BE MADE WITH SUCH STRUCTURE - ANY! (RAW, Datagram, Stream and Sequenced)

- **CREATING A WEB SERVER USING SOCKET PROGRAMMMING:**
    Objective: Theory → Practical, no deEp dive + establishing a socket using python.

    Table of contents:
    1) Sockets in Python
    2) Practical approach for previous contentsv#day1AdvSocks

    Set of questions: b4 moving forward with python..
    1. What is Sockets?
    2. Who is offering it?
    3. How API gets the job done? →

    API → there will be some components that we didn’t create but utilize it!

    eg: printf in C, is an IO API. most of them are API’s that as long as that i didnt create it but utilize it. scanf, http api, source code sdk api’s, Source code header file module API’s. The service that doesnt belong to us but a third party service, home service or someother player but to be able to access the same is possible via API.

    WHO PROVIDES THE SOCKETS HERE THEN, the Socket as delivered by the OPERATING SYSTEM. a stream of communication. iostream, flow of information. meanwhile, a socket is a stream too.
    ##THEORITICAL EYE CONCLUDED
    -x -x - x-

    ### PROGRAMMATIC EYE:

    Socket → a way for two nodes to talk to each other. node → a network component. SHOULD KNOW, WHO ARE THESE TWO NODES?
>     ⇒ SERVER and CLIENT. 
>     both are computer 🙂, what decides that which is which (server or client).

   QUE: Since both are systems, what is that thin line/what determines either its a server or client? 
   ANS:  The origin and the direction of the request. (usually the origin makes the request is going to be CLIENT, and the opposite is SERVER)

###### Where gonna see how it is in the case of Sockets:
like how a socket will be if it is server and the same for client. //listening is making the difference here.
In sockets POV, 

| Server | Client |
| ---- | ---- |
| It has to listen (s.listen) | It just connects to the server |
| **config/implementation** | **config/implementation** |
| .py: import sockets |  |
| funcs(): socket, bind, listen, accept, send/receive, close | funcs(): socket, connect, send/receive, close |
in the above table, ==socket+bind = listen (both comibines and list for a req or connect) === connect() -> ==listen() -> accepts()== -> send or receive <=> ==send or receive== (LOOP & CLOSE)

##### SERVER CONFIGURATION FOR PUBLIC INTERNET ACCESS TO ACCEPT SOCKS REQUEST
1) Assess router IP
2) Edit and port forward 3074 to the endpoint server. 
> 3) nc -l 3074 (one connect //costs one thread)
> 4) check connection via nc or telnet ip 3074

+
>Create repo + git => manage project.

###### For Single Request, 
BASE TEMPLATE (ONE THREAD)
```python
import socket as s

HOST= 'localhost' #OR 0.0.0.0 
PORT = 3074 

sck = s.socket(s.AF_INET, s.SOCK_STREAM)
sck = setsockopt(s.SOL_SOCKET, s.SO_REUSEADDR, 1)
sck.bind((HOST,PORT)) 
sck.listen()
Print("Waiting to connect...")
conn, addr = sck.accept() 
print("Connected {}:{}".format(addr[0],addr[1])) 
conn.sendall("Welcome!".encode())
print("Waiting for incoming message")
data = con.recv(1024)
while data:
 print(data.decode(), end="")
 data = conn.recv(1024)
```

Docs:
```python 
#sample_server.py
#SERVER

import socket as s #importing socket lib in python

HOST= 'localhost' #host, where to run, (local='127.0.0.1')
#HOST is to declare from here you want to receive requests from 
#HOST = '0.0.0.0' #CONFIGURE PORT FORWARDING for internet traffic to come in
PORT = 3074 #any forwarded port

sck = s.socket(s.AF_INET, s.SOCK_STREAM) #1) socket() initiation
#declaring the type of sock #socket takes two parameter = address family + sock #s.AF -> gives available addresses, +INET(aka IPv4) S.Sock_stream(Stream Socket.
sck = setsockopt(s.SOL_SOCKET, s.SO_REUSEADDR, 1) #socketConfig
#socketOptions=configSettings, #reuses addresses for multiple continous processes.
sck.bind((HOST,PORT)) #2 BIND() as a tuple, binds the host and port
sck.listen() #listen, listens requests from binded hostPort
Print("Waiting to connect...")
#sck.accept() #accept, accepts the listened requests from queues
conn, addr = sck.accept() #configuring accept()to handle one single communication + (tuple unpacking)to receive Connection's conn=obj, add=whoisIP 
print("Connected with {}:{}".format(addr[0],addr[1])) #connection's IP & Rev port
conn.sendall("Welcome!".encode())
print("Waiting for incoming message")
data = con.recv(1024)
while data:
 print(data.decode(), end="") #bytes to format decode(such as ASCII, utf-8)
 data = conn.recv(1024) #1024 - bufferLength, how many characters can be taken
 
```
RUN IT! run once, not again. ONE THREAD for a bind. 
###### For Multithreads, 

BASE TEMPLATE,
```python
import socket as s
from threading import Thread 

HOST= 'localhost' #OR 0.0.0.0 
PORT = 3074 

class Multithreading(Thread) #any names can be declared
	def __init__(self, conn, addr): #constructor
		self.conn = conn
		self.addr = addr
		
	def run(self):
		self.conn.sendall("Welcome",encode())
		data = conn.recv(1024)
		while data:
			print(data.decode(),end="")
			data = con.recv(1024)

sck = s.socket(s.AF_INET, s.SOCK_STREAM)
sck = setsockopt(s.SOL_SOCKET, s.SO_REUSEADDR, 1)
sck.bind((HOST,PORT)) 
sck.listen()
Print("Waiting to connect...")

conn, addr = sck.accept() 
print("Connected {}:{}".format(addr[0],addr[1])) 
conn.sendall("Welcome!".encode())
print("Waiting for incoming message")
data = con.recv(1024)
while data:
 print(data.decode(), end="")
 data = conn.recv(1024)
```

Docs: (a server, CONFIGURING THE ACCEPT to accept multiple connections)
> BOTTLENECK CO-RELATION that, one the Accept loop is that, Either Accepts Multiple connections or Multiple messages from One single accept connection.> 
> => Standards doesn't fit under regular programming. Sequential programming without threading. support one instruction at a time, but we need to do this in multiples. We couldnt do that in one function

For that we gotta write a seperate CLASS implementing a thread. 
```python 
#sample_server.py
#SERVER

import socket as s #importing socket lib in python
from threading import Thread #for implementing thread

HOST= 'localhost' #host, where to run, (local='127.0.0.1')
#HOST is to declare from here you want to receive requests from 
#HOST = '0.0.0.0' #CONFIGURE PORT FORWARDING for internet traffic to come in
PORT = 3074 #any forwarded port

class Multithreading(Thread) #any names can be declared
	def __init__(self, conn, addr): #constructor
		Thread._init__(self)#initiating thread by calling parent
		self.conn = conn 
		self.addr = addr
		print("Thread{}:{} started...".format(addr[0], addr[1])) 
		
	def run(self):
		self.conn.sendall("Welcome"\n,encode())
		data = conn.recv(1024)
		while data:
			print("(): ", format(self.addr[0]) + data.decode(),end="")
			data = con.recv(1024)

sck = s.socket(s.AF_INET, s.SOCK_STREAM) #1) socket() initiation
#declaring the type of sock #socket takes two parameter = address family + sock #s.AF -> gives available addresses, +INET(aka IPv4) S.Sock_stream(Stream Socket.
sck = setsockopt(s.SOL_SOCKET, s.SO_REUSEADDR, 1) #socketConfig
#socketOptions=configSettings, #reuses addresses for multiple continous processes.
sck.bind((HOST,PORT)) #2 BIND() as a tuple, binds the host and port
sck.listen() #listen, listens requests from binded hostPort
Print("Waiting to connect...")

#MATTER STARTS HERE.
#Accept has to start multiple times.
#sck.accept() #accept, accepts the listened requests from queues
#Looping the whole accept block

While True:
	#configuring accept()to handle one single communication + (tuple unpacking)to receive Connection's conn=obj, add=whoisIP 
	print("Connected with {}:{}".format(addr[0],addr[1])) 
	conn, addr = sck.accept() 
	#connection's IP & Rev port
	print("From {}:{}".format(Addr[0]), addr[1]))
	Multithreaed(conn, addr).start()
```
 




before moving on, will cover
###### SEQUENTIAL VS MULTITHREADED PROGRAMMING
in the above connect, nc -l 3074 is a connect + also ==a thread==, in a process pov.
IF, another +! connection has to established means costs ==another thread.==

> [!One connection = One thread]

where, rest of the connection will be refused or couldnt able to connect. one connection one thread at a time. Which means, the connection supported only one single thread each **No multithreads able to perform there**. cause of that, couldnt able to connect multiple clients. 


MOVING FURTHER:
	 Prerequisites:
		1. Socks
		2. Http
		3. Dos + ddos
		4. .py
		5. security practices and understanding
		6. Git
		7. Multiprocessing and multi threading
		8. Cross/Counter thinking








