# How to Solve Netpractice?

1. **Download**, extract, and open the `index.html` file using your **Intra name**.  
2. If the **check button** does not appear, try opening it in a **different browser**.  

## Level 1, 2, 3: IP and Mask

- Delete all editable numbers.
- PCs with the **same mask** belong to the **same network** and can communicate with each other.  
- the **IP addresses of same network should be almost identical**, except for the **last digit**.  
- subtract 1 or (in case of failure, adding 1) from the last digit
- If there is only a **SWITCH** in between computers/hosts, it does not affect any IP or mask


## Level 4: Router and it's Interfaces

- Each router side called **interface**,  should typically have a **separate subnet mask**, because they are in different networks.
- Hosts connected to the same interface of a router will typically have
- the same subnet mask and
- IP addresses in the same range as the router’s interface but **NOT EXACTLY** the same!


## Level 5: Route

- Each host should connect to router: make same mask as router and close IPs
- Hosts should communicate with each other through the router for which we will have **route IPs**
- Route IPs should be **SAME** as the router IP of their respective side
- Route has another option (route), that is set to default here

## Level 6: THE INTERNET

- IP address = 4 octets (32 bits total)
- Subnet masks =  4 octets (32 bits total)
- 1 octet = 8 bits

**IP and Subnet Mask Relationship**

- The subnet mask determines which part of the IP address is used for the network and which part is used for hosts.
- The network part can be in different octets depending on the subnet mask.
- The host part is whatever remains after the network part is decided.

**Subnet and IP Visualization Examples**

- For subnet mask `255.255.255.0`, the network part is the first 3 octets, and the host part is the last octet.  
  **IP**: `192.168.1.100` → `Network: 192.168.1 | Host: 100`

- For subnet mask `255.255.255.128`, the network part is the first 3 octets plus 1 bit of the 4th octet, and the host part is the remaining bits of the 4th octet.  
  **IP**: `192.168.1.100` → `Network: 192.168.1. | Host: 100`

- For subnet mask `255.255.254.0`, the network part is the first 2 octets plus part of the 3rd octet, and the host part is the remaining bits.  
  **IP**: `192.168.1.100` → `Network: 192.168. | Host: 1.100`

These are the possible values that set the bits in a subnet mask, from 8 bits to 0 bits.

General Pattern (8 bits range): The formula to calculate group size is always 2^host-bits
- 255 = 8 bits (binary: 11111111) — 8 bits for the network part, 0 IPs for hosts (no space left for hosts).
- 254 = 7 bits (binary: 11111110) — 7 bits for the network part, leaving 1 bit for the host part. groupsize: 2^1 = 2.
- 252 = 6 bits (binary: 11111100) — 6 bits for the network part, leaving 2 bits for the host part. Usable IPs: 2^2 = 4.
- 248 = 5 bits (binary: 11111000) — 5 bits for the network part, leaving 3 bits for the host part. Usable IPs: 2^3 = 8.
- 240 = 4 bits (binary: 11110000) — 4 bits for the network part, leaving 4 bits for the host part. Usable IPs: 2^4 = 16.
- 224 = 3 bits (binary: 11100000) — 3 bits for the network part, leaving 5 bits for the host part. Usable IPs: 2^5 = 32.
- 192 = 2 bits (binary: 11000000) — 2 bits for the network part, leaving 6 bits for the host part. Usable IPs: 2^6 = 64.
- 128 = 1 bit (binary: 10000000) — 1 bit for the network part, leaving 7 bits for the host part. Usable IPs: 2^7 = 128.
- 0 = 0 bits (binary: 00000000) — 0 bits for the network part, leaving 8 bits for the host part. Usable IPs: 2^8 = 256.

# Subnet Mask diving "Pizza" Examples

When dealing with a router and multiple interfaces, the **IP address block** can be visualized like a pizza. The **subnet mask** determines how the pizza is divided into slices (subnets).

- **Subnet mask 255.255.255.0**  
  The whole pizza is divided into 1 large slice with **256 total IPs**. For example, `192.168.1.0 - 192.168.1.255`.

- **Subnet mask 255.255.255.128**  
  The pizza is divided into **2 slices**, each with **128 IPs**. For example, `192.168.1.0 - 192.168.1.127` and `192.168.1.128 - 192.168.1.255`.

- **Subnet mask 255.255.255.192**  
  The pizza is divided into **4 slices**, each with **64 IPs**. For example, `192.168.1.0 - 192.168.1.63`, `192.168.1.64 - 192.168.1.127`, `192.168.1.128 - 192.168.1.191`, and `192.168.1.192 - 192.168.1.255`.-Here we have internet and it's route, ip given
- Router has 2 interfaces R1 and R2
- There are two more routes:\
   1. router R: gate.non-real.com
   2. host A: webserv.non-real.com
- to solve, do the same for the host and routes except the internet
- for the internet route-
  1. take first 3 octets from host **IP**
  2. last octet of host mask or 128 or 0 and make it the fourth octet
  3. add /0 or /any number upto 25 

**why 128 or 0?**\
- If mask is 255.255.255.0, it means the entire last octet is for host addresses, so all IPs from 192.168.1.0 to 192.168.1.255 belong to the same network.\
- This is a larger subnet, covering all 256 possible IPs in that range.\
- If mask is 255.255.255.128, it is a smaller network range.\
- 192.168.1.128 would cover 192.168.1.128 to 192.168.1.255 (a range of 128 IP addresses).\


**why till /25??**\
- /0 means all IPs (it’s a catch-all for everything).\
-/25 gives you the first 128 IPs in a network range, which is sufficient for general internet addressing (it's a broader match for networks).\
- /26 would mean the first 64 IPs in a network. This subnet mask is too narrow for defining an internet-wide route because it restricts the address range too much.\

## Level 7: TWO ROUTERS

- with two routers R1 and R2, will come four interfaces R11 R12 R21 R22 and two routes\
- first we start solving from host A1 with given IP- R1's fisrt\
- make a random mask for A1 and it's adjacent router 255.255.255.128 coz no mask is given here\
- At this point we have routes and interfaces **INBETWEEN** two routers\
- Use R1's second given IP to reduce and make IPs for the rest of the interfaces and host- nothing new here



- **Subnet mask 255.255.255.224**  
  The pizza is divided into **8 slices**, each with **32 IPs**. For example, `192.168.1.0 - 192.168.1.31`, `192.168.1.32 - 192.168.1.63`, and so on.
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


