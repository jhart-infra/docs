# Kubernetes on Talos + Argo CD

## Overview
Building a production-grade Kubernetes cluster on Talos Linux with GitOps automation and Ceph CSI-backed storage.

![Diagram](../assets/images/talos-argo-diagram.png)

## Architecture
- 3x control plane nodes  
- 3x worker nodes  
- CephFS for persistent volumes  
- Traefik ingress with Cloudflare DNS-01 certificates  

## Highlights
- Immutable OS + declarative config with Talos  
- Argo CD for continuous delivery  
- External Secrets Operator pulling from Azure Key Vault  

## Lessons Learned
- Tune Ceph pool replication and PG counts for latency.  
- Watch Argo sync windows for StatefulSets and CRDs.
