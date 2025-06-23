Title: Secure Local Area Network Simulation Using Cisco Packet Tracer

Objective:  
To simulate a secure, scalable, and structured office network using Cisco Packet Tracer. This includes dynamic routing, access control, secure remote access, and basic monitoring features.

1. Network Design and Topology  
- Topology Type: Mesh  
- Devices:  
  • 3 Routers (R0, R1, R2)  
  • 3 Switches (S0, S1, S2)  
  • Multiple PCs connected to each switch  
  • One attacker PC for ACL simulation  
- Each router connects to a switch. Each switch connects to end-user PCs.  
- Routers are linked using GigabitEthernet interfaces in a closed loop.

2. IP Addressing Scheme  
LAN Subnets:  
  • R0 LAN: 192.168.10.0/24  
  • R1 LAN: 192.168.20.0/24  
  • R2 LAN: 192.168.30.0/24  

Router-to-Router Links (/30):  
  • R0 to R1: 10.0.0.0/30  
  • R1 to R2: 10.0.0.4/30  
  • R2 to R0: 10.0.0.8/30  

3. Cabling and Interfaces  
- Copper Straight-Through Cables: PC to Switch and Switch to Router  
- Copper Cross-Over Cables: Router to Router  
- All router interfaces were enabled using:  
  > no shutdown  

4. Routing Configuration (RIP)  
Each router:  
  > router rip  
  > version 2  
  > network <LAN subnet>  
  > network <inter-router subnet>  

RIP shares routes between routers for complete connectivity.  
Verify using:  
  > show ip route  

5. Password and Encryption Setup  
Set console and VTY passwords:  
  > line console 0  
  > password cisco  
  > login  

  > line vty 0 4  
  > password vtypass  
  > login  

Enable password encryption:  
  > service password-encryption  

Enable privileged EXEC password:  
  > enable secret class  

6. SSH Configuration for Secure Remote Access  
Each Router:  
  > hostname R1  (Change as needed)  
  > ip domain-name office.local  
  > username admin privilege 15 secret admin123  
  > crypto key generate rsa  
    (Choose key size: 1024)  
  > ip ssh version 2  

  > line vty 0 4  
  > login local  
  > transport input ssh  

Test SSH access:  
On command prompt:  
  > ssh -l admin <router_ip>
7. ACL (Access Control List) Implementation  
To block traffic from R2 (192.168.30.0/24) to R1 (192.168.20.0/24):  

On R1:  
  > access-list 20 deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255  
  > access-list 20 permit ip any any  
  > interface GigabitEthernet0/0 (link to R2)  
  > ip access-group 20 in  

Verify:  
  > show access-lists  
  > ping from R2 PC to R1 PC; this should be blocked  

8. Simulation of Unauthorized Access  
An attacker PC was added on R2’s side to simulate external access attempts to R1. ACLs blocked unauthorized communication, confirmed by ICMP unreachable messages and increases in deny counters shown in access-lists.  

9. Testing and Verification  
- Ping Tests between all allowed LANs  
- SSH Test using:  
  > ssh -l admin <router_ip>  
- ACL Verification via:  
  > show access-lists  
- RIP Verification:  
  > show ip route  

10. Future Work  
- VPN Tunneling (GRE) can be added to securely connect communication between routers while enforcing ACL-based restrictions.  
- Syslog Monitoring for centralized logging and diagnostics  
- HTTP Services for internal documentation or monitoring dashboards  
- DHCP Snooping and ARP Inspection for extra switch-level security  

Conclusion:  
This project shows the design and implementation of a secure office LAN environment using key networking concepts:  
  • Subnetting and IP planning  
  • RIP dynamic routing  
  • Encrypted SSH access  
  • Access control via ACLs  

It ensures layered security and reliable communication across departments, consistent with real-world IT network security practices.