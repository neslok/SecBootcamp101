.. _chg_passwd:

-------------------
Change Passwords
-------------------

In this lab you will change the default passwords.

**Expected Module Duration:** 5 minutes

**Covered Test IDs:** N/A

It goes without saying, but we’re going to say it anyway, one of the first things that should be done on a newly deployed system is to change the passwords used to install.

Change the default passwords!
+++++++++++++++++++++++++++++++

1. **Prism Central** > :fa:`bars` > **Virtual Infrastructure** > **VMs**

2.	Check the box next to **WinTools** vm > **Actions** > **Launch Console**

    .. figure:: images/1.png

3.	Login in as ``administrator`` with a password of ``nutanix/4u``

2.	Launch PuTTy

    .. figure:: images/4.png

5.	Enter the IP address of your CVM (from the lab information sheet)

#.	Accept the security alert.

#.	Logon as ``admin`` with the password provided in the lab information sheet.

    .. note::
      There are 2 system accounts within AOS which have default passwords after cluster initialization: **nutanix** and **root**. To change these, issue the following commands, providing the admin password when prompted.

8.	For the nutanix account:
      .. code-block:: bash

          sudo passwd nutanix

9.	For the root account:
      .. code-block:: bash

          sudo passwd root

    .. figure:: images/9.png

.. note::
