# edgeos_fiber7
Ubiquiti EdgeOS router configuration for the Swiss FTTH provider [Fiber7](https://fiber7.ch). Including VLANs (Guest, Internal, Mgmt), IPv6 and Firewall

## Hardware

* Ubiquiti edge router x sfp: EdgeOS v1.9.1
* 2x Ubiquiti Networks 2.4GHz/5GHz, 867Mbit, 24V Passiv PoE, UAP-AC-LITE: v3.8.3
* UniFi Switch 8 US-8: v3.8.3

## Firewall

### WANv6_IN

Allow traffic from external (Internet) to the internal network. 
Only accept established/related sessions for connections which have SYN/ACK.
(started from our internal network)

### WANv6_LOCAL

Accept local connnections with the `icmpv6` protocol and allow internal DHCPv6 PD server and client connections.

### VLAN_IN

** unifi_devices **

### WAN_IN

### WAN_LOCAL


## Interfaces (including DHCPv6, VLANs)

### Ethernet

### Switch

### VIF


## Performance

Enable `offload { hwnat enable }` to boost your WAN to LAN performance to 1Gbit up/down.

