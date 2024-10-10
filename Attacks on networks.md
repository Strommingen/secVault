## LAN attacks
#### VLAN Hopping Attacks.

#### VLAN Double-Tagging Attack

### Address Spoofing Attack

## other?

#### C2
Command and Conquer.
A high amount of DNS calls in the network traffic might indicate C2 activity.

**This is because:**

- **Malware often uses DNS** queries to communicate with C2 servers.
- Attackers may use **dynamic DNS** services to change IP address associated with a domain frequently, this is because it is harder to block. If DNS calls are going to domains that change often or appear suspicious that could mean it is C2 activity.
- **High volume of unique domains** might indicate that the compromised system is attempting to communicate with multiple C2 servers or resolve multiple command locations.
- **Unusual Domains** that look strange or is not something normally present in the network might also be a sign of C2.
- **Non-Standard Ports**: If DNS requests are being made on non-standard ports or using unusual DNS record types (like TXT or CNAME), it could indicate attempts to communicate covertly with a C2 server.

 **Steps for Analysis:**

- **Check the DNS Response**: Look at the responses to these DNS queries to see if they resolve to known malicious IP addresses.
- **Analyze Traffic Patterns**: Examine the timestamps and source/destination IP addresses of the DNS queries for patterns that might indicate automated behavior typical of malware.
- **Correlate with Other Traffic**: Cross-reference the DNS traffic with other protocols (like HTTP or HTTPS) to see if there are corresponding outbound connections to suspicious domains or IPs.
- **Use Threat Intelligence**: Utilize threat intelligence feeds to identify known malicious domains or C2 infrastructure.