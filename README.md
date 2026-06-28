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

<img width="1920" height="1080" alt="Screenshot (170)" src="https://github.com/user-attachments/assets/d3d5caa5-a12e-48a7-aae9-e9d2a78de39b" />

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

<img width="1920" height="1080" alt="Screenshot (168)" src="https://github.com/user-attachments/assets/c31ab6f7-3e63-4923-a1b8-92c9b803f386" />

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
<img width="1920" height="1080" alt="Screenshot (171)" src="https://github.com/user-attachments/assets/07befc37-0d33-492e-85c8-33978eaf5457" />

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
<img width="2340" height="1333" alt="Automated Node js Deployment with GitHub Actions, Docker, and AWS - visual selection" src="https://github.com/user-attachments/assets/22e51928-bd2f-484e-a97a-0970f05640ac" />


## 🤖 Step 4: CI/CD Automation Setup (GitHub Actions)

[cite_start]This section details the final stage of configuring GitHub Actions to establish an automated pipeline, allowing any future code updates pushed to the repository to deploy seamlessly to the AWS server[cite: 28].

---

### 1. Project Directory Structure
[cite_start]Create the tracking directories required by GitHub Actions inside your project's root folder:

```text
📁 .github/
└── 📁 workflows/
    └── 📄 deploy.yaml
```

## 2. Safeguarding Environment Secrets (GitHub Settings)
To prevent unauthorized access, never expose sensitive server credentials directly in your public code. Secure them using GitHub Repository Secrets:
1. On GitHub, navigate to your repository page.
2. Click Settings ➔ Secrets and variables ➔ Actions.
3. Click New repository secret to store the following variables:
Secret Name                       Secret Value Descriptionv
   ||                                         ||
   \/                                         \/
1. SSH_HOST                              The Public IP address of your running AWS Ubuntu EC2 instance.
2. SSH_PRIVATE_KEY                       The complete text contents of your secure private key file (.pem).

<img width="1920" height="1080" alt="Screenshot (166)" src="https://github.com/user-attachments/assets/2b7b4433-c08d-4986-8bba-ba283d865df6" />

### 3. Creating the Automation Workflow Logic (deploy.yaml)
Add the following workflow pipeline orchestration code to your .github/workflows/deploy.yaml configuration:

```Bash
name: Deploy NodeJS Application to Hostinger

on:
 push:
   branches:
     - master

jobs:
  deploy:
     runs-on: ubuntu-latest
     
     steps:
       - name: Checkout Code
         uses: actions/checkout@v4
        
       - name: Deploy Via SSH
         uses: appleboy/ssh-action@v1.0.3
         with: 
           host: ${{secrets.SSH_HOST}}
           username: root
           key: ${{secrets.SSH_KEY}}
           script:
             cd /root/nodejs_application_deploy
             git pull
             docker compose up -d --build
```
### 4. Triggering the Automated Deployment Sequence
Commit and synchronize the latest structural pipeline rules to your remote repository to test live execution.

### 🎯 Pipeline Outcome: >
Navigate to the Actions tab of your GitHub repository to view your running pipeline. Any future code modifications pushed to your main branch will automatically update the tracking code, build the container environment, and update your AWS production instance dynamically without manual server intervention.
<img width="2484" height="1927" alt="Automated Node js Deployment with GitHub Actions, Docker, and AWS - visual selection (1)" src="https://github.com/user-attachments/assets/4b533d2e-ca99-4fb1-882e-d8096471a18a" />
