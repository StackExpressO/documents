# How to Create AWS EFS and Mount on EC2 instance

## 1. Come to the AWS management Console and create the EC2 instance there

## 2. Click on Instance and then go to Launch instance option:

## 3. Choose and Amazon Machine Image(AMI)

## 4. Configure instance details

## 5. Configure security Group:

## 6. This is the created instance:

## 7. Now go to all services page and select the EFS service in storage section

## 8. Now create file system
 
## 9. Customize all the Configuration according to your requirement

## 10. Assign mount target here

## 11. Created file system with Name Demo

## 12. Now check the availability zone in security group

## 13. Now to take control your EC2 instances using ssh from command prompt using the following command

## 14. Install the amazon utils there using the following the command

## 15. Now create a directory here name as efs

## 16. Now using this command, we mount our file system in the efs directory

## 17. Using the following command, you can see that efs mount has been completed

## 18. Now create a file there and provide some content in that file

## 19. Take ssh for another instance with the same method install the amazon utils there and create efs directory, and mount the efs with same command as before

## 20. Now move into that efs directory So, when you list the file inside that directory you will find the file which i have created in another instance but with the help of EFS I can also get that here

## 21. Configuration of EFS on EC2 instance comes to an end.
