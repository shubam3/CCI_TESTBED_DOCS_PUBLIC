FAQs
=====

1. How do I request access to the CCI xG Testbed?
-------------------------------------------------

- Email the admin team at cci.xg.testbed.admin@cyberinitiative.org with your affiliation, project description, and resource needs.
- You will receive onboarding details (accounts, VPN instructions, and OpenStack access) after approval.

2. How do I get help or report an issue?
----------------------------------------

- Email: cci.xg.testbed.admin@cyberinitiative.org for urgent issues.
- Redmine (preferred for tracking): `https://redmine.ccixgtestbed.org/redmine/ <https://redmine.ccixgtestbed.org/redmine/>`_. Create a ticket with your project, VM name, and screenshots/logs.

3. How do I connect remotely (VPN/SSH)?
---------------------------------------

- Follow the VPN instructions provided during onboarding to reach the testbed network.
- SSH to your VM using the keypair selected at creation (e.g., `ssh -i <key.pem> ubuntu@<IP>`). Make sure your security group allows TCP/22.

4. How do I make my VM reachable from the Internet?
---------------------------------------------------

- Allocate and associate a Floating IP: Project → Network → Floating IPs → Allocate → Associate to your instance’s port.
- Update security groups to allow the needed inbound ports (e.g., 22 for SSH, 80/443 for web services).

5. How do I assign two IPs to one OpenStack instance?
-----------------------------------------------------

- Option 1 (same network): Create a second port on the same network and attach it to the instance.
  1) Project → Network → Ports → Create Port (choose same network/subnet)
  2) Compute → Instances → Actions → Attach Interface → select the new port

- Option 2 (second network): Attach an additional interface from a different network for routing/NAT use cases.

- CLI example:

.. code-block:: bash

   openstack port create --network NET1 vm1-port-2
   openstack server add port <SERVER_ID_OR_NAME> vm1-port-2

- Inside the VM, configure the additional interface (netplan/systemd-networkd) and ensure security groups permit traffic.