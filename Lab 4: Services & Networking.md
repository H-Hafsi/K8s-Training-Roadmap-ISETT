# 🧪 Lab 4: Services & Networking 

## 🧠 Theoretical Introduction

In Kubernetes, **networking** and **Services** are key components that allow applications to communicate both internally and externally.  
Every Pod receives its own IP address, but Pods are **ephemeral** — they can be created or destroyed at any time. To ensure stable connectivity, Kubernetes introduces the **Service** abstraction, which acts as a consistent access point to a dynamic set of Pods.

### 🔹 Pod Networking Model

Kubernetes follows the **flat networking model**, where:
- Each Pod gets a unique IP address.
- All Pods can communicate with each other without NAT (Network Address Translation).
- The container network interface (**CNI plugin**) manages this connectivity (e.g., Flannel, Calico, Cilium).

In **K3s**, Flannel is the default CNI, providing an overlay network that ensures Pods across different nodes can communicate seamlessly.

---

## 🎯 Learning Objectives

- Understand the Kubernetes networking model and Service abstraction.  
- Configure and test different Service types (`ClusterIP`, `NodePort`, `LoadBalancer`, and `ExternalName`).  
- Explore how traffic flows between clients, Services, and Pods.  
- Identify appropriate use cases for each Service type.  

---

## ⚙️ Environment Setup

| Component | Description |
|------------|-------------|
| **Platform** | VirtualBox or KVM/QEMU |
| **Operating System** | Ubuntu Server 24.04 LTS |
| **Kubernetes Distribution** | K3s v1.30+ |
| **Namespace** | `lab4-networking` |
| **Base Application** | Nginx Web Server (from previous lab) |

Ensure your K3s cluster is up:
```bash
kubectl get nodes
````

---

## 🧪 Lab Steps

### Step 1 – Create a Namespace

`namespace.yaml`

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: lab4-networking    # All networking resources for this lab
```

Apply it:

```bash
kubectl apply -f namespace.yaml
```

---

### Step 2 – Deploy a Web Application

We’ll reuse a simple Nginx Deployment as our application backend.

`web-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
  namespace: lab4-networking
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80   # Exposes HTTP port
```

Apply:

```bash
kubectl apply -f web-deployment.yaml
kubectl get pods -n lab4-networking
```

---

## 🌐 Step 3 – Explore Service Types

### 1️⃣ ClusterIP – Internal Communication

This is the **default Service type**. It exposes the application **only within the cluster**.
Typical use case: internal microservice communication (e.g., backend accessed by frontend Pods).

`service-clusterip.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-clusterip
  namespace: lab4-networking
spec:
  type: ClusterIP             # Default type, internal-only
  selector:
    app: web                  # Targets Pods with this label
  ports:
  - protocol: TCP
    port: 80                  # Port accessible via Service IP
    targetPort: 80            # Port exposed by the container
```

Apply:

```bash
kubectl apply -f service-clusterip.yaml
kubectl get svc -n lab4-networking
```

**Test:**
Run a test Pod inside the same namespace:

```bash
kubectl run -it --rm --image=busybox --restart=Never -n lab4-networking test -- wget -O- web-clusterip
```

✅ Should display the Nginx welcome page.

---

### 2️⃣ NodePort – External Access via Node IP

A `NodePort` Service exposes the application on **a static port (30000–32767)** on every cluster node.
Clients can access the app using `<NodeIP>:<NodePort>`.

Use case: exposing small apps for testing or development environments.

`service-nodeport.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-nodeport
  namespace: lab4-networking
spec:
  type: NodePort              # Exposes app externally via each Node’s IP
  selector:
    app: web
  ports:
  - protocol: TCP
    port: 80                  # Internal cluster port
    targetPort: 80            # Container port
    nodePort: 30080           # External port exposed on all nodes
```

Apply and test:

```bash
kubectl apply -f service-nodeport.yaml
kubectl get svc -n lab4-networking
```

Access via your browser or curl:

```
http://<NODE-IP>:30080
```

---

### 3️⃣ LoadBalancer – External Access via Cloud or Local LB

In cloud environments (or locally with MetalLB), the `LoadBalancer` type automatically provisions an external IP that routes traffic to the Service.

Use case: production-grade exposure of front-end or public APIs.

`service-loadbalancer.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-loadbalancer
  namespace: lab4-networking
spec:
  type: LoadBalancer          # External exposure via load balancer
  selector:
    app: web
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```

Apply:

```bash
kubectl apply -f service-loadbalancer.yaml
kubectl get svc -n lab4-networking
```

> Note: In local K3s, you can install **MetalLB** to simulate external LoadBalancer behavior.

---

### 4️⃣ ExternalName – Mapping to External DNS

This Service type doesn’t forward traffic to Pods. Instead, it maps a name inside the cluster to an external DNS name.

Use case: accessing external services (e.g., databases or APIs) with a Kubernetes-style name.

`service-externalname.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: external-docs
  namespace: lab4-networking
spec:
  type: ExternalName
  externalName: www.example.com   # Redirects DNS lookup
```

Apply and test:

```bash
kubectl apply -f service-externalname.yaml
kubectl get svc -n lab4-networking
```

Within a Pod:

```bash
kubectl run -it --rm --image=busybox -n lab4-networking test -- nslookup external-docs
```

✅ Output should resolve `external-docs.lab4-networking.svc.cluster.local` to `www.example.com`.

---

## 🧩 Step 4 – Visualize Network Flow

```
[ Client ] → [ Service (NodePort or LB) ] → [ ClusterIP Service ] → [ Pods ]
```

Each layer abstracts lower-level complexity:

* **Pods**: handle the actual workload.
* **ClusterIP**: routes internal traffic.
* **NodePort / LoadBalancer**: exposes the app externally.
* **ExternalName**: provides DNS-level redirection.

---

## 📝 Evaluation Questions

1. What problem do Services solve in Kubernetes networking?
2. How does ClusterIP differ from NodePort?
3. What is the typical use case for an ExternalName Service?
4. Why do Pods require a Service for stable communication?
5. What port range does NodePort use by default?
6. How does the CNI plugin (Flannel) enable Pod-to-Pod communication?
7. In which situation would you use a LoadBalancer instead of NodePort?
8. How does label selection connect Services to Deployments?
9. What is the DNS format for accessing a Service within a namespace?
10. Describe the traffic flow from an external user to a Pod using NodePort.

---

## 📤 Deliverable

Students must submit a **4–6 page lab report** containing:

* YAML files for each Service type.
* Diagrams showing internal and external network flow.
* Screenshots of Service tables (`kubectl get svc -n lab4-networking`).
* A summary comparing ClusterIP, NodePort, LoadBalancer, and ExternalName.
* Answers to all evaluation questions.

---

**✅ End of Lab 4 – Services & Networking **
