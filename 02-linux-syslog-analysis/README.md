# Linux Syslog Analysis Lab 02: Log Investigation and Authentication Monitoring

## Lab Overview

**Lab Focus:** Comprehensive Linux syslog analysis, authentication monitoring, and security event investigation for incident response operations  
**Difficulty:** Intermediate    
**Author:** Mykyta Palamarchuk  
**Role:** SOC Analyst | Linux Log Analysis & Security Monitoring  
**Certification:** CompTIA Security+ (SY0-701)

### Environment Specifications
- **Platform:** Ubuntu Linux with centralized logging infrastructure
- **Administrative Privileges:** Sudo access required for log file analysis
- **Business Context:** Enterprise security monitoring and incident investigation
- **Technology Focus:** Syslog analysis, authentication monitoring, web server log investigation

---

## Learning Objectives

Upon completion of this lab, participants will demonstrate:

1. **Syslog architecture understanding** including log file locations, formats, and analysis techniques
2. **Authentication monitoring proficiency** detecting brute force attacks and unauthorized access attempts
3. **Log parsing and analysis skills** using command-line utilities for security event investigation
4. **Remote log analysis capabilities** investigating distributed systems and application-specific logs
5. **Security incident correlation** identifying attack patterns and suspicious activities across multiple log sources
6. **Web server security analysis** examining Nginx logs for traffic anomalies and potential threats

---

## Business Scenario

You are a SOC analyst responsible for monitoring and investigating security events across enterprise Linux infrastructure. Recent security alerts indicate potential unauthorized access attempts and suspicious network activity requiring comprehensive log analysis. Your objective is to investigate system logs, authentication events, and application logs to identify security threats, determine attack vectors, and establish incident timelines for response coordination.

---

## Lab Exercises

### Exercise 1: Linux Syslog Infrastructure Analysis

#### Task 1.1: System Log Architecture Understanding

**Objective:** Master Linux logging infrastructure and identify critical log files for security monitoring.

**Critical System Log Files:**
- **`/var/log/syslog`** - Global system activity including startup messages and system events
- **`/var/log/auth.log`** - Security-related events including logins, authentication, and privilege escalation
- **`/var/log/kern.log`** - Kernel events, errors, and warnings for system-level security monitoring
- **`/var/log/cron`** - Scheduled task execution logs for automated process monitoring

**Log File Security Assessment:**
1. **Authentication monitoring log file:** /var/log/auth.log
2. **Kernel security issues investigation:** /var/log/kern.log

**Security Context:** Understanding log file locations and purposes enables systematic security monitoring and incident response preparation across enterprise Linux environments.

#### Task 1.2: Syslog Format Analysis and Structure

**Objective:** Analyze syslog message structure for effective security event parsing and investigation.

**Syslog Message Components:**
1. **Timestamp** - Event occurrence time for incident timeline construction
2. **Hostname/System** - Source system identification for distributed environment analysis
3. **Application/Service** - Process or service generating the log entry
4. **Message Content** - Detailed event information for security analysis

![Syslog Format Structure](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/syslog-format-structure.png)

**Log Analysis Utilities:**
```bash
# Interactive log viewing with search capabilities
sudo less /var/log/syslog

# Pattern matching for specific events
grep <pattern> /var/log/syslog

# Search navigation in less
# Press '/' for search prompt
# Press 'n' for next match
# Press 'N' for previous match
# Press 'q' to exit
```

**Security Analysis Methodology:** Systematic log parsing using timestamp correlation, application filtering, and message pattern recognition enables efficient security event investigation and threat identification.

---

### Exercise 2: System Event Investigation

#### Task 2.1: Application Behavior Analysis

**Objective:** Investigate system logs for suspicious application activities and security-relevant events.

**Investigation Scenario:** Analyze system logs for unusual application behavior that may indicate security compromise or system anomalies.

**Analysis Procedure:**
1. Access system log with elevated privileges: `sudo less /var/log/syslog`
2. Search for specific application events: `/skynet`
3. Analyze timestamp and message content for security implications

**Investigation Results:**
- **Skynet self-awareness timestamp:** Oct 5 22:10:09
- **System correlation:** Event occurred during system service activation
- **Security assessment:** Suspicious application behavior requiring further investigation

**Forensic Value:** Timestamp analysis and application correlation provide critical incident timeline data for security event reconstruction and threat assessment.

---

### Exercise 3: Authentication Security Monitoring

#### Task 3.1: SSH Brute Force Attack Detection

**Objective:** Identify and analyze brute force authentication attacks against SSH services.

**Investigation Focus:** Examine authentication logs to detect systematic password guessing attempts and identify attack sources.

**Authentication Log Analysis:**
```bash
# Access authentication logs with administrative privileges
sudo less /var/log/auth.log

# Search for failed authentication patterns
# Look for "Failed" entries with SSH service indicators
```

**Brute Force Attack Analysis:**
1. **Attack source identification:** 115.238.245.5
2. **Attack pattern:** Multiple failed authentication attempts for invalid user "morgan"
3. **Service targeted:** SSH daemon (sshd) on port 60877
4. **Attack methodology:** Systematic username/password enumeration

**Security Assessment:** Brute force attacks represent systematic attempts to compromise system security through credential guessing, requiring immediate response including IP blocking and authentication strengthening.

#### Task 3.2: Legitimate User Authentication Analysis

**Objective:** Differentiate between malicious attacks and legitimate user access patterns.

**Successful Authentication Investigation:**
1. **Regular user access:** User "addie" successfully authenticated from 71.6.146.185
2. **Authentication method:** Public key authentication using ED25519 cryptography
3. **Session establishment:** PAM session opened with proper user ID mapping

**Root Access Monitoring:**
1. **Privileged access source:** 46.148.20.61
2. **Authentication method:** Public key authentication with ED25519
3. **Session tracking:** SystemD session management with unique session ID 1296

**Security Correlation:** Distinguishing legitimate authentication patterns from malicious attempts enables accurate threat assessment and reduces false positive incident responses.

---

### Exercise 4: Distributed Log Analysis

#### Task 4.1: Remote Server Investigation

**Objective:** Conduct log analysis across distributed systems using SSH connectivity for comprehensive security monitoring.

**Remote Access Configuration:**
```bash
# SSH connection to web server infrastructure
ssh student@angelsscooters.lab

# Navigate to centralized log directory
cd /var/log
```

**Web Server Security Analysis:** Angels Scooters web server (angelsscooters.lab) hosts production website with Nginx web engine requiring security monitoring for application-level threats.

#### Task 4.2: Nginx Web Server Log Analysis

**Objective:** Analyze web server access logs for traffic anomalies, suspicious requests, and potential security threats.

**Nginx Log Analysis Infrastructure:**
- **`/var/log/nginx/access.log`** - Client request logging for traffic pattern analysis
- **`/var/log/nginx/error.log`** - Error condition logging for security issue identification

**Advanced Log Parsing Techniques:**
```bash
# Comprehensive IP address frequency analysis
sudo awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr

# Results interpretation:
# 3830 requests from 60.191.38.77 (suspicious high volume)
# 1643 requests from 192.168.6.254 (normal gateway traffic)
```

**Traffic Anomaly Detection:**
- **Suspicious IP identification:** 60.191.38.77
- **Request volume:** 3,830 requests (unusually high frequency)
- **Security assessment:** Potential DDoS, scraping, or reconnaissance activity requiring investigation

**Threat Analysis:** High-volume request patterns from single IP addresses indicate potential automated attacks, requiring immediate security response including traffic filtering and source investigation.

---

## Technical Assessment

### Log Analysis Proficiency
- Systematic syslog navigation and search technique implementation
- Authentication log parsing for brute force detection and legitimate access differentiation
- Application log analysis for security event correlation and timeline reconstruction

### Security Monitoring Capabilities
- SSH attack pattern recognition and source identification
- Web server traffic anomaly detection using statistical analysis
- Distributed system log correlation across multiple infrastructure components

### Command-Line Investigation Skills
- Advanced log parsing using awk, sort, and uniq for data analysis
- Interactive log navigation using less with search and pattern matching
- Remote system access and log analysis using SSH connectivity

### Incident Response Application
- Timeline reconstruction using timestamp correlation across multiple log sources
- Attack vector identification through authentication pattern analysis
- Threat assessment using traffic volume analysis and source correlation

---

## SOC Applications

### Operational Use Cases
- **Brute Force Detection:** Systematic identification of authentication attacks for immediate response
- **Traffic Analysis:** Web server monitoring for DDoS, scraping, and reconnaissance activities
- **Incident Timeline Reconstruction:** Multi-source log correlation for comprehensive incident analysis
- **Distributed Monitoring:** Cross-system log analysis for enterprise-wide security visibility

### Security Metrics Enhancement
- **Authentication Failure Tracking:** Brute force attempt quantification and source identification
- **Web Traffic Baseline Establishment:** Normal vs. anomalous request pattern differentiation
- **System Behavior Monitoring:** Application anomaly detection for compromise indicators
- **Response Time Optimization:** Rapid log analysis techniques for time-critical investigations

---

## Lab Completion

**Skills Validated:**
- Linux syslog architecture understanding and log file location knowledge
- Authentication monitoring and brute force attack detection capabilities
- Advanced log parsing using command-line utilities for security analysis
- Remote system log investigation and distributed security monitoring
- Web server security analysis and traffic anomaly identification

**Technical Competencies:**
- Systematic log analysis methodology for security event investigation
- Command-line proficiency for efficient log parsing and pattern recognition
- Multi-system correlation techniques for comprehensive incident analysis
- Security event timeline reconstruction using timestamp and source correlation

---

*This intermediate lab develops essential Linux log analysis skills required for professional SOC operations, incident response, and enterprise security monitoring in distributed Linux environments.*
