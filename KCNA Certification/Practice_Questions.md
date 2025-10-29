# KCNA Practice Questions

A collection of practice questions to reinforce key KCNA concepts.

> 💡 **Tip:** Cover the answers while studying to test your knowledge!

---

## Core Concepts

**1.** What component ensures containers inside Pods are running?  
**Answer:** **kubelet**

---

**2.** Which Kubernetes component stores the cluster state?  
**Answer:** **etcd**

---

**3.** What is the purpose of the kube-scheduler?  
**Answer:** To decide which node each Pod should be placed on.

---

**4.** What type of controller manages stateless apps?  
**Answer:** **Deployment**

---

## Controllers & Workloads

**5.** What is the main purpose of a DaemonSet?  
**Answer:** To ensure that all (or some) nodes run a copy of a Pod.

---

**6.** What's the difference between ConfigMaps and Secrets?  
**Answer:** ConfigMaps hold non-sensitive configuration data, Secrets hold sensitive data (base64-encoded).

---

## Networking

**7.** What is the default Service type in Kubernetes?  
**Answer:** **ClusterIP**

---

**8.** What is a Headless Service used for?  
**Answer:** Direct access to individual Pod IPs (no load balancer).

---

## Tools & Commands

**9.** What command is used to get all resources in the cluster?  
**Answer:** ```bash
kubectl get all --all-namespaces
```

---

**10.** What is Helm?  
**Answer:** A package manager for Kubernetes applications.
