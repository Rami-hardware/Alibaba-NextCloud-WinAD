# Alibaba-NextCloud-WinAD

Automated deployment of **NextCloud** integrated with **Windows Active Directory (WinAD)** using **Alibaba Cloud** for storage, powered by **Docker**, **Terraform**, and **Ansible**.

## Table of Contents

- [Overview](#overview)  
- [Features](#features)  
- [Prerequisites](#prerequisites)  
- [Architecture](#architecture)  
- [Installation](#installation)  
- [Configuration](#configuration)  
- [Quick Start](#quick-start)  
- [Usage](#usage)  
- [Troubleshooting](#troubleshooting)  
- [Contributing](#contributing)  
- [License](#license)  

## Overview

This project automates the provisioning of a **NextCloud** environment integrated with **Windows Active Directory** for authentication, while storing data securely on **Alibaba Cloud OSS**. Infrastructure is fully orchestrated with **Terraform** and **Ansible**, and services run inside **Docker containers**, providing a reproducible, scalable, and maintainable deployment.

## Features

- **Automated Deployment:** Full setup with Docker containers, Terraform for infrastructure, and Ansible for configuration.  
- **Active Directory Integration:** Centralized authentication via Windows AD.  
- **Alibaba Cloud OSS Storage:** Cloud-native, scalable backend storage.  
- **Single Sign-On (SSO):** Seamless login experience for AD users.  
- **Secure and Containerized:** Runs in Docker with isolated, reproducible environments.  

## Prerequisites

Before deployment, ensure you have:

- **Alibaba Cloud Account** with OSS and ECS access.  
- **Terraform** installed (v1.5+ recommended).  
- **Ansible** installed (v2.14+ recommended).  
- **Docker & Docker Compose** installed on target servers.  
- **Windows Active Directory** available for authentication.  
- Admin access to ECS or servers where containers will run.  

## Architecture

```text
+-------------------+        +-------------------+
| Windows AD Server | <----> | NextCloud LDAP    |
+-------------------+        | Integration       |
                             +-------------------+
                                   |
                                   v
                            +----------------+
                            | Dockerized     |
                            | NextCloud      |
                            +----------------+
                                   |
                                   v
                            +----------------+
                            | Alibaba OSS    |
                            | Storage        |
                            +----------------+
```

- **Terraform** provisions ECS instances, networking, and storage buckets.  
- **Ansible** configures Docker, NextCloud, and AD integration.  
- **Docker** runs NextCloud and dependencies in containers.  

## Installation

1. **Clone the repository**  

```bash
git clone https://github.com/yourusername/Alibaba-NextCloud-WinAD.git
cd Alibaba-NextCloud-WinAD
```

2. **Terraform: Provision Infrastructure**  

```bash
cd terraform
terraform init
terraform apply
```

- This will create ECS instances, networking, and OSS buckets on Alibaba Cloud.  

3. **Ansible: Configure Servers and Deploy Docker Containers**  

```bash
cd ../ansible
ansible-playbook -i inventory.yml deploy-nextcloud.yml
```

- Configures Docker, deploys NextCloud containers, and integrates LDAP with Windows AD.  

4. **Verify Deployment**  

- Access NextCloud via browser: `https://<ecs-public-ip>`  
- Login with AD credentials.  

## Configuration

- **NextCloud LDAP/AD Integration:**  

  - AD Server: `ad.yourdomain.com`  
  - Base DN: `DC=yourdomain,DC=com`  
  - Bind DN: `CN=admin,CN=Users,DC=yourdomain,DC=com`  

- **Alibaba OSS Storage:**  

  - OSS Bucket: `nextcloud-data`  
  - Endpoint: `oss-yourregion.aliyuncs.com`  
  - Access Key & Secret Key: from Alibaba Cloud console  

- **Docker Compose:** Modify `docker-compose.yml` for environment variables and volumes.  

## Quick Start

1. **Create a `.env` file** in the project root:

```env
NEXTCLOUD_ADMIN_USER=admin
NEXTCLOUD_ADMIN_PASSWORD=YourStrongPassword
AD_SERVER=ad.yourdomain.com
AD_BASE_DN=DC=yourdomain,DC=com
AD_BIND_DN=CN=admin,CN=Users,DC=yourdomain,DC=com
ALIYUN_OSS_BUCKET=nextcloud-data
ALIYUN_OSS_ENDPOINT=oss-yourregion.aliyuncs.com
ALIYUN_ACCESS_KEY=YourAccessKey
ALIYUN_SECRET_KEY=YourSecretKey
```

2. **Start Docker Containers**  

```bash
docker-compose up -d
```

- The containers will run NextCloud with AD integration and Alibaba OSS storage.  

## Usage

- Users log in to NextCloud using **Windows AD credentials**.  
- Files are stored in **Alibaba OSS**, accessible via web or client apps.  
- Admins manage users and permissions centrally via AD.  

## Troubleshooting

- **Terraform Errors:** Verify Alibaba Cloud credentials and network settings.  
- **Ansible Failures:** Check SSH connectivity, permissions, and inventory variables.  
- **LDAP Integration Issues:** Confirm AD server, ports, and Bind DN.  
- **Docker Container Issues:** Check `docker logs nextcloud` and `docker-compose ps`.  

## Contributing

Contributions are welcome via GitHub issues or pull requests.  

## License

MIT License. See the [LICENSE](LICENSE) file for details.
