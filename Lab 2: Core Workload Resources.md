# 🧪 Lab 2: Core Workload Resources

## 🧠 Theoretical Introduction

In Kubernetes, **workload resources** represent how applications are deployed and managed within a cluster.  
The key objects include **Pods**, **ReplicaSets**, and **Deployments** — forming the foundation of container orchestration.

### 🔹 Core Concepts

- **Pod:** The smallest deployable unit in Kubernetes. A pod may contain one or more tightly coupled containers that share storage and networking.
- **ReplicaSet:** Ensures a specified number of identical pods are running at all times.
- **Deployment:** A higher-level abstraction managing ReplicaSets and providing features like rolling updates and rollbacks.
- **Labels and Selectors:** Used to logically group and select resources.
- **Annotations:** Metadata attached to objects for tool-specific or non-identifying information.
- **Resource Requests & Limits:** Define how much CPU and memory a pod can use, ensuring fair scheduling and avoiding resource starvation.

Understanding these components is crucial for managing scalable, reliable, and self-healing applications on Kubernetes.

---

## 🎯 Learning Objectives

- Deploy and manage Pods, ReplicaSets, and Deployments on K3s.  
- Perform rolling updates and rollbacks safely.  
- Configure resource requests and limits.  
- Use Labels, Selectors, and Annotations effectively.  
- Understand the pod lifecycle and its phases.

---

## ⚙️ Environment Setup

| Resource | Description |
|-----------|--------------|
| **Platform** | VirtualBox or KVM/QEMU |
| **Operating System** | Ubuntu Server 24.04 LTS |
| **Cluster** | K3s v1.30+ (multi-node) |
| **kubectl Version** | v1.30+ |
| **Namespace** | `lab2-core` (create it for this lab) |

Before starting, make sure your K3s cluster is running and accessible with `kubectl`.

---

## 🧪 Lab Steps

### Step 1 – Create a New Namespace

To organize resources for this lab:

        kubectl create namespace lab2-core
        kubectl config set-context --current --namespace=lab2-core

---

### Step 2 – Deploy Your First Pod

Create a manifest named `nginx-pod.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
````

Apply it:

```
    kubectl apply -f nginx-pod.yaml
    kubectl get pods -o wide
```

Describe the pod to inspect its lifecycle and assigned node:

```
    kubectl describe pod nginx-pod
```

---

### Step 3 – Create a ReplicaSet

Create `nginx-rs.yaml`:

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

Apply and check:

```
    kubectl apply -f nginx-rs.yaml
    kubectl get pods
    kubectl describe rs nginx-rs
```

Scale the ReplicaSet manually:

```
    kubectl scale rs nginx-rs --replicas=5
```

---

### Step 4 – Create a Deployment

Create `nginx-deploy.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.25
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: "100m"
            memory: "128Mi"
          limits:
            cpu: "300m"
            memory: "256Mi"
```

Deploy it:

```
    kubectl apply -f nginx-deploy.yaml
    kubectl get deployments
    kubectl get pods
```

---

### Step 5 – Rolling Updates and Rollbacks

Update the Deployment to a new image version:

```
    kubectl set image deployment/nginx-deploy nginx=nginx:1.26 --record
    kubectl rollout status deployment/nginx-deploy
```

To verify rollout history:

```
    kubectl rollout history deployment/nginx-deploy
```

If the update causes issues, rollback:

```
    kubectl rollout undo deployment/nginx-deploy
```

---

### Step 6 – Labels, Selectors, and Annotations

Add an annotation:

```
    kubectl annotate deployment nginx-deploy maintainer="devops-team@university.edu"
```

Query pods by label:

```
    kubectl get pods -l app=nginx
```

Remove a label dynamically:

```
    kubectl label pod <pod-name> app-
```

---

### Step 7 – Observe Pod Lifecycle

Force-delete a pod and watch Kubernetes recreate it automatically:

```
    kubectl delete pod <pod-name>
    kubectl get pods -w
```

Observe pod status phases (`Pending`, `Running`, `Succeeded`, `Failed`, `CrashLoopBackOff`).

---

## 📝 Evaluation Questions

1. What are the main differences between a Pod, ReplicaSet, and Deployment?
2. How does a Deployment ensure application availability during an update?
3. What is the purpose of Labels and Selectors?
4. How can you limit a container’s CPU and memory usage?
5. How does Kubernetes handle a failed Pod?
6. How can you check the rollout history of a Deployment?
7. What happens if a ReplicaSet loses one of its Pods?
8. How can you annotate an existing resource?
9. What’s the difference between `kubectl delete` and `kubectl scale`?
10. Explain a real-world use case where rolling updates are essential.

---

## 📤 Deliverable

Students must submit a **lab report (3–5 pages)** containing:

* YAML manifests used for Pods, ReplicaSets, and Deployments.
* Screenshots showing successful deployments and updates.
* Explanation of resource scaling and rollback results.
* Responses to all evaluation questions.

---

**✅ End of Lab 2 **

```
```
