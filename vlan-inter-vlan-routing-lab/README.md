<h3>VLANs and Inter-VLAN Routing</h3>
<p>This project involved designing, configuring, and testing a simulated company network using Cisco Packet Tracer. The network separates three departments (HR, Finance, and IT) into VLANs.

The design demonstrates departmental network segmentation, inter-VLAN routing, subnetting, and the use of centralised network services across multiple subnets.

The network consists of a router, a Cisco 2960 switch, six PCs, and a server providing DHCP, DNS, and HTTP services.</p>

## <h3>Network Design</h3>
The network represents a small company with three departments:

| Department | VLAN | Network |
|---|---:|---|
| HR | 10 | `192.168.2.0/26` |
| Finance | 20 | `192.168.2.64/26` |
| IT | 30 | `192.168.2.128/26` |

The overall LAN address block is `192.168.2.0/24`, divided into four `/26` subnets. Three were assigned to the departmental VLANs, leaving the fourth subnet, `192.168.2.192/26`, unassigned to a VLAN.

Each department has two PCs. The server is connected to VLAN 30 and provides network services to all three departments. Keeping the server in the IT VLAN maintained the original project scope without introducing a separate server VLAN.

## <h3>VLAN Configuration and Inter-VLAN Routing</h3>
Three VLANs were created on the Cisco 2960 switch, with the appropriate access ports assigned to each department.

The switch-to-router connection was configured as an 802.1Q trunk, allowing traffic from all three VLANs to pass through a single physical connection.

Inter-VLAN routing was implemented using router-on-a-stick. Three subinterfaces were configured on the router's `FastEthernet 0/0` interface, each associated with a VLAN and assigned its corresponding default gateway.

| VLAN | Subinterface | Default Gateway |
|---|---|---|
| HR - 10 | Fa0/0.10 | `192.168.2.1` |
| Finance - 20 | Fa0/0.20 | `192.168.2.65` |
| IT - 30 | Fa0/0.30 | `192.168.2.129` |

This allows devices in different departmental VLANs to communicate through the router.

## <h3>IP Addressing and DHCP</h3>
The server was assigned the static IP address `192.168.2.130`. Three DHCP pools were configured to provide automatic IP address allocation for the departmental subnets.

| DHCP Pool | Starting IP | Default Gateway | DNS Server |
|---|---|---|---|
| HR_POOL | `192.168.2.4` | `192.168.2.1` | `192.168.2.130` |
| FINANCE_POOL | `192.168.2.68` | `192.168.2.65` | `192.168.2.130` |
| IT_POOL | `192.168.2.132` | `192.168.2.129` | `192.168.2.130` |

Since the DHCP server is in VLAN 30, only the router subinterfaces for VLANs 10 and 20 were configured with DHCP relay. This forwards DHCP requests from those subnets to the central server. Relay was unnecessary on VLAN 30 because its clients share the server's subnet.

## <h3>DNS and HTTP Services</h3>
The server was configured with DNS and HTTP services.

An A record was created to map `www.office.local` to the server's IP address `192.168.2.130`.

The default HTTP webpage was customised to create a simple internal company portal for a fictional company called NEXUS. This provided a practical way to test hostname resolution and webpage access across the three departmental VLANs.

## <h3>Testing and Verification</h3>
The following tests were performed to verify network connectivity and service availability:
- **DHCP allocation:** Confirmed that all six PCs received the correct IP addresses, subnet masks, default gateways, and DNS server addresses.
- **Default gateway connectivity:** Verified communication between clients and their respective gateways.
- **Inter-VLAN connectivity:** Tested communication between devices across all three departmental VLANs.
- **Server connectivity:** Confirmed that clients in all three VLANs could reach the server.
- **DNS resolution:** Used `nslookup` to verify that `www.office.local` resolved to `192.168.2.130` from all three VLANs.
- **HTTP access:** Opened the NEXUS internal company portal from all three VLANs using `http://www.office.local`.

All tests were successful after troubleshooting the DNS configuration issue described below.

## <h3>Challenges and Troubleshooting</h3>

- **DHCP Pool Overlap**: The built-in `serverPool` in Packet Tracer could not be removed or edited through the interface. Its original address range overlapped with the IT subnet, interfering with DHCP allocation for IT clients.
  
  This was resolved by changing the default pool's starting address to `192.168.2.192`, placing its scope in the fourth `/26` subnet that was unassigned to a VLAN.

- **DNS Configuration**: During testing, HR and Finance clients successfully resolved the internal hostname, but IT clients could not.
  
  Comparing the IP configurations of the working and failing clients revealed an incorrect DNS server address in the IT DHCP pool. It had been entered as `192.186.2.130` instead of `192.168.2.130`.
  
  After correcting the address and renewing the DHCP leases in VLAN 30, DNS resolution and HTTP access worked successfully across all three VLANs.

## <h3>Project Files</h3>
- [Cisco Packet Tracer Project File](vlan-and-inter-vlan-routing.pkt)
- [Screenshots of the network topology, configurations, and test results](assets/).

<i>Environment: Cisco Packet Tracer 9.0.1</i>
