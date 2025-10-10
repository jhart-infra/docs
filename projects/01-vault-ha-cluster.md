# Project: Vault HA Cluster on Proxmox

**Date:** July 2025  
**Category:** Infrastructure Automation  
**Stack:** Terraform, Ansible, HashiCorp Vault, Proxmox

---

## 🎯 Goal
Build a production-grade, HA Vault cluster using Terraform and Ansible, integrated with Azure Key Vault for secrets management.

## 🧩 Architecture
- 3 Vault VMs (Terraform-managed)
- Raft storage backend
- TLS termination via NGINX reverse proxy
- Secrets synced from Azure Key Vault

![Vault Architecture](../assets/diagrams/vault-ha.png)

## ⚙️ Automation Flow
1. **Terraform** provisions the VMs.
2. **Ansible** installs Vault and configures Raft.
3. Root token and unseal keys are stored in Azure Key Vault.
4. Certificates auto-issued via Traefik with Cloudflare DNS challenge.

## ✅ Outcome
- 100% automated HA Vault cluster
- Zero manual secrets handling
- GitOps-ready configuration for future clusters

## 💡 Lessons Learned
- Ansible `serial: 1` is critical for Raft join stability.
- ExternalSecrets Operator simplifies secret rotation drastically.
