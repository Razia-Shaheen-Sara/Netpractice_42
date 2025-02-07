# How to Solve Netpractice?

1. **Download**, extract, and open the `index.html` file using your **Intra name**.  
2. If the **check button** does not appear, try opening it in a **different browser**.  

## Level 1, 2, 3

- Each PC has two numbers: an **IP address** and a **subnet mask**.  Delete all editable numbers.
- PCs with the **same mask** belong to the **same network** and can communicate with each other.  
- However, for communication to work, their **IP addresses should be almost identical**, except for the **last digit**.  
- subtract 1 or (in case of failure, adding 1) from the last digit
- If there is only a switch in between computers/hosts, it does not affect any IP or mask


## Level 4: IP Blocks and Usable IPs

-**Router** will affect IPs. Routers usually have different IPs and subnet masks on each interface to connect to multiple networks or subnets. 
- Hosts connected to the same interface of a router will typically have the same subnet mask and IP addresses in the same range as the router’s interface./
- So make the IPs close enough in one side of the router and make the masks same as that side of router

IP addresses come in groups called **blocks**. Each block has:
- **Network Address** = First IP in the block (**not usable**).
- **Broadcast Address** = Last IP in the block (**not usable**).
- **Usable IPs** = Everything between **Network Address +1** and **Broadcast Address -1**.

### CIDR Notation
- CIDR notation looks like **`IP_address/number`** (e.g., 192.168.1.0/27).
- **Subnet Mask** example:
  - **255.255.255.0** → First 3 parts are for the network, the rest is for devices.
  - **255.255.0.0** → First 2 parts are for the network, the rest is for devices.

### Examples: IP Ranges for /24, /27, and /30

The **number of usable IP addresses** depends on the subnet mask:
- **/24 Subnet (255.255.255.0)**:  
  - **256 IPs** total (192.168.1.0 to 192.168.1.255).  
  - Calculation: **256 - 0** (Network Address) = **256 total IPs** in the block.

- **/27 Subnet (255.255.255.224)**:  
  - **32 IPs** total (192.168.1.0 to 192.168.1.31).  
  - Calculation: **256 - 224 = 32** IPs in the block.

- **/30 Subnet (255.255.255.252)**:  
  - **4 IPs** total (192.168.1.0 to 192.168.1.3).  
  - Calculation: **256 - 252 = 4** IPs in the block.

### Why Subtract from 256?
- IPs are in groups of 256 (from 0 to 255).  
- **Subtracting from 256** helps determine the size of each subnet.
  - For example: **/27** → `256 - 224 = 32` (32 IPs per block).
  - **/30** → `256 - 252 = 4` (4 IPs per block).

### Summary:
- **Subnet Mask** defines how big a subnet is and how many IPs it has.
- **CIDR Notation** tells you how many bits are used for the network.
- **Subtracting from 256** helps find the block size and number of usable IPs.


