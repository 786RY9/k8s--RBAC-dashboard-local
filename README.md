# Kubernetes RBAC Dashboard — Local Demo Setup

This repository demonstrates running a **Kubernetes Dashboard** in a local environment with **RBAC (Role-Based Access Control)** enabled, using two sample applications:

- **Django Notes App** — shown with **Horizontal Pod Autoscaling (HPA)**.
- **Nginx App** — shown with **Persistent Volume (PV) and Persistent Volume Claim (PVC)** configuration.

---

## 📌 Overview

1. **Purpose**  
   Showcase how to set up Kubernetes RBAC with a web dashboard locally, along with real-world workloads:
   - A Django app that automatically scales (HPA).
   - An Nginx server that persists data via PV & PVC.

2. **Tech Highlights**  
   - Kubernetes RBAC for secure access control  
   - Kubernetes Dashboard UI for cluster management  
   - Demo apps illustrating scaling and persistent storage concepts

  ![Dashboard Demo](dashboard-local.gif)


---

## 📂 Repository Structure

```

├── rbac/                     # RBAC manifests for dashboard access
├── django-hpa/               # Django app deployment + HPA config
├── nginx-storage/            # Nginx app with PV & PVC setup
└── README.md                 # This documentation

````

---

## 🚀 Getting Started

### 1️⃣ Set Up Kubernetes Cluster
- Use a local K8s cluster like **Kind**, **Minikube**, or **MicroK8s**.
- Ensure **RBAC** is enabled in your cluster.

### 2️⃣ Apply RBAC Configuration
```bash
kubectl apply -f rbac/
````

### 3️⃣ Deploy Kubernetes Dashboard

* Install Dashboard via Helm or YAML manifest.
* Use RBAC tokens or ServiceAccount to access it safely.
* Forward the Dashboard port with:

```bash
kubectl -n kubernetes-dashboard port-forward service/kubernetes-dashboard 8443:443
```

### 4️⃣ Deploy Django Notes App with HPA

```bash
kubectl apply -f django-hpa/
```

* After deployment, simulate load and watch the Dashboard scale pods horizontally.

### 5️⃣ Deploy Nginx App with PV & PVC

```bash
kubectl apply -f nginx-storage/
```

* This will:

  * Create **PersistentVolume (PV)**
  * Create **PersistentVolumeClaim (PVC)**
  * Deploy Nginx using the PVC for persistent storage

---

## 💡 Why These Features Matter

* **RBAC** — ensures secure, role-based access to cluster resources.
* **Dashboard UI** — gives a visual and interactive interface to understand cluster and workload status.
* **HPA (Horizontal Pod Autoscaler)** — enables workload to automatically scale with demand.
* **PV & PVC** — handle stateful applications by decoupling storage lifecycle from pod lifecycle.
  📚 [Kubernetes Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

---

## 🖥 Usage Example

```bash
# 1. RBAC setup
kubectl apply -f rbac/

# 2. Dashboard deployment
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
helm install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard \
  --namespace kubernetes-dashboard --create-namespace

# 3. Django app + HPA
kubectl apply -f django-hpa/

# 4. Nginx app + PV & PVC
kubectl apply -f nginx-storage/

# 5. Dashboard access
kubectl -n kubernetes-dashboard port-forward service/kubernetes-dashboard 8443:443
```

Then open **[[https://localhost:8443](https://localhost:8](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/pod/nginx/nginx-deployment-55bd8ff695-zxzgd?namespace=nginx))** in your browser, log in using the RBAC token or ServiceAccount, and explore your deployed workloads.

---

## 📚 Learn More

* [RBAC Authorization in Kubernetes](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
* [PersistentVolume & PVC Concepts](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
