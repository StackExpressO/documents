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

![Aspose Words e32ec329-bbed-4a05-b8ea-b07af5c490c5 001](https://user-images.githubusercontent.com/95607370/173498420-acaaa24a-015c-4dfb-a002-1ed7995ee3cb.png)

![Aspose Words e32ec329-bbed-4a05-b8ea-b07af5c490c5 002](https://user-images.githubusercontent.com/95607370/173498465-75c9fd12-9fc3-4608-8571-f5b7d6c9d7d0.png)

Step 5:  Add “public key” in Gitlab

- Go to “user setting”
- Click on “preferences”
- Click on “ssh keys” in left side
- Add “ssh key”

![Aspose Words e32ec329-bbed-4a05-b8ea-b07af5c490c5 003](https://user-images.githubusercontent.com/95607370/173498502-0ac6c13b-0223-4fda-be8b-8d1c3e63adec.png)

![Aspose Words e32ec329-bbed-4a05-b8ea-b07af5c490c5 004](https://user-images.githubusercontent.com/95607370/173498525-6714cdee-3b82-4c6d-9061-9fb1cac8ec49.png)

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

![Aspose Words e32ec329-bbed-4a05-b8ea-b07af5c490c5 005](https://user-images.githubusercontent.com/95607370/173498606-c46e8c11-f703-4fd7-a8c0-04eeb9ab9ee4.png)

![Aspose Words e32ec329-bbed-4a05-b8ea-b07af5c490c5 006](https://user-images.githubusercontent.com/95607370/173498638-587783f7-eb77-4eef-8d68-65bde0a49407.png)

Step 8: Call “private key” in Gitlab CI / CD in Gitlab

- edit “.gitlab-ci.yml” file
- Call “SSH\_PRIVATE\_KEY”

![Aspose Words e32ec329-bbed-4a05-b8ea-b07af5c490c5 007](https://user-images.githubusercontent.com/95607370/173498687-43ef2af7-9fd3-49a8-b377-d964a327e6a5.png)

Step 9: Run and Check Pipeline in Gitlab

![Aspose Words e32ec329-bbed-4a05-b8ea-b07af5c490c5 008](https://user-images.githubusercontent.com/95607370/173498726-2650e445-362c-4385-8924-760634a3076a.png)




