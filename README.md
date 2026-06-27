# Automated Node.js Deployment with GitHub Actions, Docker, and AWS

This repository contains a fully automated CI/CD pipeline for a high-performance Node.js Express application. On every code push, GitHub Actions automatically builds, transfers, and deploys the containerized application directly onto an AWS Virtual Machine using Docker Compose.

## 🏗️ System Architecture

<img width="1600" height="450" alt="overview" src="https://github.com/user-attachments/assets/fbc8c576-42d5-4101-b0a6-830d984a1c34" />
*Figure 1: Automated deployment flow from local workstation push to AWS Virtual Machine execution.*

---

## Step1: 🛠️ Application Setup

### 1. Dependencies and Initialization
The project uses Express for routing along with TypeScript declarations for development safety.

```bash
{
  "name": "node-aws-cicd",
  "version": "1.0.0",
  "description": "CI/CD Pipeline project",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.19.2"
  },
  "devDependencies": {
    "@types/express": "^4.17.21"
  }
}
```
### 2. Core Server Application (index.js)
The application instantiates a minimal backend service that responds with JSON data.

### 🐳 Containerization Layer
To ensure identical execution across staging and production systems, the application is packaged as a standard container.

1.Dockerfile

2.Docker Compose (docker-compose.yaml)

## 📦 Step 2: Source Control Tracking & GitHub Initialization
Before staging the codebase for remote publication, configure git rules to stop temporary logs and heavy local modules (node_modules) from polluting the repository.

### 1. Generating the Environment Exclusion Rules
Generate a standard Node.js exclusion template using your tracking initialization commands:

```Bash
# Generate the standard Node.js ecosystem exclusion layout
npx gitignore node
```
### ⚠️ Important Note:
Verify your generated .gitignore file contains a dedicated entry blocking the node_modules/ directory before finalizing your stage snapshot.

### 2. Pushing the Local Workspace to GitHub
Execute the local initialization sequence and link your system workspace to your upstream cloud repository.

## ☁️ Step 3: AWS EC2 Provisioning, Docker Installation & Manual Verification

This section covers provisioning the cloud infrastructure environment, installing the container runtime engine, and performing a manual smoke test to verify network accessibility before automating the pipeline.

---

### 1. Provisioning the Cloud Instance
* **Operating System:** Ubuntu Server (Latest LTS version).
* **Security Group Rules:** Ensure incoming firewall settings are configured to allow traffic on the following ports:
    * `Port 22`: For remote SSH management access.
    * `Port 8080`: To access the running Node.js application service.

---

### 2. Installing Docker Engine on Ubuntu
Connect to your remote AWS instance via your terminal using your private key (`.pem`) file, then execute the official Docker installation commands. The whole procedure will get from (https://docs.docker.com/engine/install/ubuntu/).

---
### 3. Cloning and Manually Testing the Code
Clone your repository directly onto the EC2 virtual machine and launch your environment using Docker Compose to verify that everything works correctly.

---
### 4. Verifying Runtime Service Access
To test if your application is successfully serving traffic on the open port, run a local request test within the instance or from your external terminal using the instance's Public IP address:

```Bash
# Execute an HTTP request check against port 8080
curl http://YOUR_AWS_EC2_PUBLIC_IP:8080
```
### Expected Response Output:
```Bash
{"msg": "Hello from the server "}
```
