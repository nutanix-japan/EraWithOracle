.. _deploy_oracle_rac_era:

--------------------------------------------------
Deploying Oracle RAC with Era
--------------------------------------------------

Oracle RAC is multi node clustered Oracle Instance implementation. Depending on the requirements there are as many instances (each in one Oracle VM) connecting to same disks where Oracle stores data. These disks are shared amongst all RAC hosts.

The estimated time to complete this lab is 1 hour. Oracle RAC deployment using Era takes about 1 hour for creating VMs, attaching disks and configuring the Oracle RAC Gridware software.

Create Oracle RAC Cluster with Era
++++++++++++++++++++++++++++++++++++

In this exercise you will deploy a fresh Oracle RAC server with 2 nodes.

Before we can create this Oracle RAC cluster, we need the building blocks of the Oracle RAC server.

Each of the Oracle RAC cluster building block is implemented using Era Profiles as follows:

.. list-table::
  :widths: 25 25
  :header-rows: 1

  * - Oracle RAC Component
    - Nutanix Era Building Block
  * - Network Connectivity
    - Network Profile
  * - Oracle Servers/VM/Nodes
    - Database Server Profile
  * - Oracle Software
    - Software Profile
  * - Oracle Database Parameters
    - Database Parameters Profile

We will now quickly create all these building blocks as Era Profiles.

Create Era Network Profile
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Oracle RAC Cluster has the following Networking interfaces in at least two different managed VLANs. These VLANs need to be able to provide IPs for the clients requesting it.

- Private Service
- Client Access
- Public Service
- Scan Service

The Private and Public Service connections cannot be in the same VLAN. Larger productions environments may have different VLANs for each of the component's connectivity.

In our lab we will only create and assign two VLANs inside Era and the DHCP distribution to be managed by Era. This would be the equivalent of the network administration providing IP blocks to be exclusively used for Oracle RAC servers.

#. In Era GUI click **Menu > Administration > Networks**

#. Click on **Add**

#. 





Create Oracle RAC Cluster with Era
++++++++++++++++++++++++++++++++++++

In this exercise you will deploy a fresh Oracle RAC database using your *Initials*\ **_ORACLE_19C** 1.0 Software Profile.

#. Select **Databases** from the dropdown menu and **Sources** from the lefthand menu.

#. Click **+ Provision > Single Node Database**.

#. In the **Provision a Database** wizard, fill out the following fields to configure the Database Server:

   - **Engine** - Oracle
   - **Database Server** - Create New Server
   - **Database Server Name** - *Initials*\ _oracle_prod
   - **Description** - (Optional)
   - **Software Profile** - *Initials*\ _ORACLE_19C
   - **Compute Profile** - ORACLE_SMALL
   - **Network Profile** - Primary_ORACLE_NETWORK
   - Select **Enable High Availability**
   - **SYS ASM Password** - oracle
   - **SSH Public Key for Node Access** - Select **Text**

   ::

      ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAii7qFDhVadLx5lULAG/ooCUTA/ATSmXbArs+GdHxbUWd/bNGZCXnaQ2L1mSVVGDxfTbSaTJ3En3tVlMtD2RjZPdhqWESCaoj2kXLYSiNDS9qz3SK6h822je/f9O9CzCTrw2XGhnDVwmNraUvO5wmQObCDthTXc72PcBOd6oa4ENsnuY9HtiETg29TZXgCYPFXipLBHSZYkBmGgccAeY9dq5ywiywBJLuoSovXkkRJk3cd7GyhCRIwYzqfdgSmiAMYgJLrz/UuLxatPqXts2D8v1xqR9EPNZNzgd4QHK4of1lqsNRuz2SxkwqLcXSw0mGcAL8mIwVpzhPzwmENC5Orw==


   .. note::

         By selecting Enable High Availability, Oracle Grid is configured as part of the deployment and Oracle Automatic Storage Management (ASM) is used for volume management. Without High Availability enabled, Linux LVM and file systems would be used for database storage. Grid and ASM are required for clustered Oracle RAC deployments.

   .. figure:: images/4.png
