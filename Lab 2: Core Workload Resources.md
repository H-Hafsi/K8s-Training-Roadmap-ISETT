# 🧪 Lab 2: Deployments, ReplicaSets, and Basic Services (2025 Edition – K3s)

## 🧠 Theoretical Introduction

In Kubernetes, application deployment and management are achieved through **Pods**, **ReplicaSets**, and **Deployments**.  
While Pods represent the running instances of an application, ReplicaSets maintain the desired number of those Pods, and Deployments provide an abstraction that simplifies management, scaling, and version control.  
Finally, **Services** provide stable access points to these Pods, ensuring connectivity within or outside the cluster.

This lab introduces these four essential Kubernetes objects using **K3s**.

### 🔹 Pods
- Pods are the smallest deployable units in Kubernetes.  
- Each Pod encapsulates one or more containers that share the same network and storage.  
- Pods are ephemeral — if one fails, it is recreated by a higher-level controller (like a ReplicaSet).  

Relationship:
- **Managed by ReplicaSets and Deployments** to maintain the desired number of instances.  
- **Accessed via Services** to provide stable networking endpoints.  

### 🔹 ReplicaSets
- Ensure a specific number of identical Pods are always running.  
- Automatically recreate Pods if they fail or are deleted.  
- Are typically **not created directly** — Deployments generate them automatically.  

Relationship:
- **Controlled by Deployments.**  
- **Responsible for pod lifecycle consistency.**

### 🔹 Deployments
- A Deployment defines the desired state for an application (e.g., how many Pods, what image to use).  
- It handles:
  - Rolling updates (without downtime).  
  - Rollbacks (returning to a previous stable version).  
- Deployments rely on ReplicaSets for scalability and reliability.  

Relationship:
- A **Deployment → creates a ReplicaSet → manages Pods**.  
- Simplifies operations like upgrades and scaling.

### 🔹 Services (Basic Overview)
- A Service exposes a set of Pods and provides a stable network endpoint.  
- Pods receive dynamic IP addresses — Services ensure clients can always reach them.  
- Types of Services: `ClusterIP`, `NodePort`, `LoadBalancer`, `ExternalName`.  
  (Only **ClusterIP** will be used in this lab — external exposure will be covered later.)

Relationship:
- **Targets Pods** using **labels**.  
- **Bridges Deployments and network access.**

---

## 🎯 Learning Objectives

- Create and manage Pods, ReplicaSets, and Deployments on **K3s**.  
- Access deployed applications using a **ClusterIP Service**.  
- Understand the YAML structure of each resource with detailed explanations.  
- Practice scaling, updating, and rolling back Deployments.  

---

## ⚙️ Environment Setup

| Component | Description |
|------------|-------------|
| **Platform** | VirtualBox 7.x or KVM/QEMU |
| **Operating System** | Ubuntu Server 24.04 LTS |
| **Kubernetes Distribution** | K3s v1.30+ |
| **kubectl Version** | v1.30+ |
| **Namespace** | `lab2-app` |

Ensure your K3s cluster is up and running:
```bash
kubectl get nodes
````

---

## 🧪 Lab Steps

### Step 1 – Create a Namespace

Create a dedicated namespace to isolate your resources.

`namespace.yaml`

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: lab2-app   # Name of the namespace that will contain all resources in this lab
```

Apply it:

```bash
kubectl apply -f namespace.yaml
kubectl get ns
```

---

### Step 2 – Create a Deployment (which manages a ReplicaSet and Pods)

`deployment.yaml`

```yaml
apiVersion: apps/v1               # API version for Deployment objects
kind: Deployment                  # Resource type: Deployment
metadata:
  name: web-deployment            # Name of the Deployment
  namespace: lab2-app             # Namespace defined earlier
  labels:
    app: web-app                  # Label used for identification
spec:
  replicas: 3                     # Desired number of Pods
  selector:                       # Tells Deployment which Pods to manage
    matchLabels:
      app: web-app                # Must match the labels inside the Pod template
  template:                       # Template describing the Pods to create
    metadata:
      labels:
        app: web-app              # Pods created by this Deployment will have this label
    spec:
      containers:
      - name: nginx-container     # Name of the container
        image: nginx:latest       # Container image from Docker Hub
        ports:
        - containerPort: 80       # Port exposed by the container
        resources:                # Optional resource configuration
          requests:
            cpu: "100m"
            memory: "128Mi"
          limits:
            cpu: "300m"
            memory: "256Mi"
```

Apply it:

```bash
kubectl apply -f deployment.yaml
kubectl get deployments -n lab2-app
kubectl get pods -n lab2-app
```

**Explanation:**
The Deployment automatically creates a ReplicaSet which ensures that **three Pods** of the Nginx container are always running inside the `lab2-app` namespace.

---

### Step 3 – Expose the Deployment Using a Service (ClusterIP)

To make your Pods accessible inside the cluster, create a Service.

`service.yaml`

```yaml
apiVersion: v1                # API version for Service
kind: Service                 # Resource type: Service
metadata:
  name: web-service           # Name of the Service
  namespace: lab2-app         # Namespace must match the Deployment
spec:
  type: ClusterIP             # Default type; accessible only within the cluster
  selector:                   # Defines which Pods this Service targets
    app: web-app              # Must match Pod label (app: web-app)
  ports:
  - protocol: TCP             # Protocol used for communication
    port: 80                  # Port exposed by the Service
    targetPort: 80            # Container port to forward traffic to
```

Apply it:

```bash
kubectl apply -f service.yaml
kubectl get svc -n lab2-app
```

Now you have:

* A **Deployment** managing 3 Pods.
* A **ReplicaSet** maintaining those Pods.
* A **Service** exposing them internally (stable endpoint).

---

### Step 4 – Verify and Access the Deployment

List all resources in the namespace:

```bash
kubectl get all -n lab2-app
```

To test connectivity within the cluster, run:

```bash
kubectl run -it --rm --image=busybox --restart=Never -n lab2-app test-pod -- wget -O- web-service
```

If successful, you should see the **default Nginx welcome page** output in the terminal.

---

### Step 5 – Scale and Update the Deployment

#### 🔹 A. Scaling

Edit the Deployment and change the replicas value from 3 → 5:

```bash
kubectl scale deployment web-deployment --replicas=5 -n lab2-app
```

Check the number of Pods:

```bash
kubectl get pods -n lab2-app
```

#### 🔹 B. Updating the Image Version

Update the Deployment image:

```bash
kubectl set image deployment/web-deployment nginx-container=nginx:1.25 --record -n lab2-app
kubectl rollout status deployment/web-deployment -n lab2-app
```

View rollout history:

```bash
kubectl rollout history deployment/web-deployment -n lab2-app
```

#### 🔹 C. Rollback

If something goes wrong:

```bash
kubectl rollout undo deployment/web-deployment -n lab2-app
```

---

### Step 6 – Clean Up

After testing:

```bash
kubectl delete namespace lab2-app
```

---

## 📝 Evaluation Questions

1. Explain the relationship between Deployment, ReplicaSet, and Pods.
2. Why are Pods considered ephemeral?
3. How does a Service provide stability for Pod access?
4. What is the purpose of the selector in the Service definition?
5. How can you verify that your Service correctly targets the Deployment’s Pods?
6. What command allows you to scale a Deployment?
7. How does a rolling update differ from a rollback?
8. What is the role of the Namespace in resource organization?
9. What would happen if the Service selector didn’t match any Pod labels?
10. Why is `ClusterIP` sufficient for internal application communication?

---

## 📤 Deliverable

Students must submit a short **report (3–5 pages)** containing:

* YAML files (Namespace, Deployment, Service) with inline comments.
* Screenshots of the running Pods, ReplicaSet, Deployment, and Service.
* Results of `kubectl get all -n lab2-app`.
* Answers to all evaluation questions.

---

**✅ End of Lab 2 – Deployments, ReplicaSets, and Services (K3s – 2025 Edition)**

