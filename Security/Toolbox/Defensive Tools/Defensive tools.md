#defence
## DAI
#CCNA #l2
Dynamic ARP inspection (DAI)

## IPSG
#CCNA #l2
IP Source Guard (IPSG)

## BPDU Guard
#CCNA 
Configured on switchports that are not trunking ports. Access ports should have BPDU Guard on. This is to mitigate a [[Attacks on networks#STP Attack|STP Attack]] even if they are not intential. 

## Port Security
[[Port Security]]

### VLAN specific attacks
#CCNA 
### VLAN Hopping attacks
#CCNA 
[[Attacks on networks#VLAN Hopping Attacks.|VLAN Hopping]]
To mitigate these attacks there are some steps that are needed. 
1. Disable [[Network Protocols#DTP|DTP]] by using *switchport mode access* interface command.
2. Disable unused ports and put them in an unused VLAN.
3. Manually enable the trunk link on a trunking port by using the *switchport mode trunk* command.
4. Disable DTP negotiations on trunking ports *switchport nonegotiate*.
5. Set the native VLAN to a VLAN other than 1. *switchport trunk native vlan*

## DHCP Snooping
#CCNA 
[[Attacks on networks#DHCP Attacks|DHCP Attacks]]

DHCP Snooping is required by [[Defensive tools#DAI|DAI]].
### Untrusted Sources

[[Network Protocols#DHCP|DHCP]] snooping does not rely on source MAC addresses. Instead, DHCP snooping determines whether DHCP messages are from an administratively configured trusted or untrusted source. It then filters DHCP messages and rate-limits DHCP traffic from untrusted sources.

Devices under your administrative control, such as switches, routers and servers are trusted sources. Any device beyon the firewall or outside the network is an untrusted source. All access ports are also untrusted. 

Typically, trusted soources are trunk links and ports directly connected to a legit DHCP server. But all interfaces are treated as untrusted by default and these interfaces must be explicitly configured as trusted.

A DHCP table is built that includes the source MAC address of a device on an untrusted port and the IP address assigned by the DHCP server to that device. The MAC and IP address are bound together. Therefore this table is called the DHCP snooping binding table.

### Implementing DHCP Snooping
*This section is specifically for Cisco proprietary devices, I do not know if all this is also true for other devices.*

1. Enable DHCP snooping: *ip dhcp snooping* (global configuration command).
2. On trusted ports: *ip dhcp snooping trust* (interface configuration command).
3. Limit the number of DHCP discovery messages that can be recieved per second on untrusted ports: *ip dhcp snooping limit rate* (interface configuration command).
4. Enable DHCP snooping by VLAN or by a range of VLANs: *ip dhcp snooping vlan* (global configuration command).

#### Verifying configuration

*show ip dhcp snooping*
*show ip dhcp snooping binding*