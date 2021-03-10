.. _pltfrm_hrdn:

-------------------
Platform Hardening
-------------------
The Nutanix platform (AOS and AHV) include several security features natively. These include:

•	Advanced Intrusion Detection Environment (AIDE)
•	Core - Generates stack traces when there's an issue or SCMA is unable to remediate.
•	High Strength Passwords (Min Length = 15, Difference of previous = 8, remember previous = 24)
•	Banner – Presents an informational banner upon login to the CVM.
•	SNMPv3 Only – Forces use of SNMPv3 instead of v2
•	SCMA schedule – Nutanix has implemented security configuration management automation (SCMA) to check multiple security entities (STIG) for both Nutanix storage and AHV. Nutanix automatically reports log inconsistencies and reverts them to the baseline.

In this lab you will check the currnet settings, enable AIDE and set SCMA to run hourly.

**Expected Module Duration:** 5 minutes

**Covered Test IDs:** N/A

To view the current configuration:

1.	In Prism Central click on :fa:`bars` **> Virtual Infrastructure >VMs**

  .. figure:: images/1-1.png

2.	Check the box next to **WinTools** vm, then **Actions > Launch Console**

  .. figure:: images/1-2.png

3.	Click the **CTL-ALT-DEL** button in the upper right corner.

  .. figure:: images/1-3.png

4. Login in as administrator with a password of nutanix/4u.

  .. figure:: images/1-4.png

4.	Launch **PuTTY**.

5.	Enter the IP address of your CVM (from the lab information sheet)

  .. figure:: images/1-6.png

6.	Accept the security alert

  .. figure:: images/1-7.png

7.	Logon as **admin** with the password from the lab information sheet.

8.	Enter the following command on the CVM to view settings for these security settings.
    .. code-block:: bash

      ncli cluster get-cvm-security-config

  .. figure:: images/1-8.png

    .. note::

      These settings are for the CVM only, the hypervisor maybe configured differently. Please refer to security documentation from your hypervisor vendor for more information.

9.	To view these settings on AHV, issue following command:
    .. code-block:: bash

      ncli cluster get-hypervisor-security-config

  .. figure:: images/1-9.png


*Note that everything is disabled by default, and the SCMA schedule is set to run daily.*

Let’s enable AIDE and change the schedule SCMA to scan the system on an hourly basis.

10.	At the CVM ssh prompt, enter the following commands:
    .. code-block:: bash

      ncli cluster edit-cvm-security-params enable-aide=true

    .. code-block:: bash

      ncli cluster edit-cvm-security-params schedule=HOURLY

  .. figure:: images/1-10.png

      .. note::

        Core should only be should only be enabled on direction of Nutanix Support.
