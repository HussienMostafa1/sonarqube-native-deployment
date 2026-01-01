# Production-Ready SonarQube Deployment for Code Quality & Security Analysis

## 📌 Project Overview

This project demonstrates a **production-ready deployment of SonarQube**
used to perform **code quality analysis, security testing, and vulnerability detection**
on real-world applications.

The setup is designed to simulate **enterprise DevSecOps environments**
where **static code analysis** acts as a **critical security gate**
before applications are promoted to production.

## 🎯 Use Cases

- Detect code smells and software bugs
- Identify security hotspots in source code
- Detect vulnerable third-party dependencies
- Enforce quality gates before production release

## 🧰 Tech Stack

- **Operating System:** Ubuntu 22.04 LTS
- **Static Code Analysis:** SonarQube
- **Programming Language Support:** Java, Python, JavaScript
- **Java Runtime:** OpenJDK 17
- **Database:** PostgreSQL
- **Reverse Proxy:** Nginx
- - **Deployment Type:** Native (Non-containerized)
- **Target Environment:** Production & Pre-Production
- **Security Focus:**
  - Code Quality Analysis
  - Security Hotspot Detection
  - Vulnerability Detection
  - Quality Gates Enforcement

## 🏗 Architecture Overview

The deployment follows a **secure, production-oriented architecture** where
SonarQube is not exposed directly to users.

User traffic is routed through **Nginx (Reverse Proxy)** which forwards requests
to the **SonarQube application running on port 9000**.
SonarQube communicates with a **PostgreSQL database** to store analysis results
and configuration data.

### Architecture Flow

User → Nginx (Port 80) → SonarQube (Port 9000) → PostgreSQL

This architecture allows secure access control, traffic management,
and future integration with CI/CD pipelines.

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

### 1️⃣Install Java (OpenJDK 17)
i) Install OpenJDK 17 (needed for the latest version of SonarQube (version 10.0).
```
sudo apt update
sudo apt install -y openjdk-17-jdk
java -version
```
![install java.](screenshot/Screenshot%20from%202025-12-24%2018-50-37.png?raw=true")
![java version.](https://github.com/HussienMostafa1/sonarqube-native-deployment/blob/main/screenshot/Screenshot%20from%202025-12-24%2018-51-55.png?raw=true")

### Install and Configure PostgreSQL
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

### Download and Install SonarQube
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
iv) Move the unzipped files to /opt/sonarqube directory
```
sudo mv sonarqube-10.0.0.68432 sonarqube
```
sudo mv sonarqube /opt/
```











































