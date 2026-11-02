# Network-Traffic-Analysis-with-Wireshark
Hands-on network traffic analysis with Wireshark, covering packet capture, traffic filtering, protocol inspection, and security analysis of HTTP, HTTPS, DNS, RADIUS, Telnet, and SSH.

## Project Overview
To be filled in later...

## Lab Environment
To be filled in later...

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

<img width="939" height="720" alt="Client Hello for two websites" src="https://github.com/user-attachments/assets/db19c4d0-18d0-40a9-b05a-bcbb8bf4c764" />


**Filtering Traffic by IP Address**

After identifying the relevant IP address, additional display filters were applied to examine traffic associated with the remote server.

<img width="970" height="724" alt="Display all traffic from that website" src="https://github.com/user-attachments/assets/d53a30fa-6289-487e-962f-c6804a5f7446" />

The captured traffic was then examined by communication direction.

**Incoming Traffic**

Packets originating from the remote server were isolated to examine traffic sent toward the local machine.

<img width="964" height="707" alt="Display when the website is sending something" src="https://github.com/user-attachments/assets/1af2fd84-1d2c-487a-8aa9-14a324c7b07f" />

**Outgoing Traffic**

Packets sent from the local machine toward the remote server were also examined.

<img width="960" height="702" alt="Display all cases when we are sending something to that website" src="https://github.com/user-attachments/assets/7e6a2d42-783c-480b-b0e5-847a254cb0c5" />


**Security Relevance**

Identifying remote IP addresses and filtering traffic by source and destination can help a security analyst investigate connections involving known or suspicious hosts.

TLS Client Hello messages provide information about connection establishment, but do not by themselves establish that a connection is malicious. Likewise, an IP address observed in a TLS connection does not necessarily identify a unique website, since multiple websites may share infrastructure.

### 1.5 Combining Display Filters and Excluding IP Addresses

The objective was to isolate relevant network traffic by combining IP address and TCP port filters. This allows an analyst to focus on specific connections or exclude traffic that is not relevant to an investigation.

**Filtering Traffic by IP Address**

First, traffic associated with a selected website's IP address was displayed.

<img width="970" height="746" alt="Traffic from one of the websites" src="https://github.com/user-attachments/assets/7bee617b-610b-47b2-9c07-95bfe9ce181d" />

**Combining IP Address and HTTPS Port Filters**

The filtering was then narrowed to traffic associated with the same IP address and TCP port 443, commonly used for HTTPS.

<img width="963" height="742" alt="Traffic from one of the websites and only HTTPS" src="https://github.com/user-attachments/assets/1a30ac8c-34ac-41b3-85f0-179ce9e138f0" />


**Excluding an IP Address**

Next, traffic associated with the selected IP address was excluded while retaining traffic using TCP port 443.

<img width="963" height="688" alt="The traffic that does not come from that website and is HTTPS" src="https://github.com/user-attachments/assets/d860ecb7-722f-4fa0-bce4-3d7faccabdfe" />

**Displaying HTTP and HTTPS Traffic While Excluding an IP Address**

Finally, the filtering was expanded to include traffic using TCP ports 80 and 443, while continuing to exclude the selected IP address.

<img width="967" height="742" alt="Traffic not from that website and are both HTTP and HTTPS" src="https://github.com/user-attachments/assets/47c1da5e-a60c-420f-b36c-727954800022" />


**Security Relevance**

Combining display filters makes it possible to reduce irrelevant traffic and investigate selected network connections more efficiently. Excluding known IP addresses can be useful when examining other connections within a large packet capture.

Display filters change which packets are visible in Wireshark without removing them from the original capture.

### 1.6 Capstone – Combined Traffic Filtering

The final exercise combined the packet capture and display filtering techniques practiced in the preceding objectives.

The task was to capture network traffic, save the capture file, identify HTTP and HTTPS traffic, and exclude a selected IP address from the displayed results.

The display filter `tcp.port == 80` was initially used to identify traffic associated with HTTP and select an IP address for exclusion. The filtering was then adjusted to exclude the selected IP address and examine the remaining traffic.

<img width="956" height="718" alt="Capstone final" src="https://github.com/user-attachments/assets/81519f33-23bc-4aab-ac33-98a8c7ba8b4c" />

The exercise demonstrated how multiple display filter conditions can be combined to narrow the scope of network traffic analysis.

### Part 1 – Summary

This guided project covered the fundamentals of packet capture and traffic inspection using Wireshark on Ubuntu.

The exercises included configuring packet capture permissions, capturing and saving network traffic, examining HTTP- and HTTPS-associated TCP traffic, identifying IP addresses through TLS handshake inspection, and combining display filters to include or exclude selected connections.

These techniques provide a foundation for network troubleshooting and security investigations, where analysts need to isolate relevant communications from larger packet captures.

## Part 2 – Protocol Security Analysis
[Sections to be developed]

## Security Analysis
## Learning Outcomes

## Acknowledgments

This project is based on two guided projects completed on Coursera:

- [Wireshark for Beginners: Capture Packets](https://www.coursera.org/projects/wireshark-for-beginners-capture-packets), instructed by David Dalsveen.
- [Wireshark for Basic Network Security Analysis](https://www.coursera.org/projects/wireshark-for-network-security), instructed by Menna ElSharkawy.

The exercises were completed in guided lab environments and have been consolidated into a single portfolio project. The documentation includes practical observations, explanations of the network traffic examined, and discussion of relevant security implications and limitations.
