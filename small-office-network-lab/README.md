<h3>Small Office Network Design and Implementation</h3>
<p>This project involved designing, configuring, and testing a simulated small-office Local Area Network (LAN) using Cisco Packet Tracer.

The network supported wired and wireless connectivity, automatic IP address allocation, internal DNS resolution, and an HTTP-hosted company webpage.</p>

## <h3>Network Design and Topology</h3>
The network consists of a router, a Cisco 2960 switch, a server, and a wireless access point. It supports ten client devices: four wired desktop PCs, four wireless desktop PCs, and two wireless laptops.

The router connects to the central switch, which links the server, access point, and four wired PCs. The access point connects the remaining six wireless clients.

All devices operate within a single IPv4 subnet.

## <h3>IP Addressing Scheme</h3>
The network uses the private IPv4 address range `192.168.1.0/24`.

| Device | IP address | Assignment |
|---|---|---|
| Router LAN interface | 192.168.1.1 | Static |
| Server | 192.168.1.2 | Static |
| Wired clients | 192.168.1.21 - 192.168.1.24 | DHCP |
| Wireless clients | 192.168.1.25 – 192.168.1.30 | DHCP |

**Subnet mask:** `255.255.255.0`

**Default gateway:** `192.168.1.1`

**DNS server:** `192.168.1.2`

The address range `192.168.1.1 – 192.168.1.20` was excluded from DHCP allocation to reserve addresses for network infrastructure and future use.

## <h3>Network Configuration</h3>
**Router and DHCP**

The router was assigned `192.168.1.1` and configured as the DHCP server. Its DHCP pool supplied all ten clients with IP addresses, the default gateway, and DNS server information.

## <h3>Server Configuration</h3>
The server was assigned the static IP address `192.168.1.2` and configured to provide the following services:
- **DNS**: The DNS service was enabled and configured with an A record mapping `www.office.local` to `192.168.1.2`, allowing clients to access the internal server using its hostname.
- **HTTP**: The HTTP service was enabled, and the default webpage was customised to display an internal company webpage titled Welcome to Our Office Network.

## <h3>Wireless Configuration</h3>
The access point was configured with the SSID `Office-WiFi` and WPA2-PSK security using AES encryption.

The wireless desktop computers and laptops required **WPC300N** wireless network interface modules before they could connect to the access point. After installation, all six wireless devices successfully connected and obtained their IP configurations through DHCP.

## <h3>Testing and Verification</h3>
The following tests were performed:
- **DHCP**: Verified the allocated client addresses, DHCP pool utilisation, and absence of detected IP address conflicts.
- **Connectivity**: Wired and wireless clients successfully communicated with the default gateway, server, and other client devices.
- **DNS**: Confirmed that `www.office.local` successfully resolved to `192.168.1.2`.
- **HTTP**: Accessed the internal webpage at `http://www.office.local`.
- **Wireless**: Confirmed successful wireless association and network access for the six wireless clients.

## <h3>Challenges and Troubleshooting</h3>
**Router interface**: The router-to-switch link was initially down because the router interface was administratively disabled. This was resolved by enabling the interface.
**Wireless interface requirement**: Packet Tracer required WPC300N wireless network interface modules for the desktop computers and laptops. The modules were installed through the devices' Physical tabs before establishing wireless connections.

## <h3>Documentation</h3>
- [Cisco Packet Tracer project file](small-office-network.pkt)
- [Screenshots](assets/) of the topology, configurations, and test results.

<i>Environment: Cisco Packet Tracer 9.0.1.</i>
