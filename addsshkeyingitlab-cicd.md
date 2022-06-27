                                                 Add ssh key with Gitlab CI/CD

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

![Aspose Words 99266f9d-b975-4fef-ad30-12ccbf44a7b2 001](https://user-images.githubusercontent.com/95607370/175900522-4325a8be-f1f1-4601-8b61-eed973b745a3.png)


Add the URL in config file

![Aspose Words 99266f9d-b975-4fef-ad30-12ccbf44a7b2 002](https://user-images.githubusercontent.com/95607370/175900577-552e3d3c-36c9-4945-a9fd-49d140981cb5.png)


Step 5:  Add “public key” in Gitlab

Go to “user setting”

Click on “preferences”

Click on “ssh keys” in left side

Add “ssh key”

![Aspose Words 99266f9d-b975-4fef-ad30-12ccbf44a7b2 003](https://user-images.githubusercontent.com/95607370/175900646-04621d93-a97f-4b0b-a484-bb2246f4bd61.png)


![Aspose Words 99266f9d-b975-4fef-ad30-12ccbf44a7b2 004](https://user-images.githubusercontent.com/95607370/175900687-0ff1fe76-6941-4d47-a55b-e04445f52a37.png)


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

![Aspose Words 99266f9d-b975-4fef-ad30-12ccbf44a7b2 005](https://user-images.githubusercontent.com/95607370/175900766-95025f1c-3e7b-4164-8ba3-f3d1f563a380.png)


![Aspose Words 99266f9d-b975-4fef-ad30-12ccbf44a7b2 006](https://user-images.githubusercontent.com/95607370/175900812-899232dd-d112-4379-b21d-8f30579366c7.png)


Step 8: Call “private key” in Gitlab CI / CD in Gitlab

edit “.gitlab-ci.yml” file

Call “SSH\\_PRIVATE\\_KEY”

![Aspose Words 99266f9d-b975-4fef-ad30-12ccbf44a7b2 007](https://user-images.githubusercontent.com/95607370/175900875-2de35c0e-e7d6-4c85-94d5-1279a4b7fd5d.png)


Step 9: Run and Check Pipeline in Gitlab

![Aspose Words 99266f9d-b975-4fef-ad30-12ccbf44a7b2 008](https://user-images.githubusercontent.com/95607370/175900967-ad954eb8-1766-420f-8bda-a6b67f54d87b.png)

