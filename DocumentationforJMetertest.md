**Documentation for JMeter test**

Installation of JMeter on Ubuntu server:


1. Install java in the ec2 instance
```bash
   sudo apt install openjdk-11-jre-headless
```

2. Install Apache JMeter by using the following command
```bash
   wget https://www.apache.org/dist/jmeter/binaries/apache-jmeter-5.5.zip.sha512
``` 

3. Unzip JMeter zip file
```bash
   unzip apache-jmeter-5.5.zip`
```

4. Create  **apache-jmeter-5.5** directory

Run JMeter test:

Prerequisite:

.jmx file

1. Navigate to **/apache-jmeter-5.5/bin** directory and create output by using below caommand
```bash
   sh jmeter.sh -n -t /location of .jmx file -l /destination of output file
```
Ex:
```bash
   sh jmeter.sh -n -t /home/ubuntu/file.jmx -l /home/ubuntu/test.csv
```
   Note : it takes approximately 20 mins to create the output file

2. The above command will create the output file in the destination folder

3. Create the report from the output file by using below command
```bash
   sh jmeter.sh -g /destination of output.csv file -o /destination of the report`
```
Ex:
```bash
   sh jmeter.sh -g /home/ubuntu/test.csv -o /home/ubuntu/report
```
this command will create a folder with the name of report in the destination folder

4. In that report folder we have the folders and files

    ![](Aspose.Words.e8408beb-1c36-4897-ab66-dfeb907ca75a.001.png)

5. Copy the report folder from remote to local machine by using filezilla

6. After copied the folder open that report folder in local

    ![](Aspose.Words.e8408beb-1c36-4897-ab66-dfeb907ca75a.002.png)

7. Double click on the index.html file it will automatically redirect to the browser and open the report like GUI.

