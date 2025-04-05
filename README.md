# **Examples of Monitoring Network Traffic & Configuring Firewall Rules**
This is a brief and simple guide on how to monitor various network traffic protocols and how to set up simple Firewall rules.

## **Table of Contents**
1. [Observing ICMP Traffic](#1-observing-icmp-traffic)
2. [Configuring a Firewall & Observing Network Traffic](#2-configuring-a-firewall--observing-network-traffic)
3. [Observing SSH Traffic](#3-observing-ssh-traffic)
4. [Observing DHCP Traffic](#4-observing-dhcp-traffic)
5. [Observing DNS Traffic](#5-observing-dns-traffic)
6. [Observing RDP Traffic](#6-observing-rdp-traffic)

## **Technologies Used**
- **Microsoft Azure** 
- **Windows 10 Virtual Machine**
  - **Wireshark** (network traffic analyzer)
  - **Remote Desktop** (for remote access)
  - **PowerShell / Command Prompt**
- **Ubuntu Virtual Machine**
  - **OpenSSH Server** (for remote SSH access)
  - **Network Security Groups (NSGs)** for firewall configuration
  
## **1. Observing ICMP Traffic**

### Set Up Remote Desktop & Wireshark
- If using **Mac**, install **Microsoft Remote Desktop**.
- Use **Remote Desktop** to connect to your **Windows 10 Virtual Machine**.
- Within the **Windows 10 VM**, install **Wireshark**.
- Open **Wireshark** and start a packet capture.

### Capture ICMP Traffic
- In **Wireshark**, filter for **ICMP traffic**: `icmp`
- Retrieve the **private IP address** of your **Ubuntu VM**.
- In **Windows 10 VM**, open **PowerShell** and ping the Ubuntu VM Private IP:
  ```powershell
  ping <VM_IP>
  ```
![Pinging the VM](images/Screenshot(3).png)
- Observe the **ping requests and replies** in Wireshark.
![Traffic in Wireshark](images/Screenshot(4).png)
- Now, attempt to ping a public website (e.g., Google):
  ```powershell
  ping www.google.com
  ```
![Pinging Google.com](images/Screenshot(5).png)
- Observe the ICMP traffic in **Wireshark**.
![Traffic in Wireshark](images/Screenshot(6).png)

## **2. Configuring a Firewall & Observing Network Traffic**

### Configuring an NSG

- Initiate a continuous ping:
  ```powershell
  ping <Ubuntu_IP> -t
  ```
  ![Pinging the VM](images/Screenshot(7).png)
- Go to **Azure Portal > Network Security Group** used by your **Ubuntu VM**.
![Network Security Groups](images/Screenshot(301).png)
- Disable or delete the **inbound ICMP rule**.
![Disabling ICMP](images/Screenshot(303).png) 

### Observe Traffic in Wireshark
- Open **Wireshark** and filter for ICMP traffic: `icmp`
![Viewing Traffic](images/Screenshot(10).png)
- Observe that **ping requests are sent but no responses are received**.

### Re-enable ICMP Traffic
- Re-enable **ICMP rule** in the **Network Security Group**.
- Observe that **ping responses** now appear again in **Wireshark**.
![Pings](images/Screenshot(11).png)
![Traffic in Wireshark](images/Screenshot(12).png)
- Stop the ping process: `Ctrl+C`

## **3. Observing SSH Traffic**

### Start Wireshark & Filter for SSH
- Establish an SSH Connection
```powershell
ssh user@<Ubuntu_IP>
```
- Enter username and password.
![login into ssh](images/Screenshot(15).png)

### Observe SSH Traffic
- Type commands in the SSH session and observe traffic in **Wireshark**.
![Observing Traffic](images/Screenshot(16).png)
- Exit SSH:
  ```bash
  exit
  ```

## **4. Observing DHCP Traffic**

### Start Wireshark & Filter for DHCP
- Request a New IP Address
```powershell
ipconfig /renew
```
![renewing ip](images/Screenshot(24).png)
- Observe **DHCP Discover, Offer, Request, and Acknowledge** packets.
![Observing DHCP traffic](images/Screenshot(23).png)
  

## **5. Observing DNS Traffic**

### Start Wireshark & Filter for DNS
### Perform DNS Lookups
```powershell
nslookup google.com
nslookup disney.com
```
![Pinging DNS](images/Screenshot(26).png)
- Observe **DNS requests and responses** in Wireshark.
![Observing Traffic](images/Screenshot(27).png)

## **6. Observing RDP Traffic**
### Start Wireshark & Filter for RDP
```bash
tcp.port == 3389
```
![Observing Traffic](images/Screenshot(28).png)

### Observe RDP Traffic
- If using **Remote Desktop Connection (RDP)**, notice constant RDP traffic in Wireshark.
- **Why is RDP traffic continuous?**
  - RDP is constantly streaming data, so traffic is always being transmitted.
