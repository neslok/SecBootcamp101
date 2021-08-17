.. _vdi_pol:

-----------------------------------
Flow ID Based Security (VDI Policy)
-----------------------------------

ID based firewall integrates Nutanix Flow with Microsoft Active Directory (AD), such that the groups in the AD can be imported into Prism Central as categories. These imported categories can then be used in the VDI policy as target groups, inbound traffic, and outbound traffic. Prism Central automatically places VMs inside the imported AD group categories when user logons are detected on VMs that are part of the Active Directory domain and present on Nutanix managed clusters, thus applying security policies based on user group membership.

First, we have to enable connectivity between Prism Central so that we can import AD user groups for identity based security policies.

Connecting Flow to Active Directory
+++++++++++++++++++++++++++++++++++

#. In **Prism Central**, select :fa:`bars` **> Prism Central Settings**.

      .. figure:: images/1-1.png

#. Click **ID Based Security** from the Settings menu (on the left).

      .. figure:: images/1-2.png

#. Click **Use Existing AD**.

#. Select **ntnxlab** in the AD Server dropdown.

      .. figure:: images/1-3.png

#. Enter nutanix/4u for the Service Account Password.

      .. figure:: images/1-4.png

#. Click **Next**.

#. Click the **Manually Add Domain Controller** button, then click **+ Domain Controller**.

      .. figure:: images/1-5.png

#.	Enter the IP Address or Host Name of the domain controllers in your lab environment (provided in the lab info sheet).

      .. figure:: images/1-6.png

#.  Click **Save**

#.	Confirm that the **Configured Domain** and **Domain Controllers** is as shown.

    .. figure:: images/1-7.png

      .. note::

    You can safely ignore the alert relating to upgrading the Prism-Pro-Cluster.

#. In the **Referenced AD Groups** section, click **+ Add User Group**.

    .. figure:: images/1-8.png

#. Click in the **Search AD User Groups** field and enter **developer**. SSP Developers should populate the field.

    .. figure:: images/1-9.png

#.	Click the blue check box to confirm and add to the list.

#.	Repeat steps 11 - 13, entering **consumer** in the **User Group** field.

    .. figure:: images/1-10.png

#. In **Prism Central**, select :fa:`bars` **> Policies > Security Policies**.

  .. figure:: images/1-11.png

#. Delete the **Fiesta** policy created for the **Securing Applications with Flow** lab.

#.	Click **Create Security Policy > Secure VDI Groups (VDI Policy) > Create**.

  .. figure:: images/1-12.png

#.	Flow can only have a single VDI Policy, so the name and purpose are pre-populated. You can specify how the policy will apply to VMs based on their name contents. This helps assure that the VM is categorized correctly. Check **Include VMs by name** and enter **VDI** in the VM Name Contains box.

  .. figure:: images/1-13.png

*If you previously completed the* **Deploy Graylog** *lab, click* **Enable** *for the Policy Hitlogs*

#.	Click **Next**

#. Click **OK, Got it** on the **Securing a VDI Environment** tutorial window.

#.	In **VDI ADGroups** section click in the **Select an AD Group to add** box and select **SSP Consumers**.

  .. figure:: images/1-14.png

#.	Repeat step 21 and select **SSP Developers**.

*If you hover the mouse over an ADGroup just added, and click* **Edit** *, notice that you can prevent traffic between VMs within this group.*

  .. figure:: images/1-15.png

#.	Since we’ll only have a single VM in this group, leave as default.

#.	In the Outbounds section, click **+ Add Destination**.

  .. figure:: images/1-16.png

#.	Set **Add destination by: to Category**, and type **fiesta** in the Search for a category box. This will provide all categories that include fiesta. Select **AppTier:FiestaWeb** and click **Add**.

  .. figure:: images/1-17.png

#.	Repeat step 24 and add **AppTier:FiestaDB**.

  .. figure:: images/1-18.png

#.	Lastly, we’ll need to assure connectivity to the AD server. Since it’s not categorized, we’ll need to add it by IP address. For this one, we need set **Add destination by:** to **Subnet/IP** and enter the IP address of your Auto AD VM (from the lab info sheet).

  .. figure:: images/1-19.png

#.	Click **Add**.

*Now that out outbound targets have been identified; we need to define the permitted traffic.*

#.	Click on the AD IP in the Outbounds section, and then the + icon on **SSP Consumers**.

  .. figure:: images/1-20.png

#.	Leave as defaults with **Allow all traffic** selected, and click **Save**.

  .. figure:: images/1-21.png

#.	Repeat for SSP Developers. Once complete you will see each ADGroup has a line to the AD IP.

  .. figure:: images/1-22.png

*In a production environment, it might be more appropriate to limit traffic to specific services/ports*

#.	Under Outbounds, click on **AppTier:FiestaWeb** then click the + icon for **SSP Consumers**.

  .. figure:: images/1-23.png

#.	In the rule box, click **Select a Service** and enter **http** for the service name, and select **http** from the drop down.

  .. figure:: images/1-24.png

#.	Click **Save**.

#.	Click the + icon next to **SSP Developers**.

  .. figure:: images/1-25.png

#.	Leave the defaults, **Allow all traffic**.

  .. figure:: images/1-26.png

#.	Click **Save**.

#.	Repeat step 32 using **AppTier:FiestaDB** and **SSP Developers**.

  .. figure:: images/1-27.png

#.	Leave the defaults, **Allow all traffic**.

  .. figure:: images/1-28.png

#.	Click **Save**.

*The result of this policy, is SSP Consumers are permitted to AppTier:FiestaWeb via http, and only SSP Developers is permitted to both tiers, on any port/protocol, as well as both groups can access the AD server*

  .. figure:: images/1-29.png

*In a production environment, it's recommended to only permit the required traffic, but for the purpose of this lab, we're keeping it simple.*

#.	Click **Next**.

#.	Click **Save and Monitor**.

  .. figure:: images/1-30.png

*Now we will test our policy using the 2 VDIClient VMs.*

#.	In **Prism Central**, select :fa:`bars` **> Virtual Infrastructure > VMs**.

#.	Click the checkbox next to **VDIClient1**, then click **Actions > Launch Console**.

  .. figure:: images/2-1.png

#.	Repeat step 44 for VDIClient2.

*By default the administrator user is logged into the VM, so click Switch user on each VM console.*

#.	On **VDIClient1** console, click **Switch user**, then click on the **Ctrl-Alt-Del** button in the upper right corner.

  .. figure:: images/2-2.png

#.	Click on **Other user**.

#.	Enter Consumer01 for the user and nutanix/4u for the password.

  .. figure:: images/2-3.png

#.	In the console for **VDIClient2**, repeat steps 46 to 48, with Devuser01 for the user and nutanix/4u for the password.

  .. figure:: images/2-4.png

#. Return to Prism Central VM page, and note the IP addresses for your Fiesta application VMs. They will be named FiestaWeb-X-XXXXX and FiestaMYSQL-XXXXX.

#.	In the consoles for **VDIClient1** and **VDIClient2** , open Chrome and enter the IP address for the Fiesta web server. You should see the Fiesta homepage.

  .. figure:: images/2-5.png

#.	Now open a command prompt and ping the IP addresses FiestaWeb and FiestaMYSQL servers. Repeat this on both VDIClient VMs.

  .. figure:: images/2-6.png

Why do the pings from VDIClient1 (Consumer01 user) succeed when only http traffic was configured in the policy?

#.	In **Prism Central**, select :fa:`bars` **> Policies > Security**.

#.	Click on **VDI Policy**.

  .. figure:: images/2-7.png

*Since the policy is in monitor mode, all traffic is permitted, but traffic that is not compliant with the policy is illustrated by the orange lines. In the below example, there are 52 discovered flows, which are exceptions to the configured policy.*

  .. figure:: images/2-8.png

#.	Click on the **blue right arrow** until you find the web or DB server.

  .. figure:: images/2-9.png

NOTE: They may not appear together, or on the same page.

#.	Mouse over one of the lines running from either Fiesta server. This will provide details on the traffic that was captured.

  .. figure:: images/2-10.png

*In this example, it is a ping from the VM that user Consumer01 is logged into (VDICLient1). The same would be true for the line from the between SSP Consumers and FiestaWeb server since the configured policy is to only permit http traffic between these entities.*

#.	In the upper right corner, click on **Enforce**, and confirm when prompted.

  .. figure:: images/2-11.png

*Note how the lines have changed from yellow to red. This indicates that attempted traffic, that is not within the policy, is being blocked.*

  .. figure:: images/2-12.png

#.	Click the **X** in the upper right to dismiss this window, and return to the Security Polices page.

#.	Return to the **VDIClient1** VM console (you may need to close and reopen to restore the session)

#.	In **Chrome**, on the Fiesta webpage, click on **Stores**, **Products** or **Inventory** to be sure we still have complete http access.

  .. figure:: images/2-13.png

#.	Still in **VDIClient1** console, switch to the **command prompt** and attempt the pings again. This time they will fail as the policy was changed from monitor to enforce.

  .. figure:: images/2-14.png

#.	Return to the **VDIClient2** VM console (you may need to close and reopen to restore the session)

#.	Repeat steps 61 and 62. Note that the pings succeed from VDIClient2, why is this?

  .. figure:: images/2-15.png

You can see how different policies were applied based on the userid that logged into that system. This completes this lab.
