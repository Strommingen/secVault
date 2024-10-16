## TLS
#l5
### TLS 1.2
Older version. A typical handshake is between 5-7 packets.
#### Handshake

### TLS 1.3
Faster handshake, typically 0-3 packets. This leads to faster and more responsive connections.
#### Handshake

## ARP
#CCNA #l2 
ARP (Address Resolution Protocol) Requests are a network layer protocol used by hosts to determine the MAC address of a host with a particular IPv4 address. ARP Requests are always broadcast messages. 

Typically this is done to discover the MAC address of the default gateway. All hosts on the subnet receive and process the ARP Request. But the host with the matching IPv4 address in the ARP Request is the one that sends a reply, which would be an ARP Reply.

A client is allowed to send an unsolicited ARP Request called a "gratuitous ARP". When a host sends one of these other hosts will store the MAC address and IPv4 address contained in the gratuitous ARP in their ARP tables. This is exploited by attackers in [[Attacks on networks#ARP Attacks|ARP Attacks]]. 

## Neighbor Discovery/Advertisement

## STP
#CCNA #l2
Spanning Tree Protocol (STP)

## CDP/LLDP
#CCNA #l2 
Cisco Discovery Protocol (CDP) is a cisco proprietary Layer 2 link discovery protocol. By default it is enabled on all Cisco devices. 
Link Layer Discovery Protocol (LLDP) is an open standard protocol

CDP/LLDP can automatically discover other CDP/LLDP-enabled devices and help to auto-configure their connection. It is also used to help configure and troubleshoot network devices. Like if a device cannot ping a directly connected interface, but receives CDP/LLDP information, the problem is most likely related to Layer 3 configuration. 

CDP/LLDP information is sent out CDP/LLDP-enabled ports in periodic and unencrypted multicasts. The information sent includes:
- IP address of the device.
- IOS software version.
- Platform.
- Capabilities.
- Native VLAN

The receiving device stores and updates this information in its CDP/LLDP database. 

While the protocol is useful for troubleshooting it is important to consider the [[Attacks on networks#CDP/LLDP Reconnaissance|vulnerabilities]] also, especially since, as mentioned, CDP is **enabled by default** on all cisco devices.

## 802.1Q
#CCNA #l2
## DTP
#CCNA #l2
Dynamic Trunking Protocol (DTP)

## RDP
#l7

## PaGP and LACP
#l2 #CCNA 

## DHCP
#l3 #CCNA