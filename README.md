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

![Adding user to the Wireshark group](https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/raw/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Adding%20a%20new%20user%20to%20the%20wireshark%20group%20with%20sudo%20permission.PNG)

**Security Relevance**

This configuration illustrates the principle of least privilege by allowing packet capture through designated group permissions rather than requiring the Wireshark application to run with root privileges.

Membership in the `wireshark` group does not itself grant general administrative privileges. However, packet capture permissions still provide access to potentially sensitive network traffic and should therefore be restricted to authorized users.


### 1.2 Capturing Network Traffic

Wireshark displays the available network interfaces when launched. The loopback interface (`lo`) can be used to capture traffic between applications communicating locally on the same machine, while `ens5` was selected for capturing traffic on the Ubuntu system's Ethernet interface.

![Wireshark network interfaces](https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/raw/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/The%20Wireshark%20interface.PNG)

A packet capture was started on `ens5`, allowing network traffic to be observed in real time. The capture was subsequently stopped and saved to a packet capture file for further examination.

![Capturing and saving network traffic in Wireshark](https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/raw/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Capture%2C%20stop%20capturing%20and%20save%20packets%20in%20Wireshark.PNG)

The captured packets can be examined using Wireshark's packet list, protocol details, and packet bytes panes, providing information about network communication at different protocol layers.

### 1.3 HTTP and HTTPS Traffic Filtering

Wireshark display filters were used to examine captured network traffic associated with HTTP and HTTPS.

**HTTPS Traffic – TCP Port 443**

The display filter `tcp.port == 443` was applied to show packets using TCP port 443, which is commonly associated with HTTPS.

![Filtering TCP port 443 traffic](https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/raw/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Display%20captured%20packets%20using%20tcp%20port%20443.PNG)

Individual packets were selected to examine additional information about the captured network communication.

![Inspecting a packet using TCP port 443](https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/raw/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Information%20about%20the%20captured%20package%20using%20tcp%20port%20443.PNG)

**HTTP Traffic – TCP Port 80**

A separate capture of web traffic was examined using the display filter `tcp.port == 80`, which selects packets using TCP port 80, commonly associated with unencrypted HTTP.

![Filtering TCP port 80 traffic](https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/raw/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Display%20captured%20packets%20for%20HTTP%20traffic.PNG)

**Security Relevance**

Display filters allow an analyst to isolate traffic of interest from a larger packet capture without modifying the captured data.

TCP ports 80 and 443 are commonly associated with HTTP and HTTPS, respectively. However, port numbers alone do not establish which application protocol is being used.

Unlike capture filters, display filters operate on packets that have already been captured.

### 1.4 Inspecting TLS Traffic
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
