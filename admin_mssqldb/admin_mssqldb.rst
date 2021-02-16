.. _admin_mssqldb:

--------------------------
DB Administration with Era
--------------------------

We will now see how to perform normal database admin task with Era.

**In this lab you will Administer your MSSQL DB**

Register Your Database
++++++++++++++++++++++

#. Within **Era**, select **Databases** from the dropdown menu, and then **Sources** from the left-hand menu.

   .. figure:: images/1.png

#. Click **+ Register > Microsoft SQL Server > Database** and fill out the following fields:

   - **Database is on a Server VM that is:** - Registered
   - **Registered Database Servers** - *UserXX*\ **-MSSQLSourceVM**

   .. figure:: images/2.png

#. Click **Next**.

   - **Unregistered Databases** - SampleDB
   - **Database Name in Era** - *Initials*\ -LABSQLDB

   .. figure:: images/3.png

#. Click **Next**.

   - **Recovery Model** - Full
   - **Manage Log Backups with** - Era
   - **Name** - *Initials*\ -LABSSQLDB_TM
   - **SLA** - DEFAULT_OOB_BRASS_SLA (no continuous replay)

   .. figure:: images/4.png

#. Click **Register**.

#. Select **Operations** from the dropdown menu to monitor the registration. This process should take approximately 3-5 minutes.

   .. figure:: images/4a.png

Snapshot Your Database
++++++++++++++++++++++

Before we take a manual snapshot of our Database, let's write a new table into SampleDB.

Write New Table Into Database
.............................

#. RDP/Console into your *UserXX*\ **-MSSQLSourceVM**.

#. Open SQL Server Managment Studio (SSMS), choose **Windows Authentication** from the *Authentication* dropdown, and click **Connect**.

#. Right-Click on **SampleDB**, and select **New Query**.

   .. figure:: images/5.png

#. Copy and paste the following into the query window (right-hand pane) and click **Execute**.

   .. code-block:: Bash

      select * into dbo.testlabtable from sales.orders;

   .. figure:: images/6.png

#. Verify the new table has been created by first expanding *SampleDB*, right-clicking on *Tables* and clicking **Refresh**. Then ensure the *dbo.testlabtable* exists under *Tables* before proceeding.

Take Manual Snapshot of Database
................................

#. Within **Era**, select **Databases** from the dropdown menu, and then **Sources** from the left-hand menu.

#. Click on the *Time Machine* for your Database (ex. *Initials*\ -LABSQLDB_TM).

   .. figure:: images/7.png

#. Click **Actions > Snapshot**.

   .. figure:: images/8.png

   - **Snapshot Name** - *Initials*\ -MSSQL-1st-Snapshot

   .. figure:: images/9.png

#. Click **Create**

#. Select **Operations** from the dropdown menu to monitor the registration. This process should take approximately 2-5 minutes.

   .. figure:: images/9a.png

Clone Your Database Server & Database
+++++++++++++++++++++++++++++++++++++

#. In **Era**, select **Time Machines** from the dropdown menu, and then select *Initials*\ -LABSQLDB_TM.

#. Click **Actions > Create Database Clone > Database**.

   - **Snapshot** - *Initials*\ -MSSQL-1st-Snapshot (Date Time)

   .. figure:: images/10.png

#. Click **Next**.

   - **Database Server** - Create New Server
   - **Database Server Name** - *Initials*\ -MSSQL_Clone1
   - **Compute Profile** - DEFAULT_OOB_COMPUTE
   - **Network Profile** - Primary-MSSQL-Network
   - **Administrator Password** - Nutanix/4u

   .. figure:: images/11.png

#. Click **Next**.

   - **Clone Name** - *Initials*\ -LABSQLDB_Clone1
   - **Database Name on VM** - SampleDB_Clone1
   - **Instance Name** - MSSQLSERVER

   .. figure:: images/12.png

#. Click **Clone**

#. Select **Operations** from the dropdown menu to monitor the registration. This process should take approximately 10-15 minutes.

Delete Table and Clone Refresh
++++++++++++++++++++++++++++++

There are times when a table or other data gets deleted (by accident), and you would like to get it back. Here we will delete a table and use the Era Clone Refresh action from the last snapshot we took.

Delete Table
............

#. RDP/Console into your *Initials*\ -MSSQL_Clone1 VM.

#. Open SQL Server Managment Studio (SSMS), choose **Windows Authentication** from the *Authentication* dropdown, and click **Connect**.

#. Expand **Databases > SampleDB_Clone1 > Tables**.

#. Right-click on *dbo.testlabtable*, select **Delete**, and then **OK**.

Refresh your clone
..................

#. Within **Era**, select **Databases** from the dropdown menu, and then **Clones** from the left-hand menu.

#. Select the clone for your database *Initials*\ -LABSQLDB_Clone1, and click **Refresh**.

#. Click the radio button for *Snapshot*, and choose the entry for *Initials*\ -MSSQL-1st-Snapshot (Date Time).

#. Click **Refresh**.

#. Select **Operations** from the dropdown menu to monitor the registration. This process should take approximately 2-5 minutes.

   .. figure:: images/13.png

Verify the previously deleted table has been restored
.....................................................

#. RDP/Console into your *Initials*\ -MSSQL_Clone1 VM.

#. Open SQL Server Managment Studio (SSMS), choose **Windows Authentication** from the *Authentication* dropdown, and click **Connect**.

#. Expand **Databases > SampleDB_Clone1 > Tables**.

#. Right-click on on *Tables*, and choose **Refresh**.

#. Verify the table *dbo.testlabtable* has been restored.
