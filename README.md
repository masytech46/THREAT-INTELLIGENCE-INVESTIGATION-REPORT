# 🛡️ Threat Intelligence Investigation Report: Yogi/Salat Trojan (Zapret.exe)

## Executive Summary

This repository documents a comprehensive threat intelligence investigation of a malicious executable identified as **Zapret.exe**, a malware sample associated with the **Yogi/Salat Trojan** family. The investigation was conducted to determine the malware's capabilities, infrastructure dependencies, behavioral characteristics, and potential impact on organizational assets.

Analysis revealed that the malware establishes communication with external Command-and-Control (C2) infrastructure, performs system discovery, manipulates Windows APIs, queries critical registry settings, and attempts to evade detection by removing forensic artifacts. The sample was flagged as malicious by multiple security vendors and demonstrates behaviors aligned with several MITRE ATT&CK techniques.

The findings presented in this report provide actionable intelligence to assist Security Operations Center (SOC) teams, Incident Responders, Threat Hunters, and Cybersecurity Analysts in detecting, containing, and mitigating threats associated with this malware family.

---

## Investigation Information

| Item                   | Details                                                          |
| ---------------------- | ---------------------------------------------------------------- |
| **Analyst**            | Ajayi Michael                                                    |
| **Investigation Date** | June 12, 2026                                                    |
| **Organization**       | Cleveland Fintech                                                |
| **Malware Family**     | Yogi / Salat Trojan                                              |
| **File Name**          | Zapret.exe                                                       |
| **File Type**          | 64-bit Windows Portable Executable (PE)                          |
| **SHA-256 Hash**       | d9c92563080056480c3afa2a0ecedab16bc2f480358329f65c3ceb4cead2ae06 |
| **Detection Score**    | 36 / 70 Vendors                                                  |

---

## Threat Overview

### Malware Classification

The analyzed sample belongs to the **Yogi/Salat Trojan** malware family, a threat known for establishing unauthorized remote communications, performing reconnaissance activities, and facilitating attacker-controlled operations on compromised endpoints.

### Threat Category

* Trojan
* Remote Access Trojan (RAT)
* Command-and-Control Malware
* Information Gathering Malware

### Risk Rating

| Category               | Rating   |
| ---------------------- | -------- |
| Confidentiality Impact | High     |
| Integrity Impact       | High     |
| Availability Impact    | Medium   |
| Detection Difficulty   | Medium   |
| Overall Risk           | Critical |

---

## Malware Identification

### File Attributes

```text
File Name: Zapret.exe
Architecture: 64-bit
File Type: Windows Portable Executable (PE)
Malware Family: Yogi/Salat Trojan
SHA-256:
d9c92563080056480c3afa2a0ecedab16bc2f480358329f65c3ceb4cead2ae06
```

### Detection Summary

* 36 security vendors identified the sample as malicious.
* Multiple threat intelligence engines classified the file as a Trojan.
* The sample exhibits execution, discovery, and defense-evasion capabilities.

---

## Infrastructure Analysis

### Command-and-Control (C2) Infrastructure

The malware communicates with attacker-controlled systems to receive commands, transmit information, and maintain persistence.

#### Malicious IP Address

```text
2.59.219.233
```

#### Associated Domains

```text
salatik.cn
tonapi.io
```

### Security Impact

Communication with these indicators may enable:

* Remote command execution
* Data exfiltration
* Payload delivery
* Persistence management
* Victim tracking

---

## Related Malicious Components

The following files were identified as related to the malware execution chain:

```text
smss.exe
python312.dll
VCRUNTIME140.dll
unicodedata.pyd
_socket.pyd
_decimal.pyd
```

### Potential Purpose

| File             | Function                  |
| ---------------- | ------------------------- |
| smss.exe         | Process execution support |
| python312.dll    | Embedded Python runtime   |
| VCRUNTIME140.dll | Runtime dependency        |
| unicodedata.pyd  | Python module support     |
| _socket.pyd      | Network communication     |
| _decimal.pyd     | Data processing           |

---

## MITRE ATT&CK Mapping

### T1106 — Native API

#### Tactic

**Execution**

#### Description

The malware leverages native Windows API functions to interact directly with operating system resources, allowing it to execute malicious operations while minimizing detection opportunities.

#### Observed Behavior

* Direct Windows API invocation
* System interaction
* Execution of malicious functionality
* Host discovery activities

#### Detection Opportunities

* Monitor abnormal API calls
* Detect suspicious DLL loading
* Monitor memory manipulation activities
* Investigate process hollowing attempts

#### Recommended Mitigation

* Implement Windows Defender Application Control
* Restrict unauthorized applications
* Enable EDR behavioral monitoring

---

### T1070 — Indicator Removal on Host

#### Tactic

**Defense Evasion**

#### Description

The malware attempts to hide its presence by deleting or modifying system artifacts commonly used during forensic investigations.

#### Observed Behavior

* Registry manipulation
* Removal of evidence
* Disabling user notifications
* Concealment of malicious services

#### Detection Opportunities

* Monitor registry deletions
* Detect event log tampering
* Monitor scheduled task modifications
* Alert on security artifact removal

#### Recommended Mitigation

* Protect critical log files
* Enable tamper protection
* Restrict administrative privileges

---

### T1012 — Query Registry

#### Tactic

**Discovery**

#### Description

The malware gathers information about the victim environment through registry enumeration.

#### Observed Behavior

* Reads system configuration data
* Queries RDP settings
* Enumerates network configurations
* Collects operating environment information

#### Detection Opportunities

* Monitor registry access events
* Identify unusual registry queries
* Correlate process activity with registry reads

#### Recommended Mitigation

* Implement EDR monitoring
* Enable registry auditing
* Establish behavioral baselines

---

## Attack Lifecycle Analysis

### Phase 1 — Initial Execution

The malware executes on the victim system and initializes its embedded components.

### Phase 2 — Environment Discovery

The malware queries registry settings and gathers information regarding:

* System configuration
* Remote Desktop settings
* Network infrastructure
* Security controls

### Phase 3 — Command-and-Control Communication

The malware establishes outbound communications with attacker-controlled infrastructure.

### Phase 4 — Defense Evasion

The malware removes traces of execution and modifies system settings to reduce visibility.

### Phase 5 — Persistent Operations

The attacker can continue issuing commands and conducting malicious activities through the established communication channel.

---

## Indicators of Compromise (IOCs)

### File Hashes

```text
d9c92563080056480c3afa2a0ecedab16bc2f480358329f65c3ceb4cead2ae06
```

### Domains

```text
salatik.cn
tonapi.io
```

### IP Addresses

```text
2.59.219.233
```

### Related Files

```text
Zapret.exe
smss.exe
python312.dll
VCRUNTIME140.dll
unicodedata.pyd
_socket.pyd
_decimal.pyd
```

---

## Detection Engineering Recommendations

### SIEM Detection Logic

1. Monitor outbound connections to known malicious domains.
2. Alert on connections to malicious IP addresses.
3. Detect unusual registry enumeration activity.
4. Monitor API abuse and suspicious DLL loading.
5. Alert on event log deletion attempts.
6. Correlate PowerShell activity with network connections.
7. Monitor execution of unsigned PE files.

### EDR Detection Use Cases

* Process Hollowing Detection
* Registry Discovery Detection
* Defense Evasion Detection
* Command-and-Control Beaconing Detection
* Malicious DLL Loading Detection

---

## Incident Response Recommendations

### Immediate Actions

1. Isolate affected systems.
2. Block malicious domains and IPs.
3. Reset potentially compromised credentials.
4. Perform memory acquisition.
5. Collect forensic artifacts.
6. Conduct enterprise-wide IOC scanning.
7. Update detection signatures.

### Long-Term Actions

1. Implement Zero Trust Architecture.
2. Deploy advanced EDR solutions.
3. Conduct threat hunting exercises.
4. Improve network segmentation.
5. Enhance user phishing awareness training.
6. Establish continuous threat intelligence monitoring.

---

## Threat Hunting Queries

### Hunt Objective 1

Identify systems communicating with:

```text
2.59.219.233
salatik.cn
tonapi.io
```

### Hunt Objective 2

Identify processes performing:

* Registry enumeration
* Event log deletion
* Suspicious API usage
* DLL injection activities

### Hunt Objective 3

Search endpoints for:

```text
Zapret.exe
smss.exe
python312.dll
VCRUNTIME140.dll
```

---

## Challenges and Solutions

### Challenge 1: Malware Attribution

**Problem**

Accurately determining the malware family was difficult because the sample exhibited characteristics shared across multiple Trojan variants.

**Solution**

Correlated antivirus detections, behavioral indicators, infrastructure relationships, and threat intelligence data to confidently attribute the sample to the Yogi/Salat Trojan family.

---

### Challenge 2: Infrastructure Correlation

**Problem**

Identifying active command-and-control infrastructure required validating multiple network indicators.

**Solution**

Mapped outbound communications, extracted associated domains and IP addresses, and cross-referenced findings with threat intelligence indicators.

---

### Challenge 3: Behavioral Classification

**Problem**

The malware demonstrated multiple overlapping techniques across execution, discovery, and defense evasion phases.

**Solution**

Mapped observed behaviors to the MITRE ATT&CK framework to provide standardized classification and improve detection engineering efforts.

---

### Challenge 4: Detection Development

**Problem**

Traditional signature-based detection alone could miss variants of the malware.

**Solution**

Developed behavior-based detection recommendations focused on API usage, registry activity, network communications, and defense-evasion techniques.

---

### Challenge 5: Defensive Prioritization

**Problem**

Security teams needed actionable remediation steps without disrupting business operations.

**Solution**

Prioritized high-impact containment actions including IOC blocking, endpoint monitoring, network controls, and threat hunting activities.

---

## Conclusion

The investigation confirmed that **Zapret.exe** is a high-risk Trojan associated with the **Yogi/Salat** malware family. The sample demonstrates sophisticated execution, discovery, and defense-evasion capabilities while maintaining communication with external command-and-control infrastructure.

Organizations should immediately block identified indicators, strengthen endpoint visibility, implement behavior-based detections, and conduct proactive threat hunting activities to reduce exposure to this threat. The intelligence gathered from this investigation can be leveraged to improve detection coverage, incident response readiness, and overall organizational cyber resilience.
