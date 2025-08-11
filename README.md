# Kubernetes RBAC Dashboard — Local KIND Cluster Demo

![Dashboard Demo](dashboard-local.gif)

This repository demonstrates how to set up a **local Kubernetes KIND cluster** with:
- **Kubernetes Dashboard** + **RBAC authentication**
- **Django Notes App** with **Horizontal Pod Autoscaling (HPA)**
- **Nginx App** with **Persistent Volume (PV) and Persistent Volume Claim (PVC)**

---

## 📌 Overview

This project is based on the [Kubestarter KIND cluster guide](https://github.com/LondheShubham153/kubestarter/tree/main/kind-cluster) but extended with:
- Horizontal scaling using **HPA** for the Django app
- Persistent storage using **PV & PVC** for the Nginx app

It’s a great starting point to:
- Understand RBAC-secured Kubernetes Dashboard
- Experiment with scaling workloads
- Learn persistent storage concepts

---

## 📂 Repository Structure

```

.
├── kind-config.yaml        # KIND cluster config
├── dashboard-admin-user.yml# RBAC config for Dashboard
├── rbac/                   # Additional RBAC manifests
├── django-hpa/             # Django app + HPA manifests
├── nginx-storage/          # Nginx app + PV & PVC manifests
└── README.md

````

---

## 🚀 Setup Guide

### 1️⃣ Install KIND & kubectl
Install KIND and kubectl (refer to official docs or a helper script).

---

### 2️⃣ Create a KIND Cluster
`kind-config.yaml`:
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    image: kindest/node:v1.33.1
  - role: worker
    image: kindest/node:v1.33.1
  - role: worker
    image: kindest/node:v1.33.1
````

Create the cluster:

```bash
kind create cluster --config kind-config.yaml --name local-kind
kubectl get nodes
kubectl cluster-info
```

---

### 3️⃣ Deploy Kubernetes Dashboard

Apply the Dashboard manifest:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```

---

### 4️⃣ Configure RBAC Access

`dashboard-admin-user.yml`:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```

Apply it:

```bash
kubectl apply -f dashboard-admin-user.yml
```

Get the access token:

```bash
kubectl -n kubernetes-dashboard create token admin-user
```

---

### 5️⃣ Access the Dashboard

```bash
kubectl proxy
```

Open in browser:

```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

Login using the generated token.

---

### 6️⃣ Deploy Demo Applications

#### **Django Notes App with HPA**

```bash
kubectl apply -f django-hpa/
```

* Includes a Deployment, Service, and Horizontal Pod Autoscaler.

#### **Nginx App with PV & PVC**

```bash
kubectl apply -f nginx-storage/
```

* Includes a PersistentVolume, PersistentVolumeClaim, and Nginx Deployment bound to the PVC.

---

## 💡 Why These Features Matter

* **RBAC** — Secures Dashboard access per user role
* **HPA** — Automatically scales pods based on CPU usage
* **PV & PVC** — Keeps data persistent even if pods are recreated
* **Dashboard** — Intuitive UI for visualizing Kubernetes workloads

---

## 🖥 Demo

![Dashboard Demo](dashboard-local.gif)
*(Replace the link with your actual GIF showing the Dashboard, HPA scaling, and Nginx storage.)*

---

## 🧹 Deleting the Cluster

```bash
kind delete cluster --name local-kind
```

---

## 📚 Learn More

* [Kubernetes RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
* [Kubernetes HPA](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)
* [Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
* [KIND Documentation](https://kind.sigs.k8s.io/)
