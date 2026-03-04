---
title: "BGP"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---

## BGP

What Is BGP?

BGP (Border Gateway Protocol) is the routing protocol that connects networks on the Internet.
It decides how data travels between autonomous systems (AS) — large networks like ISPs, cloud providers, enterprises, and universities.

Think of BGP as the postal system of the Internet, choosing the best path for packets to travel across thousands of networks.

🚀 What Does BGP Actually Do?
✔️ Decides which route is best

Often based on:
* AS path length
* Local preference
* Multi-exit discriminator (MED)
* Origin type
* Router ID

✔️ Ensures global Internet reachability

Without BGP, networks could not:

* Tell other networks which IP ranges they own
* Decide how traffic enters/exits their network

✔️ Provides redundancy and failover

If one ISP connection goes down, BGP reroutes traffic through another path.

🧩 Where Is BGP Used?

🔹 The entire Internet

* All inter-ISP and inter-cloud routing is BGP.

🔹 Cloud networking

Services like:

* AWS Direct Connect
* AWS Transit Gateway
* Azure ExpressRoute
* Google Cloud Interconnect
* …all rely on BGP to exchange routes between cloud and on-prem.

🔹 Data centers

* Large enterprises use iBGP/eBGP for routing internally and externally.

🔹 Any multi-homed network

* Organizations with more than one ISP use BGP for intelligent routing.

⚠️ Why Is BGP Important (and sometimes dangerous)?

* BGP lacks authentication by default.
* This means misconfigurations or malicious announcements can cause enormous outages.

Examples:

* 2008: Pakistan accidentally took down all of YouTube by bad BGP routes
* 2021: A faulty Facebook BGP update disconnected Facebook from the entire Internet

This is why BGP is both powerful and fragile.

📦 BGP in AWS (Quick Summary)

AWS uses BGP for:

* Site-to-Site VPN routing
* Direct Connect routing
* Transit Gateway dynamic route exchange
* Redundant connections (failover between tunnels)

In these scenarios:
* Your on-prem router and AWS form a BGP peering session
* BGP advertises routes dynamically
* Failover happens automatically

### Summary

| Concept      | Meaning                                             |
| ------------ | --------------------------------------------------- |
| **BGP**      | Protocol that routes traffic between large networks |
| **AS / ASN** | A network + its ID number                           |
| **eBGP**     | Between different organizations                     |
| **iBGP**     | Inside one organization                             |
| **Purpose**  | Choose best path for data across the Internet       |
| **Used in**  | ISP routing, cloud networking, VPNs, Direct Connect |

### How BGP Works in AWS Site-to-Site VPN

AWS Site-to-Site VPN uses BGP (Border Gateway Protocol) to dynamically exchange routes between:

* Your on-premises router (customer gateway, CGW)
* AWS Virtual Private Gateway (VGW) or Transit Gateway (TGW)

BGP is what allows AWS VPNs to do:

* automatic failover,
* dynamic route advertisement,
* scaling without manual route edits.


#### Two VPN Tunnels = Two BGP Sessions

When you create a Site-to-Site VPN in AWS, AWS gives you:

* 2 IPsec tunnels
* Each tunnel runs one BGP session

This looks like:

```
On-prem router
     │
 [IPsec Tunnel 1] — BGP over tunnel
     │
     ├── VGW/TGW
     │
 [IPsec Tunnel 2] — BGP over tunnel
     │
```

Each tunnel has:

* Inside IPs (169.254.x.x)
* A BGP ASN for AWS
* Your ASN (you set it)

AWS recommends using both tunnels for redundancy.

#### How Routing Works With BGP on AWS VPN

##### AWS advertises routes to you

AWS sends:

* The VPC CIDR (e.g., 10.0.0.0/16)
* Routes attached to the Virtual Gateway or Transit Gateway

So your on-prem router learns:
```
10.0.0.0/16 → via BGP tunnel
```

##### You advertise your on-prem networks to AWS

You send:

* Your internal networks (e.g., 192.168.0.0/16)
* Additional prefixes you want reachable from AWS

AWS then populates your VPC route table with:
```
192.168.0.0/16 → via VPN attachment
```

(when using a VGW)

#### How Failover Works Automatically

Because there are two tunnels:

* If Tunnel 1 goes down
* BGP session on Tunnel 1 drops
* AWS automatically selects Tunnel 2 for all routing

No manual action required.

Failover time: typically sub-30 seconds (varies by vendor timers).

This is the #1 reason AWS recommends using BGP instead of static routes.

#### BGP in VGW vs Transit Gateway

VGW (Virtual Private Gateway)

* BGP routes go directly into the VPC route table
* Simpler, but one-VPC-per-gateway

TGW (Transit Gateway)

* BGP routes go into a TGW route table
* From there they propagate to attached VPCs
* Much more scalable (hub-and-spoke)

TGW is now the recommended option for multi-VPC topologies.

#### Example BGP Configuration (Conceptual)

AWS gives you parameters such as:
```
Local ASN: 64512 (AWS)
Customer ASN: 65000
Inside tunnel IPs: 169.254.21.2/30 and 169.254.21.1/30
Tunnel BGP Peer IP: 169.254.21.1
```

Your on-prem config might say:

```
router bgp 65000
  neighbor 169.254.21.2 remote-as 64512
  network 192.168.0.0 mask 255.255.0.0
```

AWS advertises:

```
network 10.0.0.0/16
```

#### Route Propagation on AWS Side

Depending on the gateway type…

If using Virtual Private Gateway

In your VPC Route Table:
```
Destination       Target
192.168.0.0/16    vpn-123abc       (from BGP)
```

If using Transit Gateway

In TGW route table:
```
192.168.0.0/16 → VPN attachment
```

Then it’s automatically propagated to VPCs (if propagation is enabled).

#### Why AWS Uses BGP for VPN

| Feature                  | Explanation                                                         |
| ------------------------ | ------------------------------------------------------------------- |
| **Failover**             | Detects tunnel failure automatically                                |
| **Dynamic routing**      | No need to manually manage static routes                            |
| **Scalability**          | Easily add/remove prefixes without editing routing tables           |
| **Multi-pathing**        | Use both tunnels for load sharing (depending on AS-path/local-pref) |
| **Integration with TGW** | Large-scale routing between many VPCs and on-prem                   |

Static routing is still supported but not recommended for production.

#### Troubleshooting BGP on AWS VPN
❌ BGP session is DOWN

* Incorrect ASN?
* Wrong inside tunnel IP?
* Firewall blocking TCP 179?
* Tunnel not established?

❌ Routes not showing up

* You forgot to advertise on-prem networks
* Prefix too large (AWS rejects > /16?)
* TGW route propagation disabled

❌ Only one tunnel active

* On-prem vendor may not support equal-cost multipath
* BGP timers may differ
* Health checks misconfigured

🎯 Summary (Easy to Remember)

AWS VPN uses BGP to automatically exchange routes between your network and AWS, and to provide seamless failover across two IPsec tunnels. BGP handles learning, advertising, and path selection—making the connection dynamic, resilient, and cloud-ready.