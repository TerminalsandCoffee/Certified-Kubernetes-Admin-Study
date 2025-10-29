# Project 1: Install Kubernetes with Minikube & Deploy a Simple Pod

**Focus:** Installing Kubernetes locally, verifying it runs, and deploying your first Pod.

## Objective
Set up a local Kubernetes cluster with **Minikube** and deploy your first Pod using a simple YAML manifest.

---

## 1. Install Minikube

```bash
# Start Minikube
minikube start 

# Verify cluster status
kubectl cluster-info
kubectl get nodes
kubectl get pods
```

## 2. Create a Simple Pod

Create a file named `simple-pod.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-pod
spec:
  containers:
  - name: hello-container
    image: nginx
    ports:
    - containerPort: 80
```

Deploy it: 

```bash
kubectl apply -f simple-pod.yaml
kubectl get pods
kubectl describe pod hello-pod
```

## 3. Verify Access

```bash
kubectl port-forward pod/hello-pod 8080:80
```

Now open your browser at http://localhost:8080 — you should see the NGINX welcome page.


| Concept           | Description                                          |
| ----------------- | ---------------------------------------------------- |
| **Pod**           | Smallest deployable unit — wraps containers.         |
| **kubectl apply** | Declarative way to create/manage Kubernetes objects. |
| **Minikube**      | Local single-node Kubernetes cluster for testing.    |


---

# Project 2: Deployment with ReplicaSet and Persistent Storage

**Focus:** Scaling, persistence, and showing real Kubernetes control.

## Objective
Deploy a simple web app using a **Deployment**, expose it with a **Service**, and attach **Persistent Storage** using a PVC.

---

## 1. Create the Deployment

Create a file named `web-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
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
      - name: web
        image: nginx
        ports:
        - containerPort: 80
        volumeMounts:
        - name: html-volume
          mountPath: /usr/share/nginx/html
      volumes:
      - name: html-volume
        persistentVolumeClaim:
          claimName: web-pvc
```

## 2. Create the Persistent Volume and PVC

Create a file named `pvc.yaml`:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: web-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

Deploy both:

```bash
kubectl apply -f pvc.yaml
kubectl apply -f web-deployment.yaml
```

## 3. Expose the App

```bash
kubectl expose deployment web-deployment --port=80 --type=NodePort
kubectl get svc
```

## 4. Verify

```bash
minikube service web-deployment
```

---

| Concept                         | Description                                          |
| ------------------------------- | ---------------------------------------------------- |
| **Deployment**                  | Manages replicas and updates of Pods.                |
| **ReplicaSet**                  | Ensures the desired number of Pods are running.      |
| **PVC (PersistentVolumeClaim)** | Requests storage resources dynamically.              |
| **Service**                     | Exposes an application to internal/external traffic. |


You’ve deployed and exposed a scalable, persistent web app — all within a Minikube cluster.
This demonstrates a clear understanding of Kubernetes fundamentals: compute, networking, and storage.

## 🧹 Cleanup (Optional)

```bash
# Stop the cluster
minikube stop

# Or delete it entirely
minikube delete
```
Stopping keeps the cluster for next time; deleting wipes it clean so you can start fresh.