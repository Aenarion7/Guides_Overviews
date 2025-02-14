## Cloud DNS
![image](https://github.com/user-attachments/assets/7f628bdb-a4e7-44ea-a046-503ef732cbf5)

- DNS is hosted in a Project (per-project basis)
- Involves creating managed zones for DNS entries
  
**Public Zone**
- Records visible from the Internet
  
**Private Zone**
- Records not visible from the Internet
  
**Forwarding Zone**
- Forwards queries to another DNS server outside GCP
- When forwarding queries to another DNS server within GCP, use **DNS peering**



### ZONE Types:

**Private Zones**
- Records available only to selected VPCs (the ones they are assigned to)
- Allows mapping your own domains to private IP addresses without public visibility
- Supports integration with Google Cloud Services (e.g., you can create a private DNS for *.googleapis.com)

**How it works:**
  
1. You create a Private DNS Zone and assign it to one or more VPC networks.
2. You can add DNS records, for example:
-           app.internal.example.com → 10.10.10.10
- Any machine in the assigned VPC network can resolve these names, but they are not accessible from outside the VPC.

  
**Public Zones:**
- A Public DNS Zone in GCP allows you to manage public DNS records that are visible across the entire Internet.

You can host your own domains and manage their DNS records in Google Cloud DNS.
Google Cloud DNS acts as a public authoritative DNS server for your domain.

**How does a Public Zone work:**
1. You register a domain (e.g., example.com) with an external domain registrar.
2. You create a Public DNS Zone in Cloud DNS and define records, for example:
-          www.example.com → 35.190.10.1
-          mail.example.com → 74.125.200.26
3. You configure the name servers (NS) in your domain registrar to point to Google Cloud DNS.

Now your domain is publicly available—anyone in the world can resolve its name.

**Forwarding Zones:**
- A Forwarding Zone allows you to redirect DNS queries to a DNS server located outside of GCP.
- It works similarly to conditional DNS forwarding, meaning it only forwards queries for specific domains.
- It can be used for communication between GCP and on-premises or other environments.

**How does a Forwarding Zone work:**
1. You create a Forwarding DNS Zone and specify a domain, for example corp.example.com.
2. You define the external DNS servers to which the queries should be forwarded.

Machines in the VPC send queries for corp.example.com to the specified DNS server, e.g., on-premises.

**When to use a Forwarding Zone:**
- When you have a hybrid environment and want GCP to resolve DNS names located on-premises.
- When you want GCP to forward queries for a specific domain to another DNS server.
- When you use your own DNS system, e.g., Active Directory DNS on-prem.
