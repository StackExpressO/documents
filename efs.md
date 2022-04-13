# How to Create AWS EFS and Mount on EC2 instance

## 1. Come to the AWS management Console and create the EC2 instance there

## 2. Click on Instance and then go to Launch instance option:
![image](https://user-images.githubusercontent.com/95855861/163127570-5ddb02cf-9d3f-4d6a-8250-5d82bb6e6a4e.png)


## 3. Choose and Amazon Machine Image(AMI)
![image](https://user-images.githubusercontent.com/95855861/163127661-09d27a43-a9dc-4ba2-91fc-9bc8555ce9d8.png)


## 4. Configure instance details
![image](https://user-images.githubusercontent.com/95855861/163127757-6f7091e8-18a8-4a9a-82ea-d42bcb8ca8d3.png)


## 5. Configure security Group:
![image](https://user-images.githubusercontent.com/95855861/163127830-bfc8e671-0eea-410f-91f2-5d24ddbd9dae.png)


## 6. This is the created instance:
![image](https://user-images.githubusercontent.com/95855861/163127915-fe6a1bb7-a743-4367-8b7e-dfd6b9424aef.png)


## 7. Now go to all services page and select the EFS service in storage section
![image](https://user-images.githubusercontent.com/95855861/163128076-4b4a37c0-3d12-4cb7-965e-ae9b3355c08f.png)


## 8. Now create file system
![image](https://user-images.githubusercontent.com/95855861/163128142-77242081-5457-493a-9634-0d9981241117.png)

 
## 9. Customize all the Configuration according to your requirement
![image](https://user-images.githubusercontent.com/95855861/163128248-7600358f-121e-49a7-b34a-3318306cddf1.png)


## 10. Assign mount target here
![image](https://user-images.githubusercontent.com/95855861/163128358-7fa2c869-29b7-4aa2-9c0c-f69dd642bfbe.png)


## 11. Created file system with Name Demo or any name of your choice.
![image](https://user-images.githubusercontent.com/95855861/163128475-3a447823-55d4-42b4-8b92-715471a6a151.png)


## 12. Now check the availability zone in security group
![image](https://user-images.githubusercontent.com/95855861/163128673-d2d82555-67e5-4299-9e8a-72da1776541b.png)


## 13. Now to take control your EC2 instances using ssh from command prompt using the following command
![image](https://user-images.githubusercontent.com/95855861/163128788-0eace212-94d0-4c1d-97bc-4263f9c3549e.png)

## 14. Install the amazon utils there using the following the command
![image](https://user-images.githubusercontent.com/95855861/163128882-0c414136-cf84-4eb6-b07b-d62579b2507c.png)


## 15. Now create a directory here name as efs
![image](https://user-images.githubusercontent.com/95855861/163128960-14d12147-9710-4432-b0c1-0b187173078e.png)

## 16. Now using this command, we mount our file system in the efs directory
![image](https://user-images.githubusercontent.com/95855861/163129043-f87bf56e-e0b7-4679-922e-13edf69072cd.png)
![image](https://user-images.githubusercontent.com/95855861/163129136-6e67b002-7782-44c9-9c79-fdaf009a15ad.png)


## 17. Using the following command, you can see that efs mount has been completed
![image](https://user-images.githubusercontent.com/95855861/163129226-e55fe32a-d690-4300-864d-8fb165a5c92a.png)


## 18. Now create a file there and provide some content in that file
![image](https://user-images.githubusercontent.com/95855861/163129305-afc756d0-4946-4546-9a08-a1ce2315c155.png)
![image](https://user-images.githubusercontent.com/95855861/163129425-415af83d-2079-46c0-90d8-6df335fc380c.png)


## 19. Take ssh for another instance with the same method install the amazon utils there and create efs directory, and mount the efs with same command as before
![image](https://user-images.githubusercontent.com/95855861/163129632-52eda48a-1852-4c39-b37e-4ca772541236.png)

## 20. Now move into that efs directory So, when you list the file inside that directory you will find the file which you have created in another instance but with the help of EFS you can also get that here
![image](https://user-images.githubusercontent.com/95855861/163129740-87c8d565-2fe3-4053-a55c-3cec21ca5542.png)

## 21. Configuration of EFS on EC2 instance comes to an end.
