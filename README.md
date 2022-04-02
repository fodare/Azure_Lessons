# Azure_Lessons

A.MD file to track knowledge gained practising Azure Lessons

## Lesson 1: Introduction to designing infrastructure and managing migration

### Azure Models

There are 3 models to Azure cloud services:

   1. Infrastructure as a service(IaaS) - Instruments to run your applications.
   2. Platform as a service(PaaS) - An environment to host your applications.
   3. Software as a service(SaaS) - Applications users can utilize.

### Criteria for a successful cloud migration

- Identify and involve key stakeholders
- Create strategic plan
- Calculate total cost of ownership
- Discover and assess on-premises resources
- Training
- Plan for decommission on-premise resources not in use
- Plan for optimization after migration

### When not to migrate to the cloud

- You have recently heavily invested in you application development that is not cloud ready
- You do not have a clear understanding of what will be migrated
- You have not established a clear method to determine a successful implementation
- You have not identified the cost related to the migration or the performance implications
- You are not able to answer the simple question, Why migrate to the cloud
- Do you have compliance limitations

### When to migrate

- Most of the business applications are cloud ready.
- You have identified the total cost of the move as well as the cost benefits.
- Age of your existing physical devices are reaching EOL
- Your company requires remote and mobile access.
- You have a clear strategic plan that includes all of your business applications and their dependencies

## Lesson 2: Azure cloud capabilities

### How to create VM

There are multiple automation tools that help create VMs on Azure but below is an overview of how to create a virtual machine from Azure portal.

1. Login into the Azure portal - portal.azure.com
2. Go under "Services" and select "Virtual machines". And enter the minimal requires information
   - Virtual machine name
   - The resource group
   - Subscription that you are utilizing
   - Region
   - Virtual machine size
   - Credentials
   - OS type, generation / image (depending on your desired OS)
   - Inbound port rules (depending on the VM)
   - Disk option
   - Networking options
3. Click "Review + Create"
4. Once the deployment is completed, try to connect to the VM depending on the connection port selected.

### Setting up server manager

To add role. You can either do it from "Add roles and features" or "Manage" (Top right corner)

1. Select the option "Add roles and Features".
2. Click "Next" and keep it as the default settings role-based or feature-based installation.
3. Click Next and keep Server as {Server name}.
4. Scroll down to the Web Server IIS. Click Add the feature and click Next.
5. Continue till you get "Install".
6. Once successfully installed, click Close.
7. Go back under Tools, click Internet services IIS manager to access the installed web service.
8. To add a custom page, right-click Sites, and then switch over to content view, under my website.

### Publishing a custom Web page

1. Access the file explore and navigate to the directory that has your web content. Click PC, click Windows
click Inetpub, click wwwroot.
2. Right-click in the empty area, and create a new text file.
3. Type in "Rendering index file from server 1" as an example.
4. Click File, Choose Save As "index.html", and click Save.
5. Go back to the server inbound port rule and add HTTP/HTTPS connection, so you can access the application/webpage.
   - Source: Any.
   - Source port: Any.
   - Destination : Any.
   - Destination port ranges: 80(HTTP), 443(HTTPS).
   - Protocol: Any.
   - Priority: {enter desired value}.
   - Name: {Enter desired name}.
6. Once the rule is created, try to access the webpage via "<http://{Server> IP address}"

### High Availability

Allows the capabilities of the Azure resources to be accessible during multiple different issues:

- Scheduled/Unscheduled maintenance.
- Natural disaster.
To implement high availability, we can use different tools such as:
- Availability Zones.
- VM Scale set.
- Availability sets.
- Load balancer.

### Load balancing

Helps distribute connection traffic across connected VMs
Types:

- Application gateway - Routing based on other or additional attributes outside just the HTTP request. Through the application gateway.
- Front door - Handles encryption as well as the decryption request.
- Azure Load balancer - Evenly distribute traffic amongst connected VMs.
- Traffic manager - DNS based load balancing the allows distribution of traffic for application across the global Azure region.

### creating a load balancer
As an example create  multile VM (Ubuntu(Apache web server) / Windows with IIS). For ubuntu, once the VM are created ssh into the machine and type in the code below to create and start an Apache webserver. 

- sudo apt update
- sudo apt install apache2 -y
- sudo ufw allow 'Apache'
- sudo systemctl start apache2

Once this completed, copy and search the public IP address of the VM on your loacl machine web browser, you should see an Apache2 ubuntu default page. It is as well possble to create the loadbalancer first then the VM's later

Now to create the load balancer:
From Azure portal search for loadbalancer and fill in necessary information:
- Instance deatils: 
   - Name: {Desired load balancer name}
   - Region: Same regions as the VMs
   - SKU: Bacic. Standard needs a few diffrent congigutation fields.
   - Type: Public
   - Tier: Regional
- Frontend IP config 
   - Name: {Enter desired name}
   - Public IP address: Add a new public IP address, with these values.
      - Name: {Desired name}. 
      - Assignment: Static
- Backend pools:
   - Name: {Desired name}
   - Virtual network: Select the existing virtual network in which VMs are running.
   - Assiociated to: VMs. Add the VMs to the current pool.
- Inbound rule >> Load balancing rule
   - Name: {Desired name}
   - Frontend IP address: Choose the one you have created above.
   - Backend pool: Choose the one you have created above
   - Protocol: TCP 
   - Port and backend port: 80
   - Helth probe: Create a new one
      - Protocol: HTTP 
      - Port: 80
      - Default path: /
      - Interval: 5 seconds
      - Unhelthy threshold: 2 consecurtive failures

Once your Load Balancer is deployed, copy its public IP address and paste it into the browser. If everything is set up correctly, you should see the web server from one of the VMs in your backend pool. Suppose, if you see the swever1 web server, delete/stop that VM from the Virtual Machines service manually to observe if the Load balancer redirects the traffic to the other servers.

### Creating SQL database 
To create an Azure SQL service. You have to create a SQL db using Azure portal. 
Search and open Azure SQL service then select SQL database. 
Basic: 
   - Resource group: chose existing or create a new one.
   - Database name: {Enter desired name}.
   - Server - Create new one.
      - Server name: {Desired server name}
      - Server location: Choose closet region to you/ where your Web app is hosted.
      - Authentication method: Use SQL authentication.
      - Server admin login / Password: {enter desired credentials}
   - Networking: 
      - Network connectivity: Public endpoint.
      - Firewalls rules: Allow azure services and resources to access this server.
      - Connection policy: Defult
   - USer SQL eleastic pool: {Yes / No}.
   - Compute + stroge: Depend on choice.
   - Backup stroge redundacy: Locally -redundant backup storage.
   - Review and create
Once the resource is created, you would find the connection string from the resource overview page.

### Best practice design principles
When designing Azure solutions always take into considreation:
- Self-healing capablity.
- Redundancy.
- Scale-out.
- Evolution.
- Operations.
- Managed service.
- Business needs.

Best practice design styles: 
- Big computer.
- Big data.
- Event- driven architecture.
- Micro services / N-Tier application(Try to break the application into smaller modules)


### Designing a network solution in Azure.
When designing a networ ksolutions for azure resources, you will need to consider few diffrent factors such as: 
- Latency.
- Security.
- Speed.
- Redundancy.
Tools used to communicate between On-premise and Azure: 
- VNET/VNET peering
- Gateway
- VPN
- Network watcher

Tools below determinse the method: 
- Using VNTE
- Virtual Neywork endpoints
- VNET peering 
- Private Link.

Blow are some waay to comminicate with On-premise resources: 
- Point - site VPN 
- Site - Site VPN
- Azure ExpressRoute

Diffrence between policy based VPN and route based VPN
|Policy based VPN|Route based VPN|
|---|---|
| Uses a combina tion of prefixes from both networks to define how traffic is encrypted/decypted through IP tunnels| Use any-any traffic selectors and routing/forwarding tables to direct traffic to diffrent IPSec tunnels|
| Allows for multiple VPNs via singlr Vnet Gateway | If device supports it this better than policy based  |
| Does not support VPN diagnotics in Azure | Can perfrom VPN diagnotics in Azure|

### Creating route based VPN

1. Create a virtual netwotk
   - From azure portal
      - Search "Virtual networks"
      - Click Add
      - Select or create new  resource group
      - Enter name
      - Select region
   - IP Address
      - Click default subnet and change name to {Desired name}
      - Security: All disabled
      - Review and create
   - Create virtual network gateway
      - Search and select "Virtual network Gateways".
      - Click 'Crate virtual Network Gateway" or +Add.
      - Select region and enter name.
      - Select generation.
      - select vitual network
      - Enter public IP address name
      - Review and create
   - Configure VPN
      - Go to resource
      - Under settings, Click 'point-to-point configuration
      - Click 'Configure now'
      - Enter address pool {xxx.xxx.x.x/xx}
      - Select tunnel type IKE2 ans SSSTP(SSL)
      - Root certificate -> Azure certificate
      - copy/paste daa into textbox
      - Click "Download VPN clinet" 
      - Open and Run the executable in order to install VPN client.
   - Poral screen
      - Select the VPN you just created and click "Connect"
   - Agent windows: 
      - Click "connect" 

### Network Security Group (NSG)
- It's a basic cloud based firewall. 
- It doesn't have all the features of a tradintional firewall. 
- Has the basic functionality to restrict access to your resources.

You can use NSG on to limit resource exposure when: 
- Securing traffic flow between:
   - Application - Intenet 
   - Application - Applications

Tools to create NSGs
- Azure Portal
- Powershell 
- Azure CLI

Creating NSG from Azure portal: 
- Go to networking -> Network security group
- Go into Bascis Tab and define parameters(Sunscription, Resource group, Name, Region)
- Review and create -> Create

Creatting security rule:
- Find and delect the NSG you'd like to modify.
- From the NSG menu bar, click on inbound security rules or outbound security rules.
- Add values for the diffrent applicable sections(Source, Source IP, Port, destination, etc.)
- Click Review + Create -> Create

Creating a Network Endpoint to secure Azure storeage Account. 
Limit network access to Azure resources to a virtual network subnet through a network service endpoint.

Sercuring Azure storage account via endpoint: 
- Create Vnet with subnet.
- Enable endpoint.
- Restrict network access of subnet.
- Restrict network access to resource (Azure storage account).


## Lesson 3: Designing for Backup and Recovery

In order to create a plan for your on-premise enviroment, you might need to define the requirements. Below are details the you will need to be mindful of
- Identify workloads and usage.
- Plan for usage patterns.
- Establish availablity metrics
   - Mean time to RecoveryI(MTTR)
   - Mean time between Failures (MTBF)
- Establish revovery metrics
   - Recovery Time Objectives(RTO)
   - Recovery Point Objective(RPO)
- Determine worload availablity targets.
- Understand SLA

### Best practice for implementing a Backup plan
- Perform a failure mode analysis.
- Have a redundancy plan.
- Utilize resiliency strategies when applicable.
- Make availablity a considration when you are designing a solution.
- Have a plan and documentation as it relates to how you store backup andreplicate data.

### Settiings up Azure Backups
- Azure portal provides a Wizard. 
- Direct you to the appropriate components needed to be dewnloaded and deployed.
- Identifying your backup goals and help you pick the correct protection.

Adavantages:
- Full felxiblity for when backups are taken (Scheduled backups or manually run).
- Support for VMs on both Hyper-V and VMware.
- No special licensing(1.e, System center).

Disadavatges: 
- Currently unable to backup oracl workload.
- Always requies live Azure subscription.
- No support for tape backups.

Protection Plan for An On-premise Enviroment.

### Creating backup for your Virtual Machines
1. From Azure Portal find your target VM.
2. Navigate to Operations and click on Backup.
3. Create a new REcovery Services Vault: 
   - Name: {Enter desired name}.
   - Notice the Backup Frequency is dialy at a specific time. This can be modified in the backup policy.
4. Enabale Backup, this may tak some time to enable (3 - 10 min).
5. Manually run the backup by navigating to Virtual Machine > {Target machine}>Operations>Backup and select backup now.
6. Leave the Retain Backup Till as the defualt value and continue.

### Creating a new REcovery Services Vault for a Load balanced Web server
1. From the Azure Portal navigate to Recovery Services Vaults and click "+New"
2. These will be the properties for the Recovery Service vault.
   - Click through to create.
3. Once depeloyment is complete,  click Go to resource.
4. In Getting started Click on Backup and below are the properties: 
   - Where is your workload running?: Azure
   - What do you want to backup?: Virtual Machine.
   - Click Backup to complete.
5. Use an existing Backup Policy (DefaultPlicy) and click Add.
6. Confirm that you see both Virtual machines for the load balancer.
   - If you do not then this means that the Recovery Services Vault is in a different Region then the Virtual Machines. To fix this do the following:
      - Find the location of the VM.
      - Create a new Recovery Services vault in the location.
7. Finish creating the vault.

### Managing Backup Policy
1. Navigate to the web loadbalancers Recovery Services Vault, Manage and click Backup policies.
2. Click on DefaultPolicy.
3. Click on Modify with the following properties:
   - Set the Time to {desired time range} after you create the plolicy.
   - Timezone: Set it to your own time zone.
4. At the top, click Save.

### Azure Site Recovery
Is a tool the has disaster recovery as a service. With Azure site recovery, you do not have to worry about having a seperate facility that you maintain for the pueposes of having another physical site to protect against disaster occuring at your main site.

### Restore a VM from Backup

Before we can start with recovery, we will need to create a storge account.
1. From Azure portal navigate to the 3 line icin in the top lef to select Storage accounts.
2. Click Create storage account and enter the following properties:
   - Resource group resource associated with target VM
   - Storage account name: Stroage(LastName) has to be unique.
   - Region: Should be automatically set to target VM region
   - Performance: standard.
   - Account Kind: stroage V2
   - Replication: Locally redudant stroage(LRS)
3. Finish creating the stroage account.

Shutting down VM to be Restored.
Now we cango to the recovery services vault to start the revoery process. In order to do this, the VM that is being restrored VM must be shut down before you start the process.
1. In the Azure Portal go to Webserverbackups resource group and select {Target VM}.
2. Under Protected items select Backup items.
3. Under BACKUP MANAGEMENT TYPE, click on Azure Virtual Machine.
4. Confirm that the backup has already completed by looking in Latest restore point.
5. Click on the 3 dots on the far right to access the context menu and click Restore VM.
6. In the Restore Virtual Machine, under the field for Restore pointclick on Select.
7. In Select restore point, you should see at least one restore point and select the most recent restore point and click OK to select the restore point.
8. Returning to the Restore Virtual Machine you will have these options:
      - Restore configuration: Replace existing
      - Staging Location: Select the already created stroage account.
9. Finish the restore by clicking rstore.
10. Check the progress of the restore by going to {servervault}>Monitoring>Backup Jobs. There is a backup running as well because Azure Backup takes a snapshot of the VM before replacing the disk.
11. Refresh periodcally for th status to update.
12. After the virtual machine is successfully restored you should be able to start it and login.

### On-premise Backup steps

1. Login to the Azure portal.
2. Create a recovery services vault.
3. Setup the stroge replication (Locally or geo-redundant)
4. Download software packege from Azure portal and install on premise.

### cloud VM Backups steps
1. Login to the Azure portal
2. Go to VM and under operations, select Backup
3. Create a recovery services vault.
4. Select the option to Backup now.

### Restoring files from Backup
Set up a Simulation to Recover index.html
   - Login to server web01lb via RDP.
   - Delete the index.html file.

Restore index.html

   - From Azure Portal navigate to Backup items for the webloadbalancer Recovery Services.
   - Click on Azure Virtual Machines and observe that both load balance web servers are present.
   - Access the Context menu for web01lb and select File Recovery.
   - Generate the script and password and then download the script onto you local computer
   - Run the executable and it will open your command prompt on the local system asking for a password.
   - Copy and paste the password from the Azure Portal in the field Password to run the script.
   - Upon successfu connectiion, the PowerShell prompt will show Connection succeeded!.
   - In your file browser you will see two drives mounted on your local system (System Reserved (E:) and       Windows (F:)).
   - Navigate to the Windows Folder and navigate to the path F:\inetpub\wwwroot and see the custom index.html file.
   - Copy the iisstart.htm file back to your Web Server.
   - Finish the process by clicking the Unmount Disks.
   - Quit (Q) the PowerShell
   - In the Azure Portal it will show the status Unmount successful.

## Lesson 4: Migration

The section covers a short intro into the steps involved when planning to migrate into Azure cloud. Azure also numbers of tools that can be used to help simply migrations. Example is Azure migrate, DB migration tools e.t.c.

The main steps are: 
   1. Assess - Analyze and review the onppremise enviroment.
   2. Migrate - Decide what resorces will be migrated from on-premise into Azure cloud and also use pricing tools and usage hstory to determine the appropriate cloud products.
   3. Optimize- Make the necessary adjustments to ensure the bisuness applications are running at their optimal level.
   4. Monitoring - Gathering logs and data associsted with health and performance of resources migrated into Azure cloud to helo determine if there are any underutilized and potential for further adjustments to help reduce costs.

Assess stage: 

- Idetifying servers, apps and resources to be migrated.
- Inventory of on-premises cpmputing resources and dependecies.
- Developer a map of how all the diffrent pieces communicates and interact with one another.
- Done with service map

Best migration options depending on the scenario: 

- Rehost - minimal effort.
- Refactor - Optimozation. 
- Rearchitect - Significant changes. 
- Rebuild - starting from scracth. 
- Replace - Alternatives.

Migration strategy:

Do make sure all stakeholders are well informed and aware of migration steps.

Below are some final analysis before migrations. 
- Decide on either Active directory / Azure Active Directory.
- Make a list of resources that will be migrating and remain on-premise.
- Get a projected timeline of on-premise resources will be migrated in the future.
- Migrate as many applicatins to PaaS / SaaS to minimize patching and updates.
- Automate backups strategy for both on-premise and cloud resources with one vendor.

Migration stage: 

- Destination systems and services on Azure should be finalized.
- An alternative to having resources creates in Azure before migrations
   - ASR (Azure site recovery. Required resources created for you in Azure enviroment)
   - Azure Data migrations service
- Start small- get to know all of the tools before migrating Advanced solutions.
- Do not decommission on-premise infrastructure too soon.

Post Migration checklist

- Review security settings of VMs after migration. 
- Backup schedule that coordinates with your workload. 
- For additional protection, use ASR to replicate your VM's to another region.

Monitor:

Azure monitor can be utilized to provide health and perfrmance details. It can be useful when: 
- Having difficulties reviewing raw application data.
- When certain data might not be relevant to your needs.
- Alerts to notify you when recommended tresholds for resources are breached. (e.g Autoscale matric, CPU usage)

## Lesson 5: Automation

This chapter we would discuss how to utilize ARM templates and other automated tools to help speed us deployment of resources. Using ARM templates has multiple advantages over manual deployment procedures. Some of the advanatages are: 
- Consistency within deployments.
- complex deployments are made easy.
- There is a reduction of errors.
- Easily reused.
- Faster deployment as it requires human interactions.

### Elements of ARM Template: 
- Schema - JSON based entries.
- ContentVersion - Helps track versions of templates.
- Parameters - Useful to store values before the deployment procedures.
- Variables - Define expressions that would be reused trought the deployment.
- Functions - User defined functions.
- Resources - Define resources that are being defined or updated.
- Outputs - Holds outputs logs / data after the deployment is done. Can be used to validate a deployment.

Sample steps to delpoy a VM using ARM templates:

From Azure portal, search deploy and select Deploy a custom template. You would be provided with sample preconfigured templates for a linux Virtual machine, Windows Virtual machine, Create a Web app, SQL db etc... You can select one of the sample template and enter details needed for the new VM so you can download the ARM templates / simply deploy the templates from Azure portal.

Example: Deploy a storage account using an ARM template

1. From Azure portal navigate to storage accounts. 
2. Create a new storage account using the + New button with the following settings: 
   - Resource group: Select an existing resource group / create a new one.
      - Stroage-rsg is included with a series of letters/numbers.
   - Storage account name: {Enter desired name}
   - Location: {Enter desired location}
   - Performance: {Standard}
   - Account kind: StorageV2 (general purpose v2)
   - Replication: Read-access geo redundant storage (RA-GRS)
3.  Click Download a template for automation and click Download on the next page.
4.  Once downloaded, unzip the tempalte.zip and there will be 2 JSONs parameters.json and template.json.
5.  Open the parameters.json in a text editor.
6.  Find the following "storageAccountName": and change the value to null to enable a unique value to be added for each deployement.
7.  Save the parameters.json
8.  Returning to the Azure Portal navigate to a Services called Deploy a custom template.
9.  Click on Build your own template in the editor.
10. Click Load file and select template.json and Save.
11. Click Edit parameters and select parameters.json and Save.
12. In the TODO you will enter the following:
      - Resource group: Storage-rsg
      - Stroage Account Name: {Enter desired name} This should be a unique name
13. Follow the rest to deploy your new storage account using the template.

### Desired State Configuration (DSC)
Can be used to automatically check for required roles/softwares in the virtual machine and install then if needed. This is useful to help make the configurations of related virtual machines more uniform

Elements of DSC script: 
- Configuration block. 
- Node block.
- Resource block.

DCS requirement:
- Automation account
- Windows, Linux
- VMF5.1 installed. 

### Azure policies

Azure policy can be used to automate and enforce organizations / business requirements. It's useful tool to help prevent users from performing certain actions that are against company/industry requirements. In addition to restricting specific actions, Azure Policy also provides a compliance report providing a list of resources that are not compliant. This report will be useful for auditing purposes.

Sample: create an Azure Policy that will prevent users from creating Virtual Machines that are not listed in the approved VM sizes. 

1. From Azure portal. Navigate to plicy and enter the following: 
2. Navigate to plicy and enter the following:
 - Scope select the ... to change the scope to desired scope.
 - Subscription: (default)
 - Resource group: Stagging
3. Finish the selection of the policy.
4. Find the policy with the Name of allowed virtual Machine SKU sizes.
5. Click on Allowed Virtual Machine sku sizes to see the defination as code for the policy.
6. Review the compliance report which should be at 100%.
7. Notice if the provisioning when you try to create a VM in the staging resource group with a Size for a B series, E series, or most other sized Vms.
 - You should see an error message > Resource (Name) was disallowed by policy (Code: requestDisallowedByPolicy)
   - Policy: Allowed virtual machine size SKUs
8. Try creating a VM higher than the one defined in the policy. This should fail.