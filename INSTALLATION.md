## 📌 Prerequisites

Before starting the deployment, ensure the following:

- **Operating System:** Ubuntu 22.04 LTS
- **User Privileges:** User with `sudo` access
- **Memory:** Minimum 2 GB RAM
- **Storage:** At least 10 GB free disk space
- **Network:** Internet access to download packages
- **Tools Required:** wget, unzip, curl
- **Firewall Configuration:** Allow HTTP/HTTPS ports
- **Time Synchronization:** NTP configured for logs accuracy
- **Backup Plan:** Optional database backup strategy

## Installation & Configuration

### 1-Install Java (OpenJDK 17)
i) Install OpenJDK 17 (needed for the latest version of SonarQube (version 10.0).
```
sudo apt update
sudo apt install -y openjdk-17-jdk
java -version
```
![install java.](screenshot/Screenshot%20from%202025-12-24%2018-50-37.png?raw=true")
![java version.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/Screenshot%20from%202025-12-24%2018-51-55.png?raw=true")

### 2-Install and Configure PostgreSQL
i) Add the PostgreSQL repository.
```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" /etc/apt/sources.list.d/pgdg.list'
```
![Add the PostgreSQL repository.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%201.png?raw=true")

ii) Add the PostgreSQL signing key.
```
wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
```
![Add the PostgreSQL signing key.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%202.png?raw=true")

iii) Install PostgreSQL.

```
sudo apt install postgresql postgresql-contrib -y
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%203.png?raw=true")

iv) Enable the database server to start automatically on reboot.

```
sudo systemctl enable postgresql
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%204.png?raw=true")

v) Start the database server.

```
sudo systemctl start postgresql
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%205.png?raw=true")

vi) Check the status of the database server

```
sudo systemctl status postgresql
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%206.png?raw=true")

vii) Let’s check the installed version of the install Postgres DB.
```
psql --version
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%207.png?raw=true")

viii) Switch to the Postgres user.
```
sudo -i -u postgres
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%208.png?raw=true")

ix) Create a database user named sonar.

```
createuser sonar
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%209.1.png?raw=true")

x) Log in to PostgreSQL.

```
psql
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%209.2.png?raw=true")

xi) Set a password for the sonar user. Use a strong password in place of my_strong_password.
```
ALTER USER [Created_user_name] WITH ENCRYPTED password 'my_strong_password';
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%209.png?raw=true")

xii) Create a SonarQube database and set the owner to sonar.
```
CREATE DATABASE [database_name] OWNER [Created_user_name];
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2010.png?raw=true")

xiii) Grant all the privileges on the sonarqube database to the sonar user.
```
GRANT ALL PRIVILEGES ON DATABASE sonarqube to sonar;
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2011.png?raw=true")

xiv) check the created user and the database.

a) To check the created database
```
\l
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2012.png?raw=true")

b) To check the created database user
```
\du
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2013.png?raw=true")

xv) Exit PostgreSQL.
```
\q
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2014.png?raw=true")

xvi) Return to your non-root sudo user account.
```
exit
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2015.png?raw=true")

### 3-Download and Install SonarQube
i) Install the zip utility, which is needed to unzip the SonarQube files.
```
sudo apt install zip -y
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2016.png?raw=true")

ii) Locate the latest download URL from the SonarQube official download page.

Download the SonarQube distribution files. (you can download the latest SonarQube distribution using the following link)
https://www.sonarsource.com/products/sonarqube/downloads/
Here we are installing the latest version of SonarQube 10.0 community edition (free one)
```
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.0.0.68432.zip
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2017.png?raw=true")

iii) Unzip the downloaded file.
```
sudo unzip sonarqube-10.0.0.68432.zip
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2021.png?raw=true")

iv) Move the unzipped files to /opt/sonarqube directory
```
sudo mv sonarqube-10.0.0.68432 sonarqube
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2019.png?raw=true")

```
sudo mv sonarqube /opt/
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2020.png?raw=true")

### 4-Add SonarQube Group and User
Create a dedicated user and group for SonarQube, which can not run as the root user.

Note: You can give any name for the sonar user and group. I have here given the user and group name to be the same i.e sonar.

i) Create a sonar group.
```
sudo groupadd ddsonar
```
ii) Create a sonar user and set /opt/sonarqube as the home directory.
```
sudo useradd -d /opt/sonarqube -g sonar sonar
```
iii) Grant the sonar user access to the /opt/sonarqube directory.
```
sudo chown sonar:sonar /opt/sonarqube -R
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2022.png?raw=true")

### 5-Configure SonarQube
i) Edit the SonarQube configuration file.
```
sudo nano /opt/sonarqube/conf/sonar.properties
```
a) Find the following lines:

#sonar.jdbc.username=

#sonar.jdbc.password=

b) Uncomment the lines, and add the database user and Database password you created in Step 4 (xi and xii). For me, it’s:

sonar.jdbc.username=sonar

sonar.jdbc.password=your_password

c) Below these two lines, add the following line of code.

sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube

![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2023.png?raw=true")

Here, sonarqube is the database name created.

d) Save and exit the file.

ii) Edit the sonar script file.
```
sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh
```
a) Add the following line

RUN_AS_USER=sonar

Here, sonar is the name of the user that we have created in step number 6 (ii).

![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2024.png?raw=true")
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2026.png?raw=true")


b) Save and exit the file.

### 6-Setup Systemd service
i) Create a systemd service file to start SonarQube at system boot.
```
sudo nano /etc/systemd/system/sonar.service
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2027.png?raw=true")

ii) Paste the following lines to the file.
```
[Unit]
Description=SonarQube service
After=syslog.target network.target
[Service]
Type=forking
ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
User=sonar
Group=sonar
Restart=always
LimitNOFILE=65536
LimitNPROC=4096
[Install]
WantedBy=multi-user.target
```
Note: Here in the above script, make sure to change the User and Group section with the value that you have created. For me its:

User=sonar

Group=sonar

![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2028.1.png?raw=true")

iii) Save and exit the file.

iv) Enable the SonarQube service to run at system startup.
```
sudo systemctl enable sonar
```
v) Start the SonarQube service.
```
sudo systemctl start sonar
```
vi) Check the service status.
```
sudo systemctl status sonar
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2028.png?raw=true")

vii) Hurray, It's up and running.

### 7-Modify Kernel System Limits
SonarQube uses Elasticsearch to store its indices in an MMap FS directory. It requires some changes to the system defaults.

i) Edit the sysctl configuration file.
```
sudo nano /etc/sysctl.conf
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2029.png?raw=true")

ii) Add the following lines.
```
vm.max_map_count=524288
fs.file-max=131072
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2030.png?raw=true")
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2031.png?raw=true")

iii)  Edit the limits.conf configuration file.
```
sudo nano /etc/security/limits.conf
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2032.png?raw=true")

ii) Add the following lines.
```
sonarqube - nfile  131072
sonarqube - nproc  8192
```
![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/PostgreSQL%2034.png?raw=true")

iii) Save and exit the file.

iv) Reboot the system to apply the changes.
```
sudo reboot
```
### 8-Access SonarQube Web Interface
i) Access SonarQube in a web browser at your server’s IP address on port 9000.

For example, http://IP:9000

Note: the default username and password are admin and admin respectively.

![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/sonar40.png?raw=true")

ii) Change the Old password with a New one.

Log in with username admin and password admin. In the next step, SonarQube will prompt you to change your password. CHANGE THE PASSWORD.

![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/sonar41.png?raw=true")

iii) And yes, we have successfully installed the SonarQube Community version 10.0 on AWS EC2 Ubuntu 22.

![Install PostgreSQL.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/sonar42.png?raw=true")


### ###                             Congratulations, Our SonarQube has been installed successfully                                       ### ###


  ### Let’s recap what we have done till now. ###

i) Installed and configured JDK 17 and PostgreSQL.

ii) Installed and configured SonarQube.

iii) Installed and configured the Nginx to run the SonarQube on a domain.

iv) Installed the SSL certificate on the Subdomain for production.

v) And finally made it work.

