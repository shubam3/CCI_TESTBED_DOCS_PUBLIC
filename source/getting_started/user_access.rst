=================================
 Getting Started with CCI xG Testbed
=================================

CCI xG Testbed provides a platform consisting of wireless and computing
resources for researchers to carry out advanced wireless
research. Users can access the platform by completing the User Sign-up Form with their personal and project details.
Once approved, users can utilize the allocated resources for executing their wireless experiments.

Access and Account Creation
==========================

User Sign-up Process
-----------------

To gain access to the CCI xG Testbed, you need to complete the User Sign-up Form:

`CCI xG Testbed User Sign-up Form <https://docs.google.com/forms/d/e/1FAIpQLSdabgove9qaSd6HdAFQQRSCwPfLcizga8na9gwxjZaWukF9qQ/viewform>`_

.. note:: Account requests typically take 2 business days for approval.

The form collects the following information:

* **Email address**
* **First Name and Last Name**
* **Phone number** (including country code)
* **Institution/Organization**
* **Department/School/Research Center**
* **Position/Title**
* **Country of Residence**
* **Purpose of CCI xG Testbed Usage** (detailed response required)
* **Project Name** (single word)
* **Acceptance of the CCI xG Testbed User Policy**

Once your submission is reviewed and approved, the Administrative team will send you an email 
containing your access credentials:

* **OpenStack Credentials**: For accessing the OpenStack dashboard and user environment.
* **Gateway Credentials**: For secure access to the network gateway.
* **Redmine Credentials**: For project management and support.

For detailed information on using the Gateway and Redmine systems, please refer to our 
:doc:`Gateway and Redmine Access Guide <gateway_and_redmine>`.

.. attention:: When completing the form, be specific about your project requirements in the "Purpose of CCI xG Testbed Usage" field to ensure that appropriate resources are allocated for your experiment.

CCI Dashboard User Flow
=======================

After receiving your access credentials, you'll be able to log in to the CCI Dashboard, which serves as the central hub for accessing various components of the CCI xG Testbed.

Authentication Flow
------------------

.. figure:: ../user-dashboard/user-flow.png
   :alt: User-Flow 
   :align: center
   :scale: 70%
   
   Figure: CCI Dashboard User Flow Diagram showing the authentication process and navigation options


1. Navigate to the CCI Dashboard login page
2. Enter your provided username and password
3. The system will validate your credentials
   - If invalid, an error message will be displayed, prompting you to re-enter your credentials
   - If valid, you'll be redirected to the main navigation page

Main Navigation Options
---------------------

After successful login, you'll be presented with a clean, intuitive interface offering four main options:

* **Non-RT Dashboard**: Access to Non-Real-Time RAN Intelligent Controller management
* **Near-RT Dashboard**: Access to Near-Real-Time RAN Intelligent Controller management
* **Clear-ML**: Access to the Clear-ML platform for ML model training and management
* **OpenStack Login**: Button to authenticate and access the OpenStack environment

Non-RT Dashboard
--------------

If you select the Non-RT Dashboard option, you'll gain access to:

* **Non-RT RIC Management**: Monitor and configure the Non-RT RIC platform
* **rApps Management**: Deploy, configure, and monitor rApps
* **Policy Management**: Create, edit, and distribute policies to Near-RT RICs

Near-RT Dashboard
---------------

If you select the Near-RT Dashboard option, you'll gain access to:

* **Near-RT RIC Management**: Monitor and configure the Near-RT RIC platform
* **xApps Management**: Deploy, configure, and monitor xApps
* **E2 Node Management**: Monitor and manage E2 Nodes (CU/DU) connected to the Near-RT RIC

OpenStack Access
--------------

If you click the OpenStack Login button:

* You'll be redirected to the OpenStack authentication page
* After successful authentication, you'll access the OpenStack Dashboard
* From there, you can manage instances, networks, volumes, etc.

.. note:: For detailed information about using the OpenStack Dashboard, please refer to our :doc:`OpenStack Instance Launch Guide <openstack>`.

Transitioning to the Experiment Environment
=========================================

How does a user go from the CCI xG Testbed Portal to the user environment?
-----------------------------------------------------------------------

Once your account is active, you can access the experiment environment through 
the OpenStack dashboard. Detailed instructions are provided in the CCI xG Testbed 
documentation as well as within the OpenStack dashboard itself.

Follow these steps:

1. Log in to the CCI xG Testbed Portal.
2. Use your provided OpenStack credentials to access the OpenStack dashboard.
3. Navigate through the dashboard to launch your experiment environment.

For detailed instructions on creating and managing OpenStack instances, please refer to our 
:doc:`OpenStack Instance Launch Guide <openstack>`.

.. note:: For the best experience with the CCI xG Testbed portal, we recommend 
          using modern web browsers such as Google Chrome, Mozilla Firefox, or 
          Microsoft Edge.
