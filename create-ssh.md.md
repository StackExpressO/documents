# Creating SSH Keys (Command Line)
## **1. Create a .ssh in your home directory**
* ## *Create a .ssh folder in your user account's home directory if it does not exist:*
> mkdir /home/username/.ssh**
* ## *Go to the .ssh folder and continue:*
> cd /home/username/.ssh**

## **2. Run ssh-keygen to generate an ssh key-pair.**
* ## *Run the following command in the .ssh folder. The program prompts you for the key-pair's filename. Press ENTER to use the default name id_rsa. For a passphrase, you can either enter a password, or press return twice to leave it blank:*
> ssh-keygen it -rsa**

## **3. Retrieve the public key file.**
* ## *When created, the key-pair can be found in your home directory's .ssh folder (assuming you generated the key with the default name id_rsa):*
> /home/username/.ssh/id_rsa.pub**
* ## *Provide the public key file (for example, id_rsa.pub) to your server administrator, so that it can be set up for your server connection.*