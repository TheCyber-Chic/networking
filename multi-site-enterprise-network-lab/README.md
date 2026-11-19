<h3>Multi-Site Enterprise Network Design and Implementation</h3>
<p>This project expands the earlier small-office network and VLAN implementation into a three-site enterprise network using Cisco Packet Tracer. It connects the Lagos headquarters to branch offices in Abuja and Port Harcourt.

The design brings together departmental network separation, communication between sites, shared network services, wireless access for personal devices (BYOD), and basic access controls.</p>

## <h3>Network Design and Topology</h3>
The network uses a hub-and-spoke layout, with Lagos at the center. Each branch connects to Lagos through its own WAN link, so traffic between Abuja and Port Harcourt passes through the headquarters.

| Site | Role | LAN Network |
|---|---:|---|
| Lagos | Headquarters | `192.168.10.0/24` |
| Abuja | Branch 1 | `192.168.20.0/24` |
| Port Harcourt | Branch 2 | `192.168.30.0/24` |

Each site has a Cisco 2911 router, a Cisco 2960 switch, and a wireless access point. A central server at the Lagos headquarters provides DHCP, DNS, and HTTP services to all three locations.

## <h3>IP Addressing and VLAN Design</h3>
Each site has its own `/24` address range, divided into smaller networks for departments and BYOD devices. Lagos also has a separate network for the central server.

**Lagos - Headquarters (LAG)**
| VLAN | Department / Purpose | Network | Default Gateway |
|---|---|---|---|
| 10 | BYOD | `192.168.10.192/28` | `192.168.10.193` |
| 20 | HR | `192.168.10.0/26` | `192.168.10.1` |
| 30 | Finance | `192.168.10.64/26` | `192.168.10.65` |
| 40 | IT | `192.168.10.128/26` | `192.168.10.129` |
| 50 | Server | `192.168.10.208/28` | `192.168.10.209` |

**Abuja - Branch 1 (ABJ)**
| VLAN | Department / Purpose | Network | Default Gateway |
|---|---|---|---|
| 10 | Legal | `192.168.20.0/26` | `192.168.20.1` |
| 20 | Administration | `192.168.20.64/26` | `192.168.20.65` |
| 30 | BYOD | `192.168.20.128/28` | `192.168.20.129` |

**Port Harcourt - Branch 2 (PHC)**
| VLAN | Department / Purpose | Network | Default Gateway |
|---|---|---|---|
| 10 | Sales | `192.168.30.0/26` | `192.168.30.1` |
| 20 | Support | `192.168.30.64/26` | `192.168.30.65` |
| 30 | BYOD | `192.168.30.128/28` | `192.168.30.129` |

The first usable address in each subnet serves as its default gateway. Departmental networks use `/26` subnets, while BYOD and server networks use smaller `/28` subnets.

The two WAN links use separate `/30` networks:
| WAN Link | Network | Lagos Address | Branch Address |
|---|---|---|---|
| Lagos – Abuja | `192.168.100.0/30` | `192.168.100.1` | `192.168.100.2` |
| Lagos – Port Harcourt | `192.168.100.4/30` | `192.168.100.5` | `192.168.100.6` |

## <h3>Network Configuration</h3>
**VLANs and Trunking**
Each switch has separate VLANs for its departments and BYOD devices. Switch ports connect devices to the correct VLAN, while an 802.1Q trunk link carries the site's VLAN traffic between the switch and router. The Lagos server also connects to its own dedicated VLAN.

**Inter-VLAN Routing**
Router subinterfaces provide communication between VLANs at each site. Each subinterface acts as the default gateway for its VLAN.

**WAN Connectivity and Static Routing**
Static routes connect the site networks. Lagos provides the path between the two branches, and each branch uses Lagos to reach remote networks.

During testing, additional routes were needed on the branch routers so they could reach the other branch's WAN network through Lagos. The routing updates also provided the return paths needed for successful communication.

## <h3>Centralized Network Services</h3>
The central server at the Lagos headquarters uses the static address `192.168.10.210/28`, with `192.168.10.209` as its default gateway.

It provides three services across the network:
- **DHCP**: Ten address pools assign IP addresses to wired departmental devices and wireless BYOD clients at all three sites.
- **DNS**: The record `www.office.local` points to the central server at `192.168.10.210`.
- **HTTP**: The server hosts an internal web page that users can open through `http://www.office.local`.

DHCP relay settings on the routers forward address requests from the branch networks to the central server in Lagos.

## <h3>Wireless BYOD Configuration</h3>
Each site has a wireless access point connected to its BYOD VLAN.
| Site	| SSID	| VLAN |
|---|---|---|
| Lagos	| `LAG-BYOD`	| 10 |
| Abuja	| `ABJ-BYOD`	| 30 |
| Port Harcourt	| `PHC-BYOD`	| 30 |

The access points use WPA2-PSK with AES encryption. Laptops and smartphones connect to their site's wireless network and receive their IP settings automatically through DHCP.

## <h3>Network Security and Access Control</h3>
Extended Access Control Lists (ACLs) apply basic traffic restrictions to the BYOD networks. They prevent BYOD devices from accessing departmental networks while allowing them to use the central DNS and HTTP services. Access to other resources on the server network is restricted.

The ACLs provide stateless packet filtering. They add a basic layer of network control but do not replace a stateful firewall or a complete Zero Trust security model.

## <h3>Testing and Verification</h3>
Testing covered network connectivity, address assignment, shared services, and BYOD access restrictions across all three sites.
- Confirmed VLAN assignments and trunk links on all switches.
- Tested communication between VLAN gateways and across the WAN links.
- Confirmed that wired and wireless clients received the correct IP settings from the central DHCP server.
- Used nslookup to verify that `www.office.local` resolved to `192.168.10.210` from each site.
- Opened the internal web page through `http://www.office.local` from clients at all three sites.
- Tested BYOD access to departmental networks, central services, and other BYOD networks against the configured ACL rules.

## <h3>Challenges and Troubleshooting</h3>
The first WAN tests showed that both branch routers could reach Lagos but could not reach each other's WAN interfaces.

The branch routers had routes to the remote LANs, but they lacked routes to the opposite branch's /30 WAN network. Adding the missing routes through Lagos resolved the issue and allowed the WAN interface tests to succeed.

This highlighted the importance of checking both the route to a destination and the return path when diagnosing network connectivity.

## <h3>Project Files</h3>
- [Packet Tracer Project File](multi-site-enterprise-network.pkt)
- [Project screenshots and configuration evidence](assets/)

<i>Environment: Cisco Packet Tracer 9.0.1</i>
