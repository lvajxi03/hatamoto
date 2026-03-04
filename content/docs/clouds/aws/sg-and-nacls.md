+++
date = '2025-12-14T14:14:23+02:00'
title = 'Security Groups and NACLs'
menus = 'main'
type = 'page'
+++

## Security Groups and NACLs

| Security Group | Network ACL |
| --- | --- |
| Operates at the instance (interface) level | Operates at the subnet level |
| Supports allow rules only | Supports allow and deny rules |
| Stateful | Stateless |
| Evaluates all rules | Process rules in order |
| Applies to an instance only if associated with a group | Automatically applies to all instances in the subnets it's associated with |


### What Is a NACL (Network Access Control List) in AWS?

An AWS Network ACL is a stateless firewall that controls traffic entering and leaving a subnet in your VPC.

Think of a NACL as a set of inbound and outbound rules that apply at the subnet level, whereas Security Groups apply at the instance / ENI level.

### Key Characteristics

1️⃣ Stateless

* Every rule is evaluated independently for inbound and outbound directions.
* If inbound traffic is allowed, you must also allow outbound return traffic → rules don’t remember previous connections.
* This is the biggest difference from Security Groups (which are stateful).

2️⃣ Subnet-Level Firewall

A NACL is attached to a subnet, and the rules apply to all resources in that subnet:

* EC2 instances
* RDS
* Lambda ENIs
* Elastic Beanstalk instances
* ECS on EC2

3️⃣ Rules Have Numbers (Order Matters)

Rules are evaluated in ascending order, starting from the lowest number:

```
Rule #100  allow TCP 80
Rule #200  deny all
```

Once a match is found → stop evaluating.

4️⃣ Default NACL

* Allows all inbound and all outbound traffic.
* Good for testing, not for production.

5️⃣ Custom NACL

* Deny all inbound and outbound by default.
* You explicitly allow the traffic you want.

🧩 Use Cases
✔️ Add an extra layer of subnet-level protection

Especially for public-facing subnets.

✔️ Block specific IP ranges

Example: block abusive IPs before they reach EC2 instances.

✔️ High-security environments

Banks, fintech, gov workloads often use NACLs to enforce strict routing.

✔️ Protect legacy systems

Often used in tandem with security groups for multi-layered defense.


### Quick Comparison with Security Groups

| Feature                       | NACL                                   | Security Group                |
| ----------------------------- | -------------------------------------- | ----------------------------- |
| **Applies to**                | Subnet                                 | ENI / instance                |
| **Stateful?**                 | ❌ No                                   | ✅ Yes                         |
| **Rules evaluated**           | In order, first match wins             | All rules evaluated           |
| **Default behavior (custom)** | Deny all                               | Allow all outbound            |
| **Use case**                  | Block/allow traffic at subnet boundary | Instance-level access control |


A common AWS pattern:

```
Internet → NACL → Security Group → EC2
```

🔧 Example NACL Rules

Inbound (Allow HTTP)

```
Rule #: 100
Protocol: TCP
Port Range: 80
Source: 0.0.0.0/0
ALLOW
```

Outbound (Allow HTTP return traffic)

```
Rule #: 100
Protocol: TCP
Port Range: 1024-65535
Destination: 0.0.0.0/0
ALLOW
```

🛠️ Example AWS CLI
Create a custom NACL

```
aws ec2 create-network-acl --vpc-id vpc-123456
```

Add inbound rule
```
aws ec2 create-network-acl-entry \
  --network-acl-id acl-abc123 \
  --ingress \
  --rule-number 100 \
  --protocol tcp \
  --port-range From=80,To=80 \
  --cidr-block 0.0.0.0/0 \
  --rule-action allow
```

Add outbound rule
```
aws ec2 create-network-acl-entry \
  --network-acl-id acl-abc123 \
  --egress \
  --rule-number 100 \
  --protocol tcp \
  --port-range From=1024,To=65535 \
  --cidr-block 0.0.0.0/0 \
  --rule-action allow
```

Associate it with a subnet
```
aws ec2 associate-network-acl \
  --network-acl-id acl-abc123 \
  --subnet-id subnet-123456
```

### Security Group Chaining

Security group chaining is an AWS design pattern where one Security Group (SG) allows inbound traffic from another SG, rather than from specific IP addresses or CIDR ranges.
This creates a logical chain of trust between resources.

#### What Is Security Group Chaining?

It means using one SG as the source in the inbound rules of another SG.

Example:

* SG-frontend allows traffic from the Internet (port 443)
* SG-backend allows traffic only from SG-frontend, not from the Internet
* SG-database allows traffic only from SG-backend

This effectively creates a chain:

```
Internet → SG-frontend → SG-backend → SG-database
```

Each hop is controlled at the SG level.


🔗 Simple Example of a Chain
SG-frontend

```
Inbound: allow 443 from 0.0.0.0/0
```

SG-backend
```
Inbound: allow 8080 from SG-frontend
```

SG-database
```
Inbound: allow 3306 from SG-backend
```

Traffic must match the entire chain to reach the DB.

#### Why Use Security Group Chaining?
✔️ Least Privilege

Only the exact components that should communicate can do so.

✔️ Dynamic scaling

If Auto Scaling launches new EC2 instances:

they get the SG automatically

permission rules remain correct

no need to manage IPs

✔️ Zero trust between layers

Front-end, back-end, and database tiers are isolated.

✔️ No hardcoding of IPs

Security groups refer to security groups — not IP addresses — which avoids breakage when instances change.

🔧 Example AWS CLI Rule (SG referencing SG)

Allow backend SG to accept traffic from frontend SG on port 8080:

```
aws ec2 authorize-security-group-ingress \
  --group-id sg-backend \
  --protocol tcp \
  --port 8080 \
  --source-group sg-frontend
```

#### What Security Group Chaining Is Not

It’s not:

* Network routing
* NAT or proxying
* NACL rule sequencing
* Firewall packet inspection

It is simply permission linking between SGs.

Summary:

| Concept                 | Meaning                                         |
| ----------------------- | ----------------------------------------------- |
| Security Group Chaining | Allowing one SG to reference another SG         |
| Purpose                 | Control traffic by *resource role*, not IP      |
| Benefits                | Scaling, least privilege, clean architecture    |
| Used in                 | Multi-tier apps, ALB → EC2 → RDS, microservices |


