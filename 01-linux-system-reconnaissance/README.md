# Linux System Reconnaissance Lab 01: System Information Gathering and Documentation

## Lab Overview

**Lab Focus:** Comprehensive Linux system reconnaissance and infrastructure documentation for incident response preparation  
**Difficulty:** Foundation    
**Author:** Mykyta Palamarchuk  
**Role:** SOC Analyst | Linux System Administration & Incident Response  
**Certification:** CompTIA Security+ (SY0-701)

### Environment Specifications
- **Platform:** Ubuntu Server 18.04.6 LTS
- **Administrative Privileges:** Root access required for comprehensive system analysis
- **Business Context:** Enterprise infrastructure documentation and baseline establishment
- **Technology Focus:** System reconnaissance, hardware profiling, process analysis, network configuration

---

## Learning Objectives

Upon completion of this lab, participants will demonstrate:

1. **Operating system analysis proficiency** including kernel version identification and distribution assessment
2. **Hardware reconnaissance mastery** covering disk storage, memory configuration, and CPU specifications
3. **Process monitoring capabilities** using system utilities for active process identification and analysis
4. **Network configuration analysis** including interface identification, routing table examination, and service port mapping
5. **System documentation standards** producing comprehensive infrastructure reports for incident response preparation
6. **Command-line proficiency** utilizing essential Linux utilities for systematic information gathering

---

## Business Scenario

You are a junior system engineer at a paper manufacturing company conducting your first infrastructure assessment. A new Ubuntu server has been deployed for production use, and comprehensive system documentation is required for security baseline establishment and incident response preparation. Your objective is to gather complete system specifications including operating system details, hardware configuration, active processes, and network settings to establish a security baseline and support future incident investigations.

---

## Lab Exercises

### Exercise 1: Operating System Analysis

#### Task 1.1: Kernel Version and Distribution Assessment

**Objective:** Identify critical operating system information for security baseline documentation.

**System Analysis Commands:**
```bash
# Kernel release identification
uname -r

# Kernel version details
uname -v

# Distribution information (comprehensive)
lsb_release -a

# System information summary
hostnamectl
```

**Investigation Results:**
1. **Kernel release version:** 4.15.0-1142-kvm
2. **Operating system distribution:** Ubuntu 18.04.6 LTS

**Security Significance:** Kernel and distribution versions determine available security patches, vulnerability exposure, and compatibility requirements for security tools and monitoring systems.

---

### Exercise 2: Hardware Configuration Analysis

#### Task 2.1: Storage System Assessment

**Objective:** Document storage configuration and partition layout for forensic readiness.

**Storage Analysis Commands:**
```bash
# Hardware component listing (disk focus)
lshw -c disk

# Volume and partition analysis
lshw -c volume

# Partition table examination
fdisk -l

# Block device structure
lsblk

# Filesystem utilization
df -h
```

**Storage Configuration Results:**
1. **Primary disk (vda) total size:** 11GiB (11GB)
2. **Root partition (/dev/vda1) mount point:** /
3. **Partition structure:** GPT partitioning with multiple system partitions

#### Task 2.2: Memory Hardware Documentation

**Objective:** Assess system memory configuration for performance baseline and capacity planning.

**Memory Analysis Commands:**
```bash
# Memory hardware details
lshw -c memory

# Current memory utilization
free -h
```

**Memory Configuration Results:**
- **Total system memory:** 2GiB
- **Memory type:** DIMM RAM (QEMU virtualized environment)
- **Error detection:** Multi-bit ECC configuration

**Operational Impact:** Memory configuration determines system capacity for log processing, security tool deployment, and incident response activities.

---

### Exercise 3: Central Processing Unit Assessment

#### Task 3.1: CPU Specification Documentation

**Objective:** Document processor specifications for performance baseline and security tool compatibility.

**CPU Analysis Commands:**
```bash
# Detailed CPU information
lscpu

# Hardware-level CPU details
lshw -c cpu
```

**CPU Configuration Results:**
- **CPU core count:** 1
- **Architecture:** x86_64
- **CPU family:** Intel Xeon E3-12xx v2 (Ivy Bridge)
- **Virtualization:** AMD-based virtualized environment

**Security Considerations:** Single-core configuration impacts concurrent security tool performance and log processing capabilities during high-volume security events.

---

### Exercise 4: Process and Service Analysis

#### Task 4.1: Active Process Identification

**Objective:** Establish baseline process inventory for anomaly detection and security monitoring.

**Process Analysis Commands:**
```bash
# Real-time process monitoring
top

# Enhanced process display
htop

# Process search by name
pgrep -l cron

# Detailed process information
ps <PID>

# Executable path identification
which cron
```

**Critical Process Analysis:**
1. **Cron service PID:** 225
2. **Cron executable path:** /usr/sbin/cron
3. **Process priority:** Standard system service priority (20)

**Security Baseline:** Active process inventory enables detection of unauthorized processes, privilege escalation attempts, and malicious service installations during incident investigations.

---

### Exercise 5: Network Configuration Documentation

#### Task 5.1: Network Interface and Routing Analysis

**Objective:** Document network configuration for baseline establishment and incident response preparation.

**Network Analysis Commands:**
```bash
# Network interface configuration
ifconfig -a

# Modern interface management
ip addr

# Routing table examination
route

# Enhanced routing information
ip route

# Network connection analysis
netstat -tulnp

# Socket statistics
ss -tulnp
```

**Network Configuration Results:**
1. **Primary IPv4 address:** 192.168.6.2
2. **Primary network interface:** enp1s9
3. **Network configuration:** Ethernet interface with broadcast capability
4. **Default gateway:** 192.168.6.254

#### Task 5.2: Service Port Analysis

**Objective:** Identify active network services for security assessment and monitoring configuration.

**Service Analysis Process:**
1. **Process identification:** `pgrep gotty` → PID 227
2. **Port mapping:** `netstat -tulnp | grep 227`
3. **Service analysis:** gotty service listening on port 7681

**Service Configuration Results:**
- **Gotty service port:** 7681
- **Protocol:** TCP over IPv6
- **Service function:** Terminal sharing service (security assessment required)

**Security Assessment:** Network service inventory enables identification of unauthorized services, potential attack vectors, and security monitoring requirements for comprehensive incident response preparation.

---

## Technical Assessment

### System Documentation Proficiency
- Operating system and kernel version identification for vulnerability assessment
- Hardware configuration documentation for forensic and performance analysis
- Complete system specification compilation for security baseline establishment

### Command-Line Competency
- Essential Linux utility usage for systematic information gathering
- Process identification and analysis using multiple command-line tools
- Network configuration analysis and service port mapping

### Security Baseline Establishment
- Comprehensive system inventory for anomaly detection preparation
- Network service documentation for attack surface assessment
- Process baseline creation for unauthorized activity detection

### Documentation Standards
- Structured information gathering following systematic methodology
- Complete infrastructure assessment for incident response readiness
- Professional system documentation suitable for security team utilization

---

## SOC Applications

### Operational Use Cases
- **Incident Response Preparation:** Complete system baseline for anomaly detection during security incidents
- **Vulnerability Assessment:** System configuration documentation for targeted security testing
- **Security Monitoring Configuration:** Network and process baselines for SIEM rule development
- **Forensic Readiness:** Comprehensive system documentation for digital forensic investigations

### Security Metrics Enhancement
- **System Inventory Management:** Complete hardware and software configuration tracking
- **Baseline Security Posture:** Established configuration standards for security compliance
- **Network Security Assessment:** Service inventory for attack surface analysis
- **Performance Monitoring:** Hardware specifications for security tool deployment planning

---

## Lab Completion

**Skills Validated:**
- Linux system reconnaissance and information gathering techniques
- Hardware configuration analysis and documentation
- Process identification and service analysis capabilities
- Network configuration assessment and security evaluation
- Systematic documentation methodology for incident response preparation

**Technical Competencies:**
- Command-line proficiency for system administration and security operations
- Infrastructure assessment capabilities for security baseline establishment
- Network analysis skills for service inventory and security monitoring
- Professional documentation standards for security team collaboration

---

*This foundational lab establishes essential Linux system reconnaissance skills required for security operations, incident response preparation, and infrastructure security assessment in enterprise environments.*
