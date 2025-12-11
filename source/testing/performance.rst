Performance Testing
===================

Introduction
------------

**Purpose:** To ensure the given O-RAN setup delivers reliable, high-performance connectivity by validating its behavior under realistic network conditions.

**Objective:** To comprehensively assess whether all O-RAN components and interfaces meet defined performance KPIs such as throughput and BLER across diverse scenarios.

**Scope:** Covers testing of the entire O-RAN setup, including RAN nodes, fronthaul, midhaul, backhaul, and orchestration layers, focusing on interoperability and performance across all functional interfaces.

Applicable Performance Test Cases
---------------------------------

The current testing focuses specifically on Open Fronthaul (OFH) interfaces, executed in alignment with the specifications and guidelines developed by the Test and Integration Focus Group (TIFG) of O-RAN Alliance.

Types of E2E Performance Tests
------------------------------

- **Functional Tests:** The focus is on end-user functionality based on 3GPP and O-RAN specifications. This assesses the functionality of RAN from a network end-to-end perspective.
- **Performance Tests:** The focus is on end-user performance versus target KPIs. Measure key metrics like throughput and BLER to ensure the interface meets expected performance KPIs.
- **Services Tests:** Ensures the O-RAN system can deliver end-to-end telecom services by validating interoperability with other subsystems and assessing user experience (data, voice, video, slice-specific services) under varying conditions such as handovers and radio environments.

Test Environment Setup
----------------------

The following diagram illustrates a typical setup used for Performance Testing of OFH:

.. figure:: /_static/Performance.png
   :alt: OFH Performance Testing Setup
   :align: center
   :width: 600px

   Fig.: Performance Testing Setup

In this scenario, O-DU and O-RU are provided by different vendors, A and B, respectively. The DUTs are integrated with the testing equipment available at the OTIC. In the lab environment, the UE and the RAN are connected via an RF cable (not OTA). Depending on the synchronization topology (e.g., LLS-C1, LLS-C3), additional components such as switches for routing PTP might be required.

This modular architecture allows OTIC to conduct performance tests on the OFH by integrating the O-DU and O-RU from different vendors with the test equipment into a controlled lab setup to emulate real-world deployment scenarios.

Example Test Case
-----------------

Consider the following test case specified in TIFG.TS.E2E from the O-RAN Alliance:

**Test Case 5.3 — 5G SA registration and deregistration of single UE**

- **Objective:** Verify the full registration and deregistration procedure with a single UE, including PDU session establishment and release.
- **Expected Result:** Successful registration, deregistration, PDU session establishment, and PDU session release for 10 consecutive iterations without failure, capturing required radio parameters and latency KPIs. Latencies are analyzed against target KPIs with min/avg/max values reported per the specification.

Conclusion
----------

The end-to-end performance testing demonstrates that the O-RAN architecture can deliver reliable, standards-compliant services in a multi-vendor environment. While this phase focuses on Open Fronthaul, the tests validate key KPIs such as latency, throughput, and service quality, along with successful execution of registration, session management, and handover procedures. The results confirm interoperability between O-DU and O-RU components and readiness for real-world deployment, with gap analysis highlighting areas for optimization and future enhancements across other O-RAN interfaces.