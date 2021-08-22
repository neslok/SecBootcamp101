.. _cluster_lockdown:

----------------
Cluster Lockdown
----------------

To further enhance the security posture of a Nutanix environment you can enable cluster lockdown. When in lockdown, password-based SSH CVM access is disabled, and only key based access is allowed.

First, we need to generate public and private keys. There are a number of different methods to do this. For this lab we will use PuTTYGen to create the keys.

#. In Prism Central click on :fa:`bars` > **Virtual Infrastructure > VMs**

#. Check the box next to **WinToolsVM** vm, then **Actions > Launch Console**

        .. figure:: images/1-1.png

#. Login in as ``administrator`` with a password of ``nutanix/4u``

#. Launch PuTTYGen

#. Click **Generate** and move the mouse randomly in the blank area until the key is generated.

        .. figure:: images/1-2.png

#. Click **Save public key**, and save it to the desktop as ``intials-Key.pub`` (adding a .pub extension).

        .. figure:: images/1-3.png

        .. figure:: images/1-4.png

#. Click **Save private key**, click **Yes** to save without a passphrase, and save it to the desktop as ``intials-Key.ppk`` (adding a .ppk extension).

        .. figure:: images/1-5.png

        .. figure:: images/1-6.png

#.	Select and copy the public key from the Key window in PuTTYGen.

        .. figure:: images/1-7.png

#.	In Prism Element, click Home > Settings. In the Settings menu on the left side scroll down to Security and click on Cluster Lockdown.

        .. figure:: images/1-8.png

#. In the Settings menu on the left side scroll down to **Security** and click on **Cluster Lockdown**.

        .. figure:: images/1-9.png

#.	Click + New Public Key and complete as follows:

        •	Name: intitals-Key
        •	Key – Paste the key string you previously copied in step 8.

        .. figure:: images/1-10.png

        *Assure the key begins with ``ssh-rsa`` and ends with ``rsa-key-xxxxxxxx`` (where xxxxxxxx is the creation date of the key). This means the complete key has been inserted.

#.	Click **Save** to save the key to the cluster.

#.	Uncheck Enable Remote Login with Password.

        .. figure:: images/1-11.png

#.	On your **WinTools** vm, open **PuTTY**.

#.	Enter the Prism Element (CVM) IP address (from your lab information sheet).

#.	In the Connection section, click on **Data**, and enter ``nutanix`` in the Auto-login username box.

        .. figure:: images/1-12.png

#.	Expand the **SSH** section, **Auth**, click browse and add the ``intials-Key.ppk``, you saved to the desktop in step 7.

        .. figure:: images/1-13.png

#.	Click **Open**, and the login will be completed without the prompt for userid or password.

        .. figure:: images/1-14.png

#.	To validate that password based login is unavailable, right click on the top bar of the current PuTTY window, and select New Session.

        .. figure:: images/1-15.png

#.	Enter your Prism Element (CVM) IP, and click Open.

#.	At the login prompt, enter ``nutanix``, and press enter.

        .. figure:: images/1-16.png

Note that the session is immediately disconnected, without any additional prompts. This is due to the private key not being used.

This procedure is also applicable for Prism Central.
