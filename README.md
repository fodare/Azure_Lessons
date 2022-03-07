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