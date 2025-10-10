# Monitoring Stack — Prometheus, Grafana, Loki

## Overview
End-to-end observability for Linux, Windows, and network infrastructure.

![Diagram](../assets/images/monitoring-stack-diagram.png)

## Components
- **Prometheus:** metrics collection  
- **Grafana:** visualization and alerting  
- **Loki + Promtail:** log aggregation  
- **Alertmanager:** notification routing  

## Highlights
- Unified dashboards across Kubernetes and bare metal  
- Windows and Node Exporters for host metrics  
- Slack + email alert integrations  

## Lessons Learned
- Build dashboards around SLOs, not just CPU graphs.  
- Tune scrape intervals for critical vs. batch workloads.
