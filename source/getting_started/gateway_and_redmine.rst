=================================
Gateway and Redmine Access Guide
=================================

This guide provides detailed information about accessing and using the CCI xG Testbed Gateway and Redmine project management system.

Gateway Access
=============

What is the Gateway?
------------------

The Gateway serves as a secure entry point to the CCI xG Testbed network infrastructure. It provides controlled access to the testbed resources and acts as a security boundary between the public internet and the testbed's internal network.

Accessing the Gateway
-------------------

After your account request is approved, you will receive Gateway credentials via email. These credentials include:

* **Username**: Your assigned gateway username
* **Password**: Your initial gateway password
* **Gateway URL/IP**: The address used to connect to the gateway

Connection Methods
----------------

SSH Access
^^^^^^^^^^

To connect to the gateway using SSH:

.. code-block:: bash

   ssh username@gateway-address

.. note:: We recommend using SSH key-based authentication for enhanced security. Instructions for setting up SSH keys are provided below.

Setting Up SSH Keys
^^^^^^^^^^^^^^^^^

1. Generate an SSH key pair on your local machine (if you don't already have one):

   .. code-block:: bash

      ssh-keygen -t rsa -b 4096

2. Copy your public key to the gateway:

   .. code-block:: bash

      ssh-copy-id username@gateway-address

   Alternatively, you can provide your public key to the CCI xG Testbed administrators who will add it to your account.

3. After setup, you can connect without entering your password:

   .. code-block:: bash

      ssh username@gateway-address

Gateway Security Policies
-----------------------

* All connections to the gateway are logged for security purposes
* Inactive sessions will be automatically disconnected after 30 minutes
* Multiple failed login attempts may result in temporary account lockout
* Do not share your gateway credentials with others

Transferring Files Through the Gateway
-----------------------------------

To transfer files to and from the testbed through the gateway, you can use SCP or SFTP:

Using SCP:

.. code-block:: bash

   # Upload a file to the gateway
   scp /path/to/local/file username@gateway-address:/destination/path

   # Download a file from the gateway
   scp username@gateway-address:/path/to/remote/file /local/destination/path

Using SFTP:

.. code-block:: bash

   # Start an SFTP session
   sftp username@gateway-address

   # Then use put and get commands
   put /path/to/local/file
   get /path/to/remote/file

Redmine Project Management
========================

What is Redmine?
--------------

Redmine is a flexible project management and issue tracking system used by the CCI xG Testbed to:

* Track experiment progress
* Report and resolve technical issues
* Collaborate with team members and testbed administrators
* Access documentation and knowledge base articles
* Manage project timelines and milestones

Accessing Redmine
---------------

After your account request is approved, you will receive Redmine credentials via email. These credentials include:

* **Username**: Your assigned Redmine username
* **Password**: Your initial Redmine password
* **Redmine URL**: The web address to access the Redmine system

To access Redmine:

1. Open your web browser
2. Navigate to the provided Redmine URL
3. Enter your username and password
4. Click "Login"

.. note:: Upon first login, you may be prompted to change your password. Choose a strong, unique password that you don't use for other services.

Using Redmine for Project Management
----------------------------------

Creating and Managing Issues
^^^^^^^^^^^^^^^^^^^^^^^^^^

Issues are the primary way to track tasks, bugs, and feature requests in Redmine:

1. Navigate to your project
2. Click "New issue"
3. Fill in the required fields:
   * Subject: A brief, descriptive title
   * Description: Detailed information about the issue
   * Priority: The importance of the issue
   * Assignee: Who should work on this issue
4. Click "Create" to submit the issue

Tracking Experiment Progress
^^^^^^^^^^^^^^^^^^^^^^^^^

You can use Redmine to track the progress of your experiments:

1. Create a new issue for each major experiment phase
2. Update the status as you progress
3. Add comments with results or observations
4. Upload attachments such as data files or screenshots
5. Set due dates to manage your timeline

Getting Support Through Redmine
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you encounter technical issues with the testbed:

1. Create a new issue in your project
2. Set the issue type to "Support"
3. Provide detailed information about the problem:
   * What you were trying to do
   * What happened instead
   * Any error messages you received
   * Steps to reproduce the issue
4. Assign the issue to the appropriate support team member or leave it unassigned

The support team will respond to your issue as soon as possible, typically within one business day.

Best Practices for Using Redmine
------------------------------

* Check Redmine regularly for updates on your issues
* Keep issue descriptions clear and concise
* Use appropriate issue categories and priorities
* Update issue statuses as they progress
* Include relevant attachments and screenshots when reporting problems
* Subscribe to notifications for issues you're interested in

Integrating Gateway and Redmine in Your Workflow
=============================================

For the most efficient use of the CCI xG Testbed, we recommend integrating both the Gateway and Redmine into your regular workflow:

1. Use the Gateway to access testbed resources and conduct experiments
2. Document your experiment setup and results in Redmine
3. Report any issues encountered during experiments through Redmine
4. Collaborate with team members and administrators through Redmine discussions
5. Use Redmine's wiki feature to document your project's methodology and findings

This integrated approach ensures that your experiments are well-documented and any issues are promptly addressed by the support team.

.. note:: For additional assistance with either the Gateway or Redmine, please contact the CCI xG Testbed support team through Redmine or via email at the address provided in your welcome message.
