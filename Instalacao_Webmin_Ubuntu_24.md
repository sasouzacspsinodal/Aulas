# Install Webmin on Ubuntu 24.04

To install Webmin on Ubuntu 24.04, follow the steps below.

Step 1 : Update System Packages
```bash
sudo apt update
```
Step 2 : Install Dependencies
```bash
sudo apt install software-properties-common apt-transport-https curl
```
Step 3 : Add Webmin GPG Key
```bash
curl -fsSL https://download.webmin.com/developers-key.asc | sudo gpg --dearmor -o /usr/share/keyrings/webmin.gpg
```

Step 4 : Add Webmin Repository:
```bash
echo "deb [signed-by=/usr/share/keyrings/webmin.gpg] https://download.webmin.com/download/newkey/repository stable contrib" | sudo tee /etc/apt/sources.list.d/webmin.list
```
Step 5 : Update Package List
```bash
sudo apt update
```

Step 6 : Install Webmin
```bash
sudo apt install webmin
```

Step 7 : Access Webmin
```bash
https://your_server_ip:10000
```
Congratulations! You have successfully installed Webmin on Ubuntu 24.04.

referencia no link
```bash
https://devtutorial.io/how-to-install-webmin-on-ubuntu-24-04-p3497.html
```
