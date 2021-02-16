.. _patch_sql:

----------------------
Patching Microsoft SQL
----------------------

Links

   - `KB3177312 - SQL Server 2016 build versions <https://support.microsoft.com/en-us/help/3177312/kb3177312-sql-server-2016-build-versions>`_ - Refer to this article for Service Pack (SP) and Cumulative Update (CU) information. Please note that Microsoft has depricated the use of the term *Service Pack* on SQL versions after 2016.

   - SQL2016 SP1 CU15 - https://www.microsoft.com/en-us/download/details.aspx?id=54613

.. note::

   In this workshop, we will guide you through creating a software profile within the SP1 "track" (our unofficial description). You may also create additional software profiles within the SP2 "track". These individual "tracks" require those SP as a base, and any higher level CU can be applied afterwards. In the example below, we are showing the SP1 "track". SP1 (build 13.0.4001.0) is where you will begin, and create a software profile to update to CU15 (build 13.0.4574.0). For reference, you could also use any CU in between the SP and the latest CU. Any profile you create must be a higher version than previous, otherwise that profile will fail to create. For example, if you create a profile for SP1-CU9, and then attempt to create another for SP1-CU3, it will fail.

      .. figure:: images/1.png

Creating a Software Profile Version
+++++++++++++++++++++++++++++++++++

Software Profile Versions are created in Era to support patching of SQL Server database server instances. A Software Profile Version can be created simply by uploading a SQL Server update executable. The SQL Server update can then be used to patch other database server VMs or when provisioning new database server VMs with the updated Software Profile.

#. Within Era, select **Profiles** from the drop-down menu.

#. On the left-hand side, select **Software**, and then click on the software profile under which you wish to create a new version (i.e. *Initials*\ -MSSQL_2016).

   Currently you should only see the base (1.0) version of the profile.

#. Click **Create**. The *Create Software Profile Version* window appears.

#. Do the following in the indicated fields:

   - **Name** *Initials*\ -SQL2016_SP1_CU15
   - **Patch File Location** - File Share
   - **File Share** - ``\\10.42.194.11\workshop_staging\era\SQLServer\MSSQL_Patches\SQLServer2016-KB4495257-x64.exe``
   - **User Name** - PHX-SPOCxxx-x-User01 (up to PHX-SPOCxxx-x-User05)
   - **Password** - `<CLUSTER-PASSWORD>`

      .. figure:: images/2.png

   - Wait for the upload to complete, and then click **Create**.
   This should take approximately 3 minutes to complete.

Updating a Software Profile Version
+++++++++++++++++++++++++++++++++++

After profile creation is successful, you must publish the profile to make the profile version visible for updates. Perform the following procedure to update a software profile version.

#. Within Era, select **Profiles** from the drop-down menu.

#. On the left-hand side, select **Software**, and then click on the *Initials*\ -MSSQL_2016 software profile.

#. Select the *Initials*\ -SQL2016_SP1_CU15 profile version, and click **Update**. The *Update Software Profile Version* window appears.

#. In the *General* step, click on **Published**, and then click the check box for *By publishing this version of the software profile, I understand that Era will recommend that all databases using an earlier versions of this software profile should update to this new version. The recommendation will appear on the Database Server VM home page*.

   .. figure:: images/3.png

   You are selecting this option to make the profile version visible for updates. If you have selected this option, Era provides a recommendation on the database server VM homepage that all database server VMs using an earlier version of this software profile should update to this new version.

#. Click **Next > Update**.

Patching a SQL Server Database Server VM
++++++++++++++++++++++++++++++++++++++++

Perform the following procedure to apply updates from the available software profile versions to a provisioned/registered database server VM.

#. Within Era, select **Database Server VMs** from the drop-down list.

#. Click on **List** from the left-hand side, then click the *UserXX*\ **-MSSQLSourceVM** database server VM for which you want to update the software profile version. The *Database Server VM Summary* page appears.

#. Go to the *Software Profile Version* widget, and click **Update**. The *Update Database Server VM* window appears.

   The *Software Profile Version* widget displays the current version, recommended version, and the status of the software profile version.

   .. note::

      The `Update` option only appears when a new software profile version is available.

#. Select the following in the indicated fields:

   - **Software Profile** *Initials*\ -MSSQL_2016

   - **Version** *Initials*\ -SQL2016_SP1_CU15

   - **Start Update** Now

   .. figure:: images/4.png

#. Confirm the update by typing *UserXX*\ **-MSSQLSourceVM** in the text box, and click **Update**.

   A message appears at the top indicating that the operation to update a database has started. Click the message to monitor the progress of the operation. Alternatively, select **Operations** in the drop-down list of the main menu to monitor the progress of the operation.

#. You can demonstrate the patch process was successful, by opening MS SQL Server Management Studio, and observing the server version and comparing that version with the SQL Server 2016 build versions web page.

   .. figure:: images/4a.png
