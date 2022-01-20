# Renewing SSL manually through certbot.

## 1. Add the Certbot package repository.
```bash
sudo apt-get update 

sudo apt-get install software-properties-common

sudo add-apt-repository universe

sudo add-apt-repository ppa:certbot/certbot 

sudo apt-get update 
```

## 2. Install Certbot.
```bash
sudo apt-get install certbot
```
## 3. Generate a certificate.
```bash
sudo certbot certonly --manual --preferred-challenges dns
```
## 4.Type your domain and press Enter.
* ### Please enter in your domain name(s) (comma and/or space separated) (Enter "c"to cancel ): (Your Domain Here)
## Obtaining a new certificate
* ### Performing the following challenges.
* ### dns-01 challenge for (Your Domain Here)
## 5. Certbot will also ask if it is ok to log your IP. Certbot will not issue a cert without this. Type "y" and press Enter.

* ### NOTE: The IP of this machine will be publicly logged as having requested this certificate. If you're running certbot in manual mode on a machine that is not your server, please ensure you're okay with that.

### Are you OK with your IP being logged?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
* ### (Y)es/(N)o: y
## 6. You will now be asked to create a DNS TXT record. This is how Certbot verifies that you own the domain you are making a certificate for.
### Please deploy a DNS TXT record under (Your Domain Here) with the following value:

* ### onzt5fUIcbhY6t8BW4asQHi8k-Imwwi1Epxy4Q8Fb9A

* ### Before continuing, verify the record is deployed.

* ### Press Enter to Continue
## 7. After you have create and saved this record, you can press enter for Certbot to resume.
# *You now have a freshly generated certificate.*

* openssl x509 -text -noout -in filename.crtS