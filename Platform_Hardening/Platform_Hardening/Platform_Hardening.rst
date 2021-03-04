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

4.	Launch **PuTTy**.

  .. figure:: images/1-5.png

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

Cluster Lockdown
++++++++++++++++

To further enhances your security posture of your Nutanix environment you can enable cluster lockdown. When in lockdown, password-based CVM access is disabled, and only key based access is allowed.

First, we need to generate public and private keys.

1.	On your WinTools vm, open **PuTTYgen**

  .. figure:: images/2-1.png

2.	Click **Generate** and move the mouse randomly in the blank area until the key is generated.

  .. figure:: images/2-2.png

3.	Click **Save private key**, and save it to the desktop as ``intials-Key.ppk`` (adding a .ppk extension). Click **Yes** to save without a passphrase.

  .. figure:: images/2-3.png

4.	Click **Save public key**, and save it to the desktop as ``intials-Key.pub`` (adding a .pub extension).

  .. figure:: images/2-4.png

5.	Select and copy the public key from the Key window in PuTTYgen.

  .. figure:: images/2-5.png

6.	In Prism Element, click **Home > Settings**. In the Settings menu on the left side scroll down to Security and click on **Cluster Lockdown**.

  .. figure:: images/2-6.png

7.	Click **+ New Public Key** and complete as follows:
  - Name: intitals-Key
  - Key – Paste the key string you previously copied in step 5.

  .. figure:: images/2-7.png

  Ensure the key begins with “ssh-rsa” and ends with “rsa-key-xxxxxxxx” (where xxxxxxxx is the creation date of the key. This means the complete key has been inserted.

  .. figure:: images/2-8.png

8.	Click **Save**.

9.	Uncheck **Enable Remote Login with Password**.

  .. figure:: images/2-9.png

10.	In your **WinTools** vm, open **PuTTY**.

11.	Enter the Prism Element (CVM) IP address (from your lab info sheet).

  .. figure:: images/2-10.png

12.	In the Connection section, click on **Data**, and enter ``nutanix`` in the Auto-login username box.

  .. figure:: images/2-11.png

13.	Expand the **SSH** section, and click on **Auth**, click **Browse** and add the ``intials-Key.ppk``, you saved to the desktop in step 3.

   .. figure:: images/2-12.png

14.	Click **Open**, and the login will be completed without the prompt for userid or password.

   .. figure:: images/2-13.png

15.	To validate that password based login is unavailable, right click on the top bar of the current PuTTY window, and select **New Session**.

   .. figure:: images/2-14.png

16.	Enter your Prism Element (CVM) IP, and click **Open**.

   .. figure:: images/2-15.png

17.	At the login prompt, enter ``nutanix``, and press enter.

   .. figure:: images/2-16.png 

*Note that the session is immediately disconnected, without any additional prompts. This is because we are trying to login without providing any key.*

This procedure is also applicable for Prism Central.
