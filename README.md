# Network-Traffic-Analysis-with-Wireshark
Hands-on network traffic analysis with Wireshark, covering packet capture, traffic filtering, protocol inspection, and security analysis of HTTP, HTTPS, DNS, RADIUS, Telnet, and SSH.

# Network Traffic Analysis with Wireshark

## Project Overview
## Lab Environment

## Part 1 – Packet Capture and Traffic Filtering
### 1.1 Lab Environment and Wireshark Setup

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

![Installing Wireshark on Ubuntu](https://github.com/Henrik-Nordlund/Capturing-packets-with-Wireshark/raw/d8cc636657fe6f785cf3aca0af83b0dde2fb85ca/Installing%20Wireshark%20on%20Ubuntu.PNG)

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
### 1.3 HTTP and HTTPS Traffic Filtering
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
