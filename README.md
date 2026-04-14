# Azure Administrator Portfolio

Azure cloud administration and security labs. AZ-104 certified, Terraform certified, CompTIA Security+. Each lab is built from a realistic business scenario and documented with architecture, design decisions, and real troubleshooting from the actual build.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/srm2k/)

---

## Certifications

| Certification | Status |
|:--|:--|
| Microsoft AZ-104: Azure Administrator Associate | Earned |
| Terraform Associate 004 | Earned |
| CompTIA Security+ | Earned |
| Google Cybersecurity Professional Certificate | Earned |

---

## Skills at a Glance

| Domain | Skills |
|:--|:--|
| **Compute** | ![VM Deployment](https://img.shields.io/badge/VM%20Deployment-0078D4?style=flat-square&logoColor=white) ![Availability Zones](https://img.shields.io/badge/Availability%20Zones-0078D4?style=flat-square&logoColor=white) ![Disk Management](https://img.shields.io/badge/Disk%20Management-0078D4?style=flat-square&logoColor=white) ![Snapshots](https://img.shields.io/badge/Snapshots-0078D4?style=flat-square&logoColor=white) |
| **Networking** | ![Hub-Spoke Topology](https://img.shields.io/badge/Hub--Spoke%20Topology-0078D4?style=flat-square&logoColor=white) ![VNet Peering](https://img.shields.io/badge/VNet%20Peering-0078D4?style=flat-square&logoColor=white) ![NSG Segmentation](https://img.shields.io/badge/NSG%20Segmentation-0078D4?style=flat-square&logoColor=white) ![UDR Forced Tunnelling](https://img.shields.io/badge/UDR%20Forced%20Tunnelling-0078D4?style=flat-square&logoColor=white) ![Azure Bastion](https://img.shields.io/badge/Azure%20Bastion-0078D4?style=flat-square&logoColor=white) ![Load Balancer](https://img.shields.io/badge/Load%20Balancer-0078D4?style=flat-square&logoColor=white) ![NAT Gateway](https://img.shields.io/badge/NAT%20Gateway-0078D4?style=flat-square&logoColor=white) ![Private Endpoints](https://img.shields.io/badge/Private%20Endpoints-0078D4?style=flat-square&logoColor=white) ![Linux NVA](https://img.shields.io/badge/Linux%20NVA-0078D4?style=flat-square&logoColor=white) ![Network Watcher](https://img.shields.io/badge/Network%20Watcher-0078D4?style=flat-square&logoColor=white) |
| **Storage** | ![Blob Storage](https://img.shields.io/badge/Blob%20Storage-0078D4?style=flat-square&logoColor=white) ![Storage Redundancy](https://img.shields.io/badge/Storage%20Redundancy-0078D4?style=flat-square&logoColor=white) ![Lifecycle Policies](https://img.shields.io/badge/Lifecycle%20Policies-0078D4?style=flat-square&logoColor=white) ![WORM Immutability](https://img.shields.io/badge/WORM%20Immutability-0078D4?style=flat-square&logoColor=white) ![SAS Tokens](https://img.shields.io/badge/SAS%20Tokens-0078D4?style=flat-square&logoColor=white) ![Azure Files](https://img.shields.io/badge/Azure%20Files-0078D4?style=flat-square&logoColor=white) |
| **Security** | ![Zero-Trust](https://img.shields.io/badge/Zero--Trust-0078D4?style=flat-square&logoColor=white) ![RBAC](https://img.shields.io/badge/RBAC-0078D4?style=flat-square&logoColor=white) ![Entra ID](https://img.shields.io/badge/Entra%20ID-0078D4?style=flat-square&logoColor=white) ![Defender for Storage](https://img.shields.io/badge/Defender%20for%20Storage-0078D4?style=flat-square&logoColor=white) ![Storage Firewall](https://img.shields.io/badge/Storage%20Firewall-0078D4?style=flat-square&logoColor=white) ![IP Forwarding](https://img.shields.io/badge/IP%20Forwarding-0078D4?style=flat-square&logoColor=white) |
| **Operations** | ![Azure Backup](https://img.shields.io/badge/Azure%20Backup-0078D4?style=flat-square&logoColor=white) ![Log Analytics](https://img.shields.io/badge/Log%20Analytics-0078D4?style=flat-square&logoColor=white) ![KQL](https://img.shields.io/badge/KQL-0078D4?style=flat-square&logoColor=white) ![Azure Monitor Agent](https://img.shields.io/badge/Azure%20Monitor%20Agent-0078D4?style=flat-square&logoColor=white) ![Cost Optimization](https://img.shields.io/badge/Cost%20Optimization-0078D4?style=flat-square&logoColor=white) ![Resource Tagging](https://img.shields.io/badge/Resource%20Tagging-0078D4?style=flat-square&logoColor=white) |

---

## Labs

### [Lab 01: Virtual Machines and Compute](./01-virtual-machines/)

> **Scenario:** A software company migrating two legacy on-premises servers to Azure. Requirements: no public IP on any VM, automated backups with a 4-hour recovery objective, and high availability with automatic failover.

**What was built and demonstrated:**

- **Cross-zone VM deployment** (Windows Server 2022 in Zone 1, Ubuntu 22.04 in Zone 2) with no public IP; all admin access through Azure Bastion over HTTPS
- **Standard Load Balancer** with HTTP health probes across both zones; failover tested live by stopping one VM and confirming the probe removed it from rotation within 2 minutes
- **Azure Backup** with Enhanced policy (required for Trusted Launch VMs); on-demand backup triggered and restore point confirmed end-to-end
- **NAT Gateway** for controlled outbound internet from private VMs without exposing any inbound surface
- **OS disk snapshot** taken before VM resize as a rollback point; resize confirmed and VM validated post-operation
- **Subnet-level NSG** enforcing HTTP allow on port 80 with all other inbound denied by default

**Security angle:** No VM has a public IP. Bastion is the sole administrative access path. NSG applied at subnet scope so every VM in the subnet inherits the policy automatically. Trusted Launch enabled on both VMs.

---

### [Lab 02: Hub-Spoke Network with Zero-Trust Segmentation](./02-vnet-hubspoke/)

> **Scenario:** A company with three departments (IT, Finance, HR) requires Finance and HR to be completely isolated from each other, both need access to shared IT services in the hub, and all internet-bound traffic from department VMs must pass through a central inspection point.

**What was built and demonstrated:**

- **Four-VNet hub-spoke topology** with bidirectional VNet peering; spoke-to-spoke isolation enforced at the network layer through Azure's non-transitive peering behavior before any NSG rules are evaluated
- **Subnet-level NSGs** on Finance and HR with layered explicit deny rules at priority 2000, overriding Azure's default `AllowVnetInBound` at priority 65000; double enforcement: Finance outbound deny fires at source and HR inbound deny fires at destination simultaneously
- **UDR forced tunnelling** on both spoke subnets routing all traffic (0.0.0.0/0) through a Linux NVA at a static private IP (10.0.1.4); IP forwarding enabled at both the Azure NIC level and Linux kernel level, both required, neither alone is sufficient
- **Azure Network Watcher** used to verify all controls: Connection Troubleshoot returning named NSG deny rules, IP Flow Verify confirming exact rule names, Effective Routes showing the Default internet route as Invalid and the UDR as Active
- **Azure Monitor Agent + Data Collection Rules** (replacing the deprecated Diagnostics extension, retired March 31 2026) collecting Windows Event Logs from both spoke VMs into a hub Log Analytics workspace

**Security angle:** Finance and HR are isolated at two independent layers: network topology and NSG rules. Internet traffic is inspected before egress. Single Bastion in the hub reaches all spoke VMs via Standard SKU peering support. Every security control is verified with tooling output, not assumed.

---

### [Lab 03: Storage Accounts and Data Management](./03-storage-account-and-data-management/)

> **Scenario:** MediCore Diagnostics migrating their data platform to Azure. Three data categories: patient documents (PHI, geo-redundant, recoverable from overwrites), application logs (cost optimization via automated tiering), and compliance archives (immutable for 7 years, unmodifiable by anyone including admins).

**What was built and demonstrated:**

- **Two storage accounts with deliberate redundancy tiers:** LRS for regenerable application logs, RA-GRS for irreplaceable PHI with the secondary endpoint readable at all times without waiting for Microsoft's failover declaration
- **Infrastructure encryption** enabled at creation on both accounts (double-layer AES-256; cannot be changed after deployment)
- **Storage account keys disabled** on the patient account; all access forced through Entra ID with a full audit trail queryable in Log Analytics
- **WORM time-based retention policy** on the compliance archive container; delete attempt rejected at the storage platform level, confirmed with a screenshot of the exact error response
- **Lifecycle management policy** automating Hot -> Cool at 30 days and Cool -> Archive at 90 days using last-access-time tracking (more accurate than last-modified for log data that is written once but read repeatedly)
- **User Delegation SAS** (signed with Entra ID OAuth token, not a storage key) scoped to Read + List with HTTPS-only and 75-minute expiry; tested via REST API returning XML blob listing with `ServerEncrypted: true` on every entry
- **RBAC:** `Storage Blob Data Reader` (data-plane) assigned to an Entra ID user; the distinction from the built-in `Reader` role (control-plane only, grants zero access to blob data) is documented explicitly
- **Private endpoint** assigning the patient account a private IP (10.0.0.4) inside the VNet; DNS resolves to the private IP, the public endpoint surface eliminated entirely
- **Diagnostic logging + KQL:** blob operations routed to Log Analytics; query returning a mixed audit trail of `PutBlob` and `DeleteBlob` events with caller IPs, timestamps, and HTTP status codes

**Security angle:** PHI account has keys disabled, private endpoint eliminating the public surface, WORM preventing deletion by anyone for the retention period, Defender for Storage enabled for anomaly detection, and a full diagnostic audit trail queryable in KQL, satisfying HIPAA access logging requirements.

---

### Lab 04: Coming Soon

### Lab 05: Coming Soon

> Labs 04 and 05 are in progress. This portfolio is actively being built.

---

## Approach

Each lab is built from a business scenario with specific requirements, not a list of portal steps to follow. Design decisions are documented with the reasoning behind each choice: why Standard SKU over Basic, why RA-GRS over LRS, why subnet-level NSG over NIC-level. Issues encountered during the build are captured in troubleshooting sections with root cause and fix. Security controls are verified with Azure tooling (Network Watcher, IP Flow Verify, Effective Routes, KQL) rather than assumed to be working.
