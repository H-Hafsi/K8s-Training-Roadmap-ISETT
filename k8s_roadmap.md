# 🚀 Kubernetes Learning Labs Roadmap
## DevOps Master's Program | Free & Open Source

[![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![K3s](https://img.shields.io/badge/K3s-FFC61C?style=for-the-badge&logo=k3s&logoColor=black)](https://k3s.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

> 📚 A comprehensive, hands-on learning path for mastering Kubernetes using **100% free and open-source tools**

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Prerequisites](#-prerequisites)
- [Phase 1: Foundations](#-phase-1-foundations-weeks-1-3)
- [Phase 2: Networking & Service Discovery](#-phase-2-networking--service-discovery-weeks-4-5)
- [Phase 3: Configuration & Storage](#-phase-3-configuration--storage-weeks-6-7)
- [Phase 4: Advanced Workloads](#-phase-4-advanced-workloads-weeks-8-9)
- [Phase 5: Observability & Debugging](#-phase-5-observability--debugging-weeks-10-11)
- [Phase 6: Security & Governance](#-phase-6-security--governance-weeks-12-13)
- [Phase 7: CI/CD & GitOps](#-phase-7-cicd--gitops-weeks-14-15)
- [Phase 8: Advanced Topics](#-phase-8-advanced-topics-weeks-16-18)
- [Final Project](#-final-project-weeks-19-20)
- [Assessment Strategy](#-assessment-strategy)
- [Infrastructure Requirements](#-infrastructure-requirements)
- [Recommended Tools](#-recommended-tools)

---

## 🎯 Overview

This roadmap provides a progressive, hands-on approach to mastering Kubernetes for graduate-level DevOps students. The curriculum emphasizes practical skills using enterprise-grade, open-source tools that can run entirely on local infrastructure.

**Duration:** 20 weeks  
**Level:** Master's/Graduate  
**Cost:** 100% Free & Open Source

---

## ✅ Prerequisites

### Required Skills
- ✔️ Linux system administration
- ✔️ Docker and containerization (images, Docker Compose, networking, volumes)
- ✔️ Basic networking (TCP/IP, DNS, HTTP/HTTPS)
- ✔️ YAML syntax
- ✔️ Git version control

### Recommended Skills
- 📌 Basic scripting (Bash/Python)
- 📌 Virtual machine management (VirtualBox/KVM)

---

## 📚 Phase 1: Foundations (Weeks 1-3)

### 🧪 Lab 1: Kubernetes Architecture & Multi-Distribution Setup

**Objectives:** Understand K8s components, architecture, and distribution options

**Activities:**
- Deploy single-node K3s cluster on local VM
- Deploy multi-node K3s cluster (1 server, 2 agents)
- Deploy minikube for comparison
- Explore control plane components
  - API server
  - etcd
  - Scheduler
  - Controller manager
- Examine node components
  - kubelet
  - kube-proxy
  - Container runtime
- Compare K3s vs minikube vs standard K8s
- Master kubectl commands and cluster inspection

**Infrastructure:** VirtualBox or KVM/QEMU VMs

**Deliverable:** 📄 Documentation comparing cluster architectures with diagrams

---

### 🧪 Lab 2: Core Workload Resources

**Objectives:** Master fundamental K8s objects

**Activities:**
- Deploy Pods, ReplicaSets, and Deployments on K3s
- Implement rolling updates and rollbacks
- Configure resource requests and limits
- Work with Labels, Selectors, and Annotations
- Understand pod lifecycle and phases

**Tools:** `kubectl`, YAML manifests, Git

**Deliverable:** 🔧 Automated deployment pipeline with version control

---

### 🧪 Lab 3: Namespaces & Organization

**Objectives:** Structure cluster resources logically

**Activities:**
- Create and manage multiple namespaces
- Implement resource organization strategies
- Configure default namespace behaviors
- Use kubectl contexts for namespace management

**Tools:** `kubectl`, `kubectx`, `kubens`

**Deliverable:** 📊 Multi-environment namespace strategy (dev/staging/prod)

---

## 🌐 Phase 2: Networking & Service Discovery (Weeks 4-5)

### 🧪 Lab 4: Services & Networking

**Objectives:** Understand K8s networking model

**Activities:**
- Create ClusterIP, NodePort, and LoadBalancer services
- Implement service discovery (DNS, environment variables)
- Explore K3s networking with Flannel
- Configure Network Policies with Calico
- Test pod-to-pod and service-to-service communication

**Tools:** K3s + Flannel, Calico, curl/wget

**Deliverable:** 🔒 Secure microservices architecture with network segmentation

---

### 🧪 Lab 5: Ingress & API Gateway

**Objectives:** Expose applications externally

**Activities:**
- Work with K3s built-in Traefik ingress controller
- Deploy and compare NGINX ingress controller
- Implement path-based and host-based routing
- Configure TLS/SSL with self-signed certificates
- Use cert-manager for certificate automation
- Set up rate limiting and basic authentication

**Tools:** Traefik, NGINX Ingress, cert-manager, OpenSSL

**Deliverable:** 🌍 Production-ready ingress configuration for multi-tenant app

---

## ⚙️ Phase 3: Configuration & Storage (Weeks 6-7)

### 🧪 Lab 6: Configuration Management

**Objectives:** Externalize application configuration

**Activities:**
- Create and manage ConfigMaps and Secrets
- Mount configurations as volumes and environment variables
- Implement Sealed Secrets (Bitnami)
- Work with secret rotation strategies
- Handle sensitive data securely

**Tools:** `kubectl`, Sealed Secrets controller, `kubeseal`

**Deliverable:** 📦 12-factor app with externalized configuration

---

### 🧪 Lab 7: Persistent Storage

**Objectives:** Manage stateful workloads

**Activities:**
- Understand PersistentVolumes and PersistentVolumeClaims
- Work with K3s local-path provisioner
- Deploy Longhorn distributed storage
- Configure StorageClasses and dynamic provisioning
- Deploy StatefulSets for databases (PostgreSQL, MySQL)
- Implement backup strategies

**Tools:** K3s local-path, Longhorn, Velero

**Deliverable:** 💾 Highly available database deployment with persistent storage

---

## 🚀 Phase 4: Advanced Workloads (Weeks 8-9)

### 🧪 Lab 8: Jobs, CronJobs & DaemonSets

**Objectives:** Handle specialized workload patterns

**Activities:**
- Create batch processing Jobs with parallelism
- Schedule recurring tasks with CronJobs
- Deploy cluster-wide agents using DaemonSets
- Implement cleanup and retention policies
- Handle job failures and retries

**Tools:** `kubectl`, cron expressions, custom scripts

**Deliverable:** 🔄 ETL pipeline with scheduled data processing

---

### 🧪 Lab 9: Autoscaling & Resource Optimization

**Objectives:** Implement elastic scaling

**Activities:**
- Deploy metrics-server on K3s
- Configure Horizontal Pod Autoscaler (HPA)
- Deploy Vertical Pod Autoscaler (VPA)
- Use custom metrics with Prometheus adapter
- Optimize resource allocation
- Load testing with k6

**Tools:** metrics-server, VPA, Prometheus adapter, k6

**Deliverable:** 📈 Auto-scaling application responding to load patterns

---

## 🔍 Phase 5: Observability & Debugging (Weeks 10-11)

### 🧪 Lab 10: Monitoring & Logging

**Objectives:** Implement comprehensive observability

**Activities:**
- Deploy Prometheus and Grafana stack on K3s
- Deploy kube-state-metrics and node-exporter
- Configure ServiceMonitors and AlertManager
- Implement centralized logging with Grafana Loki
- Deploy Promtail for log collection
- Create custom dashboards and alerts
- Monitor K3s-specific metrics

**Tools:** Prometheus, Grafana, Loki, Promtail, AlertManager

**Deliverable:** 📊 Complete observability platform with SLI/SLO dashboards

---

### 🧪 Lab 11: Troubleshooting & Debugging

**Objectives:** Develop debugging proficiency

**Activities:**
- Debug failing pods and deployments
- Analyze logs and events effectively
- Use `kubectl debug` and ephemeral containers
- Perform network troubleshooting with netshoot
- Investigate resource contention issues
- Use k9s for interactive cluster management
- Debug K3s-specific components

**Tools:** `kubectl`, k9s, stern, netshoot container

**Deliverable:** 📖 Troubleshooting playbook with common scenarios

---

## 🔐 Phase 6: Security & Governance (Weeks 12-13)

### 🧪 Lab 12: Security Best Practices

**Objectives:** Harden cluster security

**Activities:**
- Implement RBAC (Roles, RoleBindings, ClusterRoles)
- Configure Pod Security Standards/Admission
- Use Security Context and seccomp profiles
- Scan images with Trivy
- Implement OPA/Gatekeeper policies
- Use Falco for runtime security
- Secure K3s installation

**Tools:** Trivy, OPA Gatekeeper, Falco, kube-bench

**Deliverable:** 🛡️ Security-hardened cluster with compliance documentation

---

### 🧪 Lab 13: Multi-tenancy & Resource Quotas

**Objectives:** Manage shared cluster resources

**Activities:**
- Configure Namespaces with ResourceQuotas
- Implement LimitRanges
- Set up NetworkPolicies for namespace isolation
- Configure PodDisruptionBudgets
- Create tenant isolation strategies
- Use Hierarchical Namespaces

**Tools:** `kubectl`, Hierarchical Namespace Controller (HNC)

**Deliverable:** 🏢 Multi-tenant cluster design with isolation guarantees

---

## 🔄 Phase 7: CI/CD & GitOps (Weeks 14-15)

### 🧪 Lab 14: CI/CD Integration

**Objectives:** Automate deployment pipelines

**Activities:**
- Build CI/CD pipeline with GitLab CE or GitHub Actions
- Alternative: Jenkins or Tekton
- Implement automated testing in K3s environments
- Create blue-green and canary deployments
- Integrate Trivy security scanning in pipeline
- Use K3s in CI for integration testing

**Tools:** GitLab CE/GitHub Actions/Jenkins, Tekton, Trivy

**Deliverable:** 🔧 End-to-end CI/CD pipeline with automated deployments

---

### 🧪 Lab 15: GitOps with ArgoCD or Flux

**Objectives:** Implement declarative deployment

**Activities:**
- Deploy ArgoCD or FluxCD on K3s
- Structure Git repositories for GitOps
- Implement automated sync and reconciliation
- Configure progressive delivery strategies
- Manage multi-environment deployments
- Use Helm charts

**Tools:** ArgoCD or Flux, Helm, Git

**Deliverable:** 🎯 GitOps-managed production environment

---

## 🎓 Phase 8: Advanced Topics (Weeks 16-18)

### 🧪 Lab 16: Service Mesh (Linkerd)

**Objectives:** Implement advanced microservices patterns

**Activities:**
- Deploy Linkerd service mesh
- Configure traffic management and circuit breaking
- Implement mTLS between services
- Use distributed tracing with Jaeger
- Monitor mesh performance with Linkerd viz

**Tools:** Linkerd, Jaeger

**Alternative:** Istio or Cilium Service Mesh

**Deliverable:** 🕸️ Mesh-enabled application with observability

---

### 🧪 Lab 17: Operators & Custom Resources

**Objectives:** Extend Kubernetes functionality

**Activities:**
- Understand Operator pattern
- Create Custom Resource Definitions (CRDs)
- Build simple operator with Kubebuilder
- Alternative: Operator SDK
- Deploy community operators (PostgreSQL, Redis, RabbitMQ)
- Manage operator lifecycle with OLM

**Tools:** Kubebuilder, Operator SDK, OLM

**Deliverable:** 🤖 Custom operator managing application lifecycle

---

### 🧪 Lab 18: Disaster Recovery & High Availability

**Objectives:** Ensure business continuity

**Activities:**
- Implement Velero for cluster backup on K3s
- Use MinIO as S3-compatible storage backend
- Configure backup schedules and retention
- Set up high-availability K3s cluster (embedded etcd, 3+ servers)
- Perform disaster recovery drills
- Document RTO/RPO strategies
- Test etcd backup and restore

**Tools:** Velero, MinIO, K3s HA configuration

**Deliverable:** 🔄 Comprehensive DR plan with tested procedures

---

## 🏆 Final Project (Weeks 19-20)

### 🎯 Capstone: Production-Grade Platform on K3s

**Requirements:**

| Component | Details |
|-----------|---------|
| **Infrastructure** | HA K3s cluster (3+ servers, 2+ agents) |
| **Application** | Multi-tier application (frontend, backend, database) |
| **CI/CD** | Complete pipeline with GitOps (ArgoCD/Flux) |
| **Observability** | Prometheus, Grafana, Loki stack |
| **Security** | RBAC, OPA, Trivy, Falco implementation |
| **Scaling** | HPA with multiple replicas |
| **Storage** | Longhorn distributed storage |
| **Backup** | Velero + MinIO disaster recovery |
| **IaC** | Terraform or Ansible for K3s deployment |
| **Service Mesh** | Linkerd implementation |
| **Documentation** | Complete runbooks and architecture docs |

**All tools must be free and open-source**

**Deliverable:** 🚀 Fully functional production-ready Kubernetes platform with presentation

---

## 📊 Assessment Strategy

| Component | Weight | Description |
|-----------|--------|-------------|
| **Weekly Labs** | 40% | Hands-on exercises with lab reports |
| **Mid-term Exam** | 20% | Theory, architecture, troubleshooting |
| **Final Project** | 30% | Capstone platform implementation |
| **Participation** | 10% | Documentation and collaboration |

---

## 💻 Infrastructure Requirements

### Student Hardware

**Minimum Requirements:**
- 💾 8GB RAM (16GB recommended)
- ⚡ 4 CPU cores
- 💿 50GB free disk space
- 🖥️ Linux OS (Ubuntu/Fedora) or Windows with VirtualBox/WSL2

### Virtual Machine Options (All Free)

| Tool | Platform | Description |
|------|----------|-------------|
| **VirtualBox** | Cross-platform | Full-featured virtualization |
| **KVM/QEMU** | Linux | Native Linux virtualization |
| **Multipass** | Cross-platform | Lightweight Ubuntu VMs |
| **Vagrant** | Cross-platform | VM automation |

### Lab Infrastructure Setup

| Configuration | VMs | RAM per VM | CPUs per VM |
|---------------|-----|------------|-------------|
| **Single-node K3s** | 1 | 2GB | 2 |
| **Multi-node K3s** | 3-5 | 2GB | 2 |

---

## 🛠️ Recommended Tools

### Core Kubernetes

| Tool | Category | License |
|------|----------|---------|
| **K3s** | Lightweight K8s | Apache 2.0 |
| **Minikube** | Local K8s | Apache 2.0 |
| **kubectl** | CLI | Apache 2.0 |
| **Helm** | Package Manager | Apache 2.0 |

### Development & IDE

| Tool | Purpose | License |
|------|---------|---------|
| **VS Code** | IDE | MIT |
| **k9s** | Terminal UI | Apache 2.0 |
| **kubectx/kubens** | Context switching | Apache 2.0 |
| **stern** | Log tailing | Apache 2.0 |

### Networking

| Tool | Purpose | Built-in K3s |
|------|---------|--------------|
| **Flannel** | CNI | ✅ |
| **Calico** | Network Policy | ❌ |
| **Traefik** | Ingress | ✅ |
| **NGINX Ingress** | Ingress | ❌ |

### Storage

| Tool | Purpose | License |
|------|---------|---------|
| **local-path** | Local storage | Apache 2.0 |
| **Longhorn** | Distributed storage | Apache 2.0 |
| **MinIO** | S3-compatible | AGPL v3 |

### Observability

| Tool | Purpose | License |
|------|---------|---------|
| **Prometheus** | Metrics | Apache 2.0 |
| **Grafana** | Visualization | AGPL v3 |
| **Loki** | Log aggregation | AGPL v3 |
| **Promtail** | Log collection | Apache 2.0 |
| **Jaeger** | Tracing | Apache 2.0 |
| **AlertManager** | Alerting | Apache 2.0 |

### Security

| Tool | Purpose | License |
|------|---------|---------|
| **Trivy** | Vulnerability scanning | Apache 2.0 |
| **OPA/Gatekeeper** | Policy engine | Apache 2.0 |
| **Falco** | Runtime security | Apache 2.0 |
| **kube-bench** | CIS benchmark | Apache 2.0 |
| **Sealed Secrets** | Secret encryption | Apache 2.0 |

### CI/CD & GitOps

| Tool | Purpose | License |
|------|---------|---------|
| **GitLab CE** | Self-hosted Git+CI/CD | MIT |
| **GitHub Actions** | CI/CD (free tier) | - |
| **Jenkins** | Automation server | MIT |
| **Tekton** | Cloud-native CI/CD | Apache 2.0 |
| **ArgoCD** | GitOps | Apache 2.0 |
| **Flux** | GitOps | Apache 2.0 |

### Service Mesh

| Tool | Type | License |
|------|------|---------|
| **Linkerd** | Lightweight | Apache 2.0 |
| **Istio** | Feature-rich | Apache 2.0 |
| **Cilium** | eBPF-based | Apache 2.0 |

### Operators & Extensions

| Tool | Purpose | License |
|------|---------|---------|
| **Kubebuilder** | Operator framework | Apache 2.0 |
| **Operator SDK** | Operator framework | Apache 2.0 |
| **OLM** | Operator lifecycle | Apache 2.0 |

### Backup & DR

| Tool | Purpose | License |
|------|---------|---------|
| **Velero** | Backup/restore | Apache 2.0 |

### IaC & Automation

| Tool | Purpose | License |
|------|---------|---------|
| **Terraform** | Infrastructure as code | MPL 2.0 |
| **Ansible** | Configuration mgmt | GPL v3 |
| **k3sup** | K3s installer | MIT |

### Testing & Load Generation

| Tool | Purpose | License |
|------|---------|---------|
| **k6** | Load testing | AGPL v3 |
| **Apache Bench** | HTTP benchmarking | Apache 2.0 |

---

## 📝 Notes

> ⚠️ **Important:** This curriculum uses exclusively free and open-source tools. No paid cloud services or commercial software required.

> 💡 **Tip:** Students can collaborate using university lab infrastructure or personal computers with virtualization.

> 🎓 **Certification Path:** This curriculum prepares students for CKA (Certified Kubernetes Administrator) and CKAD (Certified Kubernetes Application Developer) certifications.

---

## 📚 Additional Resources

- [Official Kubernetes Documentation](https://kubernetes.io/docs/)
- [K3s Documentation](https://docs.k3s.io/)
- [CNCF Landscape](https://landscape.cncf.io/)
- [Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)

---

## 📄 License

This curriculum is released under the MIT License. Feel free to use, modify, and distribute.

---

## 🤝 Contributing

Contributions are welcome! Please submit issues or pull requests for improvements.

---

**Created for DevOps Master's Students | 100% Free & Open Source | Last Updated: 2025**