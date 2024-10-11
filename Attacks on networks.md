## LAN attacks
#### VLAN Hopping Attacks.

#### VLAN Double-Tagging Attack

### Address Spoofing Attack

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

According to the Mitre Att&ck framework [[Pen testing standards documents#Mitre Att&ck]], there are over 16 different command-and-control tactics used by adversaries, including numerous subtechniques:

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