.. _sec_apps:

-------------------
Securing applications with Flow (Network Microsegmentation)
-------------------

Flow is an application-centric network security product tightly integrated into Nutanix AHV and Prism Central. Flow provides rich network traffic visualization, automation, and security for VMs running on AHV.
Microsegmentation is a component of Flow that uses simple policy-based management to secure VM networking. Using Prism Central categories (logical groups), you can create a powerful distributed firewall. Combining this with Calm allows automated deployment of applications that are secured as they are created.

In this exercise you will restrict access to the Fiesta application and protect traffic between the application tiers.

Securing the Fiesta Application

Flow provides multiple System categories out of the box, such as AppType, AppTier, and Environment, that are used to quickly group virtual machines. Security policies are applied using these categories. You can start using these pre-existing categories right away, or add your own categories for custom grouping.

Defining Category Values
Prism Central uses categories as metadata to tag VMs to determine how policies will be applied.

1.	In Prism Central, select  > Virtual Infrastructure > Categories.

2.	Select the checkbox for AppType and click Actions > Update.

3.	Click the icon beside the last value to add an additional Category value.
4.	Specify Initials-Fiesta as the value name.

5.	Click Save.
6.	Select the checkbox for AppTier and click Actions > Update.
7.	Click the  icon beside the last value to add an additional Category value.
8.	Specify Initials-Web as the value name. This category will be applied to the application’s web tier.
9.	Click  and specify Initials-DB. This category will be applied to the application’s MySQL database tier.

10.	Click Save.
Creating a Security Policy
Nutanix Flow includes a policy-driven security framework that uses a workload-centric approach instead of a network-centric approach. Therefore, it can scrutinize traffic to and from VMs no matter how their network configurations change and where they reside in the data center. The workload-centric, network-agnostic approach also enables the virtualization team to implement these security policies without having to rely on network security teams.
Security policies are applied to categories and not to the VMs themselves. Therefore, it does not matter how many VMs are started up in a given category. Traffic associated with the VMs in a category is secured without administrative intervention, at any scale.
Create the security policies that will protect the Fiesta application.
1.	In Prism Central, select  > Policies > Security.

2.	Click Create Security Policy > Secure Applications (App Policy) > Create.
3.	Fill out the following fields:
o	Name - Initials-Fiesta
o	Purpose - Restrict unnecessary access to Fiesta
o	Secure this app - AppType: Initials-Fiesta
o	Do NOT select Filter the app type by category.

4.	Click Next.
5.	If prompted, click OK, Got it! on the tutorial diagram of the Create App Security Policy wizard.
6.	To allow for more granular configuration of the security policy, click Set rules on App Tiers, instead rather than applying the same rules to all components of the application.

7.	Click Select a Tier to add.
8.	Select AppTier:Initials-Web from the drop down.
9.	Repeat Steps 7-8 for AppTier:Initials-DB.

Next you will define the Inbound rules, which control which sources you will allow to communicate with your application. You can allow all inbound traffic, or define whitelisted sources. By default, the security policy is set to deny all incoming traffic.
In this scenario we want to allow inbound TCP traffic to the web tier on TCP port 80 from all clients.
10.	Under Inbound, click + Add Source.
11.	Fill out the following fields to allow all inbound IP addresses:
o	Add source by: - Select Subnet/IP
o	Specify 0.0.0.0/0
o	Click Add
Note
Sources can also be specified by Categories, allowing for greater flexibility as this data can follow a VM regardless of changes to its network location.

12.	To create an inbound rule, select the + icon that appears to the left of AppTier:Initials-Web.

13.	Click on Select a Service
14.	In the Service Table, click Search for a service and enter “http”, and select http from the list.

Note
Multiple services (protocols and ports) can be added to a single rule.

15.	Click Save.
16.	Under Inbound, click + Add Source.
17.	Fill out the following fields:
o	Add source by: - Select Subnet/IP
o	Specify Your Prism Central IP/32

Note
The /32 denotes a single IP as opposed to a subnet range.

18.	Click Add.
19.	Select the + icon that appears to the left of AppTier:Initials-Web, click Select a Service, enter ssh for the Service Name, Save.

20.	Repeat Step 18 for AppTier:Initials-DB to allow Calm to communicate with the database VM.

By default, the security policy allows the application to send all outbound traffic to any destination. The only outbound communication required for your application is to communicate with your DNS server.
21.	Under Outbounds, select Allowed List Only from the drop down menu, and click + Add Destination.
22.	Fill out the following fields:
o	Add source by: - Select Subnet/IP
o	Specify Your Domain Controller IP/32

23.	Click Add.
24.	Select the + icon that appears to the right of AppTier:Initials-Web, click Select a Service, enter domain for the Service Name, click Save to allow DNS traffic.

25.	Repeat this for AppTier:Initials-DB.

Each tier of the application communicates with other tiers and the policy must allow this traffic. Some tiers such as web do not require communication within the same tier.
26.	To define intra-app communication, click Set Rules within App.

27.	Click Edit  AppTier:Initials-Web and select No to prevent communication between VMs in this tier. There is only a single web VM within the tier.

28.	While AppTier:Initials-Web is still selected, click the  icon to the right of AppTier:Initials-DB to create a tier to tier rule.

29.	Click Select a Service, enter mysql for the Service Name.

30.	Click Save.
31.	Click Next to review the security policy.
32.	Click Save and Monitor to save the policy.
Assigning Category Values
You will now apply the previously created categories to the VMs provisioned from the Fiesta blueprint. Flow categories can be assigned as part of a Calm blueprint, but the purpose of this exercise is to understand category assignment to existing virtual machines.
1.	In Prism Central, select  > Virtual Infrastructure > VMs.
2.	Click Filters and select the label for Initials AHV Fiesta VMs to display your virtual machines.

3.	Using the checkboxes, select the 2 VMs associated with the application (Web and DB) and select Actions > Manage Categories.

4.	Specify AppType:Initials-Fiesta in the search bar and click Save icon to bulk assign the category to all VMs.

5.	Select ONLY the nodereact VM, select Actions > Manage Categories, specify the AppTier:Initials-Web category and click Save.

6.	Repeat Step 5 to assign AppTier:Initials-DB to your MySQL VM.
7.	Finally, Repeat step 5 to assign Environment:Dev to your Windows Tools VM.
Monitoring and Applying a Security Policy
Before applying the Flow policy, you will ensure the Fiesta application is working as expected.
Testing the Application
1.	From Prism Central > Virtual Infrastructure > VMs, note the IP address of your nodereacte… and -MYSQL-… VMs.
2.	Launch the console for your WinTools VM.
3.	From the WinTools console open a browser and access http://node-VM-IP/ (where node-VM-IP is the IP address of your nodereacte vm)
4.	Verify that the application loads and that products can be added and deleted.

5.	Open Command Prompt and run ping -t MYSQL-VM-IP to verify connectivity between the client and database. Leave the ping running.
6.	Open a second Command Prompt and run ping -t node-VM-IP to verify connectivity between the client and web server. Leave the ping running.

Using Flow Visualization
1.	Return to Prism Central and select  > Virtual Infrastructure > Policies > Security Policies >Initials-Fiesta.
2.	Verify that your WinTools VM appears as an inbound source. The source and line appear in yellow to indicate that traffic that is an exception to the policy has been detected from your client VM.

Are there any other detected outbound traffic flows? Hover over these connections and determine what ports are in use.
3.	Click Update to edit the policy.

4.	Click Next and wait for the detected traffic flows to populate.
5.	Mouse over the VM  Wintools source that discovered and click Allow Traffic.

6.	Check the boxes next to the discovered traffic you want to permit within the policy. In this case we will permit traffic from our Wintools VM to the web server and block traffic to the DB server. Click Save.

The IP address of your Wintools VM is now added to the permitted inbound list, with a connection to the web server. Mouse over the flow line, and verify the ICMP traffic is allowed. Note that there is still a discovered connection to the DB server. This is because we did not permit this traffic, so it is still showing as an exception to our policy rule.

7.	Click Next > Save and Monitor to update the policy.
Enforcing Flow Policies

In order to enforce the policy you have defined, the policy must be enforced.
1.	Select Initials-Fiesta and click Actions > Enforce.

2.	Type ENFORCE in the confirmation dialogue and click OK to begin blocking traffic.
3.	Return to the Initials-WinToolsVm console.
What happens to the continuous ping traffic from the Windows client to the database server? Is this traffic blocked?
4.	Verify that the Windows Client VM can still access the Fiesta application using the web browser and the web server IP address.
Can you still add new products under Products and update product quantities under Inventory?

Takeaways
•	Microsegmentation offers additional protection against malicious threats that originate from within the data center and spread laterally, from one machine to another.
•	Security policies leverage the text based categories in Prism Central.
•	Flow can restrict traffic on certain ports and protocols for VMs running on AHV.
•	The policy is created in Monitor mode, meaning traffic is not blocked until the policy is enforced. This is helpful to learn the connections and ensure no traffic is blocked unintentionally.
