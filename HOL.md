<a name="HOLTop" />
# Introduction to Windows Azure Virtual Machines #

---

<a name="Overview" />
## Overview ##

Using Windows Azure as your IaaS platform, will enable you to create and manage your infrastructure quickly, provisioning and accessing any host ubiquitously. Grow your business through the cloud-based infrastructure, reducing the costs of licensing, provisioning and backup.

In this hands-on Lab, you will learn how to deploy a simple ASP.NET MVC4 Web application to a Web server hosted in Windows Azure, using SQL Server and configuring load balancing.

<a name="Objectives" />
### Objectives ###

In this hands-on lab, you will learn how to:

- Create a Web Farm using Windows Azure Management Portal
- Configure Load Balancing in IIS
- Deploy a Simple MVC4 Application that consumes SQL Server Features
- Create a VM with SQL Server Full-Text Search feature to be consumed by the MVC Application

<a name="Prerequisites" />
### Prerequisites ###

The following is required to complete this hands-on lab:

- A Windows Azure subscription with the Virtual Machines Preview enabled - you can sign up for free trial [here](http://bit.ly/WindowsAzureFreeTrial)

>**Note:** This lab was designed to use Windows 7 Operating System.

---

<a name="Exercises" />
## Exercises ##

This hands-on lab includes the following exercises:

1. [Creating VMs for IIS](#Exercise1)
1. [Creating a SQL Server VM](#Exercise2)
1. [Deploying a Simple MVC4 Application](#Exercise3)

Estimated time to complete this lab: **45 minutes**.

<a name="Exercise1" />
### Exercise 1: Creating VMs for IIS ###

In this exercise, you will learn how to create a Virtual Machine in Windows Azure. Then, you will configure an Internet Information Server adding roles to use later on in this lab.

<a name="Ex1Task1" />
#### Task 1 - Creating IIS VMs ####

In this task, you will provision a Virtual Machine and configure the Load Balancing to host an MVC4 application.

1. Open Internet Explorer and browse [https://manage.windowsazure.com/](https://manage.windowsazure.com/) to enter the Windows Azure portal. Then, log in with your credentials.

1. In the menu located at the bottom, select **New | Virtual Machine | From Gallery** to start creating a new virtual machine.
	 
	![Creating a new Virtual Machine](images/creating-a-new-virtual-machine.png?raw=true)

	_Creating a new Virtual Machine_
 
	1. In the **VM OS Selection** page, click **Platform Images** on the left menu and select the **Windows Server 2008 R2** OS image from the list. Click the arrow to continue.	

	1. In the **VM Configuration** page, enter the **Virtual Machine Name** (i.e. "_iisvm1_") and the administrator user's **Password**. Click the **right arrow** to continue.

		![Configuring a Custom VM](images/creating-a-vm-configuration.png?raw=true)
	 
		_Creating a VM - Configuration_
 
		>**Note:** It is suggested to use secure passwords for admin users, as Windows Azure virtual machines could be accessible from the Internet knowing just their DNS.
		>
		>You can also read this document on the Microsoft Security website that will help you select a secure password:  [http://www.microsoft.com/security/online-privacy/passwords-create.aspx](http://www.microsoft.com/security/online-privacy/passwords-create.aspx)
 
	1. In the **VM Mode** page, select **Standalone Virtual Machine**, enter the **DNS Name**, select a **Storage Account** or leave the default value _Use Automatically Generated Storage Account_, and select a **Region/Affinity Group/Virtual Network**. Click the **right arrow** to continue. 

		![Configuring a Custom VM, VM Mode](images/creating-a-vm-vm-mode.png?raw=true)
	 
		_Creating a VM - VM Mode_
 
	1. In the **VM Options** page, leave the default values and click the button to create a new VM.

		![Creating a VM - VM Options](images/creating-a-vm--vm-options.png?raw=true "Creating a VM - VM Options")

		_Creating a VM - VM Options_

1. In the **Virtual Machines** section, you will see the VM you created with a _provisioning_ status. Wait until it changes to _Running_ in order to continue with the following step.

 	![Creating VM for IIS Web Farm](./images/creating-vm-for-iis-web-farm.png?raw=true "Creating VM for IIS Web Farm")
 
	_Creating VM for IIS Web Farm_

	> **Note:** It might take a few minutes for the VM to appear in the Virtual Machines list. 

1. You will now add the second VM for the IIS Load Balancing. In the portal, select **New | Virtual Machine | From Gallery**. 

1. In the **VM OS Selection** page, click **Platform Images** on the left menu and select the **Windows Server 2008 R2** OS image from the list. Click the **arrow** to continue.	

1. In the **VM Configuration** page, enter the **Virtual Machine Name** (i.e. "_iisvm2_", the administrator user's **Password** and the **Size**. Click the **right arrow** to continue.
 
1. In the **VM Mode** page, select **Connect to existing Virtual Machine** and choose the first VM you created from the drop down list. Select a **Storage Account** or leave the default value _Use Automatically Generated Storage Account_ and click the **right arrow** to continue. This step adds the new virtual machine to the cloud service created in the previous step. This allows the virtual machines to be on the same network.


	![Configuring a Custom VM, VM Mode](images/creating-a-vm-vm-mode2.png?raw=true)
	 
	_Creating a VM - VM Mode_

 
1. In the **VM Options** page, leave the default values and click the button to create a new VM.

1. Wait until the second VM is created. You can check the VM status from the Virtual Machines section within the portal.

1. After creating the second VM, you will create an endpoint in the port 80 in the Virtual Machine you created first. To do this, click on the first VM Name (_iisvm1_) to go to the **Dashboard** page and then click **Endpoints**.

1. Click **Add Endpoint** on the bottom pane. Make sure that **Add Endpoint** option is selected and then click the **right arrow** button to continue.

	![Adding a new Endpoint](images/adding-a-new-endpoint.png?raw=true "Adding a new Endpoint")

	_Adding a new Endpoint_

1. In the **Specify endpoint details** page, set the **Name** to _webport_, the **Protocol** to _TCP_ and the **Public Port** and **Private Port** to _80_.

	![New Endpoint Details](images/new-endpoint-details.png?raw=true "New Endpoint Details")

	_New Endpoint Details_

1. Now, create a new Endpoint in the second VM in order to enable Load Balancing between both VMs. To do this, click **Virtual Machines** and then select the second VM you created. Then, click **Endpoints**.

1. Click **Add Endpoint**, select **Load Balance Traffic On An Existing Endpoint** option. Select the endpoint you created for the first VM from the drop down list and then click the **right arrow** to continue.

	![Load Balance Traffic An Existing Endpoint](images/load-balance-traffic-an-an-existing-endpoint.png?raw=true "Load Balance Traffic An Existing Endpoint")

	_Load Balance Traffic An Existing Endpoint_

1. In the **New Endpoint Details** page, set the **Name** to _webport_ and the **Private Port** to _80_.

1. In the **Virtual Machines** section, click on the first VM Name (_iisvm1_) and then click **Endpoints**. 

1. Select the **webport** endpoint you have created. Make sure the **Load Balancer** column value is **Yes**. 

	![Verification: enabling IIS Load Balancing](images/creating-load-balancing-endpoint-1.png?raw=true)

	_Verification: enabling IIS Load Balancing_

1. Click the **Edit Endpoint** button in the bottom bar to enter the endpoint details and verify the load balancing is enabled. Repeat this step in the second VM.

	![Verification: enabling IIS Load Balancing, details](images/creating-load-balancing-endpoint-2.png?raw=true)

	_Verification: enabling IIS Load Balancing, details_

<a name="Ex1Task2" />
#### Task 2 - Configuring IIS VMs ####

In this task, you will configure the IIS VMs by adding the necessary roles to deploy the MVC application.

1. In the Portal, click **Virtual Machines** on the left menu.

1. You will see a list with your existing VMs. Select the first one you created in Task 1 and click **Connect**. If you used the proposed name, this VM should be named **iisvm1**.

1. You will be asked to download the remote desktop client. Click **Open** and log on using the Admin credentials you defined when creating the VM.

1. In the Azure VM, open **Server Manager** from **Start | All Programs | Administrative Tools**.

1. In the **Server Manager** window, select **Roles** node.

 	![Server Manager](./images/Server-Manager.png?raw=true "Server Manager")
 
	_Server Manager_

1. Click **Add Roles** link.

 	![Adding Server Roles](./images/Adding-Server-Roles.png?raw=true "Adding Server Roles")
 
	_Adding Server Roles_

1. The **Add Roles Wizard** will appear.

	1.  In the **Before you Begin** page, read the content and click **Next**.

	1. In **Select Server Roles** page, check the **Application Server** and **Web Server (IIS)**. A warning will show, informing the Required Role Services that are missing. Click **Add Required Role Services** to install them and then click **Next**.

 		![Add Roles Wizard(2)](images/add-roles-wizard2.png?raw=true)
 
		_Add Roles Wizard_

	1. The **Application Server** page provides a brief introduction about Application Server's capabilities. Click **Next** when you complete reading it.

	1. In the **Select Role Services** page for **Application Server**, select **Web Server (IIS)** **Support** and make sure **.NET Framework 3.5.1** is selected. It will prompt a dialog warning about missing Required Role Services. Click **Add Required Role Services** to install them and then click **Next**

 		![Add Roles Wizard(3)](images/add-roles-wizard3.png?raw=true)
 
		_Add Roles Wizard_

	1. The **Web Server (IIS)** page provides a brief introduction about Web Server (IIS) capabilities. Click **Next** when you complete reading.

	1. The **Select Role Services** page for **Web Server (IIS)** page will display the selected role services that will be installed. Click **Next**.

 		![Add Roles Wizard(4)](images/add-roles-wizard4.png?raw=true)
 
		_Add Roles Wizard_

	1. In the **Confirm Installation Selections** page, make sure the displayed services that will be installed are the ones you have selected (.NET Framework 3.5.1 support and IIS), and then click **Install**.

 		![Add Roles Wizard](./images/Add-Roles-Wizard.png?raw=true "Add Roles Wizard")
 
		_Add Roles Wizard_

1. Close the **Remote Desktop Connection**.

	Repeat this task on the second VM to install IIS, starting from step 4. If you used the proposed name, the second VM should be named **iisvm2**.

<a name="Exercise2" />
### Exercise 2: Creating a SQL Server VM ###

In this exercise, you will create a new VM and learn how to install SQL Server. You will add disk images to the existing VM in order to split the data from the logs generated by SQL Server.

<a name="Ex2Task1" />
#### Task 1 - Creating a SQL Server VM ####

In this task, you will create a new VM using the Windows Azure portal in the same Cloud App you deployed the IIS VMs.

1. In the menu located at the bottom, select **New | Virtual Machine | From Gallery** to start creating a new virtual machine.
 
1. In the **VM OS Selection** page, click **Platform Images** on the left menu and select the **SQL Server 2012** OS image from the list. Click the arrow to continue.	

1. In the **VM Configuration** page, enter the **Virtual Machine Name** (i.e. "_sqlvm1_"), the administrator user's **Password** and the **Size**. Click the **right arrow** to continue.
 
1. In the **VM Mode** page, select **Connect to existing Virtual Machine** and choose the first IIS VM you created from the drop down list. Click the **right arrow** to continue.
 
1. In the **VM Options** page, leave the default values and click the button to create a new VM.

1. In the **Virtual Machines** section, you will see the VM you created with a _provisioning_ status. Wait until it changes to _Running_ in order to continue with the following step.

<a name="Ex2Task2" /> 
#### Task 2 - Attaching Empty Disk Images ####

In this task, you will create two empty data disks and attach them to an existing VM using the Windows Azure Management Portal. You will use these data disks to split SQL Server Data and Logs.

1. Now, you will create and attach empty data disks to store the SQL Server logs and data files, and you will also add an endpoint. To do this, in the **Virtual Machines** section, select the SQL Server VM you created in the previous task.

1. In the VM's **Dashboard**, click **Attach** in the menu at the bottom of the page and select **Attach Empty Disk**.

	![Attach Empty Disk](images/attach-empty-disk.png?raw=true "Attach Empty Disk")

	_Attach Empty Disk_

1. In the **Attach Empty Disk** page, set the **Size** to _50_ GB and create the Disk.

1. Wait until the process to attach the disk finishes. Repeat the steps 1 to 3 to create a second disk.

1. You will see three disks for the VM: one for the **OS** and other two for **Data** and **Logs**.

	> **Note:** It might take a few minutes until the data disks appear in the VM's dashboard within the Azure Portal.

 	![Attached Data Disks](./images/Attached-Data-Disks.png?raw=true "Attached Data Disks")
 
	_Attached Data Disks_

1. Finally, you need to format the disks in order to access them from the Virtual Machine. To do this, click **Connect** to connect to the VM using **Remote Desktop connection**.

1. It will ask you to download the remote desktop client. Click **Open** and log on using the Admin credentials you defined when creating the VM.

1. In the Virtual Machine, open **Server Manager** from **Start | All Programs | Administrative Tools**.

1. Expand **Storage** node and select **Disk Management** option.

 	![Disk Management(2)](images/disk-management2.png?raw=true)
 
	_Disks Management_

1. Locate the disks you created using the **Attach Empty Disk** feature from the Windows Azure Management Portal. Right-click the first disk and select **Initialize Disk**.

1. In the **Initialize Disk** dialog, leave the default values and click **OK**.

1. Right-click the first disk unallocated space and select **New Simple Volume**.

 	![Disk Management](images/disk-management.png?raw=true)
 
	_Disks Management_

1. Follow the **New Simple Volume Wizard**. When asked for the **Volume Label** use _SQLData_.

1. Wait until the process for the first disk is completed. Repeat the steps 15 to 16 but this time using the second disk. Set the **Volume Label** to _SQLLogs_.

1. The **Disk Management** list of available disks should now show the **SQLData** and **SQLLogs** disks like in the following figure:

 	![Disks Management](./images/Disks-Management.png?raw=true "Disks Management")
 
	_Disks Management_

	> **Note:** Do not close the **Remote Desktop Connection**. You will use it in the following task.


<a name="Ex2Task3" /> 
#### Task 3 - Configuring SQL Server in the VM ####

In this task, you will configure SQL Server 2012. You will create the database that will be used by the MVC4 application and add Full-Text Search capabilities to it. Additionally, you will create a SQL Server user for the MVC4 website.

1. Open the SQL Server Management Studio from **Start | All Programs | Microsoft SQL Server 2012 | SQL Server Management Studio**.

1. Connect to the SQL Server 2012 default instance using your Windows Account.

1. Now, you will update the database's default locations in order to split the DATA from the LOGS. To do this, right click on you SQL Server instance and select **Properties**.

1. Select **Database Settings** from the left side pane.

1. Locate the **Database default locations** section and update the default values to point to the disks you attached in the previous task.

 	![Setting Database Default Locations](./images/Setting-Database-Default-Locations.png?raw=true "Setting Database Default Locations")
 
	_Setting Database Default Locations_

1. Using Windows Explorer create the following folders: **F:\Data, G:\Logs and G:\Backups**

1.	In order to enable downloads from Internet Explorer you will need to update **Internet Explorer Enhanced Security Configuration**. In the Azure VM, open **Server Manager** from **Start | All Programs | Administrative Tools**.

1. In the **Server Manager**, click **Configure IE ESC** within **Security Information** section.

	![Internet Explorer Enhanced Security](images/Internet-Explorer-Enhanced-Security.png?raw=true)
		
	_Internet Explorer Enhanced Security_
 
1. In the **Internet explorer Enhanced Security** configuration, turn **off** the enhanced security for **Administrators** and click **OK**.

	![Internet Explorer Enhanced Security(2)](images/internet-explorer-enhanced-security2.png?raw=true)
	 
	_Internet Explorer Enhanced Security_
 
	>**Note:** Modifying **Internet Explorer Enhanced Security** configurations is not good practice and is only for the purpose of this particular lab. The correct approach should be to download the files locally and then copy them to a shared folder or directly to the VM.

1. This lab uses the **AdventureWorks** database. Open an **Internet Explorer** browser and go to <http://msftdbprodsamples.codeplex.com/> to download  the **SQL Server 2012** sample databases. Once on the page click SQL Server 2012 DW and then download Adventure Works 2012 Data File. 

1. Download the file to F:\Data.

1. Right click the database file and open the properties. Click **unblock**.

1. In the explorer, open the data disk and create the folder _Data_. Make sure you have write access to that folder.

1. Open the logs disk and create the folder _Logs_. Make sure you have write access to that folder.

1. Copy the database mdf file from the location you have installed it in the folder Data in the data disk. To do this, open the folder where you downloaded **AdventureWorks** and paste it in the data disk.


1. Add the **AdventureWorks** sample database to your SQL Server. To do this, in the **SQL Server Management Studio**, locate your SQL Server instance node and expand it. Right click the **Databases** folder and select **Attach**.

	![Attaching the database](images/attaching-adventureworks-database-menu.png?raw=true)

	_Attaching the database_

1. In the **Attach Databases** dialog, press **Add**. Browse to the data disk select the Adventure Works data file.

 	![Attaching AdventureWorks Database](images/attaching-adventureworks-database.png?raw=true)
 
	_Attaching AdventureWorks Database_

1. Select the **AdventureWorks2012** Log entry and click **Remove**.

1. Press **OK** to add the database.

1. In the **Databases** folder, locate the new **AdventureWorks2012** database and explore its tables.

 	![AdventureWorks Sample Database](./images/adventureworks-sample-database.png?raw=true "Northwind Sample Database")
 
	_AdventureWorks Sample Database_

1. Expand **Storage** node within **AdventureWorks** database , right-click **Full Text Catalogs** folder and select **New Full-Text Catalog**.

	> **Note:** You are creating a Full Text Catalog for the database that will be used later by the MVC application. 

 	![Create New Full-Text Catalog(2)](images/create-new-full-text-catalog2.png?raw=true)
 
	_Create New Full-Text Catalog_

1. In the New Full-Text Catalog dialog, set the **Name** value to _AdventureWorksCatalog_ and press **OK**.

 	![Create New Full-Text Catalog(3)](images/create-new-full-text-catalog3.png?raw=true)
 
	_Create New Full-Text Catalog_

1. Right-click **AdventureWorksCatalog** and select **Properties**. In the **Full-Text Catalog Properties** dialog, switch to **Tables/Views** page.

1. Add the **Products** table to the **Table/View objects assigned to the Catalog** list. Then, check the _Name_ column and click **OK**.

 	![Create New Full-Text Catalog(4)](images/create-new-full-text-catalog4.png?raw=true)
 
	_Create New Full-Text Catalog_

1. Check that the Full-Text Catalog you created appears in the **Full-Text Catalogs** folder.

 	![Create NEw Full-Text Catalog(5)](images/create-new-full-text-catalog5.png?raw=true)
 
	_Create New Full-Text Catalog_

1. Add a new user for the MVC4 application you will deploy in the following exercise. To do this, expand **Security** folder within the SQL Server instance. Right-click **Logins** folder and select **New Login**.

 	![Creating a New Login(2)](images/creating-a-new-login2.png?raw=true)
 
	_Creating a New Login_

1. In the **General** section, set the **Login name** to _AzureStore._ Select **SQL Server authentication** option and set the **Password** to _Azure$123_.

	> **Note:** If you enter a different username or password than those suggested in this step, do not forget in the next exercise to update the web.config file of the MVC4 application to match those values.

1. Unselect **Enforce password policy** checkbox to avoid having to change the password the first time you log on, and set the **Default database** to _AdventureWorks_.

 	![Creating a New Login](./images/Creating-a-New-Login.png?raw=true "Creating a New Login")
 
	_Creating a New Login_

1. Click **User Mapping** on the left pane. Select the map checkbox in the _AdventureWorks_ database row and click **OK**.

 	![Mapping the new User to the AdventureWorks Database](./images/mapping-new-user-database-2.png?raw=true "Mapping the new User to the AdventureWorks Database")
 
	_Mapping the new User to the AdventureWorks Database_

1. Expand **AdventureWorks** database within **Databases** folder. In the **Security/Users** folder, double-click **AzureStore** user.

1. Update the **Database role membership**. Select the _db_owner_ role checkbox for the **AzureStore** user and click **OK**.

 	![Adding Database role membership to AzureStore user](./images/Adding-Database-role-membership-to-AzureStore-user.png?raw=true "Adding Database role membership to AzureStore user")
 
	_Adding Database role membership to AzureStore user_

	> **Note:** The application you will deploy in the next exercise uses Universal Providers to manage sessions. The first time the application runs, the provider will create the Sessions table within the  database. For that reason, you are assigning a db_owner role to the AzureStore user. Once you run the application for the first time, you can remove this role as these permissions will not be needed.

1. Now, enable **Mixed Mode Authentication** to the SQL Server instance. To do this, in the **SQL Server Management Studio**, right-click the server instance and click **Properties**.

1. Click **Security** in the right side pane and then select **SQL Server and Windows Authentication** mode under **Server Authentication** section. Click **OK** to save changes.

1. Restart the SQL Server instance. To do this, right-click the SQL Server instance and click **Restart**.

1. Close the **SQL Server Management Studio**.

1. In order to allow the MVC4 application access the SQL Server database you will need to add an **Inbound Rule** for the SQL Server requests in the **Windows Firewall**. To do this, open **Windows Firewall with Advance Security** from **Start | All Programs | Administrative Tools**.

1. Right-click **Inbound Rules** node and select **New Rule** to open the **New Inbound Rule Wizard**.

 	![Creating an Inbound Rule](./images/Creating-an-Inbound-Rule.png?raw=true "Creating an Inbound Rule")
 
	_Creating an Inbound Rule_

	1. In the **Rule Type** page, select **Port** and click **Next**.

		![New Inbound Rule Wizard](images/new-inbound-rule-wizard2.png?raw=true)
 
		_New Inbound Rule Wizard_

	1. In **Protocols and Ports** page, leave TCP selected, select **Specific local ports,** and set its  value to _1433_. Click **Next** to continue.

 		![New Inbound Rule Wizard](images/new-inbound-rule-wizard.png?raw=true)
 
		_New Inbound Rule Wizard_

	1. In the **Action** page, make sure that **Allow the connection** is selected and click **Next**.

 		![New Inbound Rule Wizard(3)](images/new-inbound-rule-wizard3.png?raw=true)
 
		_New Inbound Rule Wizard_

	1. In the **Profile** page, leave the default values and click **Next**.

	1. In the **Name** page, set the Inbound Rule's **Name** to _SQLServerRule_ and click **Finish**

 		![New Inbound Rule Wizard(4)](images/new-inbound-rule-wizard4.png?raw=true)
 
	_New Inbound Rule Wizard_

1. Close **Windows Firewall with Advanced Security** window.

	> **Note:** Make sure the Named Pipes and TCP/IP protocols are enabled for the Server Instance. You can verify this by going to the SQL Server Configuration Manager and within SQL Server Network Configuration node check that these protocols' status are set to enable.
	>
	> Remember to restart the SQL Server instance after enabling a protocol.

1. Close the **Remote Desktop Connection**.

<a name="Exercise3" /> 
### Exercise 3: Deploying a Simple MVC4 Application ###

In this exercise, you will learn how to deploy a simple ASP.NET MVC4 application in the IIS of the Azure Virtual Machine you have previously configured.

>**Note:** To make this solution highly available, you need to configure the SQL Servers in an availability set and set up SQL Server Mirroring between the instances.

<a name="Ex3Task1" />
#### Task 1 - Deploying a Simple MVC4 Application ####

In this task, you will deploy the MVC4 application to the IIS VMs.

1. In the Azure Portal, Click **Virtual Machines** on the left menu.

1. You will see a list with your existing VMs. Select the first one you created in Exercise 1 and click **Connect**. If you used the proposed name, this VM's should be named **iisvm1**.

1. You will be prompted to download the remote desktop client. Click **Open** and log on using the Admin credentials you defined when creating the VM.

1. You need to install **.NET Framework 4.0** before deploying the MVC4 application. In order to do that, you will enable downloads from IE update **Internet Explorer Enhanced Security Configuration**.

	1. In the Azure VM, open Server Manager from **Start | All Programs | Administrative Tools**.

	1. In the **Server Manager**, click **Configure IE ESC** **within Security Information** **section**.

 		![Internet Explorer Enhanced Security(3)](images/internet-explorer-enhanced-security3.png?raw=true)
 
		_Internet Explorer Enhanced Security_

	1. In the **Internet explorer Enhanced Security** dialog, turn **off** enhanced security for **Administrators** and click **OK**.

 		![Internet Explorer Enhanced Security](./images/Internet-Explorer-Enhanced-Security.png?raw=true "Internet Explorer Enhanced Security")
 
		_Internet Explorer Enhanced Security_

		> **Note:** Modifying Internet Explorer Enhanced Security configurations is not a good practice and it only for the purpose of this particular lab. The correct approach would be to download the files locally and then copy them to a shared folder or directly to the VM.

1. Now that you have permissions to download files, open an **Internet Explorer** browser session and navigate to [http://go.microsoft.com/fwlink/?linkid=186916](http://go.microsoft.com/fwlink/?linkid=186916). Download and install **.NET Framework 4.0**.

1. Minimize the **Remote Desktop Connection**, and open the **Web.config** file located in **AzureStore** folder within **Source\Assets** folder of this lab. At the end of the file, replace the connection strings placeholder with the name of your SQL Server (by default, is the VM's name).

	<!--mark: 1-4-->
	````XML
	<connectionStrings>
	    <add name="AdventureWorksEntities" connectionString="metadata=res://*/Models.AdventureWorks.csdl|res://*/Models.AdventureWorks.ssdl|res://*/Models.AdventureWorks.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=[ENTER YOUR SQL SERVER NAME];initial catalog=AdventureWorks2012;Uid=AzureStore;Password=Azure$123;multipleactiveresultsets=True;App=EntityFramework&quot;" providerName="System.Data.EntityClient" />
	    <add name="DefaultConnection" connectionString="Data Source=[ENTER YOUR SQL SERVER NAME];initial catalog=AdventureWorks2012;Uid=AzureStore;Password=Azure$123;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
	</connectionStrings>
	````

1. Go back to the **Remote Desktop Connection**. Once **.Net Framework 4.0** installation finishes, open **wwwroot** folder located at **C:\inetpub\** and copy the MVC4 application located in **AzureStore** folder within **Source\Assets** folder of this lab. To do this, copy **AzureStore** folder (**Ctrl + C**) and paste it (**Ctrl + V**) in the VM's **wwwroot** folder.

 	![wwwroot folder](./images/wwwroot-folder.png?raw=true "wwwroot folder")
 
	_wwwroot folder_

1. Open the **Internet Information Services (IIS) Manager** from **Start | All Programs | Administrative Tools**.

1. In the **Connections** pane, expand **Default Web Site** within your IIS Server's node. You will see the **AzureStore** folder you copied in the **wwwroot** folder.

 	![IIS Manager](./images/IIS-Manager.png?raw=true "IIS Manager")
 
	_IIS Manager_

1. Right-click **AzureStore** folder and select **Convert to Application**.

 	![IIS Manager Convert to Application](images/iis-manager-convert-to-application.png?raw=true)
 
	_IIS Manager - Convert to Application_

1. In the **Add Application** dialog, click **OK**.

 	![Add Application dialog](./images/Add-Application-dialog.png?raw=true "Add Application dialog")
 
	_Add Application dialog_

1. Finally, select the **Application Pools** node and double-click **DefaultAppPool** application pool.

 	![Updating Default Application Pool](./images/Updating-Default-Application-Pool.png?raw=true "Updating Default Application Pool")
 
	_Updating Default Application Pool_

1. In the **Edit Application Pool** dialog, change the **.Net Framework** version to **v4.0** and click **OK**.

 	![Editing Application Pool](./images/Editing-Application-Pool.png?raw=true "Editing Application Pool")
 
	_Editing Application Pool_

1. Now, the **DefaultAppPool's** .Net Framework should be **v4.0** instead of v2.0.

 	![Updating Default Application Pool](./images/Updating-Default-Application-Pool.png?raw=true "Updating Default Application Pool")
 
	_Updating Default Application Pool_

1. Close the **Internet Information Server (IIS) Manager** window.

1. Close the **Remote Desktop Connection**.

1. Repeat this task in the second VM you created in **Exercise 1 -Task 1**. If you used the proposed name, this VM should be named **iisvm2**.

<a name="Verification" /> 
#### Verification ####

In this task, you will test the Azure Store MVC4 application you deployed in the previous task.

1. In your local machine, open **Internet Explorer**.

1. Go to http://[**YOUR-SERVICE-NAME**].cloudapp.net/AzureStore. The Service Name is the one you used when creating the IIS VMs (you can also check it in the Azure Portal, within VM's dashboard).

 	![MVC4 Application running in the Web Farm](./images/MVC4-Application-running-in-the-Web-Farm.png?raw=true "MVC4 Application running in the Web Farm")
 
	_MVC4 Application running in the Web Farm_

1. In the **Search** box, write _Classic_ and click **Search**. It will show all the products that have a product name that match the search criteria.

 	![Searching Products(2)](images/searching-products2.png?raw=true)
 
	_Searching Products_

--- 
