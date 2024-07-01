# Linux Networking Basics

## Overview
This document provides a comprehensive overview of fundamental concepts in Linux networking, including switching, routing, default gateway configuration, and DNS setup.

## Switching
Switching facilitates communication between systems within the same network. Consider Network A with an IP address range of 192.168.1.0/24:

### Example: Network A
#### System A
- `ip link`: Display network interfaces on the host.
- `ip addr add 192.168.1.10/24 dev eth0`: Assign an IP address to System A.
- `ping 192.168.1.11`: Test connectivity to System B.

#### System B
- `ip link`: Display network interfaces on the host.
- `ip addr add 192.168.1.11/24 dev eth0`: Assign an IP address to System B.
- `ping 192.168.1.10`: Test connectivity to System A.

Now, consider Network B with an IP address range of 192.168.2.0/24:

### Example: Network B
#### System C
- `ip link`: Display network interfaces on the host.
- `ip addr add 192.168.2.10/24 dev eth0`: Assign an IP address to System C.
- `ping 192.168.2.11`: Test connectivity to System D.

#### System D
- `ip link`: Display network interfaces on the host.
- `ip addr add 192.168.2.11/24 dev eth0`: Assign an IP address to System D.
- `ping 192.168.2.10`: Test connectivity to System C.

## Routing
Routing enables communication between different networks. A router acts as a gateway connecting these networks. 

For example, to allow System A in Network A to reach System C in Network B, the following routing commands can be used:

- `route`: Display existing routing configuration.
- `ip route add 192.168.2.0/24 via 192.168.1.1`: Route traffic from Network A to Network B.
- `ip route add 192.168.1.0/24 via 192.168.2.1`: Route traffic from Network B to Network A.
- `ip route add default via 192.168.2.1`: Set the default route for external network access.

### Complex Routing Scenario
Consider three systems: A, B, and C. Systems A and B are in the same network, and Systems B and C are also in the same network, but Systems A and C are in different networks. To enable communication between A and C through B, use the following commands:

- `ip route add 192.168.2.0 via 192.168.1.6`: Route from A to C via B.
- `ip route add 192.168.1.0 via 192.168.2.6`: Route from C to A via B.

To enable forwarding between networks on System B:
- Modify `/proc/sys/net/ipv4/ip_forward` and `/etc/sysctl.conf`:
  - Set the value to `1` to enable packet forwarding.

## DNS Configuration
DNS translates domain names to IP addresses. The order of DNS resolution can be configured in `/etc/nsswitch.conf`.

### DNS Server Configuration
1. Check the IP-to-hostname mapping in `/etc/hosts`.
2. Transfer this configuration to `/etc/resolv.conf`.
3. Ensure the system uses `/etc/hosts` first, followed by `/etc/resolv.conf` by configuring `/etc/nsswitch.conf`:
   ```
   hosts: files dns
   ```

### Domain Names
Common domain name categories:
- `.com`: Commercial (e.g., www.google.com, www.facebook.com)
- `.net`: Network (e.g., www.balance.net, www.speedtest.net)
- `.edu`: Educational Organizations (e.g., www.stanford.edu, www.mit.edu)
- `.org`: Non-profit Organizations (e.g., www.care.org, www.un.org)
- `.io`: Input/Output in computing (e.g., www.kubernetes.io, www.codepen.io)

Use the `search` directive in `/etc/resolv.conf` for subdomain addressing:
```
nameserver 192.168.1.100
search mycompany.com
```

### DNS Record Types
- `A` Record: Stores an IPv4 address (e.g., `web-server` 192.168.1.1).
- `AAAA` Record: Stores an IPv6 address (e.g., `web-server` 2001:0db8:85a3:0000:8a2e:0370:7334).
- `CNAME` Record: Maps one domain name to another (e.g., `food.web-server` to `eat.web-server`).

### Testing DNS Configuration
Use the following commands to test DNS settings:
- `nslookup`
- `dig`

This document outlines the basics of Linux networking, providing practical examples and commands to manage network interfaces, routing, and DNS configurations effectively.