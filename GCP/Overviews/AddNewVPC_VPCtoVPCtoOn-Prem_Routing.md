
## **How establish communication between VPC-1 and new VPC and VPN Tunnel to on-prem?**
![image](https://github.com/user-attachments/assets/c9e43d13-8704-4524-b184-3818e28737d5)
Red: steps and required communication

**Scenario:**

  - How to configure a new VPC to communicate with an existing VPC and use the existing HA-VPN tunnel?
  - Internet-bound traffic from on-premises should not pass through the cloud, thats why configure default gateway on-prem to not VPN next hop.
  - Since the HA-VPN tunnel is already configured in the existing VPC, the new VPC must establish connectivity with the existing one.

**Technical assumptions:**

2 VPCs, no overlaping addresses.
One VPC connected to on-prem trough HA-VPN.

**Steps/Solution:**

1. Peer both VPCs
    - Configure VPC Network Peering to export custom routes from VPC-1.
    - Configure VPC Network Peering to import custom routes into New-VPC
2. Advertise IP ranges to the on-prem in existing VPC
    - Use Cloud Router’s custom route advertisements to announce the peered VPC network ranges to the on-premises locations.
3. Create a Custom Route in the New VPC
    - Set up a custom next-hop route so that all on-prem-bound traffic in the new VPC is forwarded via the existing VPC.
4. VPC firewall rules must allow traffic between the new VPC, the existing VPC, and on-prem.
    - Add firewall rules to allow traffic for subnets used in both VPCs.

**Additional Description:**

**Final Result:**
- The new VPC is connected to the existing VPC via VPC Peering.
- Traffic from the new VPC can reach on-prem via the existing HA-VPN tunnel.
- On-prem can recognize and return traffic to the new VPC, thanks to Cloud Router advertisements.
- Firewall rules allow smooth communication across all networks.

Alternative Approach: Using VPC Network Connectivity Center. See other Overview in this folder
- If VPC Peering is not preferred, you can use VPC Network Connectivity Center to establish a hub-and-spoke model.
- This allows multiple VPCs to connect via a central hub without requiring full VPC Peering.
