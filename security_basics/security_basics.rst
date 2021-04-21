.. _security_basics:

---------------
Security Basics
---------------

.. _change_passwords:

Change The Default Passwords
++++++++++++++++++++++++++++

In this lab you will change the default passwords.

**Expected Module Duration:** 5 minutes

**Covered Test IDs:** N/A

It goes without saying, but we’re going to say it anyway, one of the first things that should be done on a newly deployed system is to change the passwords used to install.

#. **Prism Central** > :fa:`bars` > **Virtual Infrastructure** > **VMs**

    .. figure:: images/1.png

#.	Check the box next to **WinTools** vm > **Actions** > **Launch Console**

    .. figure:: images/2.png

#.	Click on the **CTRL-ALT-DEL** button in upper right, and login in as ``administrator`` with a password of ``nutanix/4u``

    .. figure:: images/3.png

#.	Launch PuTTY

#.	Enter the IP address of your CVM (from the lab information sheet)

#.	Accept the security alert.

#.	Logon as admin with the password provided in the lab information sheet.

    .. note::
      There are 2 system accounts within AOS which have default passwords after cluster initialization: **nutanix** and **root**. To change these, issue the following commands, providing the admin password when prompted.

#.	For the nutanix account:
      .. code-block:: bash

          sudo passwd nutanix

#.	For the root account:
      .. code-block:: bash

          sudo passwd root

    .. figure:: images/4.png

.. _check_passwords:

NCC Check for Default Passwords
+++++++++++++++++++++++++++++++

Many IT systems are compromised simply because default passwords were not changed after the system was commissioned.

Nutanix Cluster Check (NCC) provides a check to validate if there are any passwords that are set as default, so you can easily check and remediate if required.

#. In **Prism Element** select **Home** > **Health**.

    .. figure:: images/5.png

#. On the upper right of the Health page, select **Actions** > **Run NCC Checks**.

    .. figure:: images/6.png

#. In the **Run Checks** box, click **Specified Checks**, and search for **password**. Select one of the default password options.

    .. figure:: images/7.png

#. Repeat step 3 until all default password checks are included.

    .. figure:: images/8.png

#. Click **Run**

#. To monitor the process, click on the **Tasks** icon in the main bar, and click on **View All Tasks**.

    .. figure:: images/9.png

#. Once the **Health Check** has completed, click on **Succeeded** in the Status column.

    .. figure:: images/10.png

#. From the View Summary page, we can see the 3 checks passed and we have an informational alert on one. Click on **Download Output** to retrieve the report and see the details.

    .. figure:: images/11.png

#. Open the downloaded file with notepad and review the results.

    .. figure:: images/12.png

In this example we can see that there is a default password on the IPMI device(s). Due to the HPOC environment, we are not permitted to change the IPMI passwords from the defaults.
This finding is expected in this case, but what if there had been default passwords found on the CVMs in your environment?

.. _custom_banner:

Configure Custom Banner
+++++++++++++++++++++++

Login banners provide a definitive warning to any possible intruders that may want to access your system that certain types of activity are illegal, but at the same time, it also advises the authorized and legitimate users of their obligations relating to acceptable use of the computerized or networked environment(s).

In this lab you will enable and create a customer banner for Prism Element.

**Expected Module Duration:** 5 minutes

**Covered Test IDs:** N/A

#.	Prism Element, click **Home > Settings**

  .. figure:: images/13.png

#. In the Settings pan one the left, scroll to the bottom and click on **Welcome Banner**

  .. figure:: images/14.png

#.	Enter your text into the black area (you can use HTML to mark up the text).

 .. figure:: images/15.png

#.	Check **Enable Banner**

#.	Click **Save**

#.	In the upper right corner, click on **admin**, then click on **Sign Out**.

 .. figure:: images/16.png

#.	Now before seeing a login prompt, the banner is displayed, and must be accepted to login.

 .. figure:: images/17.png

 .. note::
    This procedure also is applicable for Prism Central.
