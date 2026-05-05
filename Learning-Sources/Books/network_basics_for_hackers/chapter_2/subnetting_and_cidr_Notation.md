
Understanding subnetting is a crucial and fundamental skill for a network engineer or network security engineer. We have a useful program called subnet calculator. We can find a lot of them easily online. We will dig into subnet in this chapter.

### Why Subnetting?

Imagine we have a big city (network) with a lot of streets (addresses) and apartments (ports). If we want to manage the whole city together, it's a hard task. But if we make small parts and districts, we can manage our big city much faster and better. This is exactly what subnetting is doing!

Remember we said that IPv4 IP address is 32 bits?! Network administrators can use this 32 bits range more efficiently by subnetting. They can create subnets in all classes. Subnetting means dividing your range into more small networks. So, you can have more host numbers in your network.

Each IPv4 is made of two parts. First, Network address (or network ID) and second part is Host address. Network address is a unique address that is for the network. For example, 192.168.1.0. The Host address or host ID, is the address given to each device on that network. For example in our network we have 3 devices connected, so we have 192.168.1.1, 192.168.1.2, and 192.168.1.3.

We have 3 classes of common network.

Class A:
- Start address: 0.0.0.0           End address: 127.255.255.255
- Size of Network Number Bit field: 8 bits
- Number of Networks: 128 (2^7) --- 16,777,216 (2^24) available addresses
- Leading Bits: 0 (if we write the whole address in binary, it starts with 0)
- Size of Rest Bit field: 24 (from 32-bit address, we have 8 bits for unique network address and 24 bits free for working with! 32 – 8 = 24)

Class B:
- Start address: 128.0.0.0    End address: 191.255.255.255
- Size of Network Number Bit field: 16
- Number of Networks: 16,384 (2^14) --- 65,536 (2^16) available addresses
- Leading Bits: 10 (if we write the whole address in binary, it starts with 10)
- Size of Rest Bit field: 16 (from 32-bit address, we have 16 bits for unique network address and 16 bits free for working with! 32 – 16 = 16)

Class C:
- Start address: 192.0.0.0    End address: 223.255.255.255
- Size of Network Number Bit field: 24
- Number of networks: 2,097,152 (2^21) --- 256 (2^8) available addresses
- Leading Bits: 110 (if we write the whole address in binary, it starts with 110)
- Size of Rest Bit field: 8 (from 32-bit address, we have 24 bits for unique network address and 8 bits free for working with! 32 – 24 = 8)

### Subnet

When we have a network within another network, it is called subnet. Just like primary classes that we have for networks, we have them as well for subnets. We can create a subnet by borrowing one bit or more from host ID to create more network IDs. Subnet for classes are like this:

Class A:
    255     .    0    .    0    .    0
    Network     host     host     host

Class B:
    255     .    255    .    0    .    0
    Network     Network     host     host

Class C:
    255     .    255    .    255    .    0
    Network     network     network     host

### Subnet Mask

Imagine that we have an IP address like this: 192.168.1.0. This is a Class C address, so the default subnet mask is 255.255.255.0. In binary format, the subnet mask is:

11111111.11111111.11111111.00000000

In this case, I have one network with 254 hosts (two addresses are reserved for network and broadcast. So, 256 – 2 = 254).

If we want to create two networks, we need to borrow one bit from the host portion. It will be like this:

11111111.11111111.11111111.00000000  (255.255.255.0)  
→ 1 network with 254 hosts

We borrow the one bit from the left side of the host portion:

11111111.11111111.11111111.10000000  (255.255.255.128)  
→ 2 networks with 126 hosts each

If we continue:

11111111.11111111.11111111.11000000  (255.255.255.192)  
→ 4 networks with 62 hosts each

11111111.11111111.11111111.11100000  (255.255.255.224)  
→ 8 networks with 30 hosts each

11111111.11111111.11111111.11110000  (255.255.255.240)  
→ 16 networks with 14 hosts each

**Note:** This is very important. The practical limit of doing this is 64 networks and 2 hosts: 255.255.255.252 – 11111111.11111111.11111111.11111100  
If we continue to borrow another 1 from only 2 left bits, we will have 128 networks with 0 usable hosts (unless using modern standards for point-to-point links).
[[subnetting_and_cidr_Notation]]


### CIDR Notation

CIDR (Classless Inter-Domain Routing) Notation is a way to talk and show subnetting. It appears with a slash (/) after the IP address. Look at this example:

192.168.1.0         IP address  
255.255.255.0       subnet mask  
11111111.11111111.11111111.00000000  
192.168.1.0/24      IP and subnet together

This 24 (the number after the IP address in CIDR Notation) represents the total number of ones in the binary format of the subnet.

Another example:  

58.0.0.0/8  
What can you understand?  


First, it's a Class A address.  
Next, that 8 means we have the binary of subnet mask like this: 11111111.00000000.00000000.00000000 or 255.0.0.0 (dotted decimal format)  
We can understand that it's a Class A subnet mask as well.
