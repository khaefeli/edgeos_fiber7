# edgeos_fiber7
Ubiquiti EdgeOS router configuration for the Swiss FTTH provider [Fiber7](https://fiber7.ch). Including VLANs (Guest, Internal, Mgmt), IPv6 and Firewall.
Optimized for 1Gbit WAN to LAN performance. Multiple VLANs to create a secure _#CHFreeWifi_ setup.
*Warning*: dont copy/paste. It will enable power-over-ethernet and could break our network devices.

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

Secure the networks between your VLANs (drop). But allow to access Unifi controller <> Unifi Devices and to the Router interfaces.

__rule 10__ Accept `Router_IPs` of our gateway's. List should be specified under `adress-group Router_IPs`
__rule 15__ Static whitelist to our Unifi controller. Specify the Unifi devices IPs (Switch, Wifi etc) under `address-group unifi_devices`
__rule 16__ Opposite direction of the rule 15
__rule 19__ Allow connection to the Unifi Wifi portal.
__rule 20__ Drop all other connections between VLANs.

### WAN_IN

See *WANv6_IN*


### WAN_LOCAL

See *WANv6_LOCAL*


## Interfaces (including DHCPv6, VLANs)

### Ethernet

__eth 0__ Normal port configuration.
__eth 1__ Normal port configuration.
__eth 2__ Enable power-over-ethernet (PoE) for the Unifi access-point. 
__eth 3__ Normal port configuration.
__eth 4__ Enable power-over-ethernet (PoE) for the Unifi access-point.
__eth 5__ WAN / Internet port with the SFP plugged. 

* IPv4 over DHCP
* IPv6 with DHCPv6 prefix-delegation (PD). __request your own /48 subnet from the init7 support__
* Prefix-id: add the missing 16 bits to announce a /64 to your internal network. Needed for SLAAC (Stateless Address Autoconfiguration).

### Switch

__VLAN 1__ Management network with range (192.168.0.0/24). 
_yes, should be 192.168.1 - but was to lazy to change all my internal devices which already had a 192.168.1 network)_

__VLAN  __ Internal network: secure infrastructure (NAS, Laptop, Smartphone etc) Network: 192.168.1.0/24

__VLAN 9__ Guest VLAN: .. ;) Network: 192.168.2.0/24

Assign the VLANs to the interfaces. Using trunk ports on `eth0 - eth4`.

* `pvid`: default / native VLAN (if not set it's always VLAN 1) for the untagged traffic. Set the printer interface to the internal VLAN only.
* `vid`: all traffic from the APs and the Switch is expected tagged (VLAN2 or 9).

### VIF

Add the gateway IPs to the virtual interface (VIF) of the VLANs. 

### port-forward

Some old-school port forwarding / NATing for devices and services which dont support IPv6 :(

## Performance

Enable `offload { hwnat enable }` to boost your WAN to LAN performance to 1Gbit up/down.

