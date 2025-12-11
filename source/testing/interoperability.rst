Interoperability Testing (IOT)
==============================

Introduction
------------

**Purpose:** To verify that O-DUs (Open Distributed Units) and O-RUs (Open Radio Units) from different manufacturers can operate together effectively, fostering innovation and reducing costs in the O-RAN ecosystem.

**Objective:** To validate multi-vendor interoperability across all fronthaul planes (M, S, C, and U), ensure compliance with O-RAN WG4 specifications, and assess both fundamental and optional capabilities under real-world conditions.

**Scope:** Covers system-level testing of the O-RAN fronthaul interface, including O-DU and O-RU integration, validation of interface operations for LTE and 5G NR, and end-to-end functionality testing across Management, Synchronization, Control, and User planes.

Applicable Interoperability Test Cases
--------------------------------------

The current testing focuses on the O-RAN Fronthaul Interface (O-RAN.WG4.IOT) guidelines to enable interoperability between O-DU and O-RU from different vendors. The testing approach is non-intrusive, ensuring network elements do not require functionality beyond normal operation.

Types of IOT Tests
------------------

- **M-Plane IOT Tests:** Focus on the management plane, specifically validating the startup installation where O-DU and O-RU establish service (e.g., Startup in Hierarchical or Hybrid modes).
- **S-Plane IOT Tests:** Validate synchronization plane functionality and performance. Includes tests for LLS-C1, LLS-C2, and LLS-C3 configurations using profiles like ITU-T G.8275.1 to ensure precise timing between components.
- **C/U-Plane IOT Tests:** Verify Control and User plane operations, including Radio Layer 3 establishment and initial data transfer. Key metrics include downlink/uplink throughput performance and delay management (latency) across the fronthaul.

Example Test Case
-----------------

Consider the following test case from the M-Plane IOT suite:

**Test Case 2.2.1.1: Start-up in Hierarchical Mode**

We review the prerequisites (physical connectivity, DHCP availability) and objectives before execution. Here is a summary:

- **Objective:** Validate the startup sequence of the O-RU and the interface to the DHCP server and NETCONF client in the O-DU. Ensure the O-RU can boot, obtain an IP, and establish a NETCONF session with the O-DU.
- **Expected Result:** The O-RU successfully completes its startup sequence. Verification is achieved when the fronthaul C-Plane and U-Plane become operational, specifically demonstrating that the O-RU is “in-service” and transmitting broadcast channels in the downlink as validated by the UE emulator or signal analyzer.


.. figure:: /_static/Interoperabiliy.jpg
   :alt: S-Plane IOT (LLS-C1) test setup
   :align: center
   :width: 600px

   **Fig.:** S-Plane IOT (LLS-C1) test setup
Conclusion
----------

Interoperability Testing (IOT) confirms that the multi-vendor O-RAN architecture functions as a cohesive system. By validating the M-Plane, S-Plane, and C/U-Plane interfaces, we ensure that the O-DU and O-RU can synchronize, manage configurations, and transfer user data reliably. Passing these mandatory and conditional tests is required for O-RAN IOT Badging, signaling readiness for deployment in diverse network environments.