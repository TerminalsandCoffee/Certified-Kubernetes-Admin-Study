# KCNA Common Terms Glossary

A quick reference of essential Kubernetes and Cloud Native terms — written in plain English.

---

## Core Concepts

| Term | Definition |
|------|-------------|
| **Pod** | The smallest deployable unit in Kubernetes; a wrapper around one or more containers. |
| **Deployment** | A controller that manages Pods and ReplicaSets; enables rolling updates and scaling. |
| **ReplicaSet** | Ensures the desired number of identical Pods are running at all times. |
| **Service** | Exposes a set of Pods on a stable IP and DNS name for reliable networking. |
| **Namespace** | Logical isolation within a cluster, like separate "folders" for workloads. |

---

## Controller Types

| Term | Definition |
|------|-------------|
| **DaemonSet** | Ensures a copy of a Pod runs on each node (e.g., log collectors). |
| **StatefulSet** | Manages Pods that need stable, unique identities and persistent storage. |

---

## Storage & Configuration

| Term | Definition |
|------|-------------|
| **ConfigMap** | Non-sensitive key/value configuration data injected into Pods. |
| **Secret** | Sensitive configuration data like passwords or tokens (base64 encoded). |
| **PersistentVolumeClaim (PVC)** | A user's request for storage — bound to a PersistentVolume. |

---

## Networking

| Term | Definition |
|------|-------------|
| **Ingress** | Manages external HTTP/HTTPS access to Services inside the cluster. |
| **CNI (Container Network Interface)** | Defines how Pods connect to each other across nodes. |

---

## Cluster Architecture

| Term | Definition |
|------|-------------|
| **kubelet** | The node agent that ensures containers are running as specified. |
| **kube-scheduler** | Decides which node a Pod should run on based on resource availability. |
| **etcd** | Key-value store that holds the entire cluster state. |

---

## Security & Access Control

| Term | Definition |
|------|-------------|
| **RBAC (Role-Based Access Control)** | Defines who can do what in the cluster. |

---

## Scaling & Autoscaling

| Term | Definition |
|------|-------------|
| **Cluster Autoscaler** | Automatically adds/removes nodes based on workload demand. |
| **HPA (Horizontal Pod Autoscaler)** | Scales Pods based on CPU/memory utilization. |

---

## Tools & Utilities

| Term | Definition |
|------|-------------|
| **kubectl** | Command-line tool to interact with the Kubernetes API server. |
| **Minikube** | Local single-node Kubernetes cluster for learning and testing. |
