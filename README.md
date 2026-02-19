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
<img width="967" height="1080" alt="Screenshot (73)" src="https://github.com/user-attachments/assets/94769862-06d5-4dea-82e2-0fcd6d5cd1e4" />
<img width="959" height="1080" alt="Screenshot (74)" src="https://github.com/user-attachments/assets/48e4f31c-4021-460b-9ed9-a375e56373b5" />
<img width="962" height="1080" alt="Screenshot (75)" src="https://github.com/user-attachments/assets/3afb3c70-e008-46dc-b0aa-5a7961f55c95" />

---

### Creating DHCP Scopes

Scopes were created to represent each subnet in the network. Each scope defines an address pool and usage policy.
<img width="1290" height="1080" alt="Screenshot (76)" src="https://github.com/user-attachments/assets/9598d536-f490-48f1-bfe9-9a3f50ca2213" />

#### Users LAN Scope

- Name: Users LAN
- Range: 192.168.1.10 – 192.168.1.200
- Subnet mask: 255.255.255.0
- Exclusion range: 192.168.1.10 – 192.168.1.20
- Lease duration: 8 hours
- Scope activated

This scope supports employee workstations while reserving infrastructure addresses inside the same subnet.
<img width="1290" height="1080" alt="Screenshot (77)" src="https://github.com/user-attachments/assets/0924189c-fed4-4a80-b9b6-3c8fd4909458" />
<img width="1266" height="1080" alt="Screenshot (78)" src="https://github.com/user-attachments/assets/22aca301-7b70-4fb2-b325-3f78876b6a92" />

---

#### Corporate Voice Scope
<img width="1279" height="1080" alt="Screenshot (79)" src="https://github.com/user-attachments/assets/3ea44f2b-a20a-4a73-bb5a-99f390546947" />

- Name: Corporate Voice
- Range: 192.168.2.10 – 192.168.2.150
- Exclusion range: 192.168.2.10 – 192.168.2.30
- Lease duration: 1 day
- Scope activated

This subnet simulates VOIP infrastructure and phone deployments.
<img width="1279" height="1080" alt="Screenshot (80)" src="https://github.com/user-attachments/assets/4812eda3-dfd2-467d-9f7c-4b0594d174b3" />
<img width="1279" height="1080" alt="Screenshot (81)" src="https://github.com/user-attachments/assets/7b96380e-85b9-467a-ad2f-329ee399e367" />

---

#### Guest Wi-Fi Scope
<img width="1279" height="1080" alt="Screenshot (82)" src="https://github.com/user-attachments/assets/f9d5434f-91e1-48b6-aa37-678101bae72a" />

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
<img width="1285" height="1080" alt="Screenshot (83)" src="https://github.com/user-attachments/assets/b185ad96-a226-4001-99ab-a41ea20e1ea0" />

---

### Creating DHCP Reservations

A reservation was created to demonstrate centralized IP assignment for infrastructure devices.

Example reservation:

- Name: Printer-01
- IP Address: 192.168.1.15
- MAC Address: placeholder value

Reservations provide consistent addressing while maintaining centralized management.
<img width="1290" height="1080" alt="Screenshot (84)" src="https://github.com/user-attachments/assets/513c0a8d-e8e4-4ef0-93c8-141a5d77ff47" />
<img width="1282" height="1080" alt="Screenshot (85)" src="https://github.com/user-attachments/assets/bf891e11-c83f-4b4b-96cb-083246024fc3" />

---

### Validating DHCP Configuration

Server-side validation confirmed:

- All scopes were active
- <img width="1279" height="1080" alt="Screenshot (86)" src="https://github.com/user-attachments/assets/cb4c86fb-5757-43bc-90bd-172586fe27ee" />
- Exclusions were within scope ranges
-<img width="1285" height="1080" alt="Screenshot (87)" src="https://github.com/user-attachments/assets/4152ff99-1bef-4b2c-a90f-16de26c26ee7" />
-<img width="1277" height="1080" alt="Screenshot (88)" src="https://github.com/user-attachments/assets/a632dba1-9bd8-45ea-887f-b43f174b76b9" />
-<img width="1277" height="1080" alt="Screenshot (89)" src="https://github.com/user-attachments/assets/d5a20fc0-8102-4b41-8c9f-66b5fef18826" />
- DHCP options were correctly configured
- <img width="1271" height="1080" alt="Screenshot (90)" src="https://github.com/user-attachments/assets/ad31732b-80cd-4ce6-8146-bfb1746ae00e" />
- Reservations appeared under their respective scopes
- <img width="1285" height="1080" alt="Screenshot (91)" src="https://github.com/user-attachments/assets/daff90f3-4aab-48eb-8b2e-2e23f88f065b" />

- DHCP services were running normally
<img width="1271" height="1080" alt="Screenshot (92)" src="https://github.com/user-attachments/assets/c8d0d7a1-bd42-45aa-a7e2-c40c9c9f174f" />

No client devices were required to verify configuration accuracy.

---

### Key Takeaways

- Scopes represent network subnets and define address pools
- Exclusions protect infrastructure IPs within scopes
- Lease duration should match device usage patterns
- Reservations allow centralized static addressing
- DHCP configuration can be validated server-side

---

