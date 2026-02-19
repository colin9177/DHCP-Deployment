---

## DHCP Server Deployment and Scope Configuration

This section documents the installation and configuration of a DHCP server to simulate centralized IP address management in a corporate network environment.

The lab models a multi-subnet business network with separate address pools for users, voice devices, and guest access.

---

### Lab Network Design

The simulated corporate network includes three logical subnets:

- Users LAN: 192.168.1.0/24
- Corporate Voice: 192.168.2.0/24
- Guest Wi-Fi: 192.168.3.0/24

Infrastructure details:

- DHCP Server: dc1
- Default Gateway: .254 in each subnet
- DNS Server: dc1

In real enterprise networks, routers or Layer 3 switches would relay DHCP traffic from each subnet to the DHCP server.

---

### Installing the DHCP Server Role

The DHCP Server role was installed on the domain controller (dc1) using Server Manager.

Steps performed:

- Logged into dc1 and opened Server Manager
- Selected Manage → Add Roles and Features
- Chose Role-based or feature-based installation
- Confirmed dc1 as the installation target
- Selected the DHCP Server role and accepted required features
- Completed the installation wizard
- Verified DHCP appeared under Server Manager → Tools

At this stage, the DHCP role was installed but not yet configured to issue addresses.

---

### Creating DHCP Scopes

Scopes were created to represent each subnet in the network. Each scope defines an address pool and usage policy.

#### Users LAN Scope

- Name: Users LAN
- Range: 192.168.1.10 – 192.168.1.200
- Subnet mask: 255.255.255.0
- Exclusion range: 192.168.1.10 – 192.168.1.20
- Lease duration: 8 hours
- Scope activated

This scope supports employee workstations while reserving infrastructure addresses inside the same subnet.

---

#### Corporate Voice Scope

- Name: Corporate Voice
- Range: 192.168.2.10 – 192.168.2.150
- Exclusion range: 192.168.2.10 – 192.168.2.30
- Lease duration: 1 day
- Scope activated

This subnet simulates VOIP infrastructure and phone deployments.

---

#### Guest Wi-Fi Scope

- Name: Guest Wi-Fi
- Range: 192.168.3.50 – 192.168.3.250
- Exclusion range: 192.168.3.50 – 192.168.3.60
- Lease duration: 2 hours
- Scope activated

Short lease durations were used to support frequent device turnover.

---

### Configuring DHCP Scope Options

Each scope was configured with essential network options:

- Router (Default Gateway): 192.168.X.254
- DNS Server: IP address of dc1
- DNS Domain Name: lab.local

Although gateway devices were not physically present in the lab, the configuration reflects common enterprise IP schemes.

---

### Creating DHCP Reservations

A reservation was created to demonstrate centralized IP assignment for infrastructure devices.

Example reservation:

- Name: Printer-01
- IP Address: 192.168.1.15
- MAC Address: placeholder value

Reservations provide consistent addressing while maintaining centralized management.

---

### Validating DHCP Configuration

Server-side validation confirmed:

- All scopes were active
- Exclusions were within scope ranges
- DHCP options were correctly configured
- Reservations appeared under their respective scopes
- DHCP services were running normally

No client devices were required to verify configuration accuracy.

---

### Key Takeaways

- Scopes represent network subnets and define address pools
- Exclusions protect infrastructure IPs within scopes
- Lease duration should match device usage patterns
- Reservations allow centralized static addressing
- DHCP configuration can be validated server-side

---

