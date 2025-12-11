Security Testing
================

Introduction
------------

**Purpose:** To ensure that the O-RAN system—including all disaggregated components, interfaces, and management layers—maintains confidentiality, integrity, and availability against cyber threats. Security testing validates that every network element (O-DU, O-RU, SMO, Near-RT RIC, O-Cloud, and supporting infrastructure) adheres to O-RAN Alliance security specifications and industry-standard security practices.

**Objective:** To comprehensively assess whether all O-RAN components, including Open Fronthaul (O-FH), comply with security requirements such as authentication, authorization, encryption, secure transport, and access control. Testing verifies that interfaces are resilient to attacks, securely configured, and enforce proper security controls.

**Scope:** Security testing covers the entire O-RAN deployment, focusing on:

- Authentication mechanisms (e.g., SSH, TLS)
- Secure management interfaces (NETCONF, O1 interface)
- Encryption and secure transport (DTLS, IPSec)
- Integrity protection of O-FH (eCPRI) traffic
- Access control on O-DU/O-RU nodes
- Vulnerability scanning and configuration checks
- Interoperability of security configurations in a multi-vendor setup

While this phase emphasizes O-FH, security tests extend to interworking elements, ensuring that threats are mitigated across all layers of the disaggregated architecture.

Security Considerations for Open Fronthaul (O-FH)
-------------------------------------------------

The Open Fronthaul interface (between O-DU and O-RU) is critical, as it carries real-time and near-real-time control/user plane traffic. Security testing for O-FH ensures:

**Key Requirements:**

- **Secure Management Plane Configuration:** Ensure that SSH/NETCONF access to O-RU and O-DU is authenticated, encrypted, and restricted.
- **Transport Security:** Validate the use of MACsec/IPsec/DTLS where required for protecting non-real-time traffic.
- **Access Control and Hardening:** Verify that only authorized hosts can access O-RU/O-DU management ports.
- **Protection Against Attack Vectors:** Brute-force login attempts, unauthorized configuration access, tampering with fronthaul synchronization messages, replay/injection attacks.
- **Compliance with O-RAN Security Specifications:** O-RAN.WG11.

Applicable Security Test Cases
------------------------------

OTIC security testing aligns with test plans defined by O-RAN Alliance Security WG11 and Test & Integration Focus Group (TIFG). The focus includes:

- Authentication validation
- Credential and key management
- Hardening of management interfaces (SSH, NETCONF, WebUI)
- Secure Boot and integrity protection
- Logging, monitoring, and audit verification
- Vulnerability scanning and configuration assessment

Network Security Protocol - SSH
-----------------------------------

**Objective**  
Verify that the DUT (O-RU or O-DU) only supports SSH algorithms that comply with the O-RAN Security Configuration Guidelines (SCG). This validates SSH Key Exchange (KEX), Host Key, Encryption (Ciphers), and MAC algorithm sets during SSH handshake negotiation.

**Goal:**  
- Allowed algorithms are supported  
- Not allowed algorithms are absent  
- Optional algorithms behave correctly

**Applicability:**  
O-RU / O-DU management plane access; O-FH security compliance; OTIC Security Testing; O-RAN WG11 compliance validation.

Test Environment Setup
----------------------

The following diagram illustrates a typical setup used for verifying SSH server/client authentication on the O-RAN M-Plane:

.. figure:: /_static/Security.png
   :alt: Example SSH Security (Server/Client) test setup for O-RAN M-Plane
   :align: center
   :width: 600px

In this scenario, the DUT (O-RU/O-DU acting as SSH server or client) is isolated within a secure VLAN and integrated with the Security Test Controller at the OTIC. The setup allows validation of SSH handshake, authentication (password/cert), and negotiation of cryptographic algorithms.

- **When testing the SSH Server (e.g., O-RU):** The Test Controller acts as the SSH Client (simulating O-DU/SMO) to initiate connections, attempt valid/invalid logins, and verify access controls.
- **When testing the SSH Client (e.g., O-DU):** The Test Controller emulates a malicious or standard SSH Server to verify how the DUT handles server host key verification and connection requests.

Pre-Conditions
--------------

- DUT reachable on management interface  
- SSH port enabled  
- List of algorithms (Allowed / Not Allowed / Optional) from O-RAN SCG available  
- SSH client capable of printing negotiated algorithms

Test Procedure
--------------

**Step 1 — Enumerate Supported SSH Algorithms**  
- Use tools to retrieve negotiated algorithm lists.  
- Record categories: KEX, Host Key, Ciphers, MAC, Compression.

**Step 2 — Compare DUT Algorithms Against O-RAN SCG Tables**  
- Match DUT-advertised algorithms to:  
  - Allowed (must appear)  
  - Not Allowed (must not appear)  
  - Optional (may appear but must be secure)

Expected Results
----------------

**Pass Criteria**  
- Supports all required allowed algorithms  
- Rejects all not allowed algorithms  
- Optional algorithms handled per vendor design  
- Negotiates secure algorithms during normal SSH login  
- No indication of weak configurations  
- Logs negotiation failures if mandated by SCG

**Fail Criteria**  
- Any not allowed algorithm is supported  
- Allowed algorithms are missing  
- Weak SSH algorithms are present or negotiated


Conclusion
----------

Validating the SSH algorithm suite ensures SSH access to O-RU and O-DU follows O-RAN security guidelines on cryptographic strength. This is essential for securing O-FH management interfaces in a multi-vendor deployment.