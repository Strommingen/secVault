## DAI
#CCNA 
Dynamic ARP inspection (DAI)

## IPSG
#CCNA 
IP Source Guard (IPSG)

## BPDU Guard
#CCNA 
Configured on switchports that are not trunking ports. Access ports should have BPDU Guard on. This is to mitigate a [[Attacks on networks#STP Attack|STP Attack]] even if they are not intential. 

## Port Security
#CCNA 
![[Misc#Note on Layer 2 devices]]

### Mitigate MAC Address Table Attacks
#CCNA 
To mitigate [[Attacks on networks#MAC Address Table Flooding|these]] types of attacks a simple and effective way is to enable port security. Port security limits the number of valid MAC addresses allowed on a port. Also allows administrators to manuallt configure MAC addresses for a port or to permit the switch to dynamically learn a limited number of MAC addresses.

When a port that is configured with port security recieves a frame, it is compared to the list of secure source MAC addresses that were manually configured or dynamically learned on the port.

### Enabling Port Security
#CCNA 
*This section is specifically for Cisco proprietary devices, I do not know if all this is also true for other devices.*

