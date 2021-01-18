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

Create Era Network Profiles for Oracle RAC
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Oracle RAC Cluster has the following Networking interfaces in at least two different managed VLANs. These VLANs need to be able to provide IPs for the clients requesting it.

- Private Service
- Client Access
- Public Service
- Scan Service

The Private Service connections cannot be in the same VLAN as the others.

Each HPOC cluster is pre-configured with 2 VLANs which can be used for VMs: This is sufficient for our Oracle RAC implementation. Larger productions environments may have different VLANs for each of the component's connectivity.

.. list-table::
  :widths: 25 25 25 10 30
  :header-rows: 1

  * - Network Name
    - Network Address
    - Subnet Mask
    - VLAN
    - DHCP Scope
  * - Primary
    - 10.42.\ *XYZ*\ .1/25
    - 255.255.255.128
    - 0
    - 10.42.\ *XYZ*\ .50-10.42.\ *XYZ*\ .124
  * - Secondary
    - 10.42.\ *XYZ*\ .129/25
    - 255.255.255.128
    - *XYZ1*
    - 10.42.\ *XYZ*\ .132-10.42.\ *XYZ*\ .253

In our lab we will only create and assign two VLANs inside Era and the DHCP distribution to be managed by Era.

.. note::

  We are assuming that the following IPs are available for use. If you are unsure please use a IP scanner `tool <https://angryip.org/download/>`_ to choose 2 blocks of 5 IP addresses. This will be enough for our Oracle RAC 2 node cluster.


**RAC Network Design Visualisation**

.. figure:: images/rac1.png

**RAC IP Requirements**

We will need the following number of IPs for a two node Oracle RAC Clusters

.. list-table::
  :widths: 10 10 10 10
  :header-rows: 1

  * - Network Name
    - No. of RAC Nodes
    - No. of IPs per Node
    - Total IPs required
  * - Public
    - 2
    - 1
    - 2
  * - Private + Scan + CA
    - 2
    - 3
    - 6
  * - Totals
    -
    -
    - 8

Based on these design requirements we will create networks in Prism Central and then configure DHCP pool in Era. We will configure a larger block to make sure it will suffice a future deployment for testing.

.. list-table::
  :widths: 25 20 20 10 30 15
  :header-rows: 1

  * - Network Name
    - Network Address
    - Subnet Mask
    - VLAN
    - DHCP Scope
    - No. of IPs
  * - XYZ-RAC-Private
    - 10.42.\ *XYZ*\ .1/25
    - 255.255.255.128
    - 0
    - 10.42.\ *XYZ*\ .101-10.42.\ *XYZ*\ .105
    - 5
  * - XYZ-RAC-Public-Scan-CA
    - 10.42.\ *XYZ*\ .129/25
    - 255.255.255.128
    - *XYZ1*
    - 10.42.\ *XYZ*\ .141-10.42.\ *XYZ*\ .160
    - 20

**Create Networks in Prism Element**

#. In Prism Element, click **Settings > Network Configuration**

#. Click on **+ Create Network**

#. Enter **XYZ-RAC-Private** as Network Name and **0** as VLAN ID

#. Click on **Save**

#. Click on **+ Create Network**

#. Enter **XYZ-RAC-Public-Scan-CA** as Network Name and **XYZ1** as VLAN ID (for example: 71 is the VLAN ID)

#. Click on **Save**

**Create Networks in Era**

Now we will create corresponding networks in Era and assign DHCP pool. This would be the equivalent of the network administration providing IP blocks to be exclusively used for Oracle RAC servers.

#. Switch your browser to Era

#. In Era Menu, select **Administration**

#. Click on **Networks > Add**

#. Choose **XYZ-RAC-Private**

#. Select **Manage IP Address Pool**

#. Enter **Gateway, Subnet Mask, Primary DNS, First Address** and **Last Address** as shown below. Refer the above table for values.

   .. figure:: images/rac2.png

#. Click on **Add**

Now let's create the XYZ-RAC-Public-Scan-CA network in Era

#. Click on **Networks > Add**

#. Choose **XYZ-RAC-Public-SCAN-CA**

#. Select **Manage IP Address Pool**

#. Enter **Gateway, Subnet Mask, Primary DNS, First Address** and **Last Address** as shown below. Refer the above table for values.

   .. figure:: images/rac3.png

#. Click on **Add**


Create Oracle RAC Cluster with Era
++++++++++++++++++++++++++++++++++++

In this exercise you will deploy a fresh Oracle RAC database using your *Initials*\ **_ORACLE_19C** 1.0 Software Profile.

#. Select **Databases** from the dropdown menu and **Sources** from the lefthand menu.

#. Click **+ Provision > RAC Database**.

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
