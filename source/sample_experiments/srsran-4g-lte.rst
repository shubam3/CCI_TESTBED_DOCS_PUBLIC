End-to-end 4G LTE with srsRAN
=======================================


Overview
--------
This tutorial details the experimental setup and procedures for establishing a 4G LTE network using srsRAN and Ettus X310 USRPs. It also covers connecting a commercial off-the-shelf (COTS) UE (e.g., a smartphone) to the network using a SIM toolkit. 

**Note:** Before deploying the experiment, ensure you have proper access to the testbed (e.g., SSH access to the gateway node and virtual machines).

Objective
---------
- **Set Up a 4G LTE Network:** Deploy and configure a 4G network using srsRAN, two Ettus X310 USRPs, and two Ubuntu-based VMs.
- **Integrate a COTS UE:** Connect a mobile phone to the experimental network using a SIM toolkit along with the required software and hardware.
- **Performance Verification:** Validate network operation via performance tests (using iperf) and monitor outputs for troubleshooting.

Resources
---------
- **Hardware:**
   - Personal Computer (Host machine)
   - Two Ettus X310 USRPs
   - Laptop running Ubuntu (for COTS UE experiment)
   - USB Hub
   - Sysmocom USIM
   - Samsung Galaxy S20 (or equivalent smartphone)
   - Omnikey SIM Reader
  
- **Software:**
   - Ubuntu OS (on host and VMs)
   - srsRAN 4G Software Suite
   - UHD (USRP Hardware Driver, version 3.15.00)
   - iperf (for network performance testing)
   - pcsc-tools and pcscd (for SIM detection)
   - Sysmocom SIM Toolkit

Testbed Access Requirement
---------------------------
**Important:** Before deploying any experiment, ensure:
- SSH connectivity to the gateway node.
- Verified access to both VM-1 and VM-2.
- Confirmation that the USRPs are visible (e.g., via ``uhd_find_devices``).

.. image:: placeholders/testbed_access.png
   :alt: Testbed access diagram
   :align: center

Experimental Procedure
----------------------

### 1. 4G USRP Setup Using srsRAN and X310s
- **Date:** 8/21/2023

#### Host Machine Setup
1. Open a terminal on your host machine.
2. Connect to the gateway node via SSH:

   .. code-block:: sh

      ssh X@<gateway-node-IP>

3. Log in and then connect to VM-1/VM-2:

   .. code-block:: sh

      ssh XXX1@<VM-1/2-IP>

4. Enter passwords as prompted.

.. image:: placeholders/host_setup.png
   :alt: SSH connection to gateway node and VMs

#### Install UHD (For X310)
1. Update packages and install build tools:

   .. code-block:: sh

      sudo apt update && sudo apt upgrade -y
      sudo apt install cmake git libboost-all-dev libusb-1.0-0-dev libudev-dev libncurses5-dev

2. Clone and build UHD:

   .. code-block:: sh

      git clone https://github.com/EttusResearch/uhd.git
      cd uhd
      git checkout v3.15.0.0
      cd host
      mkdir build && cd build
      cmake ..
      make
      sudo make install
      sudo ldconfig

3. Test the installation:

   .. code-block:: sh

      uhd_find_devices

.. image:: placeholders/uhd_install.png
   :alt: UHD installation and device detection

#### Install srsRAN 4G Software Suite
1. Install required libraries:

   .. code-block:: sh

      sudo apt-get install build-essential cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev libconfig++-dev libsctp-dev

2. Clone and build srsRAN 4G:

   .. code-block:: sh

      cd ~
      git clone https://github.com/srsRAN/srsRAN_4G.git
      cd srsRAN_4G
      mkdir build && cd build
      cmake ../
      make
      make test

3. Install the suite:

   .. code-block:: sh

      sudo make install

.. image:: placeholders/srsran_build.png
   :alt: srsRAN 4G build process

#### VM Setup and Application Launch
- **On VM-1:**
  - Run ``sudo srsepc`` to start the EPC (core network).
  - Run ``sudo srsenb`` to start the eNodeB.
- **On VM-2:**
  - Run ``sudo srsue`` to start the UE process.

.. image:: placeholders/vm_setup.png
   :alt: VM setup and error resolution

#### Network Performance Testing with iperf
1. On VM-1, start the iperf server:

   .. code-block:: sh

      iperf -s

2. On VM-2, run the iperf client test:

   .. code-block:: sh

      iperf -c 172.16.0.1 -i1 -t60 -u -b 40M

3. Record the throughput (DL and UL performance).

.. image:: placeholders/iperf_results.png
   :alt: iPerf network performance results
   :align: center

### 2. Connecting Phone to 4G Network Using COTS UE
#### SIM Card and Reader Setup
1. Install the necessary tools:

   .. code-block:: sh

      sudo apt install pcsc-tools pcscd

2. Verify SIM detection using:

   .. code-block:: sh

      pcsc_scan

3. If the SIM card isn’t detected, restart the pcscd daemon:

   .. code-block:: sh

      sudo systemctl start pcscd

.. image:: placeholders/sim_detection.png
   :alt: SIM card detection using pcsc_scan

#### Sysmocom SIM Toolkit Installation
1. Clone the Sysmocom toolkit repository:

   .. code-block:: sh

      git clone https://gitea.sysmocom.de/sysmocom/sysmo-usim-tool.git

2. Install dependencies:

   .. code-block:: sh

      sudo apt-get install libpcsclite-dev swig python3-pyscard
      pip install pytlv

3. Display the toolkit help:

   .. code-block:: sh

      cd sysmo-usim-tool
      ./sysmo-isim-tool.sja2.py -h

.. image:: placeholders/usim_tool.png
   :alt: Sysmocom SIM Toolkit usage

#### Configuration File Adjustments
1. Edit configuration files on VM-1:
   - **epc.conf:**  
     Open for editing:

     .. code-block:: sh

        vi /home/cci/.config/srsran/epc.conf

     Change the MCC to 901 and MNC to 70.
   - **enb.conf:**  
     Open for editing:

     .. code-block:: sh

        vi /home/cci/.config/srsran/enb.conf

     Adjust MCC/MNC similarly.
   - **user_db.csv:**  
     Update the entry using the format:

     ``(ue_name),(algo),(IMSI),(K),(OP/OPc_type),(OP/OPc_value),(AMF),(SQN),(QCI),(IP_alloc)``

     **Example entry:**

     .. code-block:: none

        ue1,mil,901700000052036,4933f9c5a83e5718c52e54066dc78dcf,opc,fc632f97bd249ce0d16ba79e6505d300,9000,0000000060f8,9,dynamic

.. image:: placeholders/config_edit.png
   :alt: Editing configuration files

#### Deploying the COTS UE Connection
1. On VM-1, start the EPC and eNodeB services:

   .. code-block:: sh

      sudo srsepc
      sudo srsenb

2. On VM-2 (or the designated UE device), run:

   .. code-block:: sh

      sudo srsue

3. Configure the phone with the updated USIM details and connect to the network.
4. Additional configuration may be required (e.g., run ``sudo srsepc_if_masq.sh ens3``).

Results and Graphics
---------------------
- **Results Generated:**  
  - Terminal outputs (device discovery, build logs, error resolutions).
  - Network performance data (throughput values, DL/UL bitrates from iperf).
  - Connection logs confirming successful UE registration.
- **Graphics:**  
  Screenshots and plots generated from recorded data can be added here.

.. image:: placeholders/experiment_overview.png
   :alt: Overall experimental setup
   :align: center

Conclusion
----------
This experiment demonstrates how to:
- Configure a 4G LTE network using srsRAN with Ettus X310 USRPs.
- Set up multiple VMs running EPC, eNodeB, and UE processes.
- Integrate a COTS UE (mobile phone) via the Sysmocom SIM toolkit.
- Validate network performance with iperf and monitor data transmission.

References
----------
- srsRAN 4G Documentation
- Ettus Research UHD Installation Guides
- Sysmocom USIM Toolkit Documentation
