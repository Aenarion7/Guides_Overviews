## **How can redundancy be ensured for an on-prem connection? And how can routing between regions be achieved?**

**Scenario:**
Red - What you wont to achieve

![image](https://github.com/user-attachments/assets/f2ad476b-9c5c-4909-9427-4927860ba8ff)

**Technical Assumptions:**
  - One VPC
  - Three regions
  - One on-premises connection
  - Cloud interconnect from on-prem to one region
    
**Steps/Solution:**

1. Configure VPC routing in global mode. This will enable communication betwean VM instances across regions in same VPC.

2. Add an additional Cloud Interconnect VLAN attachment in a second region that does not yet have a VLAN attachment and Configure a Cloud Router. This will add redundancy to the on-prem.

**Additional Description:**

VPC Routing Global Mode:
VPC (Virtual Private Cloud) Routing Global Mode is a routing mode in Google Cloud that allows traffic to be routed between regions within a single VPC without requiring additional networking services (e.g., VPN, Cloud Interconnect, or Cloud Router). In this mode, VM instances and other network resources in different regions can communicate natively.
