# Windows Process Monitoring Lab 03: Advanced Malware Detection and System Analysis

## Lab Overview

**Lab Focus:** Advanced Windows process monitoring, malware detection, and system behavior analysis using Process Monitor for incident response operations  
**Difficulty:** Advanced    
**Author:** Mykyta Palamarchuk  
**Role:** SOC Analyst | Windows Security & Malware Analysis  
**Certification:** CompTIA Security+ (SY0-701)

### Environment Specifications
- **Platform:** Windows workstation with Sysinternals Process Monitor
- **Administrative Privileges:** Administrator access required for process monitoring and service management
- **Business Context:** Enterprise malware detection and incident response
- **Technology Focus:** Process Monitor, system behavior analysis, malware remediation

---

## Learning Objectives

Upon completion of this lab, participants will demonstrate:

1. **Process Monitor proficiency** including interface navigation, event capture, and real-time system monitoring
2. **Advanced filtering techniques** for isolating suspicious activities and malware behavior patterns
3. **Process relationship analysis** using process tree visualization for parent-child process investigation
4. **Malware detection capabilities** identifying unauthorized file operations and suspicious system behavior
5. **Command-line automation** implementing Process Monitor CLI for automated monitoring and log analysis
6. **Incident remediation skills** terminating malicious processes and services for threat containment

---

## Business Scenario

You are a SOC analyst responding to a security incident involving unauthorized file activities on an enterprise workstation. Reports indicate suspicious "cat picture hoarding" behavior suggesting potential malware or data exfiltration activities. Your objective is to use Process Monitor to identify the source of unauthorized file operations, analyze system behavior patterns, and implement appropriate remediation measures to contain the threat.

---

## Lab Exercises

### Exercise 1: Process Monitor Interface and Event Capture

#### Task 1.1: Process Monitor Interface Navigation

**Objective:** Master Process Monitor interface components and establish monitoring capabilities for security analysis.

**Interface Initialization:**
1. Launch Process Monitor (procmon.exe) from desktop
2. Familiarize with main interface components and monitoring capabilities

![Process Monitor Initial Interface](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/procmon-initial-interface.png)

**Event Capture Activation:**
1. Navigate to **File** menu
2. Enable **Capture Events** option for real-time monitoring

![Capture Events Activation](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/capture-events-activation.png)

**Real-time Event Monitoring:**
Once capture is activated, Process Monitor begins displaying system events in real-time including process, registry, network, and file system activities.

![Event Stream Display](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/event-stream-display.png)

**Security Monitoring Context:** Real-time event capture provides comprehensive visibility into system activities, enabling detection of unauthorized processes, file modifications, and potential malware behavior.

---

### Exercise 2: Advanced Filtering and Event Analysis

#### Task 2.1: Toolbar-Based Event Filtering

**Objective:** Implement quick filtering techniques for focused security analysis.

**Toolbar Filter Controls:**
Process Monitor provides rapid event type filtering using toolbar buttons for immediate analysis focus.

![Toolbar Filter Buttons](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/toolbar-filter-buttons.png)

**Event Type Categories:**
- **Process and Thread Activity** - Process creation, termination, and thread operations
- **File System Activity** - File read, write, delete, and modification operations
- **Registry Activity** - Registry key and value modifications
- **Network Activity** - Network connections and communications

#### Task 2.2: Advanced Filter Configuration

**Objective:** Configure comprehensive filters for targeted malware detection and analysis.

**Detailed Filter Interface:**
Access advanced filtering through **Filter > Filter** menu for precise event targeting.

![Advanced Filter Configuration](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/advanced-filter-configuration.png)

**Filter Configuration Parameters:**
- **Process Name** - Target specific executables for analysis
- **Process ID** - Monitor specific process instances
- **Path** - Focus on specific file system locations
- **Username** - Filter by user context for privilege analysis
- **Time/Date** - Temporal filtering for incident correlation

**Filter Export and Management:**
Save configured filters for reuse and standardization across security investigations.

![Filter Export Configuration](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/filter-export-configuration.png)

**Operational Benefit:** Standardized filters enable consistent analysis methodology and efficient incident response across multiple security investigations.

---

### Exercise 3: Process Relationship Analysis

#### Task 3.1: Process Tree Investigation

**Objective:** Analyze process hierarchies and parent-child relationships for comprehensive security assessment.

**Process Tree Access:**
Navigate to **Tools > Process Tree** for hierarchical process analysis.

![Process Tree Interface](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/process-tree-interface.png)

**Process Hierarchy Analysis:**
- **Parent Process Identification** - Determine process origins and execution chains
- **Child Process Monitoring** - Track spawned processes and sub-activities
- **Execution Timeline** - Understand process creation sequences
- **Privilege Escalation Detection** - Identify unauthorized privilege changes

**Security Significance:** Process tree analysis reveals attack chains, identifies process injection attempts, and enables comprehensive malware behavior assessment.

---

### Exercise 4: Command-Line Automation and Advanced Monitoring

#### Task 4.1: Process Monitor CLI Operations

**Objective:** Implement automated monitoring using command-line parameters for scalable security operations.

**Essential CLI Parameters:**
- **`/AcceptEula`** - Bypass license agreement for automated deployment
- **`/Quiet`** - Suppress confirmation dialogs for scripted operations
- **`/Backingfile`** - Direct log output to specified file location
- **`/Runtime`** - Define monitoring duration for controlled capture
- **`/LoadConfig`** - Apply pre-configured filter sets
- **`/SaveAs`** - Export logs in CSV, XML, or PML formats

**Automated Monitoring Example:**
```cmd
C:\Tools\SysinternalsSuite\Procmon.exe /Quiet /AcceptEula /Backingfile C:\Users\Admin\Desktop\ProcmonLog.PML /Runtime 100
```

**Advanced Integration Example:**
```cmd
C:\Tools\SysinternalsSuite\Procmon.exe /Quiet /LoadConfig C:\Users\Admin\Desktop\Filter.pmc /SaveAs C:\Users\Admin\Desktop\ProcmonLog.xml
```

**Operational Value:** CLI automation enables integration with security orchestration platforms, scheduled monitoring, and large-scale incident response operations.

---

### Exercise 5: Malware Detection and Analysis

#### Task 5.1: Suspicious Activity Investigation

**Objective:** Detect and analyze unauthorized file operations indicating malware presence.

**Investigation Scenario:** Workstation exhibits unauthorized file creation behavior requiring systematic analysis to identify malware source and operation patterns.

**Filter Configuration for Malware Detection:**
1. **Capture Events Activation:**

![Malware Detection Capture](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/malware-detection-capture.png)

2. **Filter Menu Access:**

![Filter Menu Access](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/filter-menu-access.png)

3. **Desktop Path Filtering:**
Configure path-based filtering to monitor desktop file operations.

![Desktop Path Filter](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/desktop-path-filter.png)

4. **Write Operation Filtering:**
Focus on file write operations to detect unauthorized file creation.

![Write Operation Filter](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/write-operation-filter.png)

5. **Filter Application:**
Apply configured filters for targeted monitoring.

![Filter Application Interface](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/filter-application-interface.png)

#### Task 5.2: Malware Source Identification

**Objective:** Identify the specific process responsible for unauthorized file operations.

**Analysis Results:**
Filtered monitoring reveals unauthorized file creation activities originating from system process.

![Process Identification Results](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/process-identification-results.png)

**Investigation Findings:**
- **Malicious Process:** spoolsv.exe
- **Unauthorized Activity:** Desktop file creation and modification
- **Security Assessment:** System service compromise requiring immediate remediation

**Threat Analysis:** Print Spooler service (spoolsv.exe) exhibiting unauthorized file operations indicates potential service compromise or malware injection requiring immediate containment.

---

### Exercise 6: Incident Remediation and Threat Containment

#### Task 6.1: Malicious Process Investigation

**Objective:** Correlate process identification with system service analysis for comprehensive threat assessment.

**Service Analysis Procedure:**
1. Open Command Prompt with administrative privileges
2. Execute service enumeration: `tasklist /svc`
3. Identify Print Spooler service association

![Service Analysis Results](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/service-analysis-results.png)

**Service Correlation:** Print Spooler service (spoolsv.exe) identified as source of unauthorized file operations, confirming service compromise requiring immediate termination.

#### Task 6.2: Service Termination and Remediation

**Objective:** Implement immediate threat containment through service termination.

**Remediation Procedure:**
1. Open PowerShell with administrative privileges
2. Execute service termination: `Stop-Service -Name Spooler`

![Service Termination Command](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/service-termination-command.png)

**Remediation Verification:**
- **Service Status:** Print Spooler service successfully terminated
- **File Operations:** Unauthorized desktop file creation ceased
- **System Security:** Immediate threat containment achieved

**Post-Incident Actions:**
- System integrity verification
- Malware signature analysis
- Service configuration hardening
- Continuous monitoring implementation

---

## Technical Assessment

### Process Monitor Mastery
- Interface navigation and event capture configuration
- Advanced filtering techniques for targeted security analysis
- Process tree analysis for comprehensive process relationship investigation

### Malware Detection Capabilities
- Suspicious activity pattern recognition using behavioral analysis
- Filter configuration for malware behavior isolation
- Real-time threat identification and source determination

### Incident Response Proficiency
- Systematic investigation methodology using Process Monitor
- Service correlation and threat attribution analysis
- Immediate remediation implementation for threat containment

### Automation and Integration Skills
- Command-line automation for scalable monitoring operations
- Filter standardization for consistent analysis methodology
- Log export and integration with security analysis platforms

---

## SOC Applications

### Operational Use Cases
- **Malware Detection:** Real-time identification of unauthorized file operations and suspicious process behavior
- **Incident Investigation:** Comprehensive system behavior analysis for security incident reconstruction
- **Threat Hunting:** Proactive identification of advanced persistent threats and fileless malware
- **System Baseline Establishment:** Normal behavior profiling for anomaly detection

### Security Metrics Enhancement
- **Mean Time to Detection:** Rapid malware identification through behavioral monitoring
- **Investigation Efficiency:** Systematic analysis methodology reducing investigation time
- **Remediation Speed:** Immediate threat containment through automated process termination
- **False Positive Reduction:** Precise filtering eliminating irrelevant system noise

---

## Lab Completion

**Skills Validated:**
- Process Monitor interface mastery and real-time system monitoring
- Advanced filtering configuration for malware detection and analysis
- Process relationship analysis using hierarchical process investigation
- Command-line automation for scalable security monitoring operations
- Malware detection and immediate incident remediation capabilities

**Technical Competencies:**
- Windows system behavior analysis and process monitoring
- Security incident investigation using systematic analysis methodology
- Threat containment and remediation through service management
- Security tool integration and automation for enterprise operations

---

*This advanced lab develops comprehensive Windows process monitoring skills essential for malware detection, incident response, and enterprise security operations in complex Windows environments.*
