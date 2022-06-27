`                                                   `**Add ssh key with Gitlab CI/CD**

Step 1: Login to Ec2 Instance

Step 2: Create Directory and Go to Directory
```bash
mkdir backend 
```
```bash
cd backend
```

Step 3: Clone code from Gitlab Repository

```bash
git clone https://gitlab.com/stackexpress3/testing.git
```
Step 4:  Add “ Remote Origin URL” in Code

go to code
```bash
cd .git
```
```bash
vim config
```


Copy “ssh url” form your project to “.git/config” file

![](Aspose.Words.99266f9d-b975-4fef-ad30-12ccbf44a7b2.001.png)

Add the URL in config file

![](Aspose.Words.99266f9d-b975-4fef-ad30-12ccbf44a7b2.002.png)

Step 5:  Add “public key” in Gitlab

Go to “user setting”

Click on “preferences”

Click on “ssh keys” in left side

Add “ssh key”

![](Aspose.Words.99266f9d-b975-4fef-ad30-12ccbf44a7b2.003.png)

![](Aspose.Words.99266f9d-b975-4fef-ad30-12ccbf44a7b2.004.png)

Step 6: Modify “authorized\\_keys” with “public key in Ec2 Instance”

Go to “.ssh” directory
```bash
cd /home/ubuntu/.ssh 
```

Copy public key from “id\\_rsa.pub” to “authorized\\_keys” file

Step 7: Add “private key” in Variables in Gitlab

Go to “repo setting

Click on “setting” in left side

Go to “variables”

Add “SSH\\_PRIVATE\\_KEY” in variables

![](Aspose.Words.99266f9d-b975-4fef-ad30-12ccbf44a7b2.005.png)

![](Aspose.Words.99266f9d-b975-4fef-ad30-12ccbf44a7b2.006.png)

Step 8: Call “private key” in Gitlab CI / CD in Gitlab

edit “.gitlab-ci.yml” file

Call “SSH\\_PRIVATE\\_KEY”

![](Aspose.Words.99266f9d-b975-4fef-ad30-12ccbf44a7b2.007.png)

Step 9: Run and Check Pipeline in Gitlab

![](Aspose.Words.99266f9d-b975-4fef-ad30-12ccbf44a7b2.008.png)
