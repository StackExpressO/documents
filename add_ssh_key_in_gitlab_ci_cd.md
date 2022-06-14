`                          `**Add ssh key with Gitlab CI/CD** 

` `Step 1: Login to Ec2 Instance


Step 2: Create Directory and Go to Directory
```bash
mkdir backend 
```
```bash
cd backend
```

Step 3: Clone code from Gitlab Repository

```bash
git clone https://gitlab.com/zone7/afkelephants-claims-backend.git 
```

Step 4:  Add “ Remote Origin URL” in Code

go to code
```bash
cd .git
```
```bash
vim config
```
- Copy “ssh url” form your project to “.git/config” file


![](Aspose.Words.e32ec329-bbed-4a05-b8ea-b07af5c490c5.001.png)


![](Aspose.Words.e32ec329-bbed-4a05-b8ea-b07af5c490c5.002.png)

Step 5:  Add “public key” in Gitlab

- Go to “user setting”
- Click on “preferences”
- Click on “ssh keys” in left side
- Add “ssh key”

![](Aspose.Words.e32ec329-bbed-4a05-b8ea-b07af5c490c5.003.png)


![](Aspose.Words.e32ec329-bbed-4a05-b8ea-b07af5c490c5.004.png)


Step 6: Modify “authorized\_keys” with “public key in Ec2 Instance”


Go to “.ssh” directory
```bash
cd /home/ubuntu/.ssh 
```

- Copy public key from “id\_rsa.pub” to “authorized\_keys” file


Step 7: Add “private key” in Variables in Gitlab

- Go to “repo setting
- Click on “setting” in left side
- Go to “variables”
- Add “SSH\_PRIVATE\_KEY” in variables

![](Aspose.Words.e32ec329-bbed-4a05-b8ea-b07af5c490c5.005.png)

![](Aspose.Words.e32ec329-bbed-4a05-b8ea-b07af5c490c5.006.png)

Step 8: Call “private key” in Gitlab CI / CD in Gitlab

- edit “.gitlab-ci.yml” file
- Call “SSH\_PRIVATE\_KEY”

![](Aspose.Words.e32ec329-bbed-4a05-b8ea-b07af5c490c5.007.png)



Step 9: Run and Check Pipeline in Gitlab

![](Aspose.Words.e32ec329-bbed-4a05-b8ea-b07af5c490c5.008.png)


