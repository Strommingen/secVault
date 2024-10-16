#CCNA #defence  #l2 
![[Misc#Note on Layer 2 devices]]

### Mitigate MAC Address Table Attacks

To mitigate [[Attacks on networks#MAC Address Table Flooding|these]] types of attacks a simple and effective way is to enable port security. Port security limits the number of valid MAC addresses allowed on a port. Also allows administrators to manuallt configure MAC addresses for a port or to permit the switch to dynamically learn a limited number of MAC addresses.

When a port that is configured with port security recieves a frame, it is compared to the list of secure source MAC addresses that were manually configured or dynamically learned on the port.

### Enabling Port Security

*This section is specifically for Cisco proprietary devices, I do not know if all this is also true for other devices.*

Port security can only be configured on manually configured access ports or manually configured trunk ports. And by default, layer 2 switch ports are set to dynamic auto ([[VLAN#Trunking|trunking]] is on with [[Network Protocols#PaGP and LACP|PaGP]]). So just trying to enable it wont work.

#### Switch Learning MAC Addresses

There are three ways a switch can be configured to learn MAC addresses on a secure port:
1. **Manually configured**: the admin configures static MAC addresses.
2. **Dynamically learned**: the current source MAC for the connected device is automatically secured, but it is not added to the startup configuration. 
3. **Dynamically learned -- Sticky**: the switch learns MAC addresses dynamically and *sticks* them to the running configuration. So saving the running config will commit the MAC addresses to NVRAM.

#### Port Security Aging

Is used to set the *aging* time for both sstatic an dynamic secure addresses on a port. **Absolute** and **Inactivity** are the two types supported per port. 

- **Absolute:** Addresses are deleted after a specified aging time.
- **Inactivity:** Addresses are deleted only if they are inactive for a specified aging time.

#### Port Security Violation Modes

If a MAC address of a device attached to the port differs from the list of secure addresses then a port violation occurs. By default this makes the port enter error-disabled state.

The different violation modes:

- **Shutdown**: Port enters error-disabled immediately and turns of the port and sends a syslog message. Increments the violation counter. Admin must re-enable with *no shutdown*.
- **Restrict**: Port drops packets with unknown source addresses. This mode causes the Security Violation counter to increment and generates a syslog message.
- **Protect**: Least secure of these modes. Port drops packets with unknown MAC source addresses. No syslog message is sent and no increment is made to the counter.

