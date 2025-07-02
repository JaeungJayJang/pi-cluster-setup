# Planning & Design

## Project Goals

### Primary Objectives
- [x] Learn the fundamentals of clustering and distributed systems (Slurm, Kubernetes)
- [x] Experiment with both HPC and container orchestration approaches
- [x] Build, break, and rebuild a real hardware cluster from scratch
- [x] Develop practical DevOps and infrastructure automation skills
- [x] Create a flexible home lab for ongoing experimentation
- [x] Understand hardware limitations and capacity planning through hands-on experience

### Secondary Goals
- [x] Compare and contrast different orchestration tools (Slurm vs. K3s)
- [x] Optimize for cost, power, and scalability
- [x] Document the full journey—including failures, pivots, and lessons learned
- [x] Achieve a production-like, high-availability Kubernetes cluster
- [x] Reduce reliance on cloud costs by self-hosting applications
- [x] Enable future expansion and hybrid workloads (Pi 4 + Pi 5)

## Project Evolution

### My Cluster Journey: From Slurm to Kubernetes

#### Phase 1: The Slurm Experiment
I started my cluster journey with the idea of building a low-cost HPC cluster using Slurm and several Raspberry Pi 4 boards (originally 1GB RAM models). My experience with AWS T2.micro instances made me think this would be enough for basic clustering. While I was able to get the Pis working together, I quickly realized that Slurm wasn’t a good fit for my needs—I didn’t require CPU-intensive batch jobs or heavy computation. The project was a great way to showcase my skills and past experience, but it wasn’t practical for my day-to-day goals.

![Raspberry Pi Cluster Prototype](assets/images/me_frustrating_with_pi.png)
![Initial pi Cluster Setup](assets/images/initial_pi_cluster_with_pi4.JPG)

#### Phase 2: Rethinking the Use Case
Wanting to focus more on application development and the full DevOps lifecycle, I began exploring container orchestration. I discovered K3s, a lightweight Kubernetes distribution that’s perfect for small clusters and edge devices. This shift allowed me to simulate scaling, deploy real applications, and even reduce my reliance on paid cloud services like AWS (which was costing me $10/month for just a couple of T2.micro instances and static IPs).

#### Phase 3: The K3s Reality Check
Inspired by [Jeff Geerling’s K3s tutorials](https://www.youtube.com/watch?v=N4bfNefjBSw&list=PL2_OBreMn7Frk57NLmLheAaSSpJLLL90G&index=3), I tried installing K3s on my Pi 4 cluster. Unfortunately, 1GB RAM was nowhere near enough for a Kubernetes control plane. The master node couldn’t even start reliably, and I learned the hard way how important it is to plan hardware specs before buying.

#### Phase 4: Hardware Upgrade & Cluster Expansion
Realizing the hardware bottleneck, I decided to upgrade. I started by purchasing two Raspberry Pi 5 boards (8GB RAM each) to test K3s performance. Encouraged by the results, I added two more Pi 5s, building a cluster with one master and three workers, all fitting nicely in a tower case.

#### Phase 5: Finalizing the Production Cluster
There was a long period of inactivity on the rpi5 clusters due to my personal life that I decided to disassemble everything and even repurposed one pi 4 for OctoPi (3d printer server) and added another pi 5. I was still feeling toward using slurm for some unforseen events in the future but I decided that would not come and would probably better to go with some other solutions like running directly on my mac, desktop, or even cloud. As my focus shifted fully to Kubernetes and my focus is back to building pi cluster again, I start gathering what I have. This brought my cluster to its current state: 5× Pi 5 (8GB RAM) and 3× Pi 4 (1GB RAM), all freshly re-imaged and ready for a robust, production-like Kubernetes environment.

### Current Configuration
- **Total Nodes**: 8 (5×Pi5 + 3×Pi4)
- **Architecture**: High-availability control plane with mixed worker nodes
- **Use Case**: Production-like Kubernetes learning environment
- **Status**: Fully operational and hosting real applications

## Architecture Overview

### Cluster Topology
```
                    ┌─────────────────┐
                    │   Router/Switch │
                    └─────────┬───────┘
                              │
          ┌───────────────────┼───────────────────┐
          │                   │                   │
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│  Control Plane  │  │  Control Plane  │  │  Control Plane  │
│   pi5-master-1  │  │   pi5-master-2  │  │   pi5-master-3  │
└─────────────────┘  └─────────────────┘  └─────────────────┘
          │                   │                   │
          └───────────────────┼───────────────────┘
                              │
    ┌─────────────────────────┼─────────────────────────┐
    │                         │                         │
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│  Worker (Pi5)   │  │  Worker (Pi5)   │  │  Worker (Pi4)   │
│  pi5-worker-1   │  │  pi5-worker-2   │  │  pi4-worker-1   │
└─────────────────┘  └─────────────────┘  └─────────────────┘
          │                   │                   │
          └───────────────────┼───────────────────┘
                              │
          ┌───────────────────┼───────────────────┐
          │                   │                   │
┌─────────────────┐  ┌─────────────────┐
│  Worker (Pi4)   │  │  Worker (Pi4)   │
│  pi4-worker-2   │  │  pi4-worker-3   │
└─────────────────┘  └─────────────────┘
```

### Node Roles
- **Control Plane Nodes (3x Pi 5)**: High availability Kubernetes control plane, API server, scheduler, etcd
- **Worker Nodes (2x Pi 5)**: High-performance application workloads, pod execution
- **Worker Nodes (3x Pi 4)**: General application workloads, batch processing

## Hardware Planning

### Bill of Materials (BOM)

| Component | Quantity | Unit Cost | Total Cost | Notes |
|-----------|----------|-----------|------------|-------|
| Raspberry Pi 5 (8GB) | 5 | $80 | $400 | Control plane + high-perf workers |
| Raspberry Pi 4 (2GB) | 3 | $55 | $165 | General workload workers |
### Bill of Materials (BOM)

| Component | Quantity | Unit Cost | Total Cost | Notes |
|-----------|----------|-----------|------------|-------|
| Raspberry Pi 5 (8GB) | 5 | $80 | $400 | Control plane + high-perf workers |
| Raspberry Pi 4 (2GB) | 3 | $55 | $165 | General workload workers |
| MicroSD Card (64GB) | 8 | $12 | $96 | SanDisk Extreme Pro recommended |
| USB-C Power Supply | 8 | $15 | $120 | Official Pi Foundation preferred |
| Ethernet Cables (1ft) | 8 | $5 | $40 | Cat6 for gigabit speeds |
| Gigabit Switch (16-port) | 1 | $60 | $60 | Managed switch for 8+ devices |
| Cluster Case/Rack | 1 | $80 | $80 | For 8-node organization and cooling |
| **Total** | | | **$961** | |

### Performance Expectations

#### Single Node Specs (Pi 5)
- **CPU**: 4-core ARM Cortex-A76 @ 2.4GHz
- **RAM**: 8GB LPDDR4X
- **Storage**: 64GB microSD (Class 10/U3)
- **Network**: Gigabit Ethernet

#### Single Node Specs (Pi 4)
- **CPU**: 4-core ARM Cortex-A72 @ 1.8GHz
- **RAM**: 2GB LPDDR4
- **Storage**: 64GB microSD (Class 10/U3)
- **Network**: Gigabit Ethernet

#### Cluster Aggregate
- **Total CPU Cores**: 32 (5×Pi5×4 cores + 3×Pi4×4 cores)
- **Total RAM**: 46GB (5×8GB + 3×2GB) - with OS overhead ~36GB usable
- **Network Bandwidth**: 8Gbps theoretical
- **Power Consumption**: ~120W total

## Network Design

### IP Address Scheme
```
Network: 192.168.86.0/24
Gateway: 192.168.86.1 (Router)

Control Plane Nodes:
- pi5-master-1: 192.168.86.33
- pi5-master-2: 192.168.86.29
- pi5-master-3: 192.168.86.30

Worker Nodes (Pi 5):
- pi5-worker-1: 192.168.86.32
- pi5-worker-2: 192.168.86.34

Worker Nodes (Pi 4):
- pi4-worker-1: 192.168.86.27
- pi4-worker-2: 192.168.86.26
- pi4-worker-3: 192.168.86.23
```

### DNS Configuration
- **Hostname Pattern**: `pi{5|4}-{role}-{number}.local`
- **Internal DNS**: Consider Pi-hole for local resolution
- **External Access**: Dynamic DNS or VPN for remote access

## Software Stack Planning

### Operating System
- **Choice**: Raspberry Pi OS Lite (64-bit)
- **Rationale**: Official support, optimized for Pi hardware
- **Alternative**: Ubuntu Server 22.04 LTS (better K8s support)

### Container Runtime
- **Primary**: containerd (Kubernetes default)
- **Alternative**: Docker (for development/testing)

### Orchestration Platform
- **Choice**: Kubernetes (k3s distribution)
- **Rationale**: Lightweight, Pi-optimized, production-ready
- **Version**: Latest stable (1.28+)

### Monitoring & Observability
- **Metrics**: Prometheus + Grafana
- **Logs**: Fluentd/Fluent Bit + ELK Stack
- **Tracing**: Jaeger (optional)

## Timeline & Milestones

### Phase 1: Foundation (Week 1-2) ✅ COMPLETED
- [x] Hardware procurement and assembly
- [x] OS installation and basic configuration
- [x] Network setup and connectivity testing
- [x] SSH access and security hardening

### Phase 2: Cluster Setup (Week 3-4) ✅ COMPLETED
- [x] Initial 4-node Pi 4 cluster setup
- [x] Kubernetes cluster initialization
- [x] Node joining and verification
- [x] Basic networking (CNI) configuration

### Phase 3: Cluster Expansion (Week 5-6) ✅ COMPLETED
- [x] Procurement of 5 Pi 5 8GB units
- [x] Hardware integration and testing
- [x] High-availability control plane setup
- [x] Mixed architecture worker node configuration

### Phase 4: Production Services (Week 7-8) 🔄 IN PROGRESS
- [ ] Ingress controller deployment
- [ ] Certificate management (cert-manager)
- [ ] Monitoring stack installation
- [ ] Backup and recovery procedures

### Phase 5: Applications & Optimization (Week 9-10)
- [ ] Sample application deployment
- [ ] CI/CD pipeline setup
- [ ] Load testing and optimization
- [ ] Documentation completion

## Risk Assessment

### Technical Risks
- **SD Card Failure**: High wear on storage
  - *Mitigation*: High-quality cards, regular backups
- **Power Issues**: Instability from inadequate power
  - *Mitigation*: Official power supplies, UPS consideration
- **Thermal Throttling**: Performance degradation from heat
  - *Mitigation*: Proper ventilation, monitoring
- **Mixed Architecture Complexity**: Pi 4 vs Pi 5 differences
  - *Mitigation*: Node labeling, workload placement strategies

### Project Risks
- **Complexity Underestimation**: Learning curve steeper than expected
  - *Mitigation*: Incremental approach, community resources
- **Hardware Limitations**: Pi constraints affecting goals
  - *Mitigation*: Realistic expectations, alternative solutions

## Success Criteria

### Technical Metrics
- [ ] Cluster uptime > 99% over 1 month
- [ ] All nodes joining cluster successfully
- [ ] Sample applications running reliably
- [ ] Monitoring and alerting functional

### Learning Objectives
- [x] Comfortable with kubectl and Kubernetes concepts
- [x] Understanding of distributed systems principles
- [x] Experience with infrastructure as code
- [x] Troubleshooting and debugging skills developed
- [x] High-availability cluster management
- [ ] Mixed-architecture workload optimization

---

*Planning completed: June 30, 2025*
*Last updated: July 1, 2025*
*Next: [Hardware Setup](hardware.md)*
