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
