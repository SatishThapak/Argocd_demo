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
---

**Start Minikube with Docker driver:**
```bash
- minikube start --driver=docker
- kubectl get nodes
- minikube addons enable ingress
```

**Docker Image Setup**
Login to Docker: 
> docker login

**# Build and push your Docker image:**

> docker build -t my_argocd_image/node-app:latest .

> docker tag my_argocd_image/node-app:latest satishthapak/node-app:latest

> docker push satishthapak/node-app:latest

> docker pull satishthapak/node-app


# Installing ArgoCD on Kubernetes
Let’s start by installing ArgoCD in your Kubernetes cluster. There are two different ways to do this: using manifest files or a Helm chart.

# Step 1: Install ArgoCD
**Method 1:** Using Manifest Files

You can install ArgoCD using the official manifest files provided by the ArgoCD team.
> kubectl create namespace argocd
> kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

This command installs all the necessary ArgoCD components, including the API server, controller, and UI.

**Method 2:** Using a Helm Chart
Alternatively, you can install ArgoCD using the Helm chart, which allows for more customization through Helm values.

1. Add the ArgoCD Helm repository:
> helm repo add argo https://argoproj.github.io/argo-helm

2. Install the Helm chart with a custom values file:
> helm install argocd-demo argo/argo-cd -f argocd-custom-values.yaml

In this example, argocd-custom-values.yaml might look like this:
> server:
>   service:
>     type: NodePort

This overrides the ArgoCD service type from the default ClusterIP to NodePort, allowing you to access the ArgoCD UI via a NodePort.

# Step 2: Verify the Installation
> kubectl get pods -n argocd

All pods should be in a Running state.


# 3. Accessing the ArgoCD Dashboard

**Step 1: Forward the ArgoCD Server Port**
To access the ArgoCD UI, you need to forward the ArgoCD server port(if you don’t use service type NodePort):

> kubectl port-forward svc/argocd-server -n argocd 8080:443

**Step 2: Login to the Dashboard**

Open your browser and go to `https://localhost:8080` or http:// public-ip :nodeport


The default username is # admin, and to retrieve the password, use:

> kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath={.data.password} | base64 -d



