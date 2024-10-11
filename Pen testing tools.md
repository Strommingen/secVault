### Wireshark
[[Wireshark]]
Used for analysing traffic on a network as a reconnaissance tool.
### Gzrecover
Tool for recovering corrupted .gz files. ![[Recovering corrupted files#.gz files]]
### Openssl
[[Network Protocols#TLS|TLS]]

### Gobbler

Is used in [[Attacks on networks#DHCP Starvation Attack |DHCP Starvation Attacks]].

Gobbler is able to look at the entire scope of leasable ip addresses and tries to release them all. More specifically it creates DHCP discovery messages with bogus MAC addresses.

### Yersina

### Ettercap

### Cain & Abel

### Dsniff

### Macof
Used [[Attacks on networks#MAC Address Table Flooding|to overflow a MAC address table.]] 
With this tool, an attacker can create a MAC table overflow attack very quickly. It can flood a switch with up to 8000 frames per second. While for example a Cisco Catalyst 6500 switch can store up to 132000 MAC addresses in its table, it will fill up in seconds.

It will also not only affect the local switch, but also other connected Layer 2 switches because the switch will start flooding the network when its table is full, meaning the frames will spread to all other Layer 2 connected devices.