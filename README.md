# Netpractice
# IP address
A identifier for a computer or device on network. Every device has to have an IP address for communication purpose. <br>
IPv4 address is a 32-bit numeric address written as four numbers, separated by periods and each group of numbers separated by period is called an **octet**.
The number range in each octet is from 0 - 255 <br>
```
192.168.1.0
```
+ **Private IP address** :
  This address work within local network. It's a range of non-internet facing IP addresses used in an internal network such as in residential, office and enterprise areas. They cannot be directly contacted over the internet.
  ```
  10.0.0.0 - 10.255.255.255
  172.16.0.0 - 172.31.255.255
  192.168.0.0 - 192.168.255.255
  ```
+ **Loopback address** :
  Packets sent to this address never reach the network but looped through the network interface card only. We cannot use this IP address as available IP addresses.
  ```
  127.0.0.0 - 127.255.255.255
  ```

IP address consists of two parts
+ ```Network address``` : It's a number that's assigned to a newwork
+ ```Host address``` : It's a number that's assigned to hosts within that network

So, how to know which portion of IP address is network or host ?? Well, we will have to learn about ```Subnet mask```

# Mask
It's a number that resembles on IP address and combination of bits used to divide an IP network into smaller. The process of dividing a network into two or more networks is called **Subnetting**
The main purpose of sunnetting is to help relieve network congestion and improve network performance.

To tell which part of IP address is host or network, we will need to convert IP address and subnet mask to binary number. <br>
```
IP address 192.168.0.2   -> 11000000.10101000.00000000.00000010
Mask       255.255.255.0 -> 11111111.11111111.11111111.00000000
```
When subnet mask binary digit is a 1, it will indicate the portion of IP address that defines network and remaining is host.
# CIDR
It's a short way to write a subnet mask <br>
**/24** means 24 bits in lengh 11111111.11111111.11111111.00000000 (255.255.255.0)
```
/25 -> 255.255.255.128
/26 -> 255.255.255.192
/8 -> 255.0.0.0
```
# Subnetting
Subnetting is done by changing the default subnet maks by borrowing some of bits from host portion so that we can create more networks. More bits network portion borrows from host portion, the amount of networks can be created doubles with each bit. But also the amount of hosts per network gets cut in half with each bit. <br>
To understand more, I watch this his video explains subnetting. For example, If we used /26 as mask we will be able to get 4 networks and usable IP address for host is 62 (This subnetting table shows that host is 64 but it includes network ID and broadcast ID).
```
Subnet 1    2   4   8   16  32  64 128 256
Host  256  128  64  32  16  8   4   2   1
CIDR  /24  /25 /26 /27 /28 /29 /30 /31 /32
```
https://youtu.be/ecCuyq-Wprc?si=KCOCGAcG3onrmtnm

# To get usable IP
With given IP address and mask, we can get network ID and broadcast ID. The range between network ID and broadcast ID is usable IP addresses. There are several ways to calculate but I personally prefer his way of calculating which made me understand quicker. 
```
IP address -> 104.198.241.125
Mask       -> 255.255.255.192
```
**1. Convert this numbers to binary**<br> We actually don't have to convert every octet since the bits of mask that are 1 represent the network address. The octet of mask we only focus on is that has both 1 and 0. <br>
In this case, first 3 bytes should be same as given IP address ```104.198.241.?```All we focus on is just last octet. I normally make a chart that list powers of two to convert :)
```
IP address -> 01101000.11000110.11110001.01111101 
Mask       -> 11111111.11111111.11111111.11000000
```
**2. Bitwise AND to get network ID** <br>
If both bits are 1, result is 1
```
Network ID -> 01101000.11000110.11110001.01000000
                104   .   198  .   241  .   64
```
**3. Get broadcast ID using network ID and mask** <br>
Focus only last octet of mask, find last bits of 1 and comapre chart of powers of two. The position of last bits of 1 is same as position of 64. Then add this number to last octet of network ID and subtract 1 which is **127** (64 + 64 - 1)
```
Powers of two      -> 128 64 32 16 8 4 2 1
Last octet of Mask -> 1    1  0  0 0 0 0 0
Network ID         -> 104.198.241.64
Broadcast ID       -> 104.198.241.127
Usable ID          -> 104.198.241.65 - 104.198.241.126
```
**4. What If octet of mask has only 0 ?** <br>
We don't have to do any above steps. Just remember rest of network ID is **0** and rest of broadcast ID is **255**.
```
IP address   -> 104.198.241.125      | 104.198.241.125
Mask         -> 255.255.255.0        | 255.255.0.0
Network ID   -> 104.198.241.0        | 104.198.0.0
Broadcast ID -> 104.198.241.255      | 104.198.255.255
```

https://youtu.be/POPoAjWFkGg?si=-UQAxe1LhgeXeGc7 

# Switch
A switch connects multiple devices together in a single network. Unlike router, the switch does not have any interfaces since it only distributes packets to its local network and cannot talk directly to a network outside of its own.

# Router
The router connects multiple networks together and has an interface for each network it connects to. Since the router separates different networks, the range of possible IP addresses on one of its interfaces must not overlap with the range of its other interface.

# Routing table
![routing_table1](https://github.com/mdspro15/netpractice/assets/142498552/cc395f85-56da-4521-9fb6-9eb5c799d89f)

The routing table is a data table stored in a router or a network host that lists the router to paticular network destination. In NetPractice, the routing table consists of 2 elements
+ Destination: The destination specifies a network address on which a host is the end target of the packets. The router of defalt or 0.0.0.0/0 is the route that takes effect when no other route is avaiable for an IP destination address. The default route will use the next-hop address to send packets on their way without giving a specific destination. The default route will match any network.
+ Next hop: It refers to the next closest router a packet can go through. It is IP address of the next router.
