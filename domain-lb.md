#  Create a Classic Load Balancer

##  Select a load balancer type
### Elastic Load Balancing supports different types of load balancers. For this tutorial, you create a Classic Load Balancer.

### To create a Classic Load Balancer
### 1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/
### 2. On the navigation bar, choose a Region for your load balancer. Be sure to select the same Region that you selected for your EC2 instances.
### 3. On the navigation pane, under LOAD BALANCING, choose Load Balancers.
![image](https://user-images.githubusercontent.com/95855861/158735080-a35a98aa-3207-48df-8eed-fb945eaa844a.png)

### 4. Choose Create Load Balancer.
### 5. For Classic Load Balancer, choose Create.

## Define your load balancer
### You must provide a basic configuration for your load balancer, such as a name, a network, and a listener.
### To define your load balancer and listene
### 1. For Load Balancer name, type a name for your load balancer.
![image](https://user-images.githubusercontent.com/95855861/158735380-6d838a7d-d3e0-476c-b1d0-a50d2eb2da44.png)

### The name of your Classic Load Balancer must be unique within your set of Classic Load Balancers for the region, can have a maximum of 32 characters, can contain only alphanumeric characters and hyphens, and must not begin or end with a hyphen.
![image](https://user-images.githubusercontent.com/95855861/158735489-7b29cff5-c030-40a8-80b6-55f2e62aa4cb.png)


### 2. For Create LB inside, select the same network that you selected for your instances: EC2-Classic or a specific VPC.
### 3. [Default VPC] If you selected a default VPC and would like to choose the subnets for your load balancer, select Enable advanced VPC configuration.
### 4. Leave the default listener configuration.

### 5. [EC2-VPC] For Available subnets, select at least one available public subnet using its add icon. The subnet is moved under Selected subnets. To improve the availability of your load balancer, select more than one public subnet.
### You can add at most one subnet per Availability Zone. If you select a subnet from an Availability Zone where there is already an selected subnet, this subnet replaces the currently selected subnet for the Availability Zone.
### 6. Choose Next: Assign Security Groups.
![image](https://user-images.githubusercontent.com/95855861/158735574-feb38f98-871f-4a54-9c3f-2146de92261f.png)

## Assign security groups to your load balancer in a VPC
### If you selected a VPC as your network, you must assign your load balancer a security group that allows inbound traffic to the ports that you specified for your load balancer and the health checks for your load balancer.
### To assign security group to your load balancer
### 1. On the Assign Security Groups page, select Create a new security group.
### 2. Type a name and description for your security group, or leave the default name and description. This new security group contains a rule that allows traffic to the port that you configured your load balancer to use.
### 3. Choose Next: Configure Security Settings
### 4.  Choose Next: Configure Health Check to continue to the next step.
## Configure health checks for your EC2 instances
### Elastic Load Balancing automatically checks the health of the EC2 instances for your load balancer. If Elastic Load Balancing finds an unhealthy instance, it stops sending traffic to the instance and reroutes traffic to healthy instances. In this step, you customize the health checks for your load balancer.
### To configure health checks for your instances
### 1. On the Configure Health Check page, leave Ping Protocol set to HTTP and Ping Port set to 80.
### 2. For Ping Path, replace the default value with a single forward slash ("/"). This tells Elastic Load Balancing to send health check queries to the default home page for your web server, such as index.html.
### 3. For Advanced Details, leave the default values.
### 3. Choose Next: Add EC2 Instances.
##  Register EC2 instances with your load balancer
### To register EC2 instances with your load balancer
### 1. On the Add EC2 Instances page, select the instances to register with your load balancer.
### 2. Leave cross-zone load balancing and connection draining enabled.
### 3. Choose Next: Add Tags.

# Migrate Domain and DNS to AWS Route 53
## Step 1: Create AWS Route 53 Hosted Zone
### First we must create the Hosted Zone in Route 53. This is so we can get our Amazon Name Servers for use in a later step. Go to Route 53 in the AWS console, then click Hosted Zones on the left column, then Create Hosted Zone:
![image](https://user-images.githubusercontent.com/95855861/158735789-74b0d5d7-2f53-4b5a-8102-dad51d34206a.png)

### Enter your domain name that you wanted to transfer over, select Public Hosted Zone for type, add tags if applicable, then click Create Hosted Zone:
### Should see a green message stating successfully created
## Step 2: Export Zone File Information from GoDaddy and Import into Route 53:
### This part is performed in your GoDaddy account: We need to export the DNS Zone File. A DNS Zone File is a plain text file of your current DNS configuration, it contains all your current records and their values (A, CNAME, Alias, etc).

### Select the file for the version of your operating system:
### Copy everything to your clipboard:
### Switch to your AWS Console: Back in the AWS Route 53, click your Hosted Zone we created in Step 1, then click Import Zone File:
### Paste the contents from your clipboard (should be the DNS Zone File contents) then click Import:
### Error Catch: If you did not remove the SOA and NS records from the downloaded Zone File you will get the following red text error. This error happened because the SOA records were created when we made the Hosted Zone in AWS during the very first step, and we have new nameservers with AWS versus existing ones in GoDaddy.

### You will need to delete the SOA data below the ; SOA Record (highlighted below) and the ; NS Records then click Import:

### Additional Information: SOA stands for Start of Authority record. It contains the following information:

### >The name of the server that supplied the data for the zone.
### >The administrator of the zone.
### >The current version of the data file.
### >The default number of seconds for the time-to-live file on resource records.
### NS Records aren’t needed and can be deleted as highlighted:
### Once you get the SOA Record and NS Records deleted, you should get a green message stating successfully created. Scroll through to make sure all your records look correct. Good opportunity to delete any that you don’t want anymore:
### On this same screen expand the Hosted Zone Details pane and copy the Name Servers to a Notepad so we can copy/paste them in the next steps:
## Step 3: Update Name Servers in GoDaddy:
### Now that Route 53 has all of our DNS records we can switch the name servers to the AWS Name Servers. This will let Route 53 start serving DNS requests instead of GoDaddy.

### GoDaddy Console: Back in the GoDaddy DNS console there is the option to Change the default nameservers, click Change:
### Click Enter my own nameservers (advanced):
### You will first change the drop down menu from Default to Custom, then on each line copy/paste the Name Server from Route 53 that we copied earlier. You will have to click Add Nameserver so you can get all your entries on there. Once you get the information entered click Save:
### This is a high impacting risk change, meaning the global DNS records external users use to access your domain will be changed from GoDaddy to AWS. Highly recommend you do this after hours and know that when dealing anything with DNS there is a time sync (TTL).

### If you are good with the possible risks to DNS check the box to accept then click Continue:
### If you have GoDaddy Ownership Protection enabled you should see a purple banner stating you should have a email or Two Factor Authentication request to confirm the change of your nameservers.
### Once you confirm the change or if you didn’t have GoDaddy Ownership Protection enabled, the DNS console it will state they cannot display your DNS settings because they no longer manage them and now AWS Route 53!
### Now We Wait: Since this is DNS we are working with we have to wait for the TTL (Time To Live) to expire and global DNS servers start refreshing records. To check this externally you can use a third party website like https://www.whatsmydns.net/

### Enter your domain name, change the drop down to NS (Nameserver) then click Search, this will show you what DNS servers around the globe are reporting back with as they sync:


