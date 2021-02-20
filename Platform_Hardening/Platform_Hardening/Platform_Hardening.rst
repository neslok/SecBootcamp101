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
2.	Check the box next to WinTools vm, then **Actions > Launch Console**
3.	Login in as administrator with a password of nutanix/4u.

4.	Launch PuTTy
5.	Enter the IP address of your CVM (from the lab information sheet)

6.	Accept the security alert

7.	Logon as admin with the password from the lab information sheet.

8.	Enter the following command on the CVM to view settings for these security settings.
    .. code-block:: bash

      ncli cluster get-cvm-security-config

    .. note::

      These settings are for the CVM only, the hypervisor maybe configured differently. Please refer to security documentation from your hypervisor vendor for more information.

9.	To view these settings on AHV, issue following command:
    .. code-block:: bash

      ncli cluster get-hypervisor-security-config


Note that everything is disabled by default, and the SCMA schedule is set to run daily.

Let’s enable AIDE and change the schedule SCMA to scan the system on an hourly basis.

10.	At the CVM ssh prompt, enter the following commands:
    .. code-block:: bash

      ncli cluster edit-cvm-security-params enable-aide=true

    .. code-block:: bash

      ncli cluster edit-cvm-security-params schedule=HOURLY

NOTE: Core should only be should only be enabled on direction of Nutanix Support.

Cluster Lockdown
++++++++++++++++

To further enhances your security posture of your Nutanix environment you can enable cluster lockdown. When in lockdown, password-based CVM access is disabled, and only key based access is allowed.

First, we need to generate public and private keys.

1.	On your WinTools vm, open **PuTTYgen**

2.	Click **Generate** and move the mouse randomly in the blank area until the key is generated.

3.	Click **Save private key**, and save it to the desktop as intials-Key.ppk (adding a .ppk extension). Click **Yes** to save without a passphrase.

4.	Click **Save public key**, and save it to the desktop as intials-Key.pub (adding a .pub extension).

5.	Select and copy the public key from the Key window in PuTTYgen.

6.	In Prism Element, click **Home > Settings**. In the Settings menu on the left side scroll down to Security and click on **Cluster Lockdown**.

7.	Click **+ New Public Key** and complete as follows:
  - Name: intitals-Key
  - Key – Paste the key string you previously copied in step 5.

  Ensure the key begins with “ssh-rsa” and ends with “rsa-key-xxxxxxxx” (where xxxxxxxx is the creation date of the key. This means the complete key has been inserted.

8.	Click Save and now your key is saved to the cluster.

9.	Uncheck Enable Remote Login with Password.

10.	On your WinTools vm, open PuTTY.
11.	Enter the Prism Element (CVM) IP address.
12.	In the Connection section, click on Data, and enter nutanix in the Auto-login username box.
