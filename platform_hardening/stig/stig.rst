.. _stig:

----------------------------------------------
Security Technical Implementation Guide (STIG)
----------------------------------------------

Nutanix also provides administrators with Security Technical Implementation Guides (STIGs), which are security tools encoded in a machine-readable format to facilitate automated validation, ongoing monitoring, and self-remediation. These reduce the time required to verify security compliance from weeks or months to days without slowing down product evolution. STIGs are based on well-established standards by National Institute of Standards and Technology (NIST) that can apply to multiple baseline requirements for DoD and PCI-DSS. However, Nutanix STIGs are specific to the Acropolis platform and therefore more effective.

In this lab you will see STIGs in action in order to see first-hand how they harden the Nutanix Enterprise Cloud OS and help reduce zero-day vulnerabilities.

  ..Note::
      Nutanix AOS and AHV STIGs can be downloaded here: https://portal.nutanix.com/#/page/static/stigs

Running a STIG Report
+++++++++++++++++++++

Follow the exercise below to manually run a STIG report on your Nutanix cluster. By default, these checks run on a Nutanix cluster once every 24 hours, whereas in a typical legacy infrastructure environment, STIGs would be applied manually at the time of deployment and have no automated means of ensuring the original configuration is unchanged over time.
By default, a Nutanix cluster checks for deviations from the baseline STIG settings, and remediates when applicable, on a daily basis.

#. **Prism Central** > :fa:`bars` > **Virtual Infrastructure > VMs**

#.	Check the box next to **WinToolsVM** vm, then **Actions > Launch Console**

    .. figure:: images/1-1.png

#.	Login in as ``administrator`` with a password of ``nutanix/4u``

#.	Launch PuTTy

#.	Enter the IP address of your CVM (from the lab information sheet)

#.	Accept the security alert.

#.	Logon as ``admin`` with the password provided in the lab information sheet.

#.	Change to the root directory of the CVM, and list the files available to the root user to execute within the /root directory.

      .. code-block:: bash
        cd /
        sudo ls -l root

    .. figure:: images/1-2.png

#.	In this example, we’ll run the generic text output ``report_stig.sh``.

      .. code-block:: bash

        sudo /root/report_stig.sh

*This takes ~1 minute to complete.*

    .. figure:: images/1-3.png

#.	As indicated, the report is saved to ``/home/log/STIG-report-xx-xx-xxxx-xx-xx-xx`` (where the xx’s are the date/time the report was created).


#.	List the files in the folder and note the name of the report.

      .. code-block:: bash

        sudo ls -l /home/log | grep STIG

    .. figure:: images/1-4.png

#.	Copy the report to the admin home directory, substituting the actual file name for the asterisks.

      .. code-block:: bash

        sudo cp /home/log/STIG-report-xx-xx-xxxx-xx-xx-xx /home/admin

    .. figure:: images/1-5.png

#.	Change to the admin home directory and list the files.

      .. code-block:: bash

        cd
        ls -l

    .. figure:: images/1-6.png

*Note that the report file is only readable by root, so we’ll change the ownership.*

#.	To change the file ownership to admin, enter the following command, replacing the asterisks with your actual file name:

      .. code-block:: bash

        sudo chown admin:admin /home/admin/STIG-report-xx-xx-xxxx-xx-xx-xx

    .. figure:: images/1-7.png

*From your WinToolsVM vm, use a secure copy tool (SCP, WINSCP, PSCP, etc) to copy the report results file to your workstation from the CVM. Alternatively you can open and view the text file in your SSH session using vi, more, cat, etc.*

#.	Open WinSCP, set the File protocol to **SCP**, enter the CVM IP address, ``admin`` for the User Name and the assigned password, and click **Login**.

    .. figure:: images/1-8.png

#.	Change the local directory to Desktop

    .. figure:: images/1-9.png

#.	Drag the **STIG** report from the CVM (right pane) to the local desktop (left pane).

    .. figure:: images/1-10.png

#.	From the desktop, open the STIG report – use Chrome or Sublime, it does not format well with NotePad.

Analyzing the STIG Report
+++++++++++++++++++++++++

The STIG report can be used for validation and accreditation requirements for security compliance.
The format of each result within the report is as follows:

Line 1 - Check name

Line 2 - Description of the check

Line 3 - Legend, or expected result of the check

Line 4 - Check result

Line 5 - Completion status of the check

Below is an example of a non-finding in the STIG report, meaning that the check did not discover an unwanted configuration:

    .. figure:: images/1-11.png

And an example of a finding, where the check was found to have an unwanted configuration:

    .. figure:: images/1-12.png
