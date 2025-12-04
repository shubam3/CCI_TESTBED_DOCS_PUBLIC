==================
OpenStack Guide V1
==================

This page outlines the **next-iteration enhancements** planned for the OpenStack documentation and training material.  
Use this as a companion to the existing ``OpenStack Instance Launch Guide`` and as a checklist for future content and video creation.

.. contents::
   :local:
   :depth: 2


Instance Creation Workflows
===========================

Separate Blocks for Compute, Radio, and GPU Instances
-----------------------------------------------------

The existing guide already explains how to create different instance types.  
In this V1 guide, we will maintain **separate, clearly labeled blocks** that users can follow independently:

- **Compute Instance (Default AZ: ``compute``):**
  
  - Focus on standard CPU-only workloads.
  - Emphasize choosing **Create New Volume = Yes** for data safety.
  - Include a compact step-by-step table or checklist (to be added) derived from the main guide.

- **Radio Instance (Default AZ: ``radio``):**
  
  - Explain that radio instances are used with **USRP-attached networks**.
  - Highlight the additional **USRP network port** and the need for:
    - Admin approval for USRP access.
    - Network configuration inside the instance (netplan, jumbo MTU, etc.).
  - Link back to the **CLI radio instance example** in the main OpenStack guide.

- **GPU Instance (Default AZ: ``gpu``):**
  
  - Clarify quota and approval requirements for GPU usage.
  - Provide a short checklist for:
    - Selecting a GPU flavor.
    - Verifying CUDA / GPU visibility inside the instance.


Instance Creation Videos
========================

Video: Creating an Instance **Without** a Volume
-----------------------------------------------

This planned video will demonstrate:

- Launching a small **ephemeral instance** with **Create New Volume = No**.
- Showing that:
  - The root disk is tied to the host.
  - Deleting the instance **permanently deletes** all data on that instance.
- Recommended use cases:
  - Short-lived test workloads.
  - Stateless services where data is stored externally (e.g., object storage, Git).

Video: Creating an Instance **With** a Volume
--------------------------------------------

A second video will demonstrate:

- Launching an instance with **Create New Volume = Yes** and explaining the two options:
  - **Delete Volume on Instance Delete = Yes** (cleanup-oriented).
  - **Delete Volume on Instance Delete = No** (data re-use / long-term storage).
- Pros and cons:
  - **Pros:**
    - Data survives host failures (stored on Cinder).
    - Volumes can be re-attached to new instances.
  - **Cons:**
    - Slightly more overhead in provisioning.
    - Requires quota management for volumes.

.. note::

   Once the videos are recorded, they can be embedded here similar to the
   existing videos in ``getting_started/openstack.rst`` using ``.. raw:: html``
   and the ``<video>`` tag.


Accessing Instances Through the Gateway
=======================================

Overview
--------

Most OpenStack projects are only reachable from within the **CCI xG Testbed network**.  
Access typically happens in **two hops**:

1. SSH into the **gateway node** from your local machine.
2. From the gateway node, SSH into your OpenStack instance.

Step-by-Step Access Flow
------------------------

1. **SSH to the Gateway Node:**

   .. code-block:: bash

      ssh <username>@<gateway-ip>

2. **From the Gateway, SSH to the Instance:**

   - If your instance has a **Floating IP**:

     .. code-block:: bash

        ssh ubuntu@<floating-ip>

   - If your instance only has an **internal IP**:

     .. code-block:: bash

        ssh ubuntu@<internal-ip>

3. **Key Management and Security:**

   - Use SSH keys instead of passwords where possible.
   - Keep keys private and revoke them immediately if compromised.

.. note::

   See the main OpenStack guide section on **Floating IPs** and **Security Groups**
   for details on making sure SSH (port 22) is allowed from the gateway or other
   management nodes.


Creating Zun Containers
=======================

Zun is the OpenStack service for running application containers.  
This section outlines a **high-level workflow**; actual project-specific commands
can be added as we standardize on images and flavors.

Dashboard-Based Workflow (Planned)
----------------------------------

1. Log in to the OpenStack dashboard.
2. Navigate to **Project → Containers** (Zun UI, if enabled).
3. Click **Create Container**.
4. Provide:
   - **Container Name**
   - **Image** (e.g., ``docker.io/library/ubuntu:22.04``)
   - **Command** to run.
5. Attach the container to the appropriate network.
6. Launch and verify connectivity (e.g., via ``ping`` or HTTP).

CLI-Based Workflow (Planned)
----------------------------

1. Install the Zun client:

   .. code-block:: bash

      pip install python-zunclient

2. List available images:

   .. code-block:: bash

      openstack appcontainer image list

3. Run a container:

   .. code-block:: bash

      openstack appcontainer run --name test-container \
        --image docker.io/library/ubuntu:22.04 \
        --net <network-name>

4. Check logs and status:

   .. code-block:: bash

      openstack appcontainer show test-container
      openstack appcontainer logs test-container

.. note::

   The exact commands and Zun availability depend on the deployment.
   This section will be updated once the production Zun configuration is finalized.


Managing Security Group Protocols
=================================

This section builds on the **Verifying Security Groups in OpenStack** part of the main guide,
but focuses specifically on **protocol and port management**.

Adding Protocol Rules
---------------------

1. Navigate to **Project → Network → Security Groups**.
2. Select your security group and open the **Manage Rules** page.
3. Click **Add Rule** and choose:

   - **Rule Type**: Custom TCP, Custom UDP, or Other Protocol.
   - **Port Range**:
     - e.g., ``22`` for SSH, ``80``/``443`` for HTTP/HTTPS,
       or a range like ``49152-65535`` for high ports.
   - **Remote**:
     - CIDR (e.g., ``0.0.0.0/0`` for all, or a restricted subnet).

4. Save the rule and confirm that it appears in the rule list.

Best Practices
--------------

- Allow **only the minimum required ports**.
- Prefer restricting **source CIDR** instead of opening to the entire internet.
- Reuse common security groups (e.g., ``ssh-only``, ``http-only``) across projects.


Controlling USRPs from OpenStack
================================

High-Level Concept
------------------

USRP devices are typically mapped to **radio instances** via dedicated networks and ports.  
While the **power state** is usually managed out-of-band (e.g., lab power controllers),
certain administrative operations can be driven from within OpenStack workflows.

Planned Automation Ideas
------------------------

- **Turn On / Turn Off Access to USRPs From OpenStack:**

  - Use a **small control instance** or script that:
    - Manages the USRP-facing network port (enable/disable).
    - Optionally triggers power control APIs (if integrated with lab PDUs).

  - Example (conceptual) flow:

    1. User launches a **radio instance**.
    2. A script or Heat template:
       - Attaches the correct USRP network port.
       - Optionally updates netplan inside the instance.
    3. When the instance is deleted or shut down:
       - The script detaches or disables the USRP port.

.. important::

   The exact mechanism for **physically powering USRPs on/off** depends on
   the lab’s hardware (PDUs, management interfaces).  
   Once a standard method is in place, this section will be updated with
   concrete CLI commands or API calls that can be invoked from OpenStack
   or from a management VM.


