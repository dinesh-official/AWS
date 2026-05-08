# AWS RDS Local Developer Access Methods

This document explains common and secure ways for developers to access an AWS RDS database from their local machines.

## Overview

When an RDS instance is hosted inside a **private subnet**, it cannot be accessed directly from the internet. Several secure approaches are available to enable developer access.

---

## Available Access Methods

| Method | Works with RDS | Recommended | Security Level |
|--------|---------------|-------------|----------------|
| VPN | ✅ | ✅ | High |
| SSH Tunnel (Bastion) | ✅ | ✅ | High |
| Bastion EC2 (Direct) | ✅ | ✅ | High |
| SSM Port Forwarding | ✅ | ✅ | Very High |
| Public Access | ✅ | ⚠️ Limited | Medium |
| RDS Proxy | ✅ | Specific Use Cases | Medium |

---

## 1. VPN Access

**Architecture**

```text
Developer Laptop → VPN → VPC Internal Network → Private RDS
```

**Examples**

- AWS Client VPN
- OpenVPN
- WireGuard
- Tailscale

**Advantages**

- Secure access to internal AWS resources
- Works like internal network access
- Scalable for teams
- Centralized access management

**Disadvantages**

- Initial setup complexity
- Ongoing VPN maintenance required

**Best For**

- Medium to large teams
- Enterprise environments

---

## 2. SSH Tunnel Through Bastion EC2

**Architecture**

```text
Developer Laptop → SSH Tunnel → Public Bastion EC2 → Private RDS
```

**Start Tunnel**

```bash
ssh -L 5544:RDS-ENDPOINT:5432 ubuntu@EC2-IP
```

**Local Connection String**

```text
postgresql://USER:PASSWORD@localhost:5544/DATABASE_NAME
```

**Advantages**

- Easy to configure
- Secure
- Low cost
- Industry standard

**Disadvantages**

- Manual tunnel setup
- SSH session must remain active

**Best For**

- Startups
- Small teams
- Temporary developer access

---

## 3. Bastion EC2 (Direct Access)

**Architecture**

```text
Developer → SSH → Bastion EC2 → Private RDS
```

Developers SSH into the EC2 bastion and run DB tools from there.

**Advantages**

- Simple infrastructure
- Secure internal access

**Disadvantages**

- Not convenient for GUI tools
- Requires terminal-only workflows

**Best For**

- Admin or debug access

---

## 4. AWS Systems Manager (SSM) Port Forwarding

**Architecture**

```text
Developer Laptop → AWS SSM → Private EC2 → Private RDS
```

**Advantages**

- No public SSH access required
- IAM-based authentication
- Session auditing
- Highly secure

**Disadvantages**

- Slightly advanced setup
- Requires SSM agent and configuration

**Best For**

- Secure production environments
- Enterprise infrastructure

---

## 5. Publicly Accessible RDS

**Architecture**

```text
Internet → Public RDS
```

**Requirements**

- Public subnet
- Public accessibility enabled
- Security group rules configured

**Advantages**

- Simplest direct connection
- Easy developer onboarding

**Disadvantages**

- Increased security risk
- Exposed to internet scanning
- Requires strict IP whitelisting

**Security Rule — Allowed**

```text
Developer-IP/32
```

**Security Rule — Never Use**

```text
0.0.0.0/0
```

**Best For**

- Temporary debugging
- Non-production testing

---

## 6. RDS Proxy

**Architecture**

```text
Application/Developer → RDS Proxy → RDS
```

**Advantages**

- Connection pooling
- Better scalability
- Improved failover handling

**Disadvantages**

- Additional AWS cost
- More architectural complexity

**Best For**

- High-traffic applications
- Large production workloads

---

## Recommended Architectures

### Small Teams / Startups

```text
Private RDS + Bastion EC2 + SSH Tunnel
```

### Enterprise / High Security

```text
Private RDS + VPN or SSM + IAM Access Control
```

---

## Security Recommendations

### ✅ Do

- Keep RDS in a private subnet
- Use bastion, VPN, or SSM for access
- Restrict security groups to minimal CIDR ranges
- Use strong passwords
- Enable SSL connections

### ❌ Avoid

- Public RDS with open internet access
- `0.0.0.0/0` on database ports
- Shared database credentials

---

## Common Database Ports

| Database | Default Port |
|----------|--------------|
| PostgreSQL | 5432 |
| MySQL | 3306 |
| MariaDB | 3306 |
| SQL Server | 1433 |
| Oracle | 1521 |

---

## Example SSH Tunnel Workflow (PostgreSQL)

**1. Start the tunnel**

```bash
ssh -L 5544:RDS-ENDPOINT:5432 ubuntu@EC2-IP
```

**2. Connect locally**

```text
postgresql://USER:PASSWORD@localhost:5544/DATABASE_NAME
```

---

## Conclusion

For most teams, the safest and most practical approach is:

```text
Private RDS + Bastion EC2 + SSH Tunnel
```

This provides:

- Strong security
- Low cost
- Easy developer access
- Minimal infrastructure complexity


# Ways to Access Private AWS RDS Locally

* VPN Access (AWS Client VPN / OpenVPN / WireGuard / Tailscale)
* SSH Tunnel through Bastion EC2
* Bastion EC2 Direct Access
* AWS SSM Port Forwarding
* Publicly Accessible RDS
* RDS Proxy
* AWS Cloud9
* EC2 with GUI DB Tools Installed
* Tailscale / ZeroTier Mesh VPN
* AWS Direct Connect
* Site-to-Site VPN
* Kubernetes Port Forwarding (`kubectl port-forward`)
* Temporary Security Group IP Whitelisting
* Database Access API Layer
* PgBouncer / Connection Gateway
* Remote Desktop into Private Network
* Jump Server Architecture
* VPC Peering Access
* Transit Gateway Networking
* Reverse SSH Tunnel
* SOCKS5 Proxy through EC2
* SSH Config-based Persistent Tunnel
* AWS Verified Access
* Zero Trust Access Platforms
* Internal Admin Dashboard Access
* PrivateLink-based Access
