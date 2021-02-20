.. _stig:

-------------------
Security Technical Implementation Guide (STIG)
-------------------

Nutanix also provides administrators with Security Technical Implementation Guides (STIGs), which are security tools encoded in a machine-readable format to facilitate automated validation, ongoing monitoring, and self-remediation. These reduce the time required to verify security compliance from weeks or months to days without slowing down product evolution. STIGs are based on well-established standards by National Institute of Standards and Technology (NIST) that can apply to multiple baseline requirements for DoD and PCI-DSS. However, Nutanix STIGs are specific to the Acropolis platform and therefore more effective.

In this lab you will see STIGs in action in order to see first-hand how they harden the Nutanix Enterprise Cloud OS and help reduce zero-day vulnerabilities.
Note
Nutanix AOS and AHV STIGs can be downloaded here from the Nutanix Portal.

Running a STIG Report

Follow the exercise below to manually run a STIG report on your Nutanix cluster. By default, these checks run on a Nutanix cluster once every 24 hours, whereas in a typical legacy infrastructure environment, STIGs would be applied manually at the time of deployment and have no automated means of ensuring the original configuration is unchanged over time.

1.	Connect to Controller VM (CVM) as admin user via SSH (Using Terminal, putty, or similar program)

2.	Change to the root directory of the CVM, and list the files available to the root user to execute within the /root directory.
cd /
sudo ls -l root

There should be three .sh files that end in _stig.sh and you’ll want to run the one that outputs the report in the format you prefer.

3.	In this example, we’ll run the generic text output “report_stig.sh”

sudo /root/report_stig.sh

Note: This takes ~1 minute to complete.

4.	As indicated, the report is saved to /home/log/STIG-report-xx-xx-xxxx-xx-xx-xx (where the xx’s are the date/time the report was created).



5.	List the files in the folder and note the name of the report.

sudo ls -l /home/log | grep STIG



6.	Copy the report to the admin home directory, substituting the actual file name for the asterisks.

sudo cp /home/log/STIG-report-**-**-****-**-**-** /home/admin



7.	Change to the admin home directory and list the files.

cd

ls -l



Note that the report file is only readable by root, so we’ll change the ownership.

8.	To change the file ownership to admin, enter the following command, replacing the asterisks with your actual file name:

sudo chown admin:admin /home/admin/STIG-report-**-**-****-**-**-**


From your WinTools vm, use a secure copy tool (SCP, WINSCP, PSCP, etc) to copy the report results file to your workstation from the CVM. Alternatively you can open and view the text file in your SSH session using vi, more, cat, etc.

9.	Open WinSCP, set the File Protocol to SCP, enter the CVM IP address, admin for the User Name and the assigned password.



10.	Change the local directory to Desktop


11.	Drag the STIG report from the CVM (right pane) to the local desktop (left pane)

12.	From the desktop, open the STIG report – use Chrome or Sublime, it does not format well with NotePad.



Analyzing the STIG Report

The STIG report can be used for validation and accreditation requirements for security compliance.
The format of each result within the report is as follows:

Line 1 - Check name
Line 2 - Description of the check
Line 3 - Legend, or expected result of the check
Line 4 - Check result
Line 5 - Completion status of the check

Below is an example of a non-finding in the STIG report, meaning that the check did not discover an unwanted configuration:
CAT II RHEL-07-021030 SRG-OS-000480-GPOS-00227 CCI-000366 CM-5 (1)
All world-writable directories must be group-owned by root, sys, bin, or an application group.
The result of the check should be yes.  If no, then it's a finding
yes
Completed.

And an example of a finding, where the check was found to have an unwanted configuration:

CAT I RHEL-07-021710 SRG-OS-000095-GPOS-00049 CCI-000381 CM-7 a, CM-7 b
The telnet-server package must not be installed.
The result of the check should be yes.  If no, then it's a finding
no
Completed.
