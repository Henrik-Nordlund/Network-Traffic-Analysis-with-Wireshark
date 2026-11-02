# Network-Traffic-Analysis-with-Wireshark
Hands-on network traffic analysis with Wireshark, covering packet capture, traffic filtering, protocol inspection, and security analysis of HTTP, HTTPS, DNS, RADIUS, Telnet, and SSH.

## Project Overview
## Lab Environment

## Part 1 – Packet Capture and Traffic Filtering

### 1.1 Lab Environment and Wireshark Setup

**Scenario**

In this guided lab, I worked with a simulated security analyst scenario involving the inspection of network traffic to and from a server. The objective was to capture and examine TCP/IP traffic, particularly web traffic, and identify relevant IP addresses and network connections.

**Wireshark Installation**

Wireshark was installed in an Ubuntu environment. The Wireshark stable PPA was added to provide access to Wireshark packages:

```bash
sudo add-apt-repository ppa:wireshark-dev/stable
```

Wireshark can then be installed using the APT package manager:

```bash
sudo apt install wireshark
```

<img width="731" height="441" alt="Installing Wireshark on Ubuntu" src="https://github.com/user-attachments/assets/67b93e71-c9dc-4d0a-905c-12027cc2555a" />


**User Permissions**

To allow packet capture without running the Wireshark application as root, the existing user was added to the `wireshark` group:

```bash
sudo usermod -aG wireshark $USER
```

The user then logged out and logged back in for the updated group membership to take effect.

<img width="731" height="441" alt="Adding a new user to the wireshark group with sudo permission" src="https://github.com/user-attachments/assets/14cefe56-62fb-44d5-84dd-cab420624f58" />


**Security Relevance**

This configuration illustrates the principle of least privilege by allowing packet capture through designated group permissions rather than requiring the Wireshark application to run with root privileges.

Membership in the `wireshark` group does not itself grant general administrative privileges. However, packet capture permissions still provide access to potentially sensitive network traffic and should therefore be restricted to authorized users.


### 1.2 Capturing Network Traffic

Wireshark displays the available network interfaces when launched. The loopback interface (`lo`) can be used to capture traffic between applications communicating locally on the same machine, while `ens5` was selected for capturing traffic on the Ubuntu system's Ethernet interface.

<img width="964" height="697" alt="The Wireshark interface" src="https://github.com/user-attachments/assets/649671b5-35d0-44ce-abfc-87702dd0b983" />   


A packet capture was started on `ens5`, allowing network traffic to be observed in real time. The capture was subsequently stopped and saved to a packet capture file for further examination.

<img width="1068" height="812" alt="Capture, stop capturing and save packets in Wireshark(1)" src="https://github.com/user-attachments/assets/fb9feccc-9dcd-4842-a61e-d83d27998ca6" />   

The captured packets can be examined using Wireshark's packet list, protocol details, and packet bytes panes, providing information about network communication at different protocol layers.

### 1.3 HTTP and HTTPS Traffic Filtering

Wireshark display filters were used to examine captured network traffic associated with HTTP and HTTPS.

**HTTPS Traffic – TCP Port 443**

The display filter `tcp.port == 443` was applied to show packets using TCP port 443, which is commonly associated with HTTPS.

<img width="949" height="710" alt="Display captured packets using tcp port 443" src="https://github.com/user-attachments/assets/5ab2d837-f6aa-47bc-a4bd-88912c90e8be" />

Individual packets were selected to examine additional information about the captured network communication.

<img width="764" height="587" alt="Information about the captured package using tcp port 443" src="https://github.com/user-attachments/assets/18652cf5-f68e-496d-afe5-d6d4c4e69f2c" />


**HTTP Traffic – TCP Port 80**

A separate capture of web traffic was examined using the display filter `tcp.port == 80`, which selects packets using TCP port 80, commonly associated with unencrypted HTTP.

<img width="963" height="661" alt="Display captured packets for HTTP traffic" src="https://github.com/user-attachments/assets/71048532-ddcb-4e26-933d-ee2caa2b1e4b" />


**Security Relevance**

Display filters allow an analyst to isolate traffic of interest from a larger packet capture without modifying the captured data.

TCP ports 80 and 443 are commonly associated with HTTP and HTTPS, respectively. However, port numbers alone do not establish which application protocol is being used.

Unlike capture filters, display filters operate on packets that have already been captured.

### 1.4 TLS Handshake and IP Address Filtering

The objective was to identify the IP address associated with a website and examine network traffic between the local machine and the remote server.

**Identifying TLS Connections**

The following Wireshark display filter was applied to identify TLS Client Hello messages:

```text
tls.handshake == 1
```

These messages are sent by a client when initiating a TLS handshake. The resulting packets were examined to identify destination IP addresses associated with the connections.

![TLS Client Hello messages for two websites](https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/raw/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Client%20Hello%20for%20two%20websites.PNG)

**Filtering Traffic by IP Address**

After identifying the relevant IP address, additional display filters were applied to examine traffic associated with the remote server.

![Displaying traffic associated with the website](https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/raw/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Display%20all%20traffic%20from%20that%20website.PNG)

The captured traffic was then examined by communication direction.

**Incoming Traffic**

Packets originating from the remote server were isolated to examine traffic sent toward the local machine.

![Traffic sent by the remote server](https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/raw/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Display%20when%20the%20website%20is%20sending%20something.PNG)

**Outgoing Traffic**

Packets sent from the local machine toward the remote server were also examined.

![Traffic sent to the remote server](https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/raw/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Display%20all%20cases%20when%20we%20are%20sending%20something%20to%20that%20website.PNG)

**Security Relevance**

Identifying remote IP addresses and filtering traffic by source and destination can help a security analyst investigate connections involving known or suspicious hosts.

TLS Client Hello messages provide information about connection establishment, but do not by themselves establish that a connection is malicious. Likewise, an IP address observed in a TLS connection does not necessarily identify a unique website, since multiple websites may share infrastructure.

### 1.5 Filtering Traffic by IP Address
### 1.6 Findings and Verification Limitations

## Part 2 – Protocol Security Analysis
[Sections to be developed]

## Security Analysis
## Learning Outcomes
## Acknowledgments

## Acknowledgments

This project is based on two guided projects completed on Coursera:

- [Wireshark for Beginners: Capture Packets](https://www.coursera.org/projects/wireshark-for-beginners-capture-packets), instructed by David Dalsveen.
- [Wireshark for Basic Network Security Analysis](https://www.coursera.org/projects/wireshark-for-network-security), instructed by Menna ElSharkawy.

The exercises were completed in guided lab environments and have been consolidated into a single portfolio project. The documentation includes practical observations, explanations of the network traffic examined, and discussion of relevant security implications and limitations.
