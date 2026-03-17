# 🖥️ Home Lab Proxmox — My Living Room Datacenter

![Status](https://img.shields.io/badge/status-running_24%2F7-brightgreen)
![Uptime](https://img.shields.io/badge/uptime-H24-blue)
![Stack](https://img.shields.io/badge/stack-Proxmox_·_Docker_·_Prometheus_·_MinIO-orange)
![Mood](https://img.shields.io/badge/mood-on_a_mission-purple)

---

## 🧠 What is this?
 
A physical server running **24/7** at my place — hosting real services used by real people.
 
My friends and I built websites and game servers that live on this machine. I'm the one who keeps the lights on: monitoring uptime, handling outages, managing access, and making sure nobody wakes up to a dead server on a Saturday morning.
 
Not a sandbox. Not a tutorial project. An actual production environment where downtime has real consequences and I'm the on-call engineer — whether I like it or not.
 
---
 
## 🏗️ Architecture
 
```
┌────────────────────────────────────────────────────────────┐
│                       PHYSICAL SERVER                      │
│                                                            │
│  ┌────────────────────────────────────────────────────┐    │
│  │                     PROXMOX VE                     │    │
│  │                                                    │    │
│  │  ┌───────────────────────────┐  ┌───────────────┐  │    │
│  │  │     VM — Ubuntu/Debian    │  │  VM — Windows │  │    │
│  │  │                           │  │   (gaming)    │  │    │
│  │  │  ┌─────────────────────┐  │  └───────────────┘  │    │
│  │  │  │       Docker        │  │                     │    │
│  │  │  │                     │  │                     │    │
│  │  │  │  ┌──────────────┐   │  │                     │    │
│  │  │  │  │  Prometheus  │   │  │                     │    │
│  │  │  │  │   Grafana    │   │  │                     │    │
│  │  │  │  │    MinIO     │   │  │                     │    │
│  │  │  │  └──────┬───────┘   │  │                     │    │
│  │  │  └─────────┼───────────┘  │                     │    │
│  │  │            │ journald     │                     │    │
│  │  └────────────┼──────────────┘                     │    │
│  │               │                                    │    │
│  │        Proxmox bridge (vmbr0)                      │    │
│  └────────────────────────────────────────────────────┘    │
└────────────────────────────────────────────────────────────┘
```

---

## ⚙️ Stack

| Component | Role | Why this choice |
|-----------|------|-----------------|
| **Proxmox VE** | Hypervisor | Open-source, VM + LXC management, clean web UI |
| **Docker** | Service containerization | Isolation, portability, fast deployment |
| **Prometheus** | Metrics collection | De facto standard in cloud-native observability |
| **Grafana** | Visualization & alerting | Real-time dashboards, native Prometheus integration |
| **MinIO** | S3-compatible object storage | Identical AWS S3 API, self-hosted, zero cloud dependency |
| **Linux** | Base OS | Because it's Linux. |
| **Bash** | Automation & scripting | Reusable scripts for user/group provisioning |

---

## 🔐 Access Management
 
Every service runs in its own isolated environment. Permissions are handled through **dedicated Linux users and groups per service**, provisioned via reusable Bash scripts.
 
No shared root account. No "give everyone sudo so it works" shortcuts. Principle of least privilege, applied from day one — even on a homelab.
