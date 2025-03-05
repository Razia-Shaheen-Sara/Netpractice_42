##THIS IS A WORK IN PROGRESS
## How to Solve Netpractice?

**Download**, extract, and open the `index.html` file using **chrome** then your **Intra name**.  

<details><summary>Level 1: Intro to IP and Mask</summary>

- IP address = 4 octets (32 bits total)
- Subnet masks =  4 octets (32 bits total)
- 1 octet = 8 bits
- Delete all editable numbers.
- PCs with the **same mask** belong to the **same network** and can communicate with each other.  
- the **IP addresses of same network should be almost identical**, except for the **last digit**.
- because IPs come in groups called **SUBNETS** discussed later 
- subtract 1 or (in case of failure, adding 1) from the last digit of the IP address
</details>


<details><summary>Level 2: Cider notation</summary>

-Here subnet mask is shown like this **/30** because it is another way of writing the mask called **CIDR Notation**  
CIDR (**Classless Inter-Domain Routing**) notation simply means **the number of network bits** (which is 1) in a subnet mask
### Example  
- **Subnet Mask:** `255.255.255.0`  
- **Binary:** `11111111.11111111.11111111.00000000`  
- **Total Network Bits (1s):** `24`  
- **CIDR Notation:** `/24`
- **Disclaimer** - You dont need to do binary convertions here for this project. More example for easy visualization --
- /30 → 255.255.255.252 → 11111111.11111111.11111111.11111100 (30)
- /21 → 255.255.248.0 → 11111111.11111111.11111000.00000000 (21)
- /20 → 255.255.240.0 → 11111111.11111111.11110000.00000000 (20)
- /25 → 255.255.255.128 → 11111111.11111111.11111111.10000000 (25)
- /26 → 255.255.255.192 → 11111111.11111111.11111111.11000000 (26)
- /27 → 255.255.255.224 → 11111111.11111111.11111111.11100000 (27)
- /28 → 255.255.255.240 → 11111111.11111111.11111111.11110000 (28)

![image](https://github.com/user-attachments/assets/b68862ff-f8cb-4e1f-98d9-68c88b554076)

Also, if no IP is given, just use 1.2.3 or any number as first 3 octets

</details>
<details> <summary>Level 3: Switch</summary>
  
- Switch does not affect anything in the network. Just ignore it!
</details>
<details> <summary>Level 4: Router and it's Interfaces</summary>
  
- Each router side called **interface**,  should typically have a separate mask number, because they are in different networks.
- Hosts connected to the same interface of a router will have -
1. the same subnet mask and
2. IP addresses in the same range as the router’s interface but **NOT EXACTLY** the same because of Grouping/subnet

</details>

<details> <summary>Level 5: Route</summary>

  <img width="637" alt="level5" src="https://github.com/user-attachments/assets/ccb986ba-d2e4-42f6-adb9-691397d41973" />

  - **Next-hop IP** usually means the closest interface of the next router. as shown here
  - The destination IP is default because data needs a known destination. If no specific route exists, it follows the default route
  - For this project, we use default as destination IP in most cases

</details>


<details><summary>Level 6: THE INTERNET</summary>
<img width="1054" alt="level6" src="https://github.com/user-attachments/assets/b22393bc-1b00-48f7-bbf6-bd1a256c42f7" />
  for the destination IP of internet route
  
  - take first 3 octets from destination host **IP**
  - add /0 or /any number upto 25 (play with it to see what works)
  - the next-hop ip for the internet is always the closest router interface's IP

</details>

<details><summary>Level 7: TWO ROUTERS</summary>

<img width="947" alt="level 7" src="https://github.com/user-attachments/assets/77e8cfe3-c534-4b02-be4b-24e3f1691217" />
Between two routers, the IPs must be connected, meaning they should be sequential (1 more or 1 less) and within a specific range (discussed below).

- In this example, the fourth octet of R21 must be **253**, not **255**. Here's why:
- The **CIDR notation/Subnet mask** determines group sizes, known as the **block size**.
- The **first and last IPs in a block are not usable**:
  - The first IP is the **Network Address**.
  - The last IP is the **Broadcast Address**.
- If we use a **/30 subnet mask (255.255.255.252)**:
  - Block size = `256 - 252 = 4`
  - The reserved addresses are:
    - **Network Address** → `x.x.x.252` (Not usable)
    - **Usable IPs** → `x.x.x.253`, `x.x.x.254`
    - **Broadcast Address** → `x.x.x.255` (Not usable)
  - Since `x.x.x.254` is already used, **R21 must be assigned `x.x.x.253`**.

For better understanding, the following table may come in handy.

| **Subnet Mask**     | **CIDR** | **IP Range**                  | **Network Address** | **Broadcast Address** | **Usable IP Range** | **Number of Subnets(group numbers)** |
|---------------------|---------|-------------------------------|---------------------|----------------------|---------------------|------------------|
| 255.255.255.0      | /24     | 0 - 255                       | `x.x.x.0`          | `x.x.x.255`         | `x.x.x.1 - x.x.x.254` | 1 |
| 255.255.255.128    | /25     | 0 - 127, 128 - 255            | `x.x.x.0`, `x.x.x.128` | `x.x.x.127`, `x.x.x.255` | `x.x.x.1 - x.x.x.126`, `x.x.x.129 - x.x.x.254` | 2 |
| 255.255.255.192    | /26     | 0 - 63, 64 - 127, 128 - 191, 192 - 255 | `x.x.x.0`, `x.x.x.64`, `x.x.x.128`, `x.x.x.192` | `x.x.x.63`, `x.x.x.127`, `x.x.x.191`, `x.x.x.255` | `x.x.x.1 - x.x.x.62`, `x.x.x.65 - x.x.x.126`, `x.x.x.129 - x.x.x.190`, `x.x.x.193 - x.x.x.254` | 4 |
| 255.255.255.224    | /27     | 0 - 31, 32 - 63, ..., 224 - 255 | `x.x.x.0`, `x.x.x.32`, ... | `x.x.x.31`, `x.x.x.63`, ... | `x.x.x.1 - x.x.x.30`, `x.x.x.33 - x.x.x.62`, ... | 8 |
| 255.255.255.240    | /28     | 0 - 15, 16 - 31, ..., 240 - 255 | `x.x.x.0`, `x.x.x.16`, ... | `x.x.x.15`, `x.x.x.31`, ... | `x.x.x.1 - x.x.x.14`, `x.x.x.17 - x.x.x.30`, ... | 16 |
| 255.255.255.252    | /30     | 0 - 3, 4 - 7, ..., 252 - 255 | `x.x.x.0`, `x.x.x.4`, ... | `x.x.x.3`, `x.x.x.7`, ... | `x.x.x.1 - x.x.x.2`, `x.x.x.5 - x.x.x.6`, ... | 64 |


 



</details>
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

## Router Interface info
The router can have multiple interfaces with the same subnet mask (e.g., /30) as long as the IP addresses on those interfaces fall within different subnets. Each interface needs to be on a different network, even if they share the same subnet mask.

## router to router
For router-to-router communication, both interfaces must be in the same subnet, meaning they should belong to the same IP block based on the subnet mask.

Example with /30 (Block Size = 4):
If Router 1 has:

IP: 1.2.3.1/30
Subnet Range: 1.2.3.0 - 1.2.3.3
Usable IPs: 1.2.3.1 (Router 1), 1.2.3.2 (Router 2)
Broadcast: 1.2.3.3
Then Router 2 must have:

IP: 1.2.3.2/30 (from the same subnet)
Subnet Mask: /30
Same broadcast and network address as Router 1
💡 If one router had 1.2.3.5/30, it would be in a different subnet (1.2.3.4 - 1.2.3.7) and wouldn’t connect!






## Next-hop IP usually means the closest interface of the next router that your current router can reach.




<details>
  <summary>Click to expand</summary>

  Your hidden content goes here.

</details>
## Level 7: TWO ROUTERS

- with two routers R1 and R2, will come four interfaces R11 R12 R21 R22 and two routes\
- first we start solving from host A1 with given IP- R1's fisrt\
- make a random mask for A1 and it's adjacent router 255.255.255.128 coz no mask is given here\
- At this point we have routes and interfaces **INBETWEEN** two routers\
- Use R1's second given IP to reduce and make IPs for the rest of the interfaces and host- nothing new here
<details>
  <summary>Click to expand</summary>

  Your hidden content goes here.

</details>
## Level 8: Configure Route

# 🖧 Routing Issue: Importance of Subnet Masks in Route Definitions  

## ❌ Problem:  
- A route is defined **without a subnet mask** (e.g., `X.X.X.X` instead of `X.X.X.X/XX`).  
- The network expects a **specific subnet size**.  
- **Result:** Routing failure, incorrect forwarding, or loops.  

## 🔍 Why Does It Fail?  
1. **No subnet mask = ambiguous route interpretation.**  
   - Routers may default to an unintended subnet size (e.g., `/32`).  
   - This leads to mismatched routes and forwarding errors.  

2. **Route matching fails.**  
   - If the expected route has a different subnet mask, packets may not be forwarded correctly.  
   - This can cause loops, black holes, or routing inconsistencies.  

## ✅ Fix: Explicitly Defining a Subnet Mask  
- Using a correct subnet mask ensures:  
  - **Clear network boundary recognition** 🏆  
  - **Proper route matching and priority** 🔄  
  - **Prevention of loops or misroutes** 📌  

## 🎯 Conclusion  
- **Always specify a subnet mask when defining routes.**  
- **Unspecified masks can cause incorrect behavior.**  
- **Using a well-matched subnet mask ensures stable and predictable routing.**  

<details>
  <summary>Click to expand</summary>

  Your hidden content goes here.

</details>
## Level 9: work with third octet
# Next Hop IP Routing Theory

In routing, the choice of the next hop IP depends on the network design and routing table configuration. Here’s a summary of how it works:

## 1. Closest Router’s Interface (Same Subnet)
- When the destination IP is within the **same subnet**, the **next hop** IP will be the **interface of the closest router**.
- This is because the device (host/router) can directly reach the destination network, so it sends the packet to the router in the same subnet.

### Example:
- **Host A** (IP: `192.168.1.2/24`) wants to communicate with **Host B** (IP: `192.168.1.10/24`).
- The **next hop** IP is **Router R1**'s interface (e.g., `192.168.1.1`), as it is directly connected to the same subnet.

## 2. Next Router’s Interface (Different Subnet)
- When the destination is in a **different subnet** (or farther away), the **next hop** IP will be the **interface of the next router**.
- Routers forward packets based on their routing table, and if the destination is outside of the local network, the packet is sent to the next router in the path.

### Example:
- **Host A** (IP: `192.168.1.2/24`) wants to communicate with **Host C** (IP: `10.0.0.2/24`).
- Since **Host A** is in a different subnet, **Router R1** will forward the packet to **Router R2**, using **R2's interface** (e.g., `192.168.2.1`), as the next hop.

## Why Does This Happen?
- **Routing Decision**: Routers make forwarding decisions based on their **routing table** and the **destination IP**.
  - If the destination is **directly reachable**, the next hop will be the **closest router's interface**.
  - If the destination is in a **different subnet**, the next hop will be the **next router's interface** on the way to the destination.

## Summary:
- **Same Subnet**: Next hop is the **closest router’s interface**.
- **Different Subnet**: Next hop is the **next router’s interface**.

## for a gateway (router-to-router) connection, the subnet mask of the connected interfaces of the two routers should typically be the same.



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

<details>
  <summary>Click to expand</summary>

  Your hidden content goes here.

</details>


