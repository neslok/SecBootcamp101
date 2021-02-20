.. _clstr_lckdwn:

-------------------
Cluster Lockdown
-------------------

To further enhance the security posture of a Nutanix environment you can enable cluster lockdown. When in lockdown, password-based SSH CVM access is disabled, and only key based access is allowed.

First, we need to generate public and private keys.

1.	On your WinTools vm, open PuTTYgen



2.	Click Generate and move the mouse randomly in the blank area until the key is generated.



3.	Click Save private key, and save it to the desktop as intials-Key.ppk (adding a .ppk extension). Click Yes to save without a passphrase.



4.	Click Save public key, and save it to the desktop as intials-Key.pub (adding a .pub extension).



5.	Select and copy the public key from the Key window in PuTTYgen.




6.	In Prism Element, click Home > Settings. In the Settings menu on the left side scroll down to Security and click on Cluster Lockdown.



7.	Click + New Public Key and complete as follows:
•	Name: intitals-Key
•	Key – Paste the key string you previously copied in step 5.


Assure the key begins with “ssh-rsa” and ends with “rsa-key-xxxxxxxx” (where xxxxxxxx is the creation date of the key. This means the complete key has been inserted.


8.	Click Save and now your key is saved to the cluster.

9.	Uncheck Enable Remote Login with Password.




10.	On your WinTools vm, open PuTTY.
11.	Enter the Prism Element (CVM) IP address.


12.	In the Connection section, click on Data, and enter nutanix in the Auto-login username box.



13.	Expand the SSH section, and click on Auth, click browse and add the intials-Key.ppk, you saved to the desktop in step 3.



14.	Click Open, and the login will be completed without the prompt for userid or password.



15.	To validate that password based login is unavailable, right click on the top bar of the current PuTTY window, and select New Session.



16.	Enter your Prism Element (CVM) IP, and click Open.



17.	At the login prompt, enter nutanix, and press enter.



Note that the session is immediately disconnected, without any additional prompts.

This procedure is also applicable for Prism Central.
