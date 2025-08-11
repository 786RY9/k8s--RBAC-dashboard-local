# Kubernetes RBAC Dashboard — Local Demo Setup

![Demo GIF](dashboard-local.gif)

A local Kubernetes RBAC Dashboard demo featuring two sample applications:

- **Django Notes App** with **Horizontal Pod Autoscaling (HPA)**
- **Nginx App** with **Persistent Volume (PV) & Persistent Volume Claim (PVC)**

---

##  Overview

- **Goal:** Enable a secure local Kubernetes setup with a visual Dashboard and showcase scaling and persistent storage.
- **Components:**
  - Secure access via **RBAC**
  - Interactive UI using **Kubernetes Dashboard**
  - Live demos:
    - **Django Notes App** (HPA in action)
    - **Nginx with storage persistence**

---

##  Repository Structure

```

.
├── rbac/              # ServiceAccount, Role, RoleBinding for Dashboard access
├── django-hpa/        # Deployment, Service, HPA manifest for Django app
├── nginx-storage/     # PV, PVC, Deployment, Service for Nginx demo
└── README.md          # This documentation

````

---

##  Getting Started

### 1. Set Up a Local Kubernetes Cluster
Use any local cluster environment, such as **Kind**, **Minikube**, or **MicroK8s**, with RBAC enabled.

### 2. Apply RBAC Configuration
```bash
kubectl apply -f rbac/
````

### 3. Deploy Kubernetes Dashboard

Install via Helm or YAML, then:

```bash
kubectl -n kubernetes-dashboard port-forward service/kubernetes-dashboard 8443:443
```

Access it at: `https://localhost:8443` (use the generated service account token for login).

### 4. Launch Django Notes App with HPA

```bash
kubectl apply -f django-hpa/
```

Simulate load to observe horizontal scaling in the Dashboard.

### 5. Deploy Nginx App with Persistent Storage

```bash
kubectl apply -f nginx-storage/
```

This setup includes:

* **PersistentVolume (PV)**
* **PersistentVolumeClaim (PVC)**
* **Nginx using PVC for storage**

---

## Why It Matters

* **RBAC** secures access control
* **Dashboard UI** makes visual monitoring easier
* **HPA** demonstrates responsive scaling under load
* **PV & PVC** teach persistence handling for stateful services
  Learn more: [Kubernetes Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

---

## Usage Example

```bash
# 1. RBAC setup
kubectl apply -f rbac/

# 2. Deploy Dashboard
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
helm install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard \
  --namespace kubernetes-dashboard --create-namespace

# 3. Deploy Django app + HPA
kubectl apply -f django-hpa/

# 4. Deploy Nginx app + PV/PVC
kubectl apply -f nginx-storage/

# 5. Access the Dashboard
kubectl -n kubernetes-dashboard port-forward service/kubernetes-dashboard 8443:443
```

Explore the visual interface and interact with both apps to see HPA scaling and storage persistence live.

---

## Learn More

* [RBAC in Kubernetes](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
* [Persistent Volumes & PVC](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

---

## Notes

* Replace the GIF URL above with the actual link (e.g., hosted via GitHub or an image host) to showcase your dashboard in action.
* If you’d like help generating that demo GIF (e.g., screen recording → conversion), feel free to ask!

