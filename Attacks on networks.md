## LAN attacks
#CCNA

#### DHCP Attacks
##### DHCP Starvation Attack

This is a type of DoS (Denial of Service) attack for clients trying to connect to a network. This type of attack require an attack tool, [[Pen testing tools#Gobbler |Gobbler]] is such a tool that makes it possible.

##### DHCP Spoofing Attack

A rouge DHCP server is connected to the network and provides false IP configuration parameters to legitamate clients. A rogue server can provide different types of misleading information:

- **Wrong default gateway**, the rogue server provides invalid default gateway information, or  the address of its host to create a man-in-the-middle attack, This can go entirely undetected as the intruder intercepts the data flow through the network.
- **Wrong DNS server**, the rogue server provides incorrect DNS server address pointing to a nefarious website.
- **Wrong IP address**, the rogue server provides an invalid IP address effectively creating a DoS attack on the DHCP client.

#### ARP Attacks

To mitigate ARP attacks one can implement a [[Defensive tools#Dynamic ARP inspection (DAI)|DAI]] into the network.
##### ARP Spoofing Attack / ARP Poisoning Attack

Attackers expoit the gratuitous [[Network Protocols#ARP|ARP]] message by sending a spoofed MAC address to a switch, this switch will then update its MAC table accordingly. Any hosts can therefore claim to be the owner of any IP and MAC address combination that they choose. 

This type of attack is kind of specific to IPv4 since IPv6 uses [[Network Protocols#Neighbor Discovery/Advertisement|Neighbor Discovery Protocol]] instead for its Layer 2 address resolution. IPv6 includes strategies to mitigate Neighbor Advertisement spoofing, similar to the way IPv6 prevents a spoofed ARP Reply.
#### Address Spoofing Attack

These attacks can be mitigated by implementing a [[Defensive tools#IPSG |IPSG]].
##### IP address spoofing

IP address spoofing is when a attacker hijacks a valid IP address of another device on the network. 
Or if a random IP address is used. This is difficult to mitigate especially when the IP address is used in the subnet it belongs.
##### MAC address spoofing

MAC address spoofing attacks are when the attacker alter MAC addresses of their hosts to match another know MAC address of a target host. The attacker sends the a frame through the network with the false MAC address. When a switch recieves the frame it examines the source MAC address. It then overwrites the current MAC table entry and assigns the MAC address to the new port. This means that it will forward all frames to the attacker instead of the intended host.

But when the original host sends traffic, the switch will correct its table, realigning the MAC address to the correct port. To stop the switch from doing this, a attacker can create a script that constantly sends frames to the switch so that the switch maintains the incorrect or spoofed information. 

There is no security mechanism at Layer 2 that allows the switch to verify the source of MAC addresses, this is why it is so vulnerable to spoofing. ![[Misc#Note on Layer 2 devices]]
#### MAC Address Table Flooding

MAC address tables have a fixed size, a switch can therefore run out of recources in which to store MAC addresses. Flooding attacks exploit this by bombarding the switch with fake source MAC addresses until the switch MAC address table is full.

Now when a switch recieves an unknown unicast it normally floods the message to fish for a response so that the MAC address can be saved in the table. But now that the table is full it will always flood the messages without referencing the MAC table. This causes the attacker to be able to capture all frames sent from one host to another on the *local LAN or local [[VLAN]].* 

Attackers use the tool [[Pen testing tools#Macof|macof]] to overflow a MAC address table.

##### MAC Address Table Attack Mitigation 
![[Pen testing tools#Macof]]

To mitigate this dangerous tool:

![[Defensive tools#Mitigate MAC Address Table Attacks]]
#### STP Attack
Attackers can manipulate [[Network Protocols#STP|STP]] to conduct an attack by spoofing the root bridge and changing the topology of a network. Attackers can make their hosts appear as root bridges and therefore, capture all traffic for the immedate switched domain.

To make a STP manipulation attack, the attacker broadcasts STP bridge protocol data units (BPDUs) containing configuration and topology changes that will force recalculations of the STP. The BPDUs also advertise a lower bridge priority to be elected as the root bridge.

These issues don't even have to be intentional, they can happen even when someone adds an Ethernet switch to a network.

This STP attack is mitigated by implementing BPDU Guard on all access ports.

#### CDP/LLDP Reconnaissance

[[Network Protocols#CDP/LLDP|CDP and LLDP]] is a vulnerable protocol since it shares information unencrypted. If a threat acor intercepts this traffic it can be used to discover network infrastructure vulnerabilities. 

For example since CDP/LLDP shares the devices IOS version, a attacker can use that information to research if there are any security vulnerabilities related to that version and then use that information to possibly gain further access.

A attacker can also send out bogus CDP/LLDP packets (since it is unencrypted and unauthenticated) to interfere with the network infrastructure.

To mitigate exploitation of CDP/LLDP, it should be disabled where it is not needed. A good practice is to disable it on all end-ports where it is more likely that untrusted devices connect.
#### VLAN Attacks

These attacks can be prevented by implementing the following: ![[VLAN#VLAN specific attacks]]
##### VLAN Hopping Attacks.

This type of attack enables traffic from one [[VLAN|VLAN]] to be seen by another VLAN without the aid of a router, which should not happen. In a basic attack, the attacker configures a host to act like a switch to take advantege of the automatic trunking port feature enabled by default on most switch ports.

Attacker configurest the host to spoof [[Network Protocols#802.1Q|802.1Q]] signaling and [[Network Protocols#DTP|DTP]] singnaling with the connecting switch. If this succeeds the switch establishes a trunk link with the host and now the attacker caan access all the VLANs on the switch. The attacker can send and recieve traffic on any VLAN, now "hopping" between VLANs.

##### VLAN Double-Tagging Attack

The attacker can in specific situations embed a hidden 802.1Q tag in the frame that already has a 802.1Q tag. This way the tag allows the frame to go to a VLAN that the original 802.1Q tag did not specify.

This method only works one way and only when the attacker is connected to a port in the same VLAN as the [[VLAN#Native VLAN|native VLAN]] of the trunk port. So it is limited in that way, but the idea is to send data to hosts and servers on a VLAN that otherwise would be blocked by some access control configuration. Presumably the return traffic will also be permitted, giving the attacker the ability yo communicate with devices on the normally blocked VLAN.
## other?

#### C2 or C&C
Command and Conquer (C2). Works through a ”bot master” sending instructions to its bots.
##### How it works
###### First step
First, a foothold must be established in the system. This can be done via a phishing email, security holes in a browser plugin or other infected software.

###### Recieving instructions

Now the infected machine can try making contact with the ”bot master” which van be the attackers server. The attacker then starts sending instructions that the C2 infected host carries out. Additional software can be downloaded for example.

###### Fully infected

Now the attacker has full control of the system and can excecute any code. Typically at this stage the attacker will try to spread to more computers and create a botnet. This way the attacker can gain full control of an entire network.
##### Reasons for DNS calls
A high amount of DNS calls in the network traffic might indicate C2 activity.
**This is because:**

- **Malware often uses DNS** queries to communicate with C2 servers.
- Attackers may use **dynamic DNS** services to change IP address associated with a domain frequently, this is because it is harder to block. If DNS calls are going to domains that change often or appear suspicious that could mean it is C2 activity.
- **High volume of unique domains** might indicate that the compromised system is attempting to communicate with multiple C2 servers or resolve multiple command locations.
- **Unusual Domains** that look strange or is not something normally present in the network might also be a sign of C2.
- **Non-Standard Ports**: If DNS requests are being made on non-standard ports or using unusual DNS record types (like TXT or CNAME), it could indicate attempts to communicate covertly with a C2 server.

##### Discovering C2

 **Steps for Analysis:**

- **Check the DNS Response**: Look at the responses to these DNS queries to see if they resolve to known malicious IP addresses.
- **Analyze Traffic Patterns**: Examine the timestamps and source/destination IP addresses of the DNS queries for patterns that might indicate automated behavior typical of malware.
- **Correlate with Other Traffic**: Cross-reference the DNS traffic with other protocols (like HTTP or HTTPS) to see if there are corresponding outbound connections to suspicious domains or IPs.
- **Use Threat Intelligence**: Utilize threat intelligence feeds to identify known malicious domains or C2 infrastructure.

##### C2 tactics

According to the [[Pen testing standards documents#Mitre Att&ck |Mitre Att&ck]] framework, there are over 16 different command-and-control tactics used by adversaries, including numerous subtechniques:

1. Application Layer Protocol
2. Communication Through Removable Media
3. Data Encoding
4. Data Obfuscation
5. Dynamic Resolution
6. Encrypted Channel
7. Fallback Channels
8. Ingress Tool Transfer
9. Multi-Stage Channels
10. Non-Application Layer Protocol
11. Non-Standard Port
12. Protocol Tunneling
13. Proxy
14. Remote Access Software
15. Traffic Signaling
16. Web Service

##### C2 techniques
 
 There are 3 different models for C2 attacks that determine how the infected machine will communicate with the bot master. Each designed to avoid detection as long as possible.
 - **Centralized architecture**: probably the most common model. After infection, a computer will join the botnet, establishing contact with C2 server and waiting for orders.