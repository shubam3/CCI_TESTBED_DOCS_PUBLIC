Indoor Radio-Grid Testbed
==========================

Introduction
------------

The indoor radio-grid testbed is a central component of the North American OTIC facility, housed at the Virginia Tech Research Center in North Virginia (Washington DC area). It is designed to facilitate the design, development, and testing of cloud-enhanced, open-software-defined wireless technologies in a controlled laboratory environment. This facility supports real-world experimentation, performance evaluations, and interoperability testing for next-generation wireless systems.

Grid Configuration and Setup
------------------------------
This testbed features a novel two-dimensional grid configuration that measures approximately 32' x 29'. The grid is organized as a 9x8 array of programmable advanced software-defined radio (SDR) nodes. The nodes include a mix of Ettus Research models such as X310, X410, and N310, as well as B210 and B205-mini devices. These nodes can be interconnected into specified topologies to create reproducible wireless channel models. This flexible setup enables researchers to emulate a variety of wireless scenarios—from end-to-end CBRS experimentation (leveraging Virginia Tech’s Priority Access Licenses) to closed-loop O-RAN trials.

.. figure:: ../_static/TestBedLab069.jpg
   :alt: Indoor Radio Grid Ceiling Rack
   :align: center
   :scale: 20%
|

Laboratory Infrastructure and Capabilities
--------------------------------------------
The indoor testbed is situated within a dedicated 500 m² area of the overall ~2000 m² facility at the Research Center. Key infrastructure elements include:

- **Processing and Control:** Numerous CPUs and GPUs are located near the radio grid, enabling real-time signal processing and network function virtualization (NFV).
- **Maintenance and Support:** Approximately 45 m² is allocated for maintenance workstations and network components, ensuring the facility remains highly serviceable and up-to-date.
- **Remote Management:** Core testbed services run on computing clusters that support remote experiment execution, measurements, and data collection via secure VPN and SSH connections.
- **Hardware:** The testbed is equipped with various software-defined radio devices including USRP X310, X410, B210, B2005 mini, NE310s, as well as commercial devices from SAMSUNG and COTS UE. Additionally, the lab features virtual reality equipment including VR GLASSES, OCULUS and HOLOLENS for immersive research applications.


Remote Access and Security
--------------------------
Researchers access the indoor radio-grid securely through VPN and SSH. This remote access model ensures that sensitive equipment and data are protected using restricted-access VPN profiles and selective firewalls. At the same time, it provides convenient remote operation for academic, industry, and government users.

Experimentation and Research Applications
-------------------------------------------
The reconfigurable nature of the indoor radio-grid testbed supports a wide range of experiments and research initiatives, including:

- **End-to-End CBRS Experimentation:** Testing under licensed spectrum conditions using Virginia Tech’s Priority Access Licenses.
- **O-RAN and 5G Trials:** Conducting closed-loop O-RAN experiments and evaluating the interoperability of solutions from diverse vendors.
- **Edge-Cloud Continuum Experiments:** Integrating edge computing with core cloud services to support NFV, AI/ML-driven network optimization, and other advanced wireless applications.

By providing a controlled yet flexible environment, the indoor radio-grid testbed is an invaluable resource for accelerating research and innovation in next-generation wireless communication systems.
