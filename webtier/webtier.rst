.. _webtier:

----------------------
(Optional) Deploying the Webtier
----------------------

Configuring a Project
+++++++++++++++++++++

In this lab you will leverage multiple pre-built Calm Blueprints to provision your applications.

#. Within *Prism Central*, select :fa:`bars` **> Services > Calm**.

#. Select **Projects** from the left-hand menu, and then click **+ Create Project**.

   .. figure:: images/2.png

#. Fill out the following fields:

.. note:: If you are using a Single Node Cluster (SNC), only a Primary network will be available to you. In this case, ignore any references to the Secondary network, and instead use the Primary network.

   - **Project Name** - *Initials*\ -Project
   - Within *Users, Groups, and Roles* section, click **+user** (right side)
      - **Name** - Administrators (group)
      - **Role** - Project Admin
      - **Action** - Save
   - Within *Infrastructure* section, click **Select Provider > Nutanix**
   - Click **Select Clusters & Subnets**
   - Select *Your Assigned Cluster* from the dropdown
   - Within *Subnets for cluster...*, click **Primary**, and then click **Confirm**
   - Mark **Primary** as the default network by clicking the :fa:`star`

   .. figure:: images/3.png

#. Proceed to the next section.

Provision Fiesta Web Tier
+++++++++++++++++++++++++

In this section you'll deploy the web tier of the application and connect it to your production database.

#. `Download the Fiesta Blueprint by right-clicking here <https://raw.githubusercontent.com/nutanixworkshops/EraWithMSSQL/master/webtier/FiestaNoDB.json>`_. This single-VM Blueprint is used to provision only the web tier portion of the application.

#. Within *Prism Central*, select :fa:`bars` **> Services > Calm**. Select **Blueprints** from the left-hand menu, and click **Upload Blueprint**.

   .. figure:: images/30.png

#. Select **FiestaNoDB.json**.

#. Update the **Blueprint Name** to include your initials (ex. XYZ-FiestaNoDB). Even across different projects, Calm Blueprint names must be unique.

#. Select *Initials*\ -Project as the Calm project and click **Upload**.

   .. figure:: images/31.png

#. In order to launch the blueprint, you must first assign a network to the VM. Select the **NodeReact** Service, and in the *VM Configuration* section on the right, within the *NETWORK ADAPTERS (NICS)* section, select **Primary** as the **NIC 1** network.

   .. figure:: images/32a.png

#. Click **Credentials** (top bar) to define a private key used to authenticate to the CentOS VM that will be provisioned.

#. Expand the **CENTOS** credential and use your preferred SSH key, or paste in the following value as the **SSH Private Key**:

   ::

     -----BEGIN RSA PRIVATE KEY-----
     MIIEowIBAAKCAQEAii7qFDhVadLx5lULAG/ooCUTA/ATSmXbArs+GdHxbUWd/bNG
     ZCXnaQ2L1mSVVGDxfTbSaTJ3En3tVlMtD2RjZPdhqWESCaoj2kXLYSiNDS9qz3SK
     6h822je/f9O9CzCTrw2XGhnDVwmNraUvO5wmQObCDthTXc72PcBOd6oa4ENsnuY9
     HtiETg29TZXgCYPFXipLBHSZYkBmGgccAeY9dq5ywiywBJLuoSovXkkRJk3cd7Gy
     hCRIwYzqfdgSmiAMYgJLrz/UuLxatPqXts2D8v1xqR9EPNZNzgd4QHK4of1lqsNR
     uz2SxkwqLcXSw0mGcAL8mIwVpzhPzwmENC5OrwIBJQKCAQB++q2WCkCmbtByyrAp
     6ktiukjTL6MGGGhjX/PgYA5IvINX1SvtU0NZnb7FAntiSz7GFrODQyFPQ0jL3bq0
     MrwzRDA6x+cPzMb/7RvBEIGdadfFjbAVaMqfAsul5SpBokKFLxU6lDb2CMdhS67c
     1K2Hv0qKLpHL0vAdEZQ2nFAMWETvVMzl0o1dQmyGzA0GTY8VYdCRsUbwNgvFMvBj
     8T/svzjpASDifa7IXlGaLrXfCH584zt7y+qjJ05O1G0NFslQ9n2wi7F93N8rHxgl
     JDE4OhfyaDyLL1UdBlBpjYPSUbX7D5NExLggWEVFEwx4JRaK6+aDdFDKbSBIidHf
     h45NAoGBANjANRKLBtcxmW4foK5ILTuFkOaowqj+2AIgT1ezCVpErHDFg0bkuvDk
     QVdsAJRX5//luSO30dI0OWWGjgmIUXD7iej0sjAPJjRAv8ai+MYyaLfkdqv1Oj5c
     oDC3KjmSdXTuWSYNvarsW+Uf2v7zlZlWesTnpV6gkZH3tX86iuiZAoGBAKM0mKX0
     EjFkJH65Ym7gIED2CUyuFqq4WsCUD2RakpYZyIBKZGr8MRni3I4z6Hqm+rxVW6Dj
     uFGQe5GhgPvO23UG1Y6nm0VkYgZq81TraZc/oMzignSC95w7OsLaLn6qp32Fje1M
     Ez2Yn0T3dDcu1twY8OoDuvWx5LFMJ3NoRJaHAoGBAJ4rZP+xj17DVElxBo0EPK7k
     7TKygDYhwDjnJSRSN0HfFg0agmQqXucjGuzEbyAkeN1Um9vLU+xrTHqEyIN/Jqxk
     hztKxzfTtBhK7M84p7M5iq+0jfMau8ykdOVHZAB/odHeXLrnbrr/gVQsAKw1NdDC
     kPCNXP/c9JrzB+c4juEVAoGBAJGPxmp/vTL4c5OebIxnCAKWP6VBUnyWliFhdYME
     rECvNkjoZ2ZWjKhijVw8Il+OAjlFNgwJXzP9Z0qJIAMuHa2QeUfhmFKlo4ku9LOF
     2rdUbNJpKD5m+IRsLX1az4W6zLwPVRHp56WjzFJEfGiRjzMBfOxkMSBSjbLjDm3Z
     iUf7AoGBALjvtjapDwlEa5/CFvzOVGFq4L/OJTBEBGx/SA4HUc3TFTtlY2hvTDPZ
     dQr/JBzLBUjCOBVuUuH3uW7hGhW+DnlzrfbfJATaRR8Ht6VU651T+Gbrr8EqNpCP
     gmznERCNf9Kaxl/hlyV5dZBe/2LIK+/jLGNu9EJLoraaCBFshJKF
     -----END RSA PRIVATE KEY-----

   .. figure:: images/33.png

#. Click **Save** and click **Back** once the Blueprint has completed saving.

#. Click **Launch** and fill out the following fields, and click **Create**.

   - **Name of the Application** - *Initials*\ -Fiesta
   - **db_host_address** - The IP of your *Initials*\ **-MSSQL2** VM
   - **db_username** - Administrator
   - **db_domain_name** - ntnxlab.local
   - **db_dialect** - mssql
   - **db_name** - *Initials*\ -fiesta (as configured when you deployed through Era)
   - **db_password** - nutanix/4u

   .. figure:: images/34.png

#. Select the **Audit** tab to monitor the deployment. This process should take approximately 5 minutes.

   .. figure:: images/35.png

#. Once the application status changes to **Running**, select the **Services** tab, then the **NodeReact** service to obtain the IP address of your web server.

   .. figure:: images/36.png

#. Open \http://*NODEREACT-IP-ADDRESS:5001*/ in a new browser tab to access the **Fiesta** application.

   .. figure:: images/37.png

   Congratulations! You've completed the deployment of your production application.
