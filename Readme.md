# How to Solve Netpractice?

1. **Download**, extract, and open the `index.html` file using your **Intra name**.  
2. If the **check button** does not appear, try opening it in a **different browser**.
3. Mini theory:
   - IP address = 4 octets (32 bits total)
   - Subnet masks =  4 octets (32 bits total)
   - 1 octet = 8 bits


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

- Router has 2 interfaces R1 and R2
- There are two more routes:\
   1. router R: gate.non-real.com
   2. host A: webserv.non-real.com
- to solve, do the same for the host and routes except the internet
- for the internet route-
  1. take first 3 octets from host **IP**
  2. last octet of host mask or 128 or 0 and make it the fourth octet
  3. add /0 or /any number upto 25 

**why 128 or 0?**
- If mask is 255.255.255.0, it means the entire last octet is for host addresses, so all IPs from 192.168.1.0 to 192.168.1.255 belong to the same network.
- This is a larger subnet, covering all 256 possible IPs in that range.\
- If mask is 255.255.255.128, it is a smaller network range.\
- 192.168.1.128 would cover 192.168.1.128 to 192.168.1.255 (a range of 128 IP addresses).\

**why till /25??**
- /0 means all IPs (it’s a catch-all for everything).
- /25 gives you the first 128 IPs in a network range, which is sufficient for general internet addressing (it's a broader match for networks).
- /26 would mean the first 64 IPs in a network. This subnet mask is too narrow for defining an internet-wide route because it restricts the address range too much.

## Theory to know before level 7

## 📡 Public vs Private IP Addresses  

 **🌍 Public IP Address (Globally Unique)**  
✅ **Completely unique** across the entire internet.  
✅ **Assigned by ISPs (Internet Service Providers)** to routers or devices that directly connect to the internet.  
✅ **Used for internet communication** (e.g., websites, online services).  

### 🔹 Example Public IPs  
- `8.8.8.8` (Google DNS)  
- `142.250.190.14` (Google.com)  


### 🌐 Public IP Range  
Any IP **not** in the private IP ranges (see below) is considered a **public IP**.

---

**🏠 Private IP Address (Not Unique)** 
✅ **Used within a local network (LAN)**.  
✅ **Not unique globally** — **same private IPs can exist in different networks** (e.g., multiple homes use `192.168.1.1`).  
✅ **Assigned by routers** to home devices like laptops, phones, or smart TVs.  
✅ **Cannot communicate directly with the internet** (needs **NAT** from a router).  

### 🔹 Private IP Ranges  

| **Private Network** | **IP Range** | **Common Use** |
|---------------|-----------------|-------------------|
| **Class A** | `10.0.0.0` – `10.255.255.255` | Large organizations |
| **Class B** | `172.16.0.0` – `172.31.255.255` | Medium-sized networks |
| **Class C** | `192.168.0.0` – `192.168.255.255` | Home & small business networks |

### 🖥️ Example Private IPs  
- `192.168.1.1` (Common home router IP)  
- `10.0.0.5` (Enterprise network device)  
- `172.16.100.25` (Corporate network)  

---

## 🔄 How Routers Handle IPs  
✔️ **Public IP** → Assigned by ISP, used for internet access.  
✔️ **Private IP** → Used inside a local network assigned by router.  
✔️ **Routers have both**:  
   - **Public IP** on the **WAN side** (internet-facing).  
   - **Private IP** on the **LAN side** (e.g., `192.168.1.1` for home devices).  

---

📌 **Summary**  
- **Public IPs** are unique worldwide & required for internet access.  
- **Private IPs** are used locally & not routable over the internet.  
- **Routers bridge private networks to public networks using NAT (Network Address Translation).**

# 📡 Network Bits & Host Bits  

## 🧮 Network Bits and Host Bits  
Each IP address consists of **network bits** and **host bits**.  

- **Subnet Mask:** `255.255.255.0`  
- **Binary Representation:** `11111111.11111111.11111111.00000000`  
- **Network Bits (`1`s):** Define the network portion.  
- **Host Bits (`0`s):** Define the host portion (individual devices in the network).  

---

## 🏷️ CIDR Notation  
CIDR (**Classless Inter-Domain Routing**) notation simply means **the number of network bits** in a subnet mask.  

### 🔹 Example  
- **Subnet Mask:** `255.255.255.0`  
- **Binary:** `11111111.11111111.11111111.00000000`  
- **Total Network Bits (1s):** `24`  
- **CIDR Notation:** `/24`  

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
- 252 = 6 bits (binary: 11111100) — 6 bits for the network part, leaving 2 bits for the host part. groupsize: 2^2 = 4.
- 248 = 5 bits (binary: 11111000) — 5 bits for the network part, leaving 3 bits for the host part. groupsize: 2^3 = 8.
- 240 = 4 bits (binary: 11110000) — 4 bits for the network part, leaving 4 bits for the host part. groupsize: 2^4 = 16.
- 224 = 3 bits (binary: 11100000) — 3 bits for the network part, leaving 5 bits for the host part. groupsize: 2^5 = 32.
- 192 = 2 bits (binary: 11000000) — 2 bits for the network part, leaving 6 bits for the host part. groupsize: 2^6 = 64.
- 128 = 1 bit (binary: 10000000) — 1 bit for the network part, leaving 7 bits for the host part. groupsize: 2^7 = 128.
- 0 = 0 bits (binary: 00000000) — 0 bits for the network part, leaving 8 bits for the host part. groupsize: 2^8 = 256.

  from each group size, the first and last IP's are reserved as Network address and Broadcast address. the inbetweeners are Usable IPs.
# 📡 Quick Subnet Calculation (No Binary Needed)

## **Step 1: Find the Network Address**
1. **Identify the subnet mask's "interesting octet"** (first non-255 octet).  
2. **Find the block size**: `256 - Subnet Mask Octet`.  
3. **Find the nearest multiple of the block size** **≤ given IP**  
   - Compare with the multiples: `0, block size, 2×block size, 3×block size, ...`  
   - Choose the **largest multiple that is less than or equal to the given IP** → **Network Address**.

## **Step 2: Find the Broadcast Address**
- **Broadcast Address** = `Network Address + (Block Size - 1)`

---

## **Example 1: Given IP `192.168.1.45`, Mask `255.255.255.240` (`/28`)**
- **Interesting Octet**: `240`
- **Block Size**: `256 - 240 = 16`
- **Multiples of 16**: `0, 16, 32, 48, 64...`
- **Nearest multiple ≤ 45**: `32` → **Network Address** = `192.168.1.32`
- **Broadcast Address**: `32 + (16 - 1) = 47` → **`192.168.1.47`**

---

## **Example 2: Given IP `10.0.5.200`, Mask `255.255.255.192` (`/26`)**
- **Interesting Octet**: `192`
- **Block Size**: `256 - 192 = 64`
- **Multiples of 64**: `0, 64, 128, 192, 256...`
- **Nearest multiple ≤ 200**: `192` → **Network Address** = `10.0.5.192`
- **Broadcast Address**: `192 + (64 - 1) = 255` → **`10.0.5.255`**

---

## **Example 3: Given IP `172.16.4.100`, Mask `255.255.254.0` (`/23`)**
- **Interesting Octet**: `254`
- **Block Size**: `256 - 254 = 2`
- **Multiples of 2**: `0, 2, 4, 6, 8, 10, ..., 254`
- **Nearest multiple ≤ 100**: `100` → **Network Address** = `172.16.4.0`
- **Broadcast Address**: `0 + (2 × 256 - 1) = 172.16.5.255`

---

## **Common Block Sizes**
| Subnet Mask | Block Size | Network Multiples |
|------------|-----------|------------------|
| `255.255.255.128` (`/25`) | 128 | `0, 128` |
| `255.255.255.192` (`/26`) | 64 | `0, 64, 128, 192` |
| `255.255.255.224` (`/27`) | 32 | `0, 32, 64, 96, 128, 160...` |
| `255.255.255.240` (`/28`) | 16 | `0, 16, 32, 48, 64...` |
| `255.255.255.248` (`/29`) | 8 | `0, 8, 16, 24, 32...` |

🚀 **No binary needed—just subtract, find multiples, and calculate!**  

# 📡 Common Network and Broadcast Addresses  

| Subnet Mask | Block Size | Example Network Address | Example Broadcast Address |
|------------|-----------|-------------------------|--------------------------|
| `255.255.255.128` (`/25`) | 128 | `192.168.1.0` | `192.168.1.127` |
| `255.255.255.192` (`/26`) | 64  | `192.168.1.0` | `192.168.1.63` |
| `255.255.255.224` (`/27`) | 32  | `192.168.1.32` | `192.168.1.63` |
| `255.255.255.240` (`/28`) | 16  | `192.168.1.48` | `192.168.1.63` |
| `255.255.255.248` (`/29`) | 8   | `192.168.1.56` | `192.168.1.63` |
| `255.255.255.252` (`/30`) | 4   | `192.168.1.60` | `192.168.1.63` |

---

### **How It Works**
1. **Find the block size**: `256 - subnet mask octet`
2. **Multiples of the block size**: `0, block size, 2×block size, ...`
3. **Network Address** = **Closest multiple ≤ given IP**
4. **Broadcast Address** = `Network Address + (Block Size - 1)`

🚀 **No binary needed—just compare and pick!**



# 📡 Subnet Mask, Network, and Broadcast Addresses  

| **Subnet Mask** | **Group Size** | **Example IP** | **Network Address** | **Broadcast Address** |
|---------------|------------|-----------------|----------------|-------------------|
| `255.255.255.240` (`/28`) | 16 IPs | `192.168.1.45` | `192.168.1.32` | `192.168.1.47` |
| `255.255.255.192` (`/26`) | 64 IPs | `10.0.0.150` | `10.0.0.128` | `10.0.0.191` |
| `255.255.255.252` (`/30`) | 4 IPs | `172.16.5.13` | `172.16.5.12` | `172.16.5.15` |
| `255.255.255.0` (`/24`) | 256 IPs | `192.168.50.200` | `192.168.50.0` | `192.168.50.255` |
| `255.255.254.0` (`/23`) | 512 IPs | `172.16.4.100` | `172.16.4.0` | `172.16.5.255` |
| `255.255.248.0` (`/21`) | 2048 IPs | `10.5.9.20` | `10.5.8.0` | `10.5.15.255` |

# 📊 Subnetting Block Size Table  

| **Exponent (n)** | **2ⁿ Value** | **CIDR Notation** | **Block Size** | **Usable Hosts** |
|-----------------|------------|-----------------|------------|--------------|
| 2⁰  | 1   | (Not used)   | N/A  | N/A  |
| 2¹  | 2   | (Not used)   | N/A  | N/A  |
| 2²  | 4   | `/30`  | 4  | 2  |
| 2³  | 8   | `/29`  | 8  | 6  |
| 2⁴  | 16  | `/28`  | 16 | 14  |
| 2⁵  | 32  | `/27`  | 32 | 30  |
| 2⁶  | 64  | `/26`  | 64 | 62  |
| 2⁷  | 128 | `/25`  | 128 | 126  |
| 2⁸  | 256 | `/24`  | 256 | 254  |

---

## **How to Use This Table**
- **Block Size** = `256 - subnet mask octet`
- **Usable Hosts** = `2ⁿ - 2` (subtracting network & broadcast addresses)

📌 **Example:**  
For subnet **255.255.255.192 (/26)**:  
- **Block Size** = `64`  
- **Usable Hosts** = `62`  
# 🌐 Subnet Reference Table  

| **Subnet Mask** | **CIDR** | **Block Size** | **Network Address** | **Usable Range** | **Broadcast Address** |
|---------------|--------|------------|----------------|--------------|-------------------|
| 255.255.255.252 | `/30` | 4  | `192.168.1.0`  | `192.168.1.1 - 192.168.1.2` | `192.168.1.3` |
| 255.255.255.248 | `/29` | 8  | `192.168.1.8`  | `192.168.1.9 - 192.168.1.14` | `192.168.1.15` |
| 255.255.255.240 | `/28` | 16  | `192.168.1.16` | `192.168.1.17 - 192.168.1.30` | `192.168.1.31` |
| 255.255.255.224 | `/27` | 32  | `192.168.1.32` | `192.168.1.33 - 192.168.1.62` | `192.168.1.63` |
| 255.255.255.192 | `/26` | 64  | `192.168.1.64` | `192.168.1.65 - 192.168.1.126` | `192.168.1.127` |
| 255.255.255.128 | `/25` | 128 | `192.168.1.128` | `192.168.1.129 - 192.168.1.254` | `192.168.1.255` |
| 255.255.255.0   | `/24` | 256 | `192.168.2.0` | `192.168.2.1 - 192.168.2.254` | `192.168.2.255` |
| 255.255.254.0   | `/23` | 512 | `192.168.2.0` | `192.168.2.1 - 192.168.3.254` | `192.168.3.255` |
| 255.255.252.0   | `/22` | 1024 | `192.168.4.0` | `192.168.4.1 - 192.168.7.254` | `192.168.7.255` |

---

## **How to Use This Table**
1. Find the **subnet mask** or **CIDR notation**.
2. Use the **Block Size** to determine the next network.
3. The **Network Address** is always the first IP in the block.
4. The **Broadcast Address** is the last IP in the block.
5. The **Usable Range** is everything in between.

📌 **Example:**  
- Given **IP 192.168.1.70** and **Subnet Mask 255.255.255.192 (/26)**:  
  - **Network Address:** `192.168.1.64`
  - **Usable Range:** `192.168.1.65 - 192.168.1.126`
  - **Broadcast Address:** `192.168.1.127`
 
| Subnet Mask      | Block Size | IP Range                     | Network Address | Broadcast Address | Usable IP Range                |
|------------------|------------|------------------------------|-----------------|-------------------|--------------------------------|
| 255.255.255.0    | /24        | 0 - 255                      | `x.x.x.0`       | `x.x.x.255`       | `x.x.x.1 - x.x.x.254`          |
| 255.255.255.128  | /25        | 0 - 127, 128 - 255            | `x.x.x.0`       | `x.x.x.127`       | `x.x.x.1 - x.x.x.126` (Subnet 1) |
|                  |            |                              | `x.x.x.128`     | `x.x.x.255`       | `x.x.x.129 - x.x.x.254` (Subnet 2) |
| 255.255.255.192  | /26        | 0 - 63, 64 - 127, 128 - 191, 192 - 255 | `x.x.x.0`       | `x.x.x.63`        | `x.x.x.1 - x.x.x.62` (Subnet 1)  |
|                  |            |                              | `x.x.x.64`      | `x.x.x.127`       | `x.x.x.65 - x.x.x.126` (Subnet 2)|
|                  |            |                              | `x.x.x.128`     | `x.x.x.191`       | `x.x.x.129 - x.x.x.190` (Subnet 3)|
|                  |            |                              | `x.x.x.192`     | `x.x.x.255`       | `x.x.x.193 - x.x.x.254` (Subnet 4)|
| 255.255.255.224  | /27        | 0 - 31, 32 - 63, 64 - 95, 96 - 127, 128 - 159, 160 - 191, 192 - 223, 224 - 255 | `x.x.x.0` | `x.x.x.31`        | `x.x.x.1 - x.x.x.30` (Subnet 1)  |
|                  |            |                              | `x.x.x.32`      | `x.x.x.63`        | `x.x.x.33 - x.x.x.62` (Subnet 2) |
|                  |            |                              | `x.x.x.64`      | `x.x.x.95`        | `x.x.x.65 - x.x.x.94` (Subnet 3) |
|                  |            |                              | `x.x.x.96`      | `x.x.x.127`       | `x.x.x.97 - x.x.x.126` (Subnet 4)|
| 255.255.255.240  | /28        | 0 - 15, 16 - 31, 32 - 47, 48 - 63, 64 - 79, 80 - 95, 96 - 111, 112 - 127, 128 - 143, 144 - 159, 160 - 175, 176 - 191, 192 - 207, 208 - 223, 224 - 239, 240 - 255 | `x.x.x.0` | `x.x.x.15`        | `x.x.x.1 - x.x.x.14` (Subnet 1)  |
|                  |            |                              | `x.x.x.16`      | `x.x.x.31`        | `x.x.x.17 - x.x.x.30` (Subnet 2) |
|                  |            |                              | `x.x.x.32`      | `x.x.x.47`        | `x.x.x.33 - x.x.x.46` (Subnet 3) |
|                  |            |                              | `x.x.x.48`      | `x.x.x.63`        | `x.x.x.49 - x.x.x.62` (Subnet 4) |
| 255.255.255.252  | /30        | 0 - 3, 4 - 7, 8 - 11, ... 252 - 255 | `x.x.x.0` | `x.x.x.3`         | `x.x.x.1 - x.x.x.2` (Subnet 1) |
|                  |            |                              | `x.x.x.4`       | `x.x.x.7`         | `x.x.x.5 - x.x.x.6` (Subnet 2)  |
|                  |            |                              | `x.x.x.8`       | `x.x.x.11`        | `x.x.x.9 - x.x.x.10` (Subnet 3) |
|                  |            |                              | `x.x.x.252`     | `x.x.x.255`       | `x.x.x.253 - x.x.x.254` (Subnet 4) |


## Router Interface info
The router can have multiple interfaces with the same subnet mask (e.g., /30) as long as the IP addresses on those interfaces fall within different subnets. Each interface needs to be on a different network, even if they share the same subnet mask.






## Level 7: TWO ROUTERS

- with two routers R1 and R2, will come four interfaces R11 R12 R21 R22 and two routes\
- first we start solving from host A1 with given IP- R1's fisrt\
- make a random mask for A1 and it's adjacent router 255.255.255.128 coz no mask is given here\
- At this point we have routes and interfaces **INBETWEEN** two routers\
- Use R1's second given IP to reduce and make IPs for the rest of the interfaces and host- nothing new here






# Subnet Mask diving "Pizza" Examples

When dealing with a router and multiple interfaces, the **IP address block** can be visualized like a pizza. The **subnet mask** determines how the pizza is divided into slices (subnets).

- **Subnet mask 255.255.255.0**  
  The whole pizza is divided into 1 large slice with **256 total IPs**. For example, `192.168.1.0 - 192.168.1.255`.

- **Subnet mask 255.255.255.128**  
  The pizza is divided into **2 slices**, each with **128 IPs**. For example, `192.168.1.0 - 192.168.1.127` and `192.168.1.128 - 192.168.1.255`.

- **Subnet mask 255.255.255.192**  
  The pizza is divided into **4 slices**, each with **64 IPs**. For example, `192.168.1.0 - 192.168.1.63`, `192.168.1.64 - 192.168.1.127`, `192.168.1.128 - 192.168.1.191`, and `192.168.1.192 - 192.168.1.255`.-Here we have internet and it's route, ip given

- **Subnet mask 255.255.255.224**  
  The pizza is divided into **8 slices**, each with **32 IPs**. For example, `192.168.1.0 - 192.168.1.31`, `192.168.1.32 - 192.168.1.63`, and so on.
IP addresses come in groups called **blocks**. Each block has:
- **Network Address** = First IP in the block (**not usable**).
- **Broadcast Address** = Last IP in the block (**not usable**).
- **Usable IPs** = Everything between **Network Address +1** and **Broadcast Address -1**.



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




