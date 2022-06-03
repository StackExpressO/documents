**How to Create AWS EFS and Mount on EC2 instance**

1. Come to the AWS management Console and create the EC2 instance there

1. Click on Instance and then go to Launch instance option:

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 001](https://user-images.githubusercontent.com/95607370/171828543-2c55b54f-2de2-4b5b-a684-339188832f23.jpeg)

3. Choose and Amazon Machine Image(AMI)

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 002](https://user-images.githubusercontent.com/95607370/171828600-9fca6179-8c22-4f0f-9f4e-1df030430d7a.jpeg)

4. Configure instance details

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 003](https://user-images.githubusercontent.com/95607370/171828649-aaad8ed5-203b-4bfc-82f2-d6dd5e813348.jpeg)

5. Configure security Group:

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 004](https://user-images.githubusercontent.com/95607370/171828690-ce6af206-867b-441e-b56d-5dddea4e299c.jpeg)

6. This is the created instance:

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 005](https://user-images.githubusercontent.com/95607370/171828730-57cf06dd-1dac-4017-b54c-96a7368db882.jpeg)

7. Now go to all services page and select the EFS service in storage section


8. Now create file system

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 007](https://user-images.githubusercontent.com/95607370/171828873-c291680d-060a-464e-ba24-3840e4380341.jpeg)

9. Customize all the Configuration according to your requirement

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 008](https://user-images.githubusercontent.com/95607370/171828912-82eb6378-6e4a-4445-a9b7-6db774dfb496.jpeg)

10. Assign mount target here

![Uploading Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.009.jpeg…]()

11. Created file system with Name Demo or any name of your choice.

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 010](https://user-images.githubusercontent.com/95607370/171828984-a56f573c-d109-402a-b0db-49429b273321.jpeg)

12. Now check the availability zone in security group

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 011](https://user-images.githubusercontent.com/95607370/171829035-fe92daee-704f-486a-818a-2387378e3b0d.jpeg)

13. Now to take control your EC2 instances using ssh from command prompt using the following command

```bash
ssh -i efs.pem ec2-user@13.12.2.138
```

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 012](https://user-images.githubusercontent.com/95607370/171829120-0a81f816-eccf-4259-8147-3bd5c66af990.jpeg)

14. Install the amazon utils there using the following the command 

```bash
sudo yum install -y amazon-efs-utils
 ```

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 013](https://user-images.githubusercontent.com/95607370/171829154-d17f2c1b-56a2-4e88-878f-9d3c8fda57bb.jpeg)

15. Now create a directory here name as efs 

```bash
  mkdir efs
  ```

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 014](https://user-images.githubusercontent.com/95607370/171829209-600565a9-61cf-4acf-9b38-83ef786587ed.jpeg)

16. Now using this command, we mount our file system in the efs directory 

```bash
sudo mount -t efs -o tls fs -a34b6a72:/ efs
```

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 015](https://user-images.githubusercontent.com/95607370/171829225-32d58e28-399a-45db-9e25-950d4e4def10.jpeg)
![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 016](https://user-images.githubusercontent.com/95607370/171829264-af19e99f-a3f7-4786-8b09-7f609771986b.jpeg)

17. Using the following command, you can see that efs mount has been completed 


```bash
df -h
```

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 017](https://user-images.githubusercontent.com/95607370/171829325-c4f4bdf2-ffb3-4a19-ba90-76fdd1d3a318.jpeg)

18. Now create a file there and provide some content in that file 

```bash
cd efs
   ```
   then run

```bash 
ls
```
and create a file 
 ```bash
 touch hello.txt
 ```

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 018](https://user-images.githubusercontent.com/95607370/171829374-b1dba595-6e5d-4a94-a6ae-9b45dcd42c2e.jpeg)

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 019](https://user-images.githubusercontent.com/95607370/171829410-e67d561a-0310-4500-8769-4d990c17463e.jpeg)

19. Take ssh for another instance with the same method install the amazon utils there and create efs directory, and mount the efs with same command as before

```bash
ssh -i efs.pem ec2-user@65.0.199.145
```
![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 020](https://user-images.githubusercontent.com/95607370/171829445-eea8c072-09c4-4bd2-8b4a-41c0ead26552.jpeg)

20. Now move into that efs directory So, when you list the file inside that directory you will find the file which you have created in another instance but with the help of EFS you can also get that here

```bash
sudo mount -t efs -o tls fs -a34b6a72:/ efs
```
and then run 
```bash
ls 
```
we will get efs then cd to efs 
```bash
cd efs
```
and run 
```bash
ls
```

we have hello.txt file

![Aspose Words 7177b00d-6b0c-4b77-8472-fd4fbb456c01 021](https://user-images.githubusercontent.com/95607370/171829482-59345e32-bd8c-49fc-9f1c-77521f5555e2.jpeg)

21. Configuration of EFS on EC2 instance comes to an end.
