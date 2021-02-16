.. _admin_oracle:

--------------------------
DB Administration with Era
--------------------------

**In this lab you will Administer your ORACLE DB**

Explore Your Database
++++++++++++++++++++++

#. In **Era**, select **Databases** from the dropdown menu and **Sources** from the lefthand menu.

#. Click into your *Initials*\ _proddb, this will take you back into the Database Summary page. This page provides details of the Database, Database Server access, Time Machine schedule, Compute/Network/Software profiles used to provision.

    - **Database Summary:**

    .. figure:: images/2.png

    - **Database Server VM:**

    .. figure:: images/3.png

    - **Time Machine:**

    .. figure:: images/4.png

    - **Profiles:**

    .. figure:: images/5.png

Recovering From Snapshot
++++++++++++++++++++++++

Before we take a manual snapshot of our database, let's write a new table into our *Initials*\ _proddb database.

Write New Table Into Database
.............................

#. SSH (Terminal/Putty) into your *UserXX*\ **-Oracle19cSource** VM

   - **User Name** - oracle
   - **Password** - Nutanix/4u

#. Launch **sqlplus**

     .. code-block:: Bash

       sqlplus / as sysdba

#. Execute the following to create a table:

     .. code-block:: Bash

       CREATE TABLE testlabtable
       (
       column1 VARCHAR(30),
       column2 DATE
       );

#. Verify the new table is there by executing the following to list the table:

     .. code-block:: Bash

       select owner as schema_name,
       table_name
       from sys.all_tables
       where table_name like 'TEST%';

Take Manual Snapshot of Database
................................

#. Within **Era**, select **Databases** from the dropdown menu, and then **Sources** from the left-hand menu.

#. Click on the Time Machine for your Database *Initials*\ _proddb_TM.

   .. figure:: images/6.png

#. Click **Yes**. This should take approximately 2-3 minutes to complete.

#. Click **Actions > Snapshot**. Enter *Initials*\ _proddb-1st-Snapshot as the *Snapshot Name*, and click **Create**.

   .. figure:: images/7.png

#. Select **Operations** from the dropdown menu to monitor the registration. This process should take approximately 2-5 minutes.

Clone Your Database Server & Database
+++++++++++++++++++++++++++++++++++++

#. Within **Era**, select **Time Machines** from the dropdown menu, and then select *Initials*\ _proddb_TM.

#. Click **Actions > Create Single Instance Database Clone**.

#. Click the radio button for *Snapshot*, and choose the entry for *Initials*\ proddb-1st-Snapshot (Date Time). Click **Next**.

   .. figure:: images/9.png

#. Fill out the following fields, and click **Next**.

   - **Database Server VM** - Create New Server
   - **Database Server VM Name** - *Initials*\ _oracle_prod_Clone1
   - **Compute Profile** - ORACLE_SMALL
   - **Network Profile** - Primary-ORACLE-Network
   - **SSH Public Key Through** - Select **Text**. Copy and paste the following into the text box.

   ::

      ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAii7qFDhVadLx5lULAG/ooCUTA/ATSmXbArs+GdHxbUWd/bNGZCXnaQ2L1mSVVGDxfTbSaTJ3En3tVlMtD2RjZPdhqWESCaoj2kXLYSiNDS9qz3SK6h822je/f9O9CzCTrw2XGhnDVwmNraUvO5wmQObCDthTXc72PcBOd6oa4ENsnuY9HtiETg29TZXgCYPFXipLBHSZYkBmGgccAeY9dq5ywiywBJLuoSovXkkRJk3cd7GyhCRIwYzqfdgSmiAMYgJLrz/UuLxatPqXts2D8v1xqR9EPNZNzgd4QHK4of1lqsNRuz2SxkwqLcXSw0mGcAL8mIwVpzhPzwmENC5Orw==

   .. figure:: images/10.png

#. Fill out the following fields, and click **Next**.

   - **Name** - *Initials*\ _proddb_Clone1
   -  **SID** - *Initials*\ prod
   -  **SYS and SYSTEM Password** - Nutanix/4u
   -  **Database Parameter Profile** - ORACLE_SMALL_PARAMS

   .. figure:: images/11.png

#. Click **Clone**.

#. Select **Operations** from the dropdown menu to monitor the registration. This process should take approximately 30-50 minutes.

Delete Table and Clone Refresh
++++++++++++++++++++++++++++++

There are times when a table or other data gets deleted (accidentally or maliciously), and you would like to recover it. Here we will delete a table, and then use the Era *Clone Refresh* action from the last snapshot to restore it.

Delete Table
............

#. SSH (Terminal/Putty) into your *Initials*\ _proddb_Clone1 VM

   - **User Name** - oracle
   - **Password** - Nutanix/4u

#. Launch **sqlplus**

   .. code-block:: Bash

     sqlplus / as sysdba

#. Execute the following to Drop the table:

   .. code-block:: Bash

     DROP TABLE testlabtable;

#. Verify the table is gone by executing the following to list the table:

   .. code-block:: Bash

     select owner as schema_name,
     table_name
     from sys.all_tables
     where table_name like 'TEST%';

Clone Refresh
.............

#. In **Era**, select **Databases** from the dropdown menu and **Clones** from the lefthand menu.

#. Select the Clone for your Database *Initials*\ _proddb and Click **Refresh**.

   - **Snapshot** - *Initials*\ _proddb-1st-Snapshot (Date Time)

#. Click **Refresh**

#. Select **Operations** from the dropdown menu to monitor the registration. This process should take approximately 2-5 minutes.

Verify Table is Back
....................

#. SSH (Terminal/Putty) into your *Initials*\ _proddb_Clone1 VM

   - **User Name** - oracle
   - **Password** - Nutanix/4u

   .. code-block:: Bash

     ssh oracle@PRODDB_Clone1 IP

#. Launch **sqlplus**

   .. code-block:: Bash

     sqlplus / as sysdba

#. Verify the table is back by executing the following to list the table:

   .. code-block:: Bash

     select owner as schema_name,
     table_name
     from sys.all_tables
     where table_name like 'TEST%';
