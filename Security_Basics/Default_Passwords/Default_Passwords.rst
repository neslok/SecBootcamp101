.. _def_passwd:

-------------------
Default Passwords
-------------------

In this lab you will check the system for default passwords.

**Expected Module Duration:** 5 minutes

**Covered Test IDs:** N/A

NCC Check for Default Passwords
+++++++++++++++++++++++++++++++

Many IT systems are compromised simply because default passwords were not changed after the system was commissioned.

Nutanix Cluster Check (NCC) provides a check to validate if there are any passwords that are set as default, so you can easily check and remediate if required.

#. In **Prism Element** select **Home** > **Health**.

    .. figure:: images/1-1.png

#. On the upper right of the Health page, select **Actions** > **Run NCC Checks**.

    .. figure:: images/1-2.png

#. In the **Run Checks** box, click **Specified Checks**, and search for **password**. Select one of the default password options.

    .. figure:: images/1-3.png

#. Repeat step 3 until all default password checks are included.

    .. figure:: images/1-4.png

#. Click **Run**

#. To monitor the process, click on the **Tasks** icon in the main bar, and click on **View All Tasks**.

    .. figure:: images/1-5.png

#. Once the **Health Check** has completed, click on **Succeeded** in the Status column.

    .. figure:: images/1-6.png

#. From the View Summary page, we can see the 3 checks passed and we have an informational alert on one. Click on **Download Output** to retrieve the report and see the details.

    .. figure:: images/1-7.png

#. Open the downloaded file with notepad and review the results.

    .. figure:: images/1-8.png

In this example we can see that there is a default password on the IPMI device(s). Due to the HPOC environment, we are not permitted to change the IPMI passwords from the defaults.
This finding is expected in this case, but what if there had been default passwords found on the CVMs in your environment?

In the next lab, you will perform password changes for the built-in accounts.

Change passwords
