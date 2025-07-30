# 📌 To-Do List CI/CD Project

> Deploying a Node.js To-Do List app with Docker, Ansible, Kubernetes & ArgoCD

## 🧩 Project Overview

This project demonstrates building a modern DevOps pipeline for a Node.js To-Do List application following real-world practices:

- **Base App:** Forked from [Ankit6098/Todo-List-nodejs](https://github.com/Ankit6098/Todo-List-nodejs)
- **Infrastructure:** Oracle VirtualBox VM with Ubuntu
- **Pipeline:** Complete CI/CD with GitHub Actions → Docker Hub → Auto-deployment

## 🏗 Architecture

```
┌──────────────┐    CI/CD   ┌──────────────┐
│  GitHub Repo │ ────────►  │ Docker Hub   │
└──────┬───────┘            └──────┬───────┘
       │                           │
       ▼                           ▼
┌─────────────────────────────────────────┐
│ VirtualBox VM (Ubuntu 22.04)           │
│ ┌─────────────┐     ┌─────────────────┐ │
│ │ Todo App    │     │ MongoDB         │ │
│ │ (Node.js)   │ ◄── │ (Atlas/Local)   │ │
│ └─────────────┘     └─────────────────┘ │
│   ▲   ▲                                 │
│   │   │ docker-compose                  │
│   │   └─── Watchtower (auto-update)     │
│   │                                     │
│ Ansible Automation                      │
└─────────────────────────────────────────┘
```

## ✅ Implementation Parts

### Part 1 – Containerization & CI 
- **✅ Cloned:** [Original Todo App](https://github.com/Ankit6098/Todo-List-nodejs)
- **✅ MongoDB:** Configured with Atlas connection string in `.env`
- **✅ Dockerfile:** Multi-stage build with Node.js base image
- **✅ CI Pipeline:** GitHub Actions builds and pushes to Docker Hub private registry

### Part 2 – VM & Configuration Management 
- **✅ VM Setup:** Oracle VirtualBox with Ubuntu 22.04
- **✅ Ansible Playbooks:** `config.yaml` & `config2.yaml` for automation
- **✅ Remote Execution:** Runs from local machine to VirtualBox VM using `hosts.ini`
- **✅ Secure Variables:** `vault.yml` for encrypted configuration

### Part 3 – Deployment & Auto-Update 
- **✅ Docker Compose:** Multi-service setup with health checks
- **✅ Services:** todolist-app + mongodb with persistent volumes
- **✅ Watchtower:** Auto-pulls new images from registry
- **✅ Deployment:** `config2.yaml` playbook handles container orchestration

**Tool Justification:** Watchtower chosen for its simplicity, lightweight footprint, and seamless Docker integration. It monitors registry changes automatically and performs zero-downtime updates without complex configuration - perfect for VirtualBox environment with limited resources.

### Part 4 – Kubernetes & GitOps 
- **✅ K3s Installation:** Lightweight Kubernetes on VM
- **✅ YAML Manifests:** Complete K8s deployments and services for app + MongoDB
- **✅ Persistent Storage:** `mongo-pvc.yaml` for MongoDB data persistence
- **✅ Service Exposure:** Proper service configuration for both components

## 🛠 Technology Stack

| Layer | Technology |
|-------|------------|
| **Application** | Node.js, Express.js, EJS |
| **Database** | MongoDB (Atlas) |
| **Containerization** | Docker, Docker Compose |
| **CI/CD** | GitHub Actions, ArgoCD |
| **Infrastructure** | Oracle VirtualBox, Ansible |
| **Orchestration** | Watchtower, Kubernetes (k3s) |

## 📁 Repository Structure

```
todolist/
├── .github/workflows/
│   └── docker-publish.yml      # CI pipeline for Docker Hub
├── ansible/
│   ├── group_vars/all/
│   │   └── vault.yml           # Encrypted variables
│   ├── ansible.cfg             # Ansible configuration
│   ├── config.yaml             # VM setup playbook
│   ├── config2.yaml            # App deployment playbook
│   └── hosts.ini               # Inventory file
├── k8s/
│   ├── mongo-deployment.yaml   # MongoDB K8s deployment
│   ├── mongo-pvc.yaml          # Persistent volume claim
│   ├── mongo-service.yaml      # MongoDB service
│   ├── todolist-deployment.yaml # App deployment
│   └── todolist-service.yaml   # App service
├── assets/                     # Static files (CSS, images)
├── config/                     # App configuration
├── controllers/                # Express.js controllers
├── models/                     # MongoDB/Mongoose models
├── routes/                     # Express.js routes
├── views/                      # EJS templates
├── Dockerfile                  # Multi-stage container build
├── docker-compose.yml          # Container orchestration
├── .env                        # Environment variables
├── index.js                    # Main application entry
├── package.json                # Node.js dependencies
└── README.md                   # This file
```

## 🔧 CI/CD Pipeline

1. **Trigger:** Push to main branch
2. **Build:** Docker image with latest + build number tags
3. **Push:** Private Docker Hub repository
4. **Deploy:** Watchtower auto-pulls and updates containers

## 🚦 Quick Start

### Prerequisites
- Oracle VirtualBox with Ubuntu VM
- Docker & Docker Compose
- Ansible installed locally

### Run Locally
```bash
git clone https://github.com/manaraly/todolist.git
cd todolist
docker compose up --build
```
App runs on `http://localhost:4000`

### Production Deployment
```bash
# Configure VM
ansible-playbook ansible/config.yaml

# Deploy application
ansible-playbook ansible/config2.yaml
```

## 🔑 Security & Configuration

- **Environment Variables:** MongoDB Atlas connection in `.env`
- **GitHub Secrets:** Docker Hub credentials secured
- **SSH Keys:** Passwordless access for Ansible automation
- **Health Checks:** Application and database monitoring

## 🧠 Challenges & Solutions

| Challenge | Solution |
|-----------|----------|
| MongoDB data persistence | Added volumes in compose, PVC in K8s |
| VirtualBox resource limitations | Used lightweight k3s and Watchtower |
| Image update automation | Implemented Watchtower for automatic pulls |
| CI image tagging | Added build numbers alongside latest tag |

 ## ScreenShots
 
 <img width="830" height="684" alt="image" src="https://github.com/user-attachments/assets/fc3c1fb6-28af-4e6a-b604-1438ad9fbcf3" />
<img width="1280" height="551" alt="image" src="https://github.com/user-attachments/assets/1f04d17e-4fed-4e0d-8587-17056895b821" />
<img width="976" height="583" alt="image" src="https://github.com/user-attachments/assets/32b4dc54-29a4-431b-a79e-29bb70063c4c" />
<img width="703" height="377" alt="image" src="https://github.com/user-attachments/assets/5060c4a6-1169-4e2c-a085-9adb73651317" />
<img width="794" height="453" alt="image" src="https://github.com/user-attachments/assets/a2c87ac5-f6b1-40f5-85d6-a4d62e3b2b75" />
<img width="1280" height="513" alt="image" src="https://github.com/user-attachments/assets/2f97a387-1cb5-4708-8d32-8d1c1549225b" />

## Part4

<img width="1280" height="588" alt="image" src="https://github.com/user-attachments/assets/1207c0de-08c3-4212-a66b-20c79736bd74" />
<img width="1280" height="569" alt="image" src="https://github.com/user-attachments/assets/68b9edbd-b63a-463c-9acf-cbf00cd56a17" />


👨‍💻 Author

**Manar Aly Zahran**  
DevOps Engineering Intern  
📧 manaraly136@gmail.com 
🔗 inkedin.com/in/manar-aly-/recent-activity/all/



