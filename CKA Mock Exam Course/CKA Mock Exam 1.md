# You never know what you can do until you try. - William Cobbett

### 1. Create a pod with name tester-cka02-svcn in dev-cka02-svcn namespace with image registry.k8s.io/e2e-test-images/jessie-dnsutils:1.3. Make sure to use command sleep 3600 with restart policy set to Always .

## Once the tester-cka02-svcn pod is running, store the output of the command nslookup kubernetes.default from tester pod into the file /root/dns_output on student-node.


* 'dev-cka02-svcn' namespace exists?

* 'tester-cka02-svcn' pod exists in dev-cka02-svcn namespace?

* correct image used?

* Restart policy set to "Always"?

* Command "sleep 3600" specified?

* Correct dns output stored in '/root/dns_output'?

Check to see if the namespace exist, if not then create it. 
```bash 
kubectl get ns dev-cka02-svcn

kubectl create ns dev-cka02-svcn
```

```bash
kubectl run tester-cka02-svcn --image=registry.k8s.io/e2e-test-images/jessie-dnsutils:1.3 --namespace=dev-cka02-svcn --restart=Always --command -- sleep 3600
```

##  Execute nslookup and Store Output
```bash
kubectl exec -n dev-cka02-svcn tester-cka02-svcn -- nslookup kubernetes.default > /root/dns_output
```

---
### 2. Run a pod called alpine-sleeper-cka15-arch using the alpine image in the default namespace that will sleep for 7200 seconds.

* alpine pod created?

```bash
kubectl run alpine-sleeper-cka15-arch --image=alpine --restart=Never --command -- sleep 7200
```

Verify with 

```bash
kubectl get pods | grep alpine-sleeper-cka15-arch
```

---

### 3. We have created a service account called green-sa-cka22-arch, a cluster role called green-role-cka22-arch and a cluster role binding called green-role-binding-cka22-arch.

Update the permissions of this service account so that it can only get all the namespaces in cluster1.

* service account permissions updated?

```bash
kubectl edit clusterrole green-role-cka22-arch --context cluster1

- or - 

kubectl apply -f - <<EOF
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: green-role-cka22-arch
rules:
- apiGroups: [""]
  resources: ["namespaces"]
  verbs: ["get"]
EOF

```

---
### 4.There is a Cronjob called orange-cron-cka10-trb which is supposed to run every two minutes (i.e 13:02, 13:04, 13:06…14:02, 14:04…and so on). This cron targets the application running inside the orange-app-cka10-trb pod to make sure the app is accessible. The application has been exposed internally as a ClusterIP service.

However, this cron is not running as per the expected schedule and is not running as intended.

Make the appropriate changes so that the cronjob runs as per the required schedule and it passes the accessibility checks every-time.

* Cron is running as per the required schedule?

* Cronjob is fixed?

* Look for completed Cron jobs?

```bash
kubectl get cronjob orange-cron-cka10-trb -o yaml

kubectl edit cronjob orange-cron-cka10-trb

```
* Change schedule * * * * * to */2 * * * *
* Change command curl orange-app-cka10-trb to curl orange-svc-cka10-trb


---

### 5. The pink-depl-cka14-trb Deployment was scaled to 2 replicas however, the current replicas is still 1.

Troubleshoot and fix this issue. Make sure the CURRENT count is equal to the DESIRED count.

You can SSH into the cluster4 using ssh cluster4-controlplane command.

* CURRENT count is equal to the DESIRED count?