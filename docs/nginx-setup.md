
### Configuring the Reverse Proxy using Nginx  

Now we want a domain name to take over the IP of the server so that instead of hitting the IP we can use the domain name that is TLS enabled. Let's see how to do so.

I will be using the sub-domain name www.sonar.com This is mine. Please use yours.

### 1-Install Nginx
```
sudo apt updat
sudo apt install nginx -y
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/sonar43.png?raw=true")

### 2-Create a new Nginx configuration file for the site.
```
sudo nano /etc/nginx/sites-enabled/sonarqube
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/sonar44.png?raw=true")

### 3-Add this configuration so that Nginx will route incoming traffic to SonarQube.
```
server {
listen 80;
server_name sonarqube.onecloudhelper.com;
location / {
proxy_pass http://localhost:9000;
}
}
```
Here, replace the part server_name www.sonar.com; with your domain or sub-domain name.
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/sonar45.png?raw=true")
```
sudo ln -s /etc/nginx/sites-available/sonarqube /etc/sites-enabled/
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/sonar46.png?raw=true")

### 4-Next, make sure your configuration file has no syntax errors.
```
sudo nginx -t
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/sonar47.png?raw=true")

### 5-Enable the Nginx server.
```
sudo systemctl enable nginx
```
### 6-Restart the nginx server
```
sudo systemctl restart nginx
```
### 7-Check the status of the nginx server
```
sudo systemctl status nginx
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/sonar48.png?raw=true")

### 8-Use the configured domain.

You can now visit http://www.sonar.com in your web browser. You’ll be greeted with the SonarQube web interface.

![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/sonar49.png?raw=true")

![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/sonar50.png?raw=true")


###  ###     But the domain is working on HTTP protocol and we need to make it secure by making it work with the HTTPS protocol with SSL certificate.  ###  ###

### 9-Installing SSL certificate for maximum security for production

We will use the free SSL certificate provided by Certbot for our sub-domain www.soanar.com. Let's see how we will do this.

# i) Installing certbot
```
sudo snap install --classic certbot
```
# ii) Generate SSL certificate
```
sudo certbot --nginx
```

Generating SSL certificate for the required domain. Please read the asked question carefully and answer them accordingly.
Success message that the SSL certificate has been deployed on the domain www.sonar.com

# iii) Certificate deployed successfully.

Now if we hit the domain www.sonar.com, it automatically redirects to HTTPS.

### This project demonstrates how SonarQube detects
security issues, code smells, and bugs in an any programing language  application.

The application intentionally includes insecure patterns
to validate static code analysis capabilities.

![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/Screenshot%20from%202026-01-05%2019-39-51.png?raw=true")
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/Screenshot%20from%202026-01-05%2019-40-07.png?raw=true")
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/Screenshot%20from%202026-01-05%2019-40-20.png?raw=true")
