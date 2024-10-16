#offence #linux 
## Gzrecover
#corruptfile
Tool for recovering corrupted .gz files. ![[Recovering corrupted files#.gz files]]
## TestDisk
#corruptfile 
Tool for testing disk files and extracting files from those disks. Used in [[MSB_CERT-SE_CTF_2024]].
## Openssl
#decrypt #encrypt
[[Network Protocols#TLS|TLS]]

## Gobbler
#CCNA #l7
Is used in [[Attacks on networks#DHCP Starvation Attack |DHCP Starvation Attacks]].

Gobbler is able to look at the entire scope of leasable ip addresses and tries to release them all. More specifically it creates DHCP discovery messages with bogus MAC addresses.

## Yersina
#CCNA #l2
### Key features of Yersinia:

**Protocol Exploits**: Yersinia focuses on various Layer 2 protocols, such as:

- **Spanning Tree Protocol (STP)**: Yersinia can send [[Attacks on networks#STP Attack|fake STP]] messages to become the root bridge and disrupt the network.
- **Dynamic Host Configuration Protocol (DHCP)**: It can launch [[Attacks on networks#DHCP Starvation Attack|DHCP starvation attacks]], exhausting IP addresses in a network.
- **Cisco Discovery Protocol (CDP)**: It can [[Attacks on networks#CDP/LLDP Reconnaissance|exploit vulnerabilities in CDP]], gaining information about Cisco devices.
- **VLAN Trunking Protocol (VTP)**: Yersinia can manipulate VLAN configurations by sending spoofed VTP messages.
- **802.1Q and 802.1X**: It can launch [[Attacks on networks#VLAN Hopping Attacks.|VLAN hopping]] and bypass 802.1X network access controls.

[[Attacks on networks#MAC Address Table Flooding|MAC Flooding]]: Yersinia can flood the MAC address table of a switch, forcing it into a fail-open mode, where it behaves like a hub, allowing packet sniffing on the network.

[[Attacks on networks#ARP Spoofing Attack / ARP Poisoning Attack|ARP Spoofing]]: By sending fake ARP messages, Yersinia can perform man-in-the-middle attacks, intercepting or modifying traffic between devices.

[[Attacks on networks#DHCP Spoofing Attack|DHCP Spoofing:]] Yersinia can fake DHCP responses to assign rogue IP addresses, leading to network disruption or redirection of traffic.
## Ettercap
#CCNA #MITM #l2 #l7 #l3
Ettercap can manipulate Ethernet frames, allowing for **[[Attacks on networks#ARP Spoofing Attack / ARP Poisoning Attack|ARP poisoning/spoofing]]**.

Ettercap can also manipulate network packets, working with protocols like IP to perform attacks such as **[[Attacks on networks#DHCP Spoofing Attack|DNS spoofing]]** or **packet filtering**.

Can also interact with Layer 7 by intercepting and modifying data sent between applications, such as HTTP or HTTPS, when performing MITM attacks.
## Cain & Abel
#CCNA 
## Dsniff
#CCNA 
## Macof
#CCNA #l2 
Used [[Attacks on networks#MAC Address Table Flooding|to overflow a MAC address table.]] 
With this tool, an attacker can create a MAC table overflow attack very quickly. It can flood a switch with up to 8000 frames per second. While for example a Cisco Catalyst 6500 switch can store up to 132000 MAC addresses in its table, it will fill up in seconds.

It will also not only affect the local switch, but also other connected Layer 2 switches because the switch will start flooding the network when its table is full, meaning the frames will spread to all other Layer 2 connected devices.

## Mimikatz
#postexploit 
The tool was created as a proof of concept to show Microsoft that its authentication protocols were vulnerable to an attack.
It is open-source and allows users to view and save authentication credentials such as kerberos tickets.

It is commonly used to steal credentias and escalate privileges because endpoint protection software and antivirus systems normally will not detect the attack.
- Mimikatz can be used to pass the exact hash string to a target computer to log in. No need to crack the password (older windows?).
- Newer versions of windows store password data in a construct called a ticket. With mimikatz you can pass a kerberos ticket to another computer and log in with that ticket.
- **Kerberoast golden tickets**: This is a pass-the-ticket attack, but it’s a specific ticket for a hidden account called KRBTGT, which is the account that encrypts all of the other tickets. A golden ticket provides you with non-expiring domain admin credentials to any computer on the network.
- **Kerberoast silver tickets**: Another pass-the-ticket, but a silver ticket takes advantage of a feature in Windows that makes it easy for you to use services on the network. Kerberos grants a user a ticket-granting server (TGS) ticket, and a user can use that ticket to authentic to service accounts on the network. Microsoft doesn’t always check a TGS after it’s issued, so it’s easy to slip past any safeguards.

When extracting certificates with this tool the default password is 'mimikatz'.
## Aircrack-ng
#cracking
This tool is for cracking WPA/WPA2-PSK passwords, aka wifi passwords.

Used in [[THM_AOC_2023_first]].

### Wpaclean
#pcap
To make the cracking faster you can use wpaclean to clean the file, slimming it down so that there are not as many packets to go through for aircrack-ng, or whatever used for the cracking. 

Example of this tool combination working on [[THM_AOC_2023_first|Advent of Cyber CTF:]]  ![[THM_AOC_2023_first#Cracking]]
## PyRDP
[PyRDP github](https://github.com/GoSecure/pyrdp)
### PyRDP Convert

This is a helper script (pyrdp-convert) that performs several useful conversions from various input formats to various output formats. The script has the best chance of working on traffic captured by PyRDP due to unsupported [[Network Protocols#RDP|RDP]] protocol features that might be used in a non-intercepted connection.

The following inputs are supported:
- Network Capture (PCAP) with TLS master secrets (less reliable)
- Network Capture (PCAP) in Exported PDUs Layer 7 format (more reliable)
- Replay file generated by PyRDP

The following outputs are supported:
- MP4 video file
- JSON: a sequence of low-level events serialized in JSON format
- Replay file compatible with *pyrdp-player*

If TLS is encrypted, it must be decrypted before.
Note that MP4 conversion requires libavcodec and ffmpeg, so this may require extra steps on Windows.

Manually decrypted network traces can be exported from Wireshark by selecting `File > Export PDUs` and selecting `OSI Layer 7`. But first you have to have configured the TLS secrets.