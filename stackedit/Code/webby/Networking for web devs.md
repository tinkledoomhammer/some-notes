
# Networking for Web Developers
## From Ping to HTTP
### Setup/Requirements
#### Pre-requisites
1. HTTP
2. Linux Shell
3. SSH
4. Command Line
#### Setting Up
1. Udacity classes often use AWS Lightsail
2. Local linux VM
	1. The instructions use Virtual Box and Vagrant
		```
		mkdir networking
		cd networking
		vagrant init ubuntu/trusty64
		vagrant up
		```
	2. Installing Networking tools
		1. `sudo apt-get update && sudo apt-get upgrade`
		2. `sudo apt-get install netcat-openbsd tcpdump traceroute mtr`


### printf and netcat
#### printf
1. Provides formatted printing (i.e. translating /r/n to newlines)

#### netcat
1. 'swiss army knife'
2. sends and receives data over a network, protocol agnostic

`printf 'HEAD / HTTP/1.1\r\nHost: www.domain.com\r\n\r\n' | nc www.domain.com 80` 
#### Layers
|Layer|Protocols|Concepts
|--|--|--|
Application	|	HTTP, SSH	|	Urls, passwords
Transport	|	TCP, UDP	|	port numbers, sessions
Internet	|	IP	|	IP addresses, routes
Hardware	|	WIFI, Ethernet, DSL	|	signal strength, access points

|Protocol|Depends on| Layers
|------|---------|------|
DNS	|	HTTP,SSH -> UDP	|	Application, Transport
ICMP	|	TCP, UDP -> IP	|	Transport, Internet
ARP		|	IP->Wifi, ethernet	| Internet, Hardware

### Listen on a port

`nc -l 1234`
and on another console `nc localhost 1234`


* clients accessing a port that is not being listened to will get a reset packet back, which is essentially na error message

#### port numbers
1. <1024 requires admin privilege
2. `lsof` lists open files
	3. `sudo lsof -i` lists active sockets


## Lesson 2 Names and Addresses

### Intro to DNS

A record: address. Maps a name to an ipv4 address. 
* a single name can have many addresses. 
* records expire, which can lead to outages

`host` command
* sends a DNS request to the default dns server most of the time
* by default returns an a record. On my machine it also gave an IPv6 address.

`dig` command
* designed to be used with scripts
* contains lots of info including the address of the dns server
 
DNS Record Types
* `CNAME`
	:	canonical name
	* marks an alias
* `AAAA` ("quad-A")
	:	IPv6 address
	* because v6 addresses are 4x as long as v4
*	`NS`
	:	DNS name server
	* i.e. what servers have the record for that name

### Distributed with Caching
gTLD
:	global top-level domain
* resolvers usually talk to a local cache server
	* If it doesn't have an A record, then it will contact the root server for the TLD
	* It gets back an NS record which it uses to obtain an A record (possibly via additional NS records)
	* the result is stored for later use

Problems with caching
* Records have a ttl so that when they are updated all clients will get the new address in a timely fashion.
* When a record expires, the DNS server has to go back to the root server (or a name server?).

### Addressing and Networks
The address prefix (network address) is 1's in the subnet mask
* The host address is 0's
* in / notation, the # after the / is the # of 1's, or the length of the address prefix.

`ip addr show` lists interfaces, etc
`ifconfig | less`

Finding default gateway
`ip route show default`
`netstat -nr`

Private network address ranges
* `10.0.0.0/8`
* `172.16.0.0/12`
* `192.168.0.0/16`

## Protocol Layers
[https://classroom.udacity.com/courses/ud256/lessons/7118139838/concepts/71162400280923] 

### HTTP on TCP on IP

Protocol | Concepts | Where the code is | failures 
--|--|--|--|
HTTP | resources, URLs, verbs, cookies|	Flask, Apache, browsers| error codes and slow responses.
TCP| ports, sessions, stream sockets| OS kernel, system libraries| broken connections, timeouts
IP| IP addresses, packets| OS kernel, routers
Wifi| access points, WPA passwords| device drivers| network unavailable

### TCPDump
`tcpdump `
* uses pcap-filter  `man pcap-filter`
* `sudo tcpdump -n host 8.8.8.8`
	* shows more than just tcp (i.e. pings)
	* that command shows all traffic to/from 8.8.8.8
	* Ctrl-c to exit
* `sudo tcpdump -n port 53`
	* captures dns traffic
	* mine captured over 100 packets caused by `ping www.google.com`

### Sequence Diagrams
1. browser on left, server on right
2. Time starts at the top and moves down
3. arrows from the left to the right or from the right to the left indicating the direction of travel
4. Sometimes lines are slanted (with the arrow end being lower)
5. The diagram represents a single layer of the protocol
	*  i.e. http sequence diagrams don't include every TCP or IP packet

What TCP Does | How TCP Does it
-|-
Communicate between two hosts | IP Layer (addressing and routing)
multiple applications per host | port numbers
in order delivery | sequence numbers 
lossless delivery | acknowledgement and retransmission
keeping connections distinct | random starting sequence numbers

### TCP Flags

* `tcpdump` puts them in square brackets after the send and receive address
* i.e. `[S]`, `[S.]`, `[.]`, `[P.]`, and `[F.]`.
* There are 6 basic flage
	1. SYN (synchronize) `[S]` 
		* used to initiate a new session with a new initial sequence number
	2. FIN (finish) `[F]` - used to close a TCP session normally, by indicating that the sender has no more to send. The sender may still receive more data
	3. PSH (push) `[P]` - marks the end of a chunk of application data (such as an HTTP request)
	4. RST (reset) `[R]` - An error message indicating that the sender wants to abandon the session
	5. ACK (acknowledge) `[.]` - the packet acknowledges that its sender has received data. this flag is set on most packets (though not the initial SYN)
	6. URG (urgent) `[U]` - the packet contains that needs to be delivered to the application out-of-order. not used in many applications
* Three-way handshake
	* Client -> server  SYN with new randomly generated sequence number
	* server -> client SYN and ACK, with a different initial sequence number 
	* client -> server ACK
	* Also includes other information to set up the connection
* Four-way teardown
	* client -> server `[F.]` 
	* server -> client ACK to acknowledge that the client isn't sending any more payload
	* server -> client [F] 
	* client -> server ACK

## Big Networks

Congestion is the main cause of dropped packets. The solution to congestion is to reduce traffic, so retry protocols need to slow down. Also, TCP starts slow and raises speed as packets are acknowledged.

## Other nifties

[http://blog.chriszacharias.com/page-weight-matters]

[Grace Hopper's nanosecond lecture](https://www.youtube.com/watch?v=JEpsKnWZrJ8])
	
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUwMTY4ODAzMV19
-->