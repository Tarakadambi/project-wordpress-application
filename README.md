# ☸️ WordPress on Kubernetes using Helm

## 📖 Overview
This project demonstrates how to deploy a WordPress application on a Kubernetes cluster using Helm charts. It includes deployment of WordPress and MySQL as separate Kubernetes workloads and manages them using Helm for easy installation and configuration.

---

## 🚀 What I Did
- Created Kubernetes manifests for WordPress deployment  
- Configured MySQL database deployment in Kubernetes  
- Used Kubernetes Services for internal communication  
- Packaged the application using Helm charts  
- Managed full deployment using Helm commands  
- Learned Kubernetes core concepts like Pods, Deployments, and Services  

---

## 🛠️ Technologies Used
- Kubernetes  
- Helm  
- WordPress  
- MySQL  
- Docker images  

---

## ⚙️ Architecture
- WordPress runs as a Kubernetes Deployment  
- MySQL runs as a separate Deployment  
- Services connect WordPress with MySQL internally  
- Helm manages all Kubernetes resources as a single package  

---

## 🚀 How to Deploy

### Step 1: Clone the repository
```bash
git clone git@github.com:Tarakadambi/project-wordpress-application.git
