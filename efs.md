**How to Create AWS EFS and Mount on EC2 instance**

1. Come to the AWS management Console and create the EC2 instance there

1. Click on Instance and then go to Launch instance option:

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.001.jpeg)

3. Choose and Amazon Machine Image(AMI)

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.002.jpeg)

4. Configure instance details

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.003.jpeg)

5. Configure security Group:

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.004.jpeg)

6. This is the created instance:

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.005.jpeg)

7. Now go to all services page and select the EFS service in storage section

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.006.jpeg)

8. Now create file system

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.007.jpeg)

9. Customize all the Configuration according to your requirement

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.008.jpeg)

10. Assign mount target here

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.009.jpeg)

11. Created file system with Name Demo or any name of your choice.

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.010.jpeg)

12. Now check the availability zone in security group

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.011.jpeg)

13. Now to take control your EC2 instances using ssh from command prompt using the following command

```bash
ssh -i efs.pem ec2-user@13.12.2.138
```

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.012.jpeg)

14. Install the amazon utils there using the following the command 

```bash
sudo yum install -y amazon-efs-utils
 ```

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.013.jpeg)

15. Now create a directory here name as efs 

```bash
  mkdir efs
  ```

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.014.jpeg)

16. Now using this command, we mount our file system in the efs directory 

```bash
sudo mount -t efs -o tls fs -a34b6a72:/ efs
```

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.015.jpeg)

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.016.jpeg)

17. Using the following command, you can see that efs mount has been completed 


```bash
df -h
```

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.017.jpeg)

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

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.018.jpeg)

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.019.jpeg)

19. Take ssh for another instance with the same method install the amazon utils there and create efs directory, and mount the efs with same command as before

```bash
ssh -i efs.pem ec2-user@65.0.199.145
```
![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.020.jpeg)

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

![](Aspose.Words.7177b00d-6b0c-4b77-8472-fd4fbb456c01.021.jpeg)

21. Configuration of EFS on EC2 instance comes to an end.
