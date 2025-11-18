# 🚀 Argo CD Setup Guide

## 📌 What Is Argo CD?

Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes.  
It follows the GitOps paradigm, where the desired state of your application is defined in Git, and Argo CD ensures that your Kubernetes cluster matches this desired state.

This approach empowers teams to manage both infrastructure configuration and application updates from a single, centralized, version-controlled system.

---

## ✨ Key Features

- Manual or automatic deployment of applications to a Kubernetes cluster  
- Automatic synchronization of application state to the current version of declarative configuration  
- Web user interface and command-line interface (CLI)  
- Visualization of deployment issues, detection and remediation of configuration drift  
- Role-based access control (RBAC) for multi-cluster management  
- Single sign-on (SSO) with GitLab, GitHub, Microsoft, OAuth2, OIDC, LinkedIn, LDAP, and SAML 2.0  
- Webhook support for GitLab, GitHub, and BitBucket  

---

## 💡 Why Use Argo CD?

- **Declarative & Versioned**: Store your entire deployment lifecycle in Git—easy to audit, replicate, and roll back  
- **Automation-First**: Eliminate manual errors with consistent, automated deployments  
- **Transparent & Intuitive**: View app status, health, and deployment history via dashboard or CLI  
- **Built for GitOps**: Streamlines modern DevOps practices across environments and clusters  

---

## 🛠️ Prerequisites

Before we begin, ensure you have the following:

1. A running Kubernetes cluster (Minikube, EKS, GKE, etc.)  
2. `kubectl` configured to interact with your cluster  
3. Basic knowledge of Kubernetes resources  

### Install Minikube
```bash
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```

### Start Minikube with Docker Driver 
```bash
minikube start --driver=docker
kubectl get nodes
minikube addons enable ingress
```
### 🐳 Build and Push Your Docker Image
```bash
docker login
docker build -t thapak010189/argocd:latest .
docker push thapak010189/argocd:latest
docker pull thapak010189/argocd:latest
```
### 📥 Installing Argo CD on Kubernetes
Let’s start by installing Argo CD in your Kubernetes cluster. There are two different ways to do this: using manifest files or a Helm chart.

### Method 1: Using Manifest Files
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
This installs all the necessary Argo CD components, including the API server, controller, and UI.

### Method 2: Using Helm Chart

1. Add the Argo CD Helm repository:

```bash
helm repo add argo https://argoproj.github.io/argo-helm
```
2. Install the Helm chart with a custom values file:
```bash
helm install argocd-demo argo/argo-cd -f argocd-custom-values.yaml
```
Example argocd-custom-values.yaml:
```bash
server:
  service:
    type: NodePort
```
3. ### Argo CD in HA (High Availability) mode
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v3.2.0/manifests/ha/install.yaml
```

### Verify Installation
```bash
kubectl get pods -n argocd
```
### Accessing the Argo CD Dashboard

1. Step 1: Forward the Argo CD Server Port
If you don’t use NodePort, forward the Argo CD server port:
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
2. Step 2: Login to the Dashboard

Open your browser and go to:

* https://localhost:8080 (if using port-forward)
* http://<public-ip>:<nodeport> (if using NodePort)

Default username: admin

Retrieve the initial admin password: 
```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath={.data.password} | base64 -d
```
---
