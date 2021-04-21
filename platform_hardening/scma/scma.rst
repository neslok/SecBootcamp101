.. _scma:

-------------------
Security Configuration Management Automation (SCMA)
-------------------

STIGs are quite useful to assist in securing an environment, but how do you maintain compliance once the changes have been implemented?

On a Nutanix platform, SCMA automates the detection and remediation of deviations as defined in the STIG baseline.

In this exercise you will embrace your inner dark side and compromise the security of the cluster by making non-compliant changes - and you would have gotten away with it too, if it weren’t for you pesky STIGs (any Scooby Doo fans in the audience? No? OK just me then, fine.).


Since this lab exercise will generate more output than can be stored in the screen buffer, we will want to log all the output to a log file for review.

1. **Prism Central** > :fa:`bars` > **Virtual Infrastructure > VMs**

2.	Check the box next to **WinToolsVM** vm, then **Actions > Launch Console**

    .. figure:: images/1-1.png

3.	Login in as ``administrator`` with a password of ``nutanix/4u``

4.	Launch PuTTy

5.	Click on **Session > Logging** in the Putty configuration window. Under **Session logging:** check ``All session output``, and for the **Log file name**, click **Browse** and select the Desktop (for easy access) and click **Save**.

    .. figure:: images/1-2.png

6.	Click on **Session** in the left column, enter your cluster IP, and login as ``admin`` with the password provided.

7.	At the prompt, enter the command:

    .. code-block:: bash

      sudo salt-call state.highstate

    .. figure:: images/1-3.png

4.	After the execution completes (takes about a minute), open the putty file on the desktop with Notepad, and search for``resolv.conf``.

    .. figure:: images/1-4.png

*Note that no change was made.*

5.	Close Notepad.

Now we’ll “compromise” the system.

6.	In the putty session, enter the below command and provide the admin password when prompted.

    .. code-block:: bash

      sudo chmod 755 /etc/resolv.conf

    .. figure:: images/1-5.png


7.	Rerun the salt-call command and provide admin password when prompted. Wait until the execution completes (again, about a minute)

    .. code-block:: bash

      sudo salt-call state.highstate

8.	Open the putty log file in Notepad again, and this time search for ``Executing state file.managed for /etc/resolv.conf``. You want to find the **2nd instance** of this string.

    .. figure:: images/1-6.png

*Note the file permissions (mode) have been changed from the 755 we set them to, back to the 644 as specified in the STIG.*

9.	Close Notepad.

Let’s do another example, this time we will edit the sshd configuration file to permit root login, which should be noted, is very much against all security best practices, and should NEVER be done in a production environment.

10.	In putty, at the prompt, enter the command:

sudo nano /etc/ssh/sshd_config

(feel free to use the editor of your choice)

11.	Once the file is opened for editing, search for ``PermitRootLogin`` – (CTRL-W to search in nano) and change the ``no`` to ``yes``.

    .. figure:: images/1-7.png

    .. figure:: images/1-8.png

13. Enter **CTRL-O** (capital O, not zero) to save the file, and **CTRL-X** to exit nano.

14.	Run the command:

    .. code-block:: bash

      sudo salt-call state.highstate

Provide admin password when prompted and wait until the execution completes (again, about a minute)

15.	Upon completion, open the putty log file and search for the **3rd instance** of ``PermitRootLogin``.

    .. figure:: images/1-9.png


Note the ``PermitRootLogin yes`` is preceded by a - sign indicating it was removed from the file, while ``PermitRootLogin no`` is preceded by a + sign indicating it was added, restoring the file to the parameter as defined by the STIG.
