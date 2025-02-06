# Mock Exam 2.md

## Question 1

### Take a backup of the etcd cluster and save it to /opt/etcd-backup.db.

```bash
export ETCDCTL_API=3 
etcdctl snapshot save --endpoints https://[127.0.0.1]:2379 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key  /opt/etcd-backup.db

```
### Command Explanation:

> - **`ETCDCTL_API=3`**
>   - Specifies the version of etcdctl API to use
>
> - **`--endpoints=https://127.0.0.1:2379`**
>   - Specifies the etcd endpoint to connect to
>
> - **Certificate flags:**
>   - `--cacert`: Path to the CA certificate
>   - `--cert`: Path to the server certificate
>   - `--key`: Path to the server key
>
> - **`snapshot save etcd-backup.db`**
>   - Creates a snapshot backup of etcd and saves it to the specified path

---

## Question 2

### Create a Pod called redis-storage with image: redis:alpine with a Volume of type emptyDir that lasts for the life of the Pod.

```bash
kubectl run redis-storage --image=redis:alpine --restart=Never --overrides='
{
  "apiVersion": "v1",
  "kind": "Pod",
  "metadata": {
    "name": "redis-storage"
  },
  "spec": {
    "containers": [
      {
        "name": "redis",
        "image": "redis:alpine",
        "volumeMounts": [
          {
            "mountPath": "/data/redis",
            "name": "redis-storage-volume"
          }
        ]
      }
    ],
    "volumes": [
      {
        "name": "redis-storage-volume",
        "emptyDir": {}
      }
    ]
  }
}'


## Verify

```bash
kubectl get pod redis-storage -o yaml
kubectl describe pod redis-storage
```
### Command Explanation:


- **`kubectl run redis-storage`**
  - Creates a Pod named `redis-storage`

- **`--image=redis:alpine`**
  - Specifies the container image to use

- **`--restart=Never`**
  - Ensures it runs as a standalone Pod

- **`emptyDir`**
  - Volume is created and deleted with the Pod

- **`volumeMounts`**
  - Attaches the volume to `/data/redis` inside the container



---

## Question 3

### Create a new pod called super-user-pod with image busybox:1.28. Allow the pod to be able to set system_time.

> - **Requirements:**
>   - The container should sleep for 4800 seconds
>   - Pod name: `super-user-pod`
>   - Container Image: `busybox:1.28`
>   - SYS_TIME capability must be enabled for the container

```bash
kubectl run super-user-pod --image=busybox:1.28 --command -- sleep 4800
```

### Command Explanation:

- **`kubectl run super-user-pod`**
  - Creates a Pod named `super-user-pod`

- **`--image=busybox:1.28`**
  - Specifies the container image to use

- **`--command -- sleep 4800`**
  - Executes the `sleep` command to keep the container running for 4800 seconds

---

## Question 4

### A pod definition file is created at /root/CKA/use-pv.yaml. Make use of this manifest file and mount the persistent volume called pv-1. Ensure the pod is running and the PV is bound.


> - **Mount Path:** `/data`
> - **PersistentVolumeClaim Name:** `my-pvc`

```bash
kubectl apply -f /root/CKA/use-pv.yaml
```

### Command Explanation:

> - **`kubectl apply -f /root/CKA/use-pv.yaml`**
>   - Applies the pod definition file
>
> - **`kubectl get pods`**
>   - Ensures the pod is running
>
> - **`kubectl get pvc my-pvc`**
>   - Verifies that the PVC is bound
>
> - **`kubectl get pv pv-1`**
>   - Checks that the Persistent Volume is properly attached
>
> - **PersistentVolumeClaim:**
>   - Named `my-pvc`
>   - Volume is mounted at `/data` inside the container


---

## Question 5

### Create a deployment `nginx-deploy` with image `nginx:1.16` and 1 replica. Upgrade it to version `1.17` using a rolling update and record the change.

```bash
# Create deployment
kubectl create deployment nginx-deploy --image=nginx:1.16 --replicas=1

# Verify deployment
kubectl get deployments
kubectl describe deployment nginx-deploy

# Update deployment to nginx:1.17 and record the change
kubectl set image deployment/nginx-deploy nginx=nginx:1.17 --record

# Check rollout history
kubectl rollout history deployment/nginx-deploy

# Monitor rolling update
kubectl rollout status deployment/nginx-deploy

# Verify the new version
kubectl get deployments
kubectl describe deployment nginx-deploy
```

### Command Explanation:

- **`kubectl create deployment nginx-deploy --image=nginx:1.16 --replicas=1`**
  - Creates a deployment named `nginx-deploy` with image `nginx:1.16` and 1 replica

- **`kubectl set image deployment/nginx-deploy nginx=nginx:1.17 --record`**
  - Updates the deployment to use `nginx:1.17` and records the change
  - Flag --record has been deprecated

- **`kubectl rollout history deployment/nginx-deploy`**
  - Checks the rollout history of the deployment

- **`kubectl rollout status deployment/nginx-deploy`**
  - Monitors the status of the rolling update

- **`kubectl get deployments`**
  - Verifies the deployment has been updated

---

## Question 6

### Create a new user called john. Grant him access to the cluster. John should have permission to create, list, get, update and delete pods in the development namespace . The private key exists in the location: /root/CKA/john.key and csr at /root/CKA/john.csr.


```bash
kubectl create role developer -n development --resource=pods --verb=create,list,get,update,delete

kubectl create rolebinding developer-binding -n development --role developer --user=john
```

```bash
cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: john-developer
spec:
  signerName: kubernetes.io/kube-apiserver-client
  request: $(cat /root/john.csr | base64 | tr -d '\n')
  usages:
  - digital signature
  - key encipherment
  - server auth
EOF

kubectl get csr
kubectl certificate approve john-developer
```
To approve this certificate, run: kubectl certificate approve john-developer

Next, create a role developer and rolebinding developer-role-binding, run the command:

```bash
kubectl create role developer --resource=pods --verb=create,list,get,update,delete --namespace=development

kubectl create rolebinding developer-role-binding --role=developer --user=john --namespace=development
```
To verify the permission from kubectl utility tool:

```bash
kubectl auth can-i update pods --as=john --namespace=development
```

# Verify permissions
kubectl auth can-i create pods --as=john --namespace=development
kubectl auth can-i list pods --as=john --namespace=development
kubectl auth can-i get pods --as=john --namespace=development
kubectl auth can-i update pods --as=john --namespace=development
kubectl auth can-i delete pods --as=john --namespace=development

---

## Question 7

### Create a nginx pod called nginx-resolver using image nginx, expose it internally with a service called nginx-resolver-service. Test that you are able to look up the service and pod names from within the cluster. Use the image: busybox:1.28 for dns lookup. Record results in /root/CKA/nginx.svc and /root/CKA/nginx.pod

> - **Requirements:**

> - Pod: nginx-resolver created

> - Service DNS Resolution recorded correctly

> - Pod DNS resolution recorded correctly

```bash
kubectl run nginx-resolver --image=nginx
kubectl expose pod nginx-resolver --name=nginx-resolver-service --port=80 --target-port=80 --type=ClusterIP
```

To create a pod test-nslookup. Test that you are able to look up the service and pod names from within the cluster:

```bash
kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service
kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service > /root/CKA/nginx.svc
```

Get the IP of the nginx-resolver pod and replace the dots(.) with hyphon(-) which will be used below.
```bash
kubectl get pod nginx-resolver -o wide
kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup <P-O-D-I-P.default.pod> > /root/CKA/nginx.pod
```

---
## Question 8

### Create a static pod on node01 called nginx-critical with image nginx and make sure that it is recreated/restarted automatically in case of a failure.

> - **Requirements:**
> - Use /etc/kubernetes/manifests as the Static Pod path for example.
> - static pod configured under /etc/kubernetes/manifests ?
> - Pod nginx-critical-node01 is up and running

`ssh node01`
https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/
`cat /var/lib/kubelet/config.yaml | grep stat`

```bash
cat <<EOF > /etc/kubernetes/manifests/nginx-critical.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-critical
spec:
  containers:
    - name: web
      image: nginx
      ports:
        - name: web
          containerPort: 80
          protocol: TCP
EOF
```

