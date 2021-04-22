.. _cluster_lockdown:

----------------
Cluster Lockdown
----------------

To further enhance the security posture of a Nutanix environment you can enable cluster lockdown. When in lockdown, password-based SSH CVM access is disabled, and only key based access is allowed.

First, we need to generate public and private keys.

1. **Prism Central** > :fa:`bars` > **Virtual Infrastructure > VMs**

2.	Check the box next to **WinToolsVM** vm, then **Actions > Launch Console**

    .. figure:: images/1-1.png

3.	Login in as ``administrator`` with a password of ``nutanix/4u``

4.	Launch PuTTYGen

5.	Click **Generate** and move the mouse randomly in the blank area until the key is generated.

    .. figure:: images/1-2.png

6.	Click **Save public key**, and save it to the desktop as ``intials-Key.pub`` (adding a .pub extension).

    .. figure:: images/1-3.png

    .. figure:: images/1-4.png

7.	Click **Save private key**, click **Yes** to save without a passphrase, and save it to the desktop as ``intials-Key.ppk`` (adding a .ppk extension).

    .. figure:: images/1-5.png

    .. figure:: images/1-6.png

8.	Select and copy the public key from the Key window in PuTTYGen.

    .. figure:: images/1-7.png

9.	In Prism Element, click Home > Settings. In the Settings menu on the left side scroll down to Security and click on Cluster Lockdown.

    .. figure:: images/1-8.png

10. In the Settings menu on the left side scroll down to **Security** and click on **Cluster Lockdown**.

    .. figure:: images/1-9.png

11.	Click + New Public Key and complete as follows:

•	Name: intitals-Key

•	Key – Paste the key string you previously copied in step 8.

    .. figure:: images/1-10.png

Assure the key begins with ``ssh-rsa`` and ends with ``rsa-key-xxxxxxxx`` (where xxxxxxxx is the creation date of the key). This means the complete key has been inserted.

12.	Click **Save** to save the key to the cluster.

13.	Uncheck Enable Remote Login with Password.

    .. figure:: images/1-11.png

10.	On your **WinTools** vm, open **PuTTY**.

11.	Enter the Prism Element (CVM) IP address (from your lab information sheet).

12.	In the Connection section, click on **Data**, and enter ``nutanix`` in the Auto-login username box.

    .. figure:: images/1-12.png

13.	Expand the **SSH** section, **Auth**, click browse and add the ``intials-Key.ppk``, you saved to the desktop in step 7.

    .. figure:: images/1-13.png

14.	Click **Open**, and the login will be completed without the prompt for userid or password.

    .. figure:: images/1-14.png

15.	To validate that password based login is unavailable, right click on the top bar of the current PuTTY window, and select New Session.

    .. figure:: images/1-15.png

16.	Enter your Prism Element (CVM) IP, and click Open.

17.	At the login prompt, enter ``nutanix``, and press enter.

    .. figure:: images/1-16.png

Note that the session is immediately disconnected, without any additional prompts. This is due to the private key not being used.

This procedure is also applicable for Prism Central.
