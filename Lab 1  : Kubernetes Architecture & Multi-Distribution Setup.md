# 🧪 Lab 1: Kubernetes Architecture & Multi-Distribution Setup 

## 🧠 Theoretical Introduction

Kubernetes (often abbreviated **K8s**) is an open-source container orchestration platform designed to automate the deployment, scaling, and management of containerized applications.  
Its architecture is divided into two main layers: the **Control Plane** and the **Worker Nodes**.

### Control Plane Components
- **API Server:** Entry point for all administrative operations and cluster communications.  
- **etcd:** Key-value database storing the cluster state.  
- **Scheduler:** Determines on which node each pod should run.  
- **Controller Manager:** Maintains desired cluster state via control loops.

### Node Components
- **kubelet:** Agent ensuring containers are running as defined.  
- **kube-proxy:** Handles network routing between pods and services.  
- **Container Runtime:** Runs containers (containerd, Docker, etc.).

Several Kubernetes **distributions** exist:
- **K3s:** Lightweight and optimized for low-resource environments.  
- **Minikube:** Local distribution for testing and learning.  
- **Standard Kubernetes (kubeadm):** Full-scale deployment used in production.

This first lab aims to **understand and compare Kubernetes architectures and distributions** while setting up practical environments using K3s and Minikube.

---

## 🎯 Learning Objectives

- Understand the key components of Kubernetes architecture.  
- Deploy a single-node and a multi-node K3s cluster.  
- Install and explore Minikube.  
- Identify Control Plane and Node components.  
- Use `kubectl` to inspect and manage cluster resources.


---

## ⚙️ Environment Setup

| Resource | Description |
|-----------|--------------|
| **Platform** | VirtualBox 7.x or KVM/QEMU |
| **Operating System** | Ubuntu Server 24.04 LTS (Noble Numbat) |
| **VMs** | 1 (single-node) / 3 (multi-node) |
| **Specs per VM** | 2 vCPU / 2 GB RAM / 20 GB disk |
| **K3s Version** | v1.30+ (latest stable) |
| **Minikube Version** | v1.34+ (latest) |
| **kubectl Version** | v1.30+ |
| **Network** | NAT or Host-Only network |

---

## 🧪 Lab Steps

### Step 1 – Deploy a Single-Node K3s Cluster

1. Create a VM and install **Ubuntu Server 24.04 LTS**.  
2. Update the system:

        sudo apt update && sudo apt upgrade -y

3. Install K3s using the official script:

        curl -sfL https://get.k3s.io | sh -

4. Verify the cluster status:

        kubectl get nodes
        kubectl get pods -A

5. Observe the `kube-system` namespace to identify Control Plane components.

---

### Step 2 – Deploy a Multi-Node K3s Cluster

1. Prepare one **server** VM and two **agent** VMs.  
2. On the server node, retrieve the join token:

        sudo cat /var/lib/rancher/k3s/server/node-token

3. On each agent node, join the cluster:

        curl -sfL https://get.k3s.io | K3S_URL=https://<MASTER_IP>:6443 K3S_TOKEN=<TOKEN> sh -

4. Confirm that all nodes have joined successfully:

        kubectl get nodes

---

### Step 3 – Install Minikube for Comparison

1. Install Minikube (latest stable v1.34+):

        curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
        sudo install minikube-linux-amd64 /usr/local/bin/minikube

2. Start the cluster:

        minikube start --driver=kvm2

3. Compare resource usage and network setup with K3s.

---

### Step 4 – Explore Cluster Components

1. List system pods:

        kubectl get pods -n kube-system

2. View detailed node information:

        kubectl describe node <node-name>
        kubectl get componentstatuses

3. Identify which pods belong to the Control Plane vs Worker Nodes.

---

### Step 5 – Documentation & Diagram

- Draw an architecture diagram for both single-node and multi-node clusters.  
- Indicate component interactions and network ports.  
- Write a short comparison report: **K3s vs Minikube vs Standard Kubernetes**.

---

## 📝 Evaluation Questions

1. What are the main differences between K3s and standard Kubernetes?  
2. What is the role of the `kubelet`?  
3. Why is K3s considered lightweight?  
4. How does K3s simplify Control Plane configuration?  
5. What is the difference between a Pod and a Node?  
6. Which command checks component health in the cluster?  
7. How can a worker node verify its registration with the master?  
8. What are the pros and cons of Minikube compared to K3s?  
9. Explain the difference between `kube-proxy` and a CNI plugin like Flannel.  
10. Illustrate communication between the Control Plane and a pod on a worker node.

---

## 📤 Deliverable

Students must submit a **3–5 page lab report** containing:

- Description of the setup (VMs, OS, versions).  
- Screenshots of operational clusters.  
- Architecture diagram.  
- Comparison table (K3s, Minikube, K8s).  
- Answers to the evaluation questions.

---

**✅ End of Lab 1 **
