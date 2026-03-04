---
title: "Direct Connect (DX)"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---

# Direct Connect (DX)

AWS Direct Connect (DX) is a dedicated, private, high-speed network connection between your on-premises data center, office, or colocation facility and AWS.

Instead of sending traffic over the public Internet, Direct Connect gives you a layer-2 or layer-3 private circuit into AWS.

## Direct Connect benefits

* Enhanced security -- uses private connectivity between AWS and your data center/office
* Consistent network performance -- increased speed/latency & bandwidth/throughput
* Lower cost -- can reduce costs for organizations that transfer large volumes of data


## Key Benefits
✔️ More predictable performance

No Internet congestion → consistent latency.

✔️ Higher bandwidth options

1 Gbps, 10 Gbps, 100 Gbps (depending on location and partner).

✔️ Private, secure routing

Traffic stays off the public Internet.

✔️ Lower data transfer costs

Outbound data from AWS over Direct Connect is cheaper than Internet data transfer.

✔️ Integrates with AWS routing

Uses BGP to exchange routes with AWS.

## How Direct Connect Works (Simple Explanation)

* You request a DX connection in AWS.
* AWS provisions a physical port in a Direct Connect location (colocation facility).
* You or a partner (e.g., Equinix, Megaport) provide a cross-connect from your router to AWS's router.
* You configure BGP to exchange routes.
* You now have a private link into your VPCs through a Virtual Private Gateway or Transit Gateway.

## Direct Connect Virtual Interfaces (Important!)

Direct Connect only gives Layer-2 connectivity at first. To actually route traffic, you create virtual interfaces (VIFs):

🔸 Private VIF

Connects to:

* VPC via Virtual Private Gateway (VGW)
* or multiple VPCs via Transit Gateway (TGW)

Used for:

* private subnets
* EC2
* RDS
* internal workloads

🔸 Public VIF

Connects privately to AWS public services like:

* S3
* DynamoDB
* CloudFront

🔸 Transit VIF

* For large multi-VPC enterprises using Transit Gateway.

## Direct Connect vs VPN

| Feature        | Direct Connect               | Site-to-Site VPN            |
| -------------- | ---------------------------- | --------------------------- |
| **Path**       | Private circuit              | Public Internet             |
| **Speed**      | up to 100 Gbps               | up to ~1.25 Gbps per tunnel |
| **Latency**    | Very low, predictable        | Can fluctuate               |
| **Redundancy** | Multi-port, link aggregation | Two tunnels only            |
| **Cost**       | Higher                       | Cheap                       |


Often, companies combine them:

```
DX = primary  
VPN = backup failover
```

## Common Uses for Direct Connect

* Hybrid cloud architectures
* Low-latency database replication
* High-bandwidth data transfers (analytics, backups)
* Private connectivity to S3
* Large enterprises linking multiple sites to AWS

## Summary

AWS Direct Connect is a private, dedicated network connection between your data center and AWS, offering higher performance, lower latency, better security, and reduced data transfer costs compared to Internet-based VPN.
