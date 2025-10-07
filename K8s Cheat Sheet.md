# 📘 Kubernetes Cheat Sheet – Labs 1 to 4 (K3s Edition 2025)

A quick reference guide for managing Kubernetes objects and Services learned through Labs 1–4.  
Compatible with **K3s v1.30+** and **Ubuntu Server 24.04 LTS**.

---

## 🧱 Cluster & Context Management

| Command | Description |
|----------|--------------|
| `kubectl version` | Display client and server versions. |
| `kubectl cluster-info` | Show master and service endpoints. |
| `kubectl get nodes` | List all cluster nodes. |
| `kubectl describe node <node-name>` | Detailed info on a specific node. |
| `kubectl get componentstatuses` | Check status of control plane components. |

---

## 🗂️ Namespace Management

| Command | Description |
|----------|--------------|
| `kubectl get ns` | List all namespaces. |
| `kubectl create namespace <name>` | Create a new namespace. |
| `kubectl delete namespace <name>` | Delete a namespace and its resources. |
| `kubectl config set-context --current --namespace=<name>` | Switch default namespace. |

📘 *Example:*
```bash
kubectl create namespace lab2-app
kubectl config set-context --current --namespace=lab2-app
````

---

## 🧩 Pods

| Command                                  | Description                             |
| ---------------------------------------- | --------------------------------------- |
| `kubectl get pods`                       | List all Pods in the current namespace. |
| `kubectl get pods -o wide`               | Show Pods with IPs and assigned nodes.  |
| `kubectl describe pod <pod-name>`        | Detailed info about a Pod.              |
| `kubectl logs <pod-name>`                | Show container logs inside the Pod.     |
| `kubectl exec -it <pod-name> -- /bin/sh` | Open a shell session inside a Pod.      |
| `kubectl delete pod <pod-name>`          | Delete a Pod.                           |
| `kubectl run <name> --image=<image>`     | Create a single Pod quickly.            |

📘 *Example:*

```bash
kubectl run test-nginx --image=nginx:latest --port=80
```

---

## 🔁 ReplicaSets

| Command                                     | Description                |
| ------------------------------------------- | -------------------------- |
| `kubectl get rs`                            | List ReplicaSets.          |
| `kubectl describe rs <rs-name>`             | Details on a ReplicaSet.   |
| `kubectl scale rs <rs-name> --replicas=<n>` | Scale ReplicaSet manually. |
| `kubectl delete rs <rs-name>`               | Delete ReplicaSet.         |

📘 *Example:*

```bash
kubectl scale rs nginx-rs --replicas=5
```

---

## 🚀 Deployments

| Command                                                       | Description                        |
| ------------------------------------------------------------- | ---------------------------------- |
| `kubectl get deployments`                                     | List all Deployments.              |
| `kubectl describe deployment <name>`                          | Show Deployment details.           |
| `kubectl get pods --selector app=<label>`                     | List Pods managed by a Deployment. |
| `kubectl scale deployment <name> --replicas=<n>`              | Scale up/down a Deployment.        |
| `kubectl set image deployment/<name> <container>=<new-image>` | Update container image version.    |
| `kubectl rollout status deployment/<name>`                    | Check the rollout progress.        |
| `kubectl rollout history deployment/<name>`                   | View Deployment history.           |
| `kubectl rollout undo deployment/<name>`                      | Roll back to the previous version. |
| `kubectl delete deployment <name>`                            | Delete a Deployment.               |

📘 *Example:*

```bash
kubectl set image deployment/web-deployment nginx=nginx:1.26 --record
kubectl rollout undo deployment/web-deployment
```

---

## 🌐 Services

| Command                                                        | Description                         |
| -------------------------------------------------------------- | ----------------------------------- |
| `kubectl get svc`                                              | List all Services in the namespace. |
| `kubectl describe svc <name>`                                  | Details of a Service.               |
| `kubectl expose deployment <name> --type=<type> --port=<port>` | Quickly create a Service.           |
| `kubectl delete svc <name>`                                    | Delete a Service.                   |

### 🧩 Service Types & Usage

| Type           | Description                                              | Example                                                                     |
| -------------- | -------------------------------------------------------- | --------------------------------------------------------------------------- |
| `ClusterIP`    | Default internal-only Service.                           | `kubectl expose deployment web --port=80 --target-port=80 --type=ClusterIP` |
| `NodePort`     | Exposes app on a static port on each Node (30000–32767). | `kubectl expose deployment web --type=NodePort --port=80 --node-port=30080` |
| `LoadBalancer` | Exposes app externally (via cloud or MetalLB).           | `kubectl expose deployment web --type=LoadBalancer --port=80`               |
| `ExternalName` | Maps a DNS name to an external service.                  | YAML-based: `externalName: example.com`                                     |

📘 *Example to access NodePort:*

```bash
curl http://<NODE-IP>:30080
```

---

## 🔎 Labels, Selectors, and Annotations

| Command                                | Description                     |
| -------------------------------------- | ------------------------------- |
| `kubectl get pods --show-labels`       | Display Pods with their labels. |
| `kubectl label pod <pod> key=value`    | Add or modify a label.          |
| `kubectl label pod <pod> key-`         | Remove a label.                 |
| `kubectl annotate pod <pod> key=value` | Add annotation metadata.        |
| `kubectl get pods -l key=value`        | Filter Pods by label.           |

📘 *Example:*

```bash
kubectl label pod nginx-pod env=prod
kubectl get pods -l env=prod
```

---

## 📦 Apply & Manage YAML Manifests

| Command                         | Description                                |
| ------------------------------- | ------------------------------------------ |
| `kubectl apply -f <file>.yaml`  | Create or update a resource.               |
| `kubectl delete -f <file>.yaml` | Delete resources defined in file.          |
| `kubectl get -f <file>.yaml`    | Show applied resources.                    |
| `kubectl explain <resource>`    | Get documentation for a Kubernetes object. |

📘 *Example:*

```bash
kubectl apply -f deployment.yaml
kubectl explain deployment.spec.template
```

---

## 🧰 Useful Shortcuts

| Command                                                                  | Description                                             |
| ------------------------------------------------------------------------ | ------------------------------------------------------- |
| `kubectl get all`                                                        | List all resources (Pods, Deployments, Services, etc.). |
| `kubectl get all -n <namespace>`                                         | List all resources in a namespace.                      |
| `kubectl delete all --all -n <namespace>`                                | Clean all resources in a namespace.                     |
| `kubectl run -it --rm --image=busybox --restart=Never <name> -- /bin/sh` | Launch temporary debugging Pod.                         |
| `kubectl top pod`                                                        | Show resource usage (if Metrics Server is installed).   |

---

## 🧮 Resource Hierarchy Recap

```
Deployment
   └── ReplicaSet
        └── Pods
             └── Containers
```

Each Service type interacts with these resources differently:

```
[ClusterIP]  → Internal access (Pod ↔ Pod)
[NodePort]   → External access via <NodeIP>:<NodePort>
[LoadBalancer] → Public access via external IP (cloud or MetalLB)
[ExternalName] → DNS alias to external service
```

---

## 📚 References

* [Kubernetes Official Docs](https://kubernetes.io/docs/)
* [K3s Documentation](https://docs.k3s.io/)
* [kubectl Cheat Sheet (Upstream)](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

---

**✅ Kubernetes Cheat Sheet – Labs 1–4 **


