# Azure_Lessons

_A README file to help document my Azure services knowledge and lessons._

## Lesson 2: Azure cloud capabilities.

### New Terms:

    	VM - An Emulation of a computing system that uses software instead of Physical systems to run apps/SW.
    	DB_Server - Uses database application to provide Database services to Web applications, Web servers or other services.
    	Email- servers - Used for handling delivery of email through a network with an email software.

#### How to create a web server:

    	1 Login to the Azure portal.
    	2 Go to Services and select Viretual machines.
    	3 Provide required information.
    	4 Optional information.
    	5 Review + create.
    		Created VM for Test
    			Name: {Unique name for the VM}
    			Username: {Enter unique username}
    			password:{Enter a secure password}

### Azure HA Details:

    	1 Avalablity Zones - (Unique physical locations within Azure Cloud. Each can contain more than one physical data center.)
    	2 Vm Scale Sets.
    	3 Availablity Sets.
    		Fault domains.
    		Update domains.

### Load balancing:

    	1 Application Gateway.
    	2 Front door.
    	3 Azure Load balancer.
    	4 Taffic manager.

#### How to create LoadBalancer:

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

### Azure APP Service: Just an HTTP service in azure for hosting applications.

#### Creating a DNN Web APP service:

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

## Creating VPN Gateway.

Creating a VPN gateway which is needed in order to province connectivity between and on-premise environment and resources within Azure. This is useful in situations that you have Azure resources that you do not want to be accessible on the general Internet.

#### How to create a VPN Gateway

1. Use the Navigation Pane on the top left to navigate to the Virtual networks to create a new virtual network.
2. Select an existing resource group from the dropdown.
3. For the instance details:
   -  Name: {Enter a unique name}.
4. In IP Addresses there should be no conflict with the default but if you are using a pre-existing Azure account with already established networks you may run into conflicts.

   -  IPv4 address space: 10.0.0.0/16.
   -  Subnet name: default.
   -  Subnet address range: 10.0.0.0/24.

5. In Security, you will want the default settings with all the options (Bastion Host, DDoS Protection Standard, and Firewall) being Disabled.
6. Finish creating the virtual network.
7. Navigate to Virtual network gateways to click on Create virtual network gateway with the following values:
   -  Name:{Unique name}
   -  Region: Same region used for virtual network.
   -  Gateway type: VPN.
   -  SKU: {Unique name}.
   -  Generation: {Select generation of choice}.
   -  Gateway subnet address range: 10.0.255.0/27 same settings shown in Step 4.
   -  Virtual network: same as step 3.
   -  Gateway subnet address range:10.0.1.0/24.
8. For Public IP address.
   -  Select Create new.
   -  Public IP address name: vpnGW1IP
   -  Public IP address SKU: Basic.
   -  Enable Active-active mode: Disabled.
   -  Configure BGP: Disabled.
9. Finish creating the Virtual network gateway. This process can take 30-45 minutes for deployment.
10.   Access Notificaions to click on Go to resource.
11.   Notice in the Overview section that the IP address is the same as in Step 8.

### Net Security group (NSG)

It's a basic cloud based firewall. However, it won't have all the features of a traditional firewall. It's mostly a basic functionality to restrict access to given resource.

#### Use NSGs on these areas to limit exposure:

-  Securing traffic flow between:
   -  Applications to internet.
   -  Applications to applications.
   -  Applications to users.

#### Tools to create NSGs

-  Azure portal.
-  Powershell / Linux terminal.
-  Azure CLI.

#### Steps to create NSG (Using Azure portal)

1. Create NSG:
   -  Login to Azure portal.
   -  Go to Networking -> Network security group.
   -  Go into basics tab and define prameters(Subcription, Resource group, Name, Region).
   -  Finally, review and create.
2. Create Security rule.
   -  Within the Axure portal, go to NSG and select the NSG you need to modify.
   -  In the NSG menu bar go to inbound security rules or outbound security rules.
   -  Add values for the diffren applicable sections (Source, Source IP, Ports, destinations ...).
   -  Review and create.
