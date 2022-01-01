Udacity Azure Program

-------------------------------------------------------------------------------------------------
Lesson 2: Azure Cloud Capabilities
	New Terms:
		VM - An Emulation of a computing system that uses software instead of Physical systems to run apps/SW.
		DB_Server - Uses database application to provide Database services to Web applications, Web servers or other services.
		Email- servers - Used for handling delivery of email through a network with an email software.
		
	How to create a web server:
		1 Login to the Azure portal.
		2 Go to Services and select Viretual machines.
		3 Provide required information.
		4 Optional information.
		5 Review + create.
			Created VM for Test
				Name: Web01
				Username: iisadmin
				password:worldchoas32!
				
	Azure HA Details:
		1 Avalablity Zones - (Unique physical locations within Azure Cloud. Each can contain more than one physical data center.)
		2 Vm Scale Sets.
		3 Availablity Sets.
			Fault domains.
			Update domains.
		
	Load balancing:
		1 Application Gateway.
		2 Front door.
		3 Azure Load balancer.
		4 Taffic manager.
			
			How to create LoadBalancer:
			1	Search for Load Balancer and create a load balancer.
			2	For the load balancer here are the unique properties
			3	Resource group create a new one called Websiteloadbalancer
			4	Instance: weblb
			5	Public IP address: Create new and use the name loadbalancerIP = (196.168.16.8)
			6	IP Address Assignment: Static
			7	IPV6: No
			8	Availability zone: Zone-redundant

				Web Server 1:
					Virtual machine name: webAdmin
					Password: webAdmin345!
					Another Password: loginAdmin1!
				
				Web Server 2:
					Virtual machine name: webAdmin2
					Password: webAdmin345!
					
				Public IP address of the LoadBalancer:52.236.152.46
	
	Azure APP Service: Just an http service in azure for hosting applications.
	
		Creating a DNN Web APP service:
			1	Login to the Portal and obtain credentials from the file labeled AzureCreds.
			2	In the Azure Portal, search and select for DNN Platform and fill out the fields as follows:
					App name: DNN(yourLastName)
						This has to be unique so if (yourLastName) is common you may need to add additional letters or numbers to make it unique
					Resource Group: Select an existing option or create a new one.
					Go to SQL Database Configure required settings and Name: DNNDB
					Go to Target server Configure required settings and enter the following:
						Server name: DNNdamilare
							Just like the App name this needs to be unique
						Server admin login: DNNadmin
					Password: Meets the password complexity requirements (dnndamilare#3)
					Select Allow Azure services to access server.
					Select Priceng tier.
			3	Deploy the resources.
			4	Click on Notifications (top right of the page as a bell icon) to see the resources through the Go to resource button.
			5	Click through the URL in the Overview section to open the site as an installation page. (May take some time to load.) Fill out the page as follows:
					Username: host
					Database Information:
						Database Type: SQL Server/SQL server express database.
						Server name: format of DNN(yourLastName).database.windows.net
							This will be the server name that you entered when you entered in the target server section(.database.windows.net)
						Database Name: DNNDB (or same as Step 2 on SQL Database)
						Security: User Defined
						Database Username: dnnadmin (or same as Step 2 on Target Server)
						Database Password: same as Step 2 on Target Server
						Run Database As: Select Database Owner.
			6	Wait for the new installation dialogue box to complete.
			7	Click on Visit Website and login
			8 	Click on Visit Website and login
			9	You will see the back end portal for DNN if at the top right you are logged in as Superuser Account.
			
	Service level agreements. (Aggrement between the data center and the consumers regarding uptime and allowed downtime).
		
