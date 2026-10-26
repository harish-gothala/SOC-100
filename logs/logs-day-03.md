# Day 3 Log: The OSI Model

**Date:** Sat 24 Oct 2026
**Topic:** The OSI model and how data passes through each layer

## What is the OSI model?

OSI stands for **Open Systems Interconnection**. It was developed by **ISO** and works like a common language for computer networking: two different computers can communicate because both follow the same layered rules.

The internet today actually runs on **TCP/IP**, not OSI. OSI is still very useful as a reference model, especially for **troubleshooting**, because it lets me ask "which layer is the problem at?"

The model has **7 abstract layers**, and each layer does a different job.

| # | Layer | Data unit |
|---|-------|-----------|
| 7 | Application | Data |
| 6 | Presentation | Data |
| 5 | Session | Data |
| 4 | Transport | Segment (TCP) / Datagram (UDP) |
| 3 | Network | Packet |
| 2 | Data Link | Frame |
| 1 | Physical | Bits |

**Memory trick (layer 7 down to 1):** All People Seem To Need Data Processing.

## The 7 layers in my own words

### 7. Application layer
This is the layer closest to the user. It gives network services to the applications the user works with, such as a browser or an email client, so the user can send requests and get what they want.
- **Protocols:** HTTPS for browsing, SMTP for sending mail, and others such as DNS and FTP.

### 6. Presentation layer
This layer prepares the data so the receiver can understand it. It handles **data format and translation, compression and encryption**. Data is encrypted at the sender's end and decrypted at the receiver's end.
- **Examples:** SSL/TLS encryption.

### 5. Session layer
This layer **establishes a session** between two devices, keeps it running, and **removes it** when the communication or task is finished. If the user goes offline or does nothing for some time, the session is stopped automatically.
- It can also set **checkpoints**, so if communication stops in the middle, it resumes from the same point instead of starting from scratch.

### 4. Transport layer
This layer is responsible for **data transfer**. It takes the data from the layers above, **breaks it into segments**, sends them, and **reassembles them at the receiver's side**. It also handles **flow control and error control**, and uses port numbers to reach the right application.
- **Two protocols:** Transmission Control Protocol (TCP) and User Datagram Protocol (UDP). TCP is reliable and sets up a connection first. UDP is faster but gives no guarantee. The application's needs decide which one is used.
- **Devices and tools:** firewalls and load balancers work at this layer when they filter by port or track connections.

### 3. Network layer
This layer is responsible for **IP addressing** and **routing**: it chooses which path will reach the destination fastest. The segments are converted into **packets**, which are passed to the next layer.
- **Devices:** routers.

### 2. Data Link layer
This layer makes sure the received data belongs to the right device and decides where to send it, using **physical MAC addresses**. Packets are put into **frames**, and the frame includes a check to detect errors.
- **Examples:** Ethernet. **Devices:** switches.

### 1. Physical layer
Here the data is converted into **0s and 1s**, a bit stream, and transferred over a physical medium such as **cables** (or radio waves in Wi-Fi). Then the user gets what they asked for.

## How data passes through the layers

- **Sending side:** the data starts at layer 7 and moves **down** to layer 1. Each layer adds its own header (this is called encapsulation): the segment gets port numbers, the packet gets IP addresses, the frame gets MAC addresses. At layer 1 everything is sent as bits.
- **Receiving side:** the bits come in at layer 1 and move **up** to layer 7. Each layer removes its own header (de-encapsulation) until the application gets the data.

## Link to my Day 2 capture

My ping capture from Day 2 shows these layers in Wireshark:

| Wireshark section | OSI layer |
|-------------------|-----------|
| Ethernet II (MAC addresses) | Layer 2, Data Link |
| Internet Protocol Version 4 (IP addresses) | Layer 3, Network |
| Internet Control Message Protocol | Carried inside IP. It has no ports, so it does not use TCP or UDP. |

## Why this matters for a SOC analyst

When I read an alert or a packet capture, the layers tell me where to look. A phishing email is a layer 7 problem. A port scan shows up at layer 4. A suspicious IP is a layer 3 clue. A device that shouldn't be on the network is a layer 2 clue.

## Result

I wrote all 7 layers, their jobs, their protocols and devices, and how data moves through them in my own words.
