# VMware Cloud Migration Project

## 🧭 Overview
This project involved migrating **100 virtual machines (VMs)** from **vCenter 7.0** to **VMware Cloud**, while redesigning the network and security architecture to meet enterprise-grade standards.  
The migration included integration of **Cisco FMC**, **FortiManager**, **FortiGate**, and **NSX** for a secure, segmented environment supporting **SAP Development, QA, and Production** workloads.

---

## 🏗️ Project Scope

| Component | Description |
|------------|--------------|
| **Source Environment** | vCenter 7.0 (on-premises datacenter) |
| **Target Environment** | VMware Cloud |
| **VMs Migrated** | 100 total (SAP Dev, QA, Prod, and supporting systems) |
| **Security Tools** | Cisco FMC/FTD, FortiManager/FortiGate |
| **Network Virtualization** | VMware NSX |
| **Connectivity** | IPsec tunnel between FortiGate and Cisco FTD |
| **Firewall Management** | Centralized via FortiManager and NSX DFW |

---

## 🔧 Implementation Steps

### 1. Pre-Migration Assessment
- Collected VM inventory and dependencies.
- Validated VMware Tools and hardware compatibility.
- Verified storage, CPU, and RAM utilization.

### 2. VMware Cloud Preparation
- Configured target VMware Cloud SDDC.
- Established vCenter Hybrid Linked Mode (HLM).
- Deployed NSX networking segments for DMZ, internal, and management traffic.

### 3. Network & Security Configuration
- Created and applied NSX **distributed firewall (DFW)** rules.
- Defined FortiManager firewall policies for internal/external segmentation.
- Configured **DMZ zones** for SAP Dev, QA, and Production tiers.
- Integrated NSX security groups for workload-based microsegmentation.

### 4. IPsec VPN Configuration
- Built IPsec tunnel between **FortiGate** and **Cisco FTD** (managed by FMC).
- Defined IKEv2/IPsec proposals on both sides.
- Verified phase 1 and phase 2 selectors.
- Tested bidirectional connectivity and failover behavior.

### 5. VM Migration
- Executed migration waves in batches (Dev → QA → Prod).
- Validated each migration using vMotion and replication testing.
- Verified DNS, routing, and firewall rules post-migration.

### 6. Post-Migration Validation
- Confirmed SAP application connectivity and user access.
- Monitored NSX and FortiGate logs for dropped traffic.
- Documented topology and security posture.

---

## 🔒 Security Architecture

```mermaid
graph TD
    subgraph On-Prem
    A[FortiGate Firewall] -- IPsec --> B[Cisco FTD]
    end
    subgraph VMware Cloud
    C[NSX DFW] --> D[DMZ - SAP Dev]
    C --> E[DMZ - SAP QA]
    C --> F[DMZ - SAP Prod]
    end
