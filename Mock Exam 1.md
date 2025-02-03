# CKA Practice Questions and Commands

## Question 1
### Deploy a pod named nginx-pod using the nginx:alpine image.
- Name: nginx-pod  
- Image: nginx:alpine  

```bash
kubectl run nginx-pod --image=nginx:alpine
```

> - `kubectl` lists all nodes in the cluster.
> - `run` creates and runs a pod with a single container.
> - `nginx-pod` is the name being assigned to the pod.
> - `--image=nginx:alpine` specifies which container image to use:
>   - `nginx` is the name of the image (a popular web server)
>   - `alpine` is the specific tag/version, based on Alpine Linux (known for being lightweight)

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

## Command Explanation:
### Purpose: This command creates a new namespace in Kubernetes, which is essentially a virtual cluster backed by the same physical cluster.
### Syntax:

kubectl create namespace is the command to create a new namespace.
apx-x9984574 is the name of the new namespace.


Why No Tags or Labels:
Namespaces do not inherently need labels for their creation because:
Namespaces are already a method of isolation and organization by themselves. They provide a scope for names, which inherently organizes resources.
You can add labels to namespaces post-creation with commands like kubectl label namespace apx-x9984574 tier=msg, but it's not required at the time of creation since the primary function of a namespace is to segregate resources, not to describe them with metadata like labels do for other resources.

---
## Question 3
### Create a namespace named apx-x9984574.
- Namespace: apx-x9984574  

```bash
kubectl create namespace apx-x9984574
```

> - A Kubernetes namespace logically divides a cluster into isolated environments. Similar to different rooms in a house, each room has its own space and purpose, so things don't get mixed up.
> - Namespaces do not inherently need labels for their creation because:
>   - Namespaces are already a method of isolation and organization by themselves
>   - They provide a scope for names, which inherently organizes resources
>   - You can add labels to namespaces post-creation (e.g., `kubectl label namespace apx-x9984574 tier=msg`)
>   - Labels aren't required at creation since the primary function of a namespace is to segregate resources, not to describe them with metadata

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
> - `kubectl get nodes` lists all nodes in the cluster.
> - `-o json` outputs the information in JSON format.
> - `>` redirects the output to a file instead of displaying it in the terminal.
> - `/opt/outputs/nodes-z3444kd9.json` is the file where the JSON output is saved.


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
kubectl run temp-bus --image=redis:alpine --namespace=finance --restart=Never
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
