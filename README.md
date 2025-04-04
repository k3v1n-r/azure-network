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
- In **Windows 10 VM**, open **Command Prompt** or **PowerShell** and ping the Ubuntu VM:
  ```powershell
  ping <Ubuntu_IP>
  ```
- Observe the **ping requests and replies** in Wireshark.
- Now, attempt to ping a public website (e.g., Google):
  ```powershell
  ping www.google.com
  ```
- Observe the ICMP traffic in **Wireshark**.

## **2. Configuring a Firewall & Observing Network Traffic**

### Configuring an NSG

- Initiate a continuous ping:
  ```powershell
  ping <Ubuntu_IP> -t
  ```
- Go to **Azure Portal > Network Security Group** used by your **Ubuntu VM**.
- Disable or delete the **inbound ICMP rule**.

### Observe Traffic in Wireshark
- Open **Wireshark** and filter for ICMP traffic: `icmp`
- Observe that **ping requests are sent but no responses are received**.

### Re-enable ICMP Traffic
- Re-enable **ICMP rule** in the **Network Security Group**.
- Observe that **ping responses** now appear again in **Wireshark**.
- Stop the ping process: `Ctrl+C`

## **3. Observing SSH Traffic**

### Start Wireshark & Filter for SSH
- Establish an SSH Connection
```powershell
ssh labuser@<Ubuntu_IP>
```
- Enter username and password.

### Observe SSH Traffic
- Type commands in the SSH session and observe traffic in **Wireshark**.
- Exit SSH:
  ```bash
  exit
  ```

## **4. Observing DHCP Traffic**

### Start Wireshark & Filter for DHCP
### Request a New IP Address
```powershell
ipconfig /renew
```
- Observe **DHCP Discover, Offer, Request, and Acknowledge** packets.
  

## **5. Observing DNS Traffic**

### Start Wireshark & Filter for DNS
### Perform DNS Lookups
```powershell
nslookup google.com
nslookup disney.com
```
- Observe **DNS requests and responses** in Wireshark.

## **6. Observing RDP Traffic**
### Start Wireshark & Filter for RDP
```bash
tcp.port == 3389
```

### Observe RDP Traffic
- If using **Remote Desktop Connection (RDP)**, notice constant RDP traffic in Wireshark.
- **Why is RDP traffic continuous?**
  - RDP is constantly streaming data, so traffic is always being transmitted.
