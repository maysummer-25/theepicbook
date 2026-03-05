# EpicBook – Production-Style Deployment on Microsoft Azure

## Project Overview

This repository documents the deployment of the **EpicBook full-stack application** on **Microsoft Azure** using a secure, segmented cloud architecture.

The objective of this project was to simulate a **real-world production deployment** by implementing proper **network isolation, infrastructure design, and application hosting practices**, rather than simply running an application on a virtual machine.

The deployment separates **frontend, backend, and database layers** while applying **least-privilege networking rules** inside an Azure Virtual Network.

---

# Architecture

The application follows a **three-tier architecture**, which is a common design pattern used in production systems.
Internet
│
▼
Public IP
│
▼
Nginx (Reverse Proxy)
│
▼
React Frontend
│
▼
Node.js / Express API
│
▼
Azure Database for MySQL (Private Subnet)

---

### Architecture Components

| Layer | Technology | Purpose |
|------|------|------|
| Frontend | React.js | User interface |
| Web Server | Nginx | Serves frontend and reverse proxies API requests |
| Backend | Node.js / Express | Application logic and API |
| Database | Azure MySQL Flexible Server | Managed relational database |
| Infrastructure | Azure Virtual Network | Network segmentation |
| Security | Network Security Groups | Traffic control |

---

# Azure Infrastructure Design

The infrastructure was designed to follow **basic cloud security principles** such as **network segmentation and least-privilege access**.

### Virtual Network
VNet: 10.0.0.0/16

---

### Subnet Segmentation

| Subnet | CIDR | Purpose |
|------|------|------|
| Public Subnet | 10.0.1.0/24 | Hosts the application VM |
| Private Subnet | 10.0.2.0/24 | Hosts Azure MySQL Database |

### Security Model

Public Subnet NSG:

- Allow HTTP (80)
- Allow SSH (22)

Private Subnet NSG:

- Allow MySQL (3306) **only from the VM subnet**

This ensures that the database **cannot be accessed directly from the internet**.

---

# Compute Layer

| Component | Configuration |
|------|------|
| Virtual Machine | Ubuntu 22.04 LTS |
| VM Size | Standard_B1s |
| Process Manager | PM2 |
| Web Server | Nginx |

The backend API runs using **PM2** to ensure the Node.js application stays running and automatically restarts if it crashes.

---

# Application Deployment

### Clone the Repository

```bash
git clone https://github.com/maysummer-25/theepicbook.git

### Install Dependencies

Frontend:

cd frontend
npm install
npm run build

Backend:

cd backend
npm install

### Serve Frontend with Nginx

The React production build is served by Nginx.

Build files are copied to: /var/www/html
Nginx then serves the static frontend from this directory.

### Run Backend with PM2
pm2 start server.js
PM2 ensures the backend service remains available and can automatically restart after failures or system reboots.

```
---

# Database Deployment

The application uses Azure Database for MySQL – Flexible Server deployed inside the private subnet.

Security controls implemented:
	•	Private VNet access
	•	No public database endpoint
	•	Access restricted to the application VM subnet
	•	Secure connection through environment variables

This ensures the database is not exposed to the public internet.

---

# Key DevOps Concepts Practiced

This project reinforced several important DevOps and cloud engineering practices:
	•	Virtual Network architecture
	•	Subnet segmentation
	•	Network Security Groups (NSGs)
	•	Private database deployment
	•	Reverse proxy configuration with Nginx
	•	Node.js process management with PM2
	•	Full-stack application deployment
	•	Environment variable configuration
	•	Secure cloud networking design

---

# Original Project

This deployment is based on the EpicBook application by Pravin Mishra

Original repository:

https://github.com/pravinmishraaws/theepicbook

This repository focuses on deployment, infrastructure configuration, and operational setup rather than application development.

---

# Future Improvements

Planned improvements for this project include:
	•	Infrastructure as Code using Terraform
	•	CI/CD pipeline using GitHub Actions
	•	Containerization with Docker
	•	Deployment with Azure Kubernetes Service (AKS)
	•	Monitoring using Azure Monitor or Prometheus
	•	Logging and observability improvements

---

# Author

**Mmesoma Chukwumezie**

DevOps Engineer (Learning Journey)

Focused on:
	•	Cloud Infrastructure
	•	Automation
	•	Production Systems

This project is part of DevOps Micro-Internship Program by Pravin Mishra.
