.. _configure_mssql:

--------------------------------------------------
Configuring Your Microsoft SQL and Era Environment
--------------------------------------------------

Traditional database VM deployment resembles the diagram below. The process generally starts with a IT ticket for a database (from Dev, Test, QA, Analytics, etc.). Next one or more teams will need to deploy the storage resources and VM(s) required. Once infrastructure is ready, a DBA needs to provision and configure database software. Once provisioned, any best practices and data protection/backup policies need to be applied. Finally the database can be handed over to the end user. That's a lot of handoffs, and the potential for a lot of friction.

.. figure:: images/0.png

Whereas with a Nutanix cluster and Era, provisioning and protecting a database should take you no longer than it took to read this intro.

Source Microsoft SQL VM
+++++++++++++++++++++++

.. note:: Your `UserXX` designation is assigned by the SE leading the Bootcamp. Please do not proceed until this has been provided to you.

#. Log in to your *UserXX*\ **-MSSQLSourceVM** with the below credentials right-clicking on the VM name, and choosing **Launch Console**.

   - **Username** - Administrator
   - **Password** - Nutanix/4u

#. Cancel *Shutdown Event Tracker*.

#. Disable Windows Firewall for all networks.

#. Open SQL Server Managment Studio (SSMS), choose **Windows Authentication** from the *Authentication* dropdown, and click **Connect**.

#. Verify you can browse the *SampleDB* database.

..  Clone Source MSSQL VM
  +++++++++++++++++++++

  **In this lab you will deploy a Microsoft SQL Server VM, by cloning a source MSSQL VM. This VM will act as a master image to create a profile for deploying additional SQL VMs using Era.**

  #. In **Prism Central**, select :fa:`bars` **> Virtual Infrastructure > VMs**.

     .. figure:: images/1.png

  #. Select the checkbox for **Win2016SQLSource**, and click **Actions > Clone**.

  #. Fill out the following fields:

     - **Number Of Clones** - 1
     - **Name** - *Initials*-MSSQL
     - **vCPU(s)** - 2
     - **Number of Cores per vCPU** - 1
     - **Memory** - 4 GiB

     .. figure:: images/clone_mssql_source.png

  #. Click **Save** to create the VM.

  #. Select your VM and click **Actions > Power On**.

  #. Log in to the VM (**Cancel** Shutdown Event Tracker):

     - **Username** - Administrator
     - **Password** - Nutanix/4u

  #. Disable Windows Firewall for all.

  #. Open SQL Server Managment Studio (SSMS), choose **Windows Authentication** from the *Authentication* dropdown, and click **Connect**.

  #. Verify you can browse the **SampleDB**.

Exploring Era Resources
+++++++++++++++++++++++

Era is distributed as a virtual appliance that can be installed on either AHV or ESXi. For the purposes of this workshop, a shared Era server has already been deployed on your cluster.

.. note::

   If you're interested, instructions for the brief installation of the Era appliance can be found `here <https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Era-User-Guide-v2_1:era-era-installation-c.html>`_. This includes instructions for both AHV and ESXi.

#. In **Prism Central**, select :fa:`bars` **> Virtual Infrastructure > VMs**. Choose **List** from the left-hand side.

#. Identify the IP address assigned to the *Era* VM using the *IP Addresses* column.

#. Open \https://`<ERA-VM-IP>`:8443/ in a new browser tab.

#. Login using the following credentials:

   - **Username** - admin
   - **Password** - `<CLUSTER-PASSWORD>`

#. From the dropdown menu, select **Administration**.

#. Select **Era Service** from the left-hand side. Note that Era has already been configured for your assigned cluster.

   .. figure:: images/6.png

   .. #. Select **Era Resources** from the left-hand menu.
   ..
   .. #. Review the configured Networks. If no Networks show under **VLANs Available for Network Profiles**, click **Add**. Select **Secondary** VLAN and click **Add**.
   ..
   ..    .. note::
   ..
   ..       Leave **Manage IP Address Pool** unchecked, as we will be leveraging the cluster's IPAM to manage addresses
   ..
   ..    .. figure:: images/era_networks_001.png

#. From the dropdown menu, select **SLAs**.

   Era has five built-in SLAs: Gold, Silver, Bronze, Brass, and Zero. SLAs control how the database server is backed up, or in the case of the *Zero* SLA, excluded from being backed up entirely. Backups can be configured with a combination of Continuous Protection, Daily, Weekly, Monthly, and Quarterly protection intervals.

#. From the dropdown menu, select **Profiles**.

   Profiles pre-define resources and configurations, making it simple to consistently provision environments and reduce configuration sprawl. For example, a *Compute* Profile specifies the size of the database server, including details such as vCPUs, cores per vCPU, and memory.

.. #. If you do not see any networks defined under **Network**, click **+ Create**.

   .. figure:: images/8.png

.. #. Fill out the following fields and click **Create**:

   - **Engine** - Microsoft SQL Server
   - **Name** - Primary-MSSQL-NETWORK
   - **Public Service VLAN** - Secondary

   .. figure:: images/9.png

Registering Your Microsoft SQL VM
+++++++++++++++++++++++++++++++++

Registering a database server with Era allows you to deploy databases to that resource, or use that resource as the basis for a Software Profile (Software Profiles will be expanded upon later in the workshop).

You must meet the following Era requirements before you register a SQL Server database with Era:

- A local user account or a domain user account with administrator privileges on the database server must be provided.
- Windows account or the SQL login account provided must be a member of sysadmin role.
- SQL Server instance must be running.
- Database files must not exist in boot (ex. C:\\) drive.
- Database must be in an online state.
- Windows remote management (WinRM) must be enabled

.. note::

   Your *UserXX*\ **-MSSQLSourceVM** VM already meets all of these criteria.

#. Within **Era**, select **Database Server VMs** from the dropdown menu, and then **List** from the left-hand menu.

   .. figure:: images/11.png

#. Click **+ Register > Microsoft SQL Server > Single Node Server VM** and fill out the following fields:

   - **IP Address or Name of VM** - *UserXX*\ **-MSSQLSourceVM**
   - **Windows Administrator Name** - Administrator
   - **Windows Administrator Password** - Nutanix/4u
   - **Instance** - MSSQLSERVER (This should auto-populate after providing credentials)
   - **Connect to SQL Server Admin** - Windows Admin User
   - **User Name** - Administrator

   .. note::

      If *MSSQLSERVER* doesn't automatically populate in the *Instance* field, this could indicate that the Windows Firewall in your *UserXX*\ **-MSSQLSourceVM** VM may not have been disabled correctly.

   .. figure:: images/12.png

   .. note::

    You can click **API Equivalent** for many operations in Era to enter an interactive wizard providing JSON payload based data you've input or selected within the UI, and examples of the API call in multiple languages (cURL, Python, Golang, Javascript, and Powershell).

    .. figure:: images/17.png

#. Click **Register** to begin ingesting the Database Server into Era.

#. Select **Operations** from the dropdown menu to monitor the registration. This process should take approximately 5 minutes.

   .. figure:: images/13.png

   .. note::

      It is also possible to register existing databases on any server, which will also register the database server it is on.

Creating A Software Profile
+++++++++++++++++++++++++++

Before additional SQL Server VMs can be provisioned, a Software Profile must first be created from the database server VM registered in the previous step. A software profile is a template that includes the SQL Server binaries and operating system. This template exists as a hidden, cloned disk image on your Nutanix storage.

#. Within **Era**, select **Profiles** from the dropdown menu, and then **Software** from the left-hand menu.

   .. figure:: images/14.png

#. Click **+ Create > Microsoft SQL Server** and fill out the following fields:

   - **Profile Name** - *Initials*\ _MSSQL_2016
   - **Description** - (Optional)
   - **Database Server** - Select your registered *Initials*\ -MSSQL VM

   .. figure:: images/15.png

#. Click **Next** and fill out the following fields:

   - **Operating System Notes** - (Optional)
   - **Database Software Notes** - (Optional)

#. Click **Create**.

#. Select **Operations** from the dropdown menu to monitor the registration. This process should take approximately 2 minutes.

   .. figure:: images/16.png

   .. note::

       If creating a profile from a server not gracefully shut down, it may be corrupt or may not provision successfully. Please ensure that *UserXX*\ **-MSSQLSourceVM** had a clean shutdown, and clean startup before registering profile to Era.
