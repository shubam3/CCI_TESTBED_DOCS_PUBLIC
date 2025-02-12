Introduction
============

Citizens Broadband Radio Service (CBRS)
---------------------------------------

The Citizens Broadband Radio Service (CBRS) spectrum (3550-3700 MHz) in the U.S. enables shared wireless communication through a three-tiered access system:

- **Incumbent Access (Tier 1)**: Reserved for military and government users with the highest priority.
- **Priority Access License (PAL, Tier 2)**: Licensed users with priority over unlicensed users, obtained via FCC auction. The Virginia Tech Foundation has acquired Priority Access Licenses (PALs) for the newly available Citizens Broadband Radio Service (CBRS). Virginia Tech’s priority access licenses include four 10-MHz blocks in Montgomery County and another four 10-MHz blocks in Craig County. The licenses are held by Virginia Tech Technology Assets (VTTA), a subsidiary of the Virginia Tech Foundation, and will be administered by the Division of Information Technology. `Virginia Tech Spectrum Information <https://it.vt.edu/partnerships/university-partnerships/spectrum.html>`_
- **General Authorized Access (GAA, Tier 3)**: Unlicensed users with access to available spectrum but must defer to higher tiers.



CBRS spectrum is managed by a Spectrum Access System (SAS) to avoid interference and priority protection. CBRS supports private LTE/5G networks, industrial IoT, and rural broadband, offering flexible, cost-effective cellular connectivity. These LTE/5G base stations are called CBRS Devices (CBSDs). SAS also has environmental sensing Capabilities (ESC) to detect incumbent users and protect them from interference. More information on Outdoor CBRS deployments can be found at `Stroubles Creek CBRS Testbed Site <https://ccixgtestbed.org/stroubles-creek-testbed-site.html>`_.

.. figure:: ../_static/opensas/cbrs_three_tier_system.png
   :align: center
   :alt: CBRS Three-Tier Priority Access System

   **Figure 1:** CBRS Three-Tier Priority Access System.

Open Source Spectrum Access System (OpenSAS)
--------------------------------------------

To enable research and experimentation in the CBRS ecosystem, Virginia Tech/CCI has developed an open-source SAS called OpenSAS `[3][4] <https://cci-opensas.readthedocs.io/en/latest/references.html>`_. OpenSAS aligns with WInnForum specifications and has successfully tested with Software Defined Radio (SDR) based CBSDs and ESCs. The role of the SAS is to allow spectrum management of CBSDs, activation of dynamic protection zones, and environmental sensing for incumbent protection. OpenSAS strives to adhere to WInnForum and FCC regulations on SAS and CBRS operations.

OpenSAS Architecture
--------------------

OpenSAS manages and enforces spectrum via the SAS-CBSD interface and the Environmental Sensing Capability (ESC) sensor nodes. The critical components incorporated inside OpenSAS to achieve this include the grant algorithm and the incumbent detection model. The architecture diagram for OpenSAS is provided in Figure 2.

.. figure:: ../_static/opensas/opensas_architecture.png
   :align: center
   :alt: OpenSAS Architecture

   **Figure 2:** OpenSAS Architecture.

The OpenSAS code has been updated to incorporate the HTTPS protocol, aligning it more closely with the ideal WInnForum SAS architecture.

CBSD (Citizens Broadband Radio Service Device)
----------------------------------------------

A CBSD is a CBRS device, typically a 5G base station with a CBSD client running on it. For example, a CBSD client with srsRAN gNodeB (gNB).

