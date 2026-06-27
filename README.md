# Automated Node.js Deployment with GitHub Actions, Docker, and AWS

This repository contains a fully automated CI/CD pipeline for a high-performance Node.js Express application. On every code push, GitHub Actions automatically builds, transfers, and deploys the containerized application directly onto an AWS Virtual Machine using Docker Compose.

## 🏗️ System Architecture

<img width="1600" height="450" alt="overview" src="https://github.com/user-attachments/assets/fbc8c576-42d5-4101-b0a6-830d984a1c34" />
*Figure 1: Automated deployment flow from local workstation push to AWS Virtual Machine execution.*

---

## 🛠️ Application Setup

### 1. Dependencies and Initialization
The project uses Express for routing along with TypeScript declarations for development safety.

```json
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

---
### 2. Core Server Application (index.js)
The application instantiates a minimal backend service that responds with JSON data.
