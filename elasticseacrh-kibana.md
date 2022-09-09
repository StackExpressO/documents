# 1. Creat an Amazon Elastic Search Domain.
  ### > Under AWS Services - Go to "Amazon OpenSearch Service"
  ### > Choose Create a New Domain
  ### > Give your Domain a name.
  ### > Choose your elastic Search Version. The concept remains the same but levels of functionalities get added with the latest versions.
  ### > Choose Deployement type out of 
  ###   - Production
  ###   - Development and testing
  ###  - Custom
  ### > Choose whether you wih to enable Custom endpoint & Auto-tune
  ### > Choose your instance type for your  nodes and also the number of nodes that you wish to have.
  ### > Select your EBS volume type. General Purpose would be the cheapest, Provional IOPS is a higher capacity and a higher IOPS Volume. 
  ### > Select the EBS storage size
  ### > Choose dedicate Master node, It basically improves the stability of your Domain.
  ### > If you wish to take a snapshot, you can configure the timing detail over here and this would be automated in nature.
  ### > Under Networking , Choose the VPC and the specefic subnets along with the security groups.
  ### > You can allow Amazon cognito authentication as well just to allow authentication for Kibana.  
  ### > Assign the IAM Roles accordingly.
  ### > Chooses whether you require an "Use AWS owned KMS key" or choose a different AWS KMS key.
  ### > Then select Create to create your AWS Elastic Search Domain.
  ### > You can allow Amazon cognito authentication as well just to allow authentication for Kibana.
**Once created you can view you domain overview, instance health , Cluster Health including the details of your nodes.**

**Now , we have to connect our Elastic Search domain.
So we have to access this domain from one of the Instances which are in the same VPC as the domain is in.
If not create an instance in that particular VPC and within that ,preferably a windows one.
Inside that open google chrome.**

## Now to connect to the elastic search domain/ service , its possible With a plugin called ES Head plugin which is a chrome plugin for Elastic Search.
## >Under that plugin , 

  **Copy your VPC Endpoint fyom AWS Elastic search and paste it in the required field in the ES Head Plugin and then choose connect.
It will show you your cluster Health.
It will also show you all of your log analysis in a visual format ,for that you need to go under the "browser" option in the Plugin.**

  **in a different tab of chrome , copy the Kibana end point from your AWS Elastic Search and paste it in thedomain field and connect.**
**In that you will be required to create an index pattern to review logs for Kibana.**



 
