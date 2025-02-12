## How to set Harpin Scenario - Traffic to Internet from Cloud goes through on-prem. Traffic to Private Google API goes directly to Internet
**Scenario:** You want internet-bound traffic to exit through the on-premises network after passing through a specific firewall, while traffic to Private Google API (over private IP adresses, not public) should go directly from the cloud without passing through on-prem.

GCP → on-prem (VPN) → security inspection → Internet

GCP → DNS → Private Google API

![image](https://github.com/user-attachments/assets/6da1c14b-53fa-44f0-a9c4-871e574d6d62)

**Technical Assumptions:**
- VPN to the on-prem is set
- Traffic has to go same way on Ingress and Egress

**Steps/Solution:**

Normally, requests to Google APIs use *.googleapis.com, which resolves to public IP addresses by default.

To keep the traffic inside Google Cloud's private network, we override the DNS resolution so that requests to *.googleapis.com resolve to restricted.googleapis.com.
restricted.googleapis.com is mapped to private IP addresses within Google Cloud instead of public IPs.

This ensures that API traffic never leaves the Google Cloud network and remains protected by VPC Service Controls.

1. Create a private DNS zone in Cloud DNS.
2. Add a CNAME record mapping *.googleapis.com → restricted.googleapis.com
3. Add an A record pointing restricted.googleapis.com to Google’s private API IP range.

Even though DNS resolution now maps API traffic to a private IP, we need to ensure that the correct route is used.
To do this, we create a more specific custom IP route that sends all traffic destined for Google's private API IP range directly to GCP's default internet gateway (instead of routing it through on-prem).

4. Create a more specific custom route sending that restricted IP range to the default internet gateway as next hop.

Without this custom route, traffic could still follow the default on-prem routing, sending API calls unnecessarily through a VPN tunnel.
By explicitly defining the next hop as the default internet gateway in GCP, we make sure that traffic stays inside Google Cloud.

By using private DNS and a custom IP route, we ensure that Google API traffic stays within Google Cloud, avoiding unnecessary on-prem routing.

5. Lastly set default route to on prem for all traffic (API traffic route will be more precise so it will take precedence)
   - and delete default internet gateway:
    
          gcloud compute routes create default-to-onprem \
          --network=my-vpc \
          --destination-range=0.0.0.0/0 \
          --next-hop-vpn-tunnel=my-ha-vpn-tunnel \
          --priority=100

         gcloud compute routes delete default-internet-gateway


**Final Effect:**

API traffic stays private (within Google Cloud).
No unnecessary routing through on-prem, reducing latency and dependency on VPN tunnels.
Enhanced security using VPC Service Controls.
