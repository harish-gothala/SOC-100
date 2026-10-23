# Day 2 Log: Host-only network and ping capture

**Date:** Fri 23 Oct 2026

## Task

Check that all my virtual machines work, move them to a host-only network, take a snapshot of each one, and capture a ping between two of them.

## What I did

### 1. Updated the VMs
Before changing the network, I updated every Linux VM (Kali and both Ubuntu machines) while they were still on NAT:

```
sudo apt update && sudo apt upgrade
```

The Windows VM was already up to date, so I did not touch it.

### 2. Switched the network to Host-only
I changed the network adapter on every VM from **NAT** to **Host-only**. This keeps my lab traffic isolated from the internet and my home network.

### 3. Took snapshots
After switching to Host-only, I took a snapshot of each VM. The snapshots save the updated state and the Host-only setting, so I can roll back at any time.

### 4. Checked the IP addresses
For the ping test I only needed two VMs: **Kali Linux** and the **Ubuntu Linux endpoint**. I ran `ip a` on both. Both have an IP address on the **same subnet**, so they can reach each other. (IP addresses hidden in this log.)

### 5. Captured the ping
1. On Kali, I opened **Wireshark** and started a capture on the network interface.
2. I opened a terminal on Kali and ran:
   ```
   ping -c 4 <ubuntu-ip>
   ```
3. I went back to Wireshark and clicked the **red square** button to stop the capture.
4. I typed `icmp` in the display filter box to show only the ping packets.

## What I saw

- Wireshark showed the columns **Time, Source, Destination, Protocol and Length**.
- There were **8 packets** in total:
  - **4 echo requests** from Kali
  - **4 echo replies** from Ubuntu
- I clicked one echo request and expanded **Internet Protocol Version 4** in the packet details. It shows the **source IP** (Kali) and the **destination IP** (Ubuntu), which matches the direction of the ping.

## What I learned

- Switching to Host-only isolates the lab, and a snapshot after the change gives me a clean state to return to.
- `ping -c 4` sends exactly 4 requests, and each request gets one reply, so a successful ping shows up as 8 packets.
- The `icmp` display filter removes everything except the ping traffic, which makes a capture much easier to read.
- The IPv4 section of a packet shows who sent it and who it was sent to.

## Evidence

- `ping-kali-ubuntu.pcapng`: the saved capture
- Screenshot of the snapshots
- Screenshot of the ping output on Kali
- Screenshot of Wireshark with the `icmp` filter and one packet expanded
- 

<img width="959" height="539" alt="snapshot" src="https://github.com/user-attachments/assets/5cbc80a4-dcd8-4269-9c14-71160d41c011" />

- <img width="955" height="539" alt="ping 1" src="https://github.com/user-attachments/assets/cd8ef83d-5b8b-4e71-b904-458798451815" />
<img width="806" height="454" alt="ping from kali" src="https://github.com/user-attachments/assets/1d4d3fec-e276-444c-9d58-eceffbcd4678" />





## Result

All VMs are working, they are on Host-only, each has a snapshot, and the ping between Kali and Ubuntu was captured and inspected.
