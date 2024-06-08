# Netpractice
# IP address
A identifier for a computer or device on network. Every device has to have an IP address for communication purpose. <br>
IPv4 address is a 32-bit numeric address written as four numbers, separated by periods and each group of numbers separated by period is called an **octet**.
The number range in each octet is from 0 - 255 <br>

IP address consists of two parts
+ ```Network address``` : It's a number that's assigned to a newwork
+ ```Host address``` : It's a number that's assigned to hosts within that network

So, how to know which portion of IP address is network or host ?? Well, we will have to learn about ```Subnet mask```

# Subnet mask
It's a number that resembles on IP address and combination of bits used to divide an IP network into smaller. The process of dividing a network into two or more networks is called **Subnetting**
The main purpose of sunnetting is to help relieve network congestion and improve network performance.

To tell which part of IP address is host or network, we will need to convert IP address and subnet mask to binary number. <br>
```
IP address 192.168.0.2   -> 11000000.10101000.00000000.00000010
Mask       255.255.255.0 -> 11111111.11111111.11111111.00000000
```
