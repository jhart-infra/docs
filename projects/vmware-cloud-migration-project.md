# VMware Cloud Migration Project

## 🧩 Overview
Migrated 100+ VMs from vCenter 7.0 to VMware Cloud, including SAP Dev/QA/Prod workloads.

## 🎯 Objectives
- Migrate workloads with zero data loss
- Implement NSX firewall segmentation
- Build IPsec tunnel between FortiGate and Cisco FTD (FMC-managed)

## 🏗️ Architecture
![Architecture Diagram](images/architecture.png)

## ⚙️ Technologies
VMware Cloud, NSX, FortiManager, Cisco FMC/FTD, IPsec, DMZ Segmentation

## 🔧 Implementation Steps
1. Configure NSX Tier-1/Edge firewall rules  
2. Create FortiManager policies  
3. Deploy IPsec between sites  
4. Migrate workloads using VCDA  
5. Validate routing and segmentation  

## ✅ Results
- 100 VMs migrated successfully
- SAP workloads segmented by environment (Dev/QA/Prod)
- Full IPsec redundancy between FortiGate ↔ FMC FTD

## 💡 Lessons Learned
- NSX routes must be advertised to the upstream FortiGate
- VCDA replication requires port 31031/tcp open both directions

## 📂 Repo Contents
