.. title:: Databases: Era with Oracle Bootcamp

.. toctree::
   :maxdepth: 2
   :caption: Era with Oracle
   :name: _dbs
   :hidden:

   deploy_oracle/deploy_oracle
   deploy_oracle_era/deploy_oracle_era
   admin_oracle/admin_oracle
   patching_oracle/patching_oracle

.. toctree::
  :maxdepth: 2
  :caption: Optional Labs
  :name: _optional_labs
  :hidden:

  era_rest_api/era_rest_api

.. toctree::
  :maxdepth: 2
  :caption: Appendix
  :name: _appendix
  :hidden:

  appendix/glossary
..  tools_vms/windows_tools_vm
  tools_vms/linux_tools_vm
  labsetup/labsetup


.. _getting_started:

---------------
Getting Started
---------------

Welcome to the Databases bootcamp. This bootcamp is meant to provide you with first hand experience in why Nutanix is an ideal platform for Database workloads.

Historically, it has been a challenge to virtualize Oracle because of the high cost of traditional virtualization stacks and the impact that a SAN-based architecture can have on performance. Businesses and their IT departments have constantly fought to balance cost, operational simplicity, and consistent predictable performance.

The Nutanix Enterprise Cloud removes many of these challenges and makes virtualizing a business-critical application such as Oracle much easier. The Acropolis Distributed Storage Fabric (DSF) is a software-defined solution that provides all the features one typically expects in an enterprise SAN, without a SAN’s physical limitations and bottlenecks. Oracle particularly benefits from the following DSF features:

- Localized I/O and the use of flash for index and key database files to lower operation latency.
- A highly distributed approach that can handle both random and sequential workloads.
- The ability to add new nodes and scale the infrastructure without system downtime or performance impact.
- Nutanix data protection and disaster recovery workflows that simplify backup operations and business continuity processes.

In addition to solving common infrastructure problems for hosting business critical applications, Nutanix also seeks to address many of the key pain points associated with managing databases.

.. figure:: images/4.png

Based on a 2018 IDC study of 500 North American companies with more than 1,000 employees, they estimate:

- 77% of the organizations have more than 200 database instances in their production
- 82% have more than 10 copies of each DB
- 45%-60% the total storage capacity is dedicated to accommodating copy data
- 32% of database clones require daily refreshes for analytics of dev/test
- Copy data will cost IT organizations $55.63 billion in 2020

Maintaining the status quo leads to inefficient usage of both storage and worse, of administrator time. Meet Nutanix Era.

.. figure:: images/5.png

Nutanix Era provides DBaaS for your Enterprise Cloud. Leveraging the Nutanix Enterprise Cloud OS, we are able to take advantage of the power of full stack - data, compute, and software. Nutanix Era hides the complexity of database operations and provides common APIs, CLI, and consumer-grade GUI experience for multiple database engines. It makes database operations such as cloning efficient, thereby driving down the TCO of database management for our customers.


What's New
++++++++++

- Workshop updated for the following software versions:
    - AOS 5.15.x | 5.16.x | 5.17.x | 5.18.x
    - Prism 2020.9
    - Era 3.0.0

- Optional Lab Updates:

Agenda
++++++

- Introductions
- Lab Setup
- Deploy Oracle
- Deploy Oracle with Era
- Patching Oracle with Era
- Admin Oracle with Era

Optional labs:

- Era API Explorer

Introductions
+++++++++++++

- Name
- Familiarity with Nutanix

Initial Setup
+++++++++++++

- Take note of the *Passwords* being used.
- Log into your virtual desktops (connection info below)

Cluster assignment
++++++++++++++++++

The instructor will tell the attendees their assigned clusters.

.. note::
  If these are Single Node Clusters (SNCs) pay close attention on the networking part. The SNCs are completely different setup and configured compared to the "normal" three/four node clusters

Environment Details
+++++++++++++++++++

Nutanix Workshops are intended to be run in the Nutanix Hosted POC environment. Your cluster will be provisioned with all necessary images, networks, and VMs required to complete the exercises.

Networking
..........

As we are able to provide three/four node clusters and single node clusters in the HPOC environment, we need to describe each sort of cluster separately. The clusters are setup and configured differently.

Three/Four node HPOC clusters
-----------------------------

Three or four node Hosted POC clusters follow a standard naming convention:

- **Cluster Name** - POC\ *XYZ*
- **Subnet** - 10.\ **21**\ .\ *XYZ*\ .0
- **Cluster IP** - 10.\ **21**\ .\ *XYZ*\ .37

For example:

- **Cluster Name** - POC055
- **Subnet** - 10.38.55.0
- **Cluster IP** - 10.21.55.37 for the VIP of the Cluster


Throughout the Workshop there are multiple instances where you will need to substitute *XYZ* with the correct octet for your subnet, for example:

.. list-table::
  :widths: 25 75
  :header-rows: 1

  * - IP Address
    - Description
  * - 10.38.\ *XYZ*\ .37
    - Nutanix Cluster Virtual IP
  * - 10.38.\ *XYZ*\ .39
    - **PC** VM IP, Prism Central
  * - 10.38.\ *XYZ*\ .41
    - **DC** VM IP, NTNXLAB.local Domain Controller

Each cluster is configured with 2 VLANs which can be used for VMs:

.. list-table::
  :widths: 25 25 10 40
  :header-rows: 1

  * - Network Name
    - Address
    - VLAN
    - DHCP Scope
  * - Primary
    - 10.38.\ *XYZ*\ .1/25
    - 0
    - 10.38.\ *XYZ*\ .50-10.21.\ *XYZ*\ .124
  * - Secondary
    - 10.38.\ *XYZ*\ .129/25
    - *XYZ1*
    - 10.38.\ *XYZ*\ .132-10.21.\ *XYZ*\ .253

Single Node HPOC Clusters
-------------------------

For some workshops we are using Single Node Clusters (SNC). The reason for this is to allow more people to have a dedicated cluster but still have enough free clusters for the bigger workshops including those for customers.

The network in the SNC config is using a /26 network. This splits the network address into four equal sizes that can be used for workshops. The below table describes the setup of the network in the four partitions. It provides essential information for the workshop with respect to the IP addresses and the services running at that IP address.

.. list-table::
  :widths: 15 15 15 15 40
  :header-rows: 1

  * - Partition 1
    - Partition 2
    - Partition 3
    - Partition 4
    - Service
    - Comment
  * - 10.38.x.1
    - 10.38.x.65
    - 10.38.x.129
    - 10.38.x.193
    - Gateway
    -
  * - 10.38.x.5
    - 10.38.x.69
    - 10.38.x.133
    - 10.38.x.197
    - AHV Host
    -
  * - 10.38.x.6
    - 10.38.x.70
    - 10.38.x.134
    - 10.38.x.198
    - CVM IP
    -
  * - 10.38.x.7
    - 10.38.x.71
    - 10.38.x.135
    - 10.38.x.199
    - Cluster IP
    -
  * - 10.38.x.8
    - 10.38.x.72
    - 10.38.x.136
    - 10.38.x.200
    - Data Services IP
    -
  * - 10.38.x.9
    - 10.38.x.73
    - 10.38.x.137
    - 10.38.x.201
    - Prism Central IP
    -
  * - 10.38.x.11
    - 10.38.x.75
    - 10.38.x.139
    - 10.38.x.203
    - AutoDC IP(DC)
    -
  * - 10.38.x.32-37
    - 10.38.x.96-101
    - 10.38.x.160-165
    - 10.38.x.224-229
    - Objects 1
    -
  * - 10.38.x.38-58
    - 10.38.x.102-122
    - 10.38.x.166-186
    - 10.38.x.230-250
    - Primary network IPAM
    - 6 Free IPs free for static assignment


Credentials
...........

.. note::

  The *<Cluster Password>* is unique to each cluster and will be provided by the leader of the Workshop.

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Credential
     - Username
     - Password
   * - Prism Element
     - admin
     - *<Cluster Password>*
   * - Prism Central
     - admin
     - *<Cluster Password>*
   * - Controller VM
     - nutanix
     - *<Cluster Password>*
   * - Prism Central VM
     - nutanix
     - *<Cluster Password>*

Each cluster has a dedicated domain controller VM, **DC**, responsible for providing AD services for the **NTNXLAB.local** domain. The domain is populated with the following Users and Groups:

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Group
     - Username(s)
     - Password
   * - Administrators
     - Administrator
     - nutanix/4u
   * - SSP Admins
     - adminuser01-adminuser25
     - nutanix/4u
   * - SSP Developers
     - devuser01-devuser25
     - nutanix/4u
   * - SSP Consumers
     - consumer01-consumer25
     - nutanix/4u
   * - SSP Operators
     - operator01-operator25
     - nutanix/4u
   * - SSP Custom
     - custom01-custom25
     - nutanix/4u
   * - Bootcamp Users
     - user01-user25
     - nutanix/4u

Access Instructions
+++++++++++++++++++

The Nutanix Hosted POC environment can be accessed a number of different ways:

Lab Access User Credentials
...........................

PHX Based Clusters:
**Username:** PHX-POCxxx-User01 (up to PHX-POCxxx-User20), **Password:** *<Provided by Instructor>*

RTP Based Clusters:
**Username:** RTP-POCxxx-User01 (up to RTP-POCxxx-User20), **Password:** *<Provided by Instructor>*

Frame VDI
.........

Login to: https://console.nutanix.com/x/labs

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Parallels VDI
.................

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Employee Pulse Secure VPN
..........................

Download the client:

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Install the client.

In Pulse Secure Client, **Add** a connection:

For PHX:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - PHX
- **Server URL** - xlv-uswest1.nutanix.com

For RTP:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - RTP
- **Server URL** - xlv-useast1.nutanix.com


Nutanix Version Info
++++++++++++++++++++

- **AHV Version** - AHV 20170830.337 (AOS 5.11+)
- **AOS Version** - 5.15.x | 5.16.x | 5.17.x | 5.18.x
- **PC Version** - Prism 2020.9
