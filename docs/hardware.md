# Hardware Setup

## Unboxing & Inventory

### Pre-Setup Checklist
- [ ] All Raspberry Pi boards received and inspected
- [ ] Power supplies tested and functional
- [ ] MicroSD cards verified (speed test recommended)
- [ ] Network cables tested for continuity
- [ ] Switch/router configuration access confirmed

### Initial Inspection
Document any issues found during unboxing:

| Component | Serial/Model | Condition | Notes |
|-----------|--------------|-----------|-------|
| Pi Board #1 | | ✅ Good | |
| Pi Board #2 | | ✅ Good | |
| Pi Board #3 | | ✅ Good | |
| Pi Board #4 | | ✅ Good | |

## Physical Assembly

### Cluster Case Assembly
If using a cluster case or rack:

1. **Case Preparation**
   - [ ] Assemble case according to manufacturer instructions
   - [ ] Install any included fans or cooling solutions
   - [ ] Verify all mounting points align with Pi boards

2. **Pi Board Installation**
   - [ ] Install heat sinks on CPU and RAM chips
   - [ ] Mount each Pi in designated slots
   - [ ] Ensure proper clearance for cables and airflow

3. **Cable Management**
   - [ ] Route power cables cleanly
   - [ ] Organize ethernet cables by length
   - [ ] Leave space for maintenance access

### Cooling Considerations

#### Passive Cooling
- **Heat Sinks**: Aluminum or copper on CPU
- **Case Design**: Ensure adequate ventilation
- **Spacing**: Maintain airflow between nodes

#### Active Cooling (Optional)
- **Case Fans**: 40mm or 80mm depending on case
- **Individual Fans**: Small fans per Pi board
- **Temperature Monitoring**: Track thermals during load

## Network Infrastructure

### Switch Configuration

#### Basic Setup
```bash
# Default switch settings (most are plug-and-play)
- Auto-negotiation: Enabled
- Duplex: Full
- Speed: Gigabit
- Flow Control: Enabled
```

#### Advanced Configuration (Managed Switch)
```bash
# VLAN setup (optional)
VLAN 10: Cluster Management (192.168.1.0/24)
VLAN 20: Application Traffic (192.168.2.0/24)

# Port assignments
Port 1-4: Pi Nodes (Access VLAN 10)
Port 5: Uplink to Router (Trunk)
Port 6-8: Future expansion
```

### Cable Organization

#### Labeling System
- **Power Cables**: "PWR-PI-01", "PWR-PI-02", etc.
- **Network Cables**: "NET-PI-01", "NET-PI-02", etc.
- **Node Labels**: Physical labels on each Pi

#### Cable Routing
- [ ] Power cables routed away from network cables
- [ ] Service loops for maintenance access
- [ ] Strain relief at connection points

## Power Management

### Power Supply Verification

#### Individual Pi Testing
```bash
# Test each power supply before deployment
1. Connect power supply to Pi
2. Boot with basic OS image
3. Monitor voltage under load:
   vcgencmd measure_volts
4. Document any voltage drops below 4.8V
```

#### Load Testing
- [ ] All Pis powered simultaneously
- [ ] Full CPU stress test on all nodes
- [ ] Monitor for brownouts or instability
- [ ] Document peak power consumption

### Power Distribution

#### Centralized vs Distributed
**Option A: Individual Power Supplies**
- Pros: Isolation, easier troubleshooting
- Cons: Cable clutter, multiple wall adapters

**Option B: USB Hub with High-Wattage Supply**
- Pros: Cleaner cable management
- Cons: Single point of failure

**Option C: PoE (Power over Ethernet)**
- Pros: Single cable per Pi, very clean
- Cons: Requires PoE HATs, more expensive

### UPS Considerations
For production-like testing:
- [ ] Calculate total power consumption
- [ ] Size UPS for 15-30 minutes runtime
- [ ] Configure graceful shutdown procedures
- [ ] Test failover scenarios

## Storage Preparation

### MicroSD Card Setup

#### Performance Testing
```bash
# Test write speed (do this before OS installation)
sudo dd if=/dev/zero of=/path/to/test bs=1M count=1024 conv=fsync
# Should achieve >50MB/s for Class 10 cards
```

#### Card Preparation
1. **Format Cards**
   ```bash
   # Use official Raspberry Pi Imager or
   sudo fdisk /dev/diskX  # Replace X with actual disk
   ```

2. **Label Cards**
   - Physical labels: "PI-MASTER", "PI-WORKER-01", etc.
   - Document serial numbers for tracking

#### Backup Strategy Planning
- [ ] Determine backup frequency (weekly recommended)
- [ ] Plan for automated backup scripts
- [ ] Consider network-attached storage for backups

## Environmental Setup

### Workspace Organization

#### Physical Workspace
- [ ] Dedicated area for cluster
- [ ] Good ventilation and temperature control
- [ ] Easy access for maintenance
- [ ] Protection from dust and interference

#### Tools & Supplies Station
- [ ] Spare cables and adapters
- [ ] Multimeter for electrical testing
- [ ] Small screwdrivers and tools
- [ ] Backup microSD cards
- [ ] Documentation and labels

### Documentation Setup

#### Photo Documentation
- [ ] Before assembly photos
- [ ] Step-by-step assembly photos
- [ ] Final setup documentation
- [ ] Cable routing and labeling photos

#### Configuration Records
Create a physical or digital log for:
- Serial numbers and MAC addresses
- Power consumption measurements
- Network configuration details
- Any modifications or issues encountered

## Testing & Validation

### Connectivity Testing

#### Network Verification
```bash
# Test each Pi's network connectivity
ping -c 4 192.168.1.1  # Gateway
ping -c 4 8.8.8.8      # Internet
ping -c 4 pi-worker1.local  # Inter-node communication
```

#### Performance Baseline
```bash
# Network speed test between nodes
iperf3 -s  # On one node
iperf3 -c pi-worker1 -t 30  # From another node
```

### Stress Testing

#### Individual Node Testing
```bash
# CPU stress test
stress-ng --cpu 4 --timeout 300s --metrics-brief

# Memory test
stress-ng --vm 2 --vm-bytes 75% --timeout 300s

# Storage I/O test
stress-ng --hdd 1 --hdd-bytes 1G --timeout 300s
```

#### Cluster-Wide Testing
- [ ] All nodes under simultaneous load
- [ ] Monitor temperatures and throttling
- [ ] Verify stable operation for extended periods
- [ ] Document any performance limitations

## Troubleshooting Common Issues

### Power-Related Problems

#### Symptoms & Solutions
- **Random reboots**: Check power supply capacity
- **Yellow lightning bolt**: Increase power supply wattage
- **Performance issues**: Monitor voltage under load

### Network Issues

#### Connectivity Problems
- **No link light**: Check cable and switch port
- **Slow speeds**: Verify cable category and switch capabilities
- **DHCP failures**: Check router DHCP pool size

### Thermal Issues

#### Overheating Symptoms
- **CPU throttling**: Monitor with `vcgencmd measure_temp`
- **Reduced performance**: Add cooling or improve ventilation
- **System instability**: Check case airflow design

## Next Steps

Once hardware setup is complete:

1. **OS Installation**: Proceed to [OS Setup](os-setup.md)
2. **Initial Configuration**: Network and SSH setup
3. **Security Hardening**: Update and secure each node
4. **Cluster Software**: Kubernetes installation and configuration

---

*Hardware setup completed: [Date]*
*Issues encountered: [List any problems and solutions]*
*Next: [Operating System Setup](os-setup.md)*
