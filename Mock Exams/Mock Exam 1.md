# CKA Practice Questions and Commands

## Question 1
### Deploy a pod named nginx-pod using the nginx:alpine image.
- Name: nginx-pod  
- Image: nginx:alpine  

```bash
kubectl run nginx-pod --image=nginx:alpine --restart=Never
```

> Note: Specifying `--restart=Never` ensures that Kubernetes creates a Pod rather than a Deployment.

---
## Question 2
### Deploy a messaging pod using the redis:alpine image with the labels set to tier=msg.
- Pod Name: messaging  
- Image: redis:alpine  
- Labels: tier=msg  

```bash
kubectl run messaging --image=redis:alpine --labels=tier=msg
```

To verify:
```bash
kubectl get pods --show-labels
```

---
## Question 3
### Create a namespace named apx-x9984574.
- Namespace: apx-x9984574  

```bash
kubectl create namespace apx-x9984574
```

> A Kubernetes namespace logically divides a cluster into isolated environments.

---
## Question 4
### Get the list of nodes in JSON format and store it in a file at `/opt/outputs/nodes-z3444kd9.json`

```bash
kubectl get nodes -o json > /opt/outputs/nodes-z3444kd9.json
```

To view the contents:
```bash
cat /opt/outputs/nodes-z3444kd9.json
```

---
## Question 5
### Create a service messaging-service to expose the messaging application within the cluster on port 6379.
- Service: messaging-service  
- Port: 6379  
- Type: ClusterIP  

```bash
kubectl expose pod messaging --name=messaging-service --port=6379 --target-port=6379 --type=ClusterIP --selector=tier=msg
```

---
## Question 6
### Create a deployment named hr-web-app using the image kodekloud/webapp-color with 2 replicas.
- Name: hr-web-app  
- Image: kodekloud/webapp-color  
- Replicas: 2  

```bash
kubectl create deployment hr-web-app --image=kodekloud/webapp-color --replicas=2
```

---
## Question 7
### Create a static pod named static-busybox on the control plane node.
- Name: static-busybox  
- Image: busybox  

A static pod must be defined in `/etc/kubernetes/manifests/`.

```bash
cat <<EOF | sudo tee /etc/kubernetes/manifests/static-busybox.yaml
apiVersion: v1
kind: Pod
metadata:
  name: static-busybox
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["sleep", "1000"]
EOF
```

To verify:
```bash
kubectl get pods -A | grep static-busybox
```

---
## Question 8
### Create a POD in the finance namespace named temp-bus with the image redis:alpine.
- Name: temp-bus  
- Image: redis:alpine  

```bash
kubectl run temp-bus --image=redis:alpine --namespace=finance
```

---
## Question 9
### Identify and fix issues in a newly deployed application named orange.
```bash
kubectl get pod orange -n default -o yaml
kubectl edit deployment orange -n default
```

---
## Question 10
### Expose the hr-web-app as a NodePort service accessible on port 30082.
- Name: hr-web-app-service  
- Type: NodePort  
- Port: 8080  
- NodePort: 30082  

```bash
kubectl expose deployment hr-web-app --name=hr-web-app-service --port=8080 --target-port=8080 --type=NodePort
kubectl patch svc hr-web-app-service -p '{"spec": {"ports": [{"port": 8080, "targetPort": 8080, "nodePort": 30082}],"type": "NodePort"}}'
```

To verify:
```bash
kubectl get svc hr-web-app-service
kubectl describe svc hr-web-app-service
```

Test the service:
```bash
curl http://<NODE-IP>:30082
```

---
## Question 11
### Retrieve osImages of all nodes and store in `/opt/outputs/nodes_os_x43kj56.txt`

```bash
kubectl get nodes -o jsonpath='{.items[*].status.nodeInfo.osImage}' > /opt/outputs/nodes_os_x43kj56.txt
```

Verify:
```bash
cat /opt/outputs/nodes_os_x43kj56.txt
```

---
## Question 12
### Create a Persistent Volume with the given specification.
- Volume name: pv-analytics  
- Storage: 100Mi  
- Access mode: ReadWriteMany  
- Host path: /pv/data-analytics  

```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-analytics
spec:
  capacity:
    storage: 100Mi
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /pv/data-analytics
EOF
```

To verify:
```bash
kubectl get pv pv-analytics
kubectl describe pv pv-analytics
```
