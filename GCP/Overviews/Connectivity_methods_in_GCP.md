## Connectivity methods in GCP

1. **VPC Peering** – A direct connection between two VPCs, enabling private communication. Does not support transit routing.
![image](https://github.com/user-attachments/assets/a8934778-d46b-4b7f-88f4-a7094f8373ad)
With VPC Peering there is no communication to the reasourcess connected to the peered VPC.
If comunication to  on-prem is needed, there needs to be VPN for example. Or mixed connectivity with BGP or Hub-and-Spoke

2. **VPC Network Connectivity Center (NCC)** – A central hub-and-spoke model for connecting multiple VPCs and on-premises networks, ideal for larger network architectures.
Transit routing is supported. It allows centrall management of traffic and security.

![image](https://github.com/user-attachments/assets/55698166-adb9-4491-861b-b763d9be155c)

Spokes are connected to the Hub. Spokes can be: VPC Networks, Cloude VPN, Cloud Interconnect, Router BGP on-prem
Connecting is done in NCC by creating Hub and connecting spokes to it as a reasource. Hub is not a VPC.

5. **Cloud VPN** – An encrypted connection between a VPC and another VPC or on-premises network via IPSec tunnels.

6. **Cloud Interconnect** – A dedicated physical connection (Dedicated/Partner Interconnect) for high performance and low latency.

7. **Hybrid Connectivity (Router-to-Router BGP)** – Enables dynamic routing between VPC networks and on-premises networks, often used with Cloud VPN or Interconnect.

8. **Shared VPC** – A shared VPC network where multiple GCP projects use a single central VPC without requiring peering.


