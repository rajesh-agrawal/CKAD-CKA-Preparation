## CKAD Practice Tasks - Answers (Q1 to Q21)

### A1. Pod with Multiple Containers

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: tres-containers-pod
  namespace: ckad-multi-containers
spec:
  containers:
    - name: primero
      image: busybox:1.28
      env:
        - name: ORDER
          value: FIRST
      command: ["/bin/sh", "-c", "sleep 3600"]
    - name: segundo
      image: nginx:1.17
      ports:
        - containerPort: 8080
    - name: tercero
      image: busybox:1.31.1
      env:
        - name: ORDER
          value: THIRD
      command: ["/bin/sh", "-c", "sleep 3600"]
  restartPolicy: Always
```

---

### A2. Create Storage Class

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: banana-sc-ckad08-str
provisioner: kubernetes.io/no-provisioner
allowVolumeExpansion: true
volumeBindingMode: WaitForFirstConsumer
```

---

### A3. CronJob with Node Image

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: simple-node-job
  namespace: ckad-job
spec:
  schedule: "*/30 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: simple-node-job
            image: node
            command: ["/bin/sh", "-c", "ps -eaf"]
          restartPolicy: OnFailure
```

---

### A4. Ubuntu Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ckad-ubuntu-qwfefefwe
  namespace: ckad-pod-design
spec:
  containers:
    - name: ubuntu-server
      image: ubuntu
      command: ["sleep", "3600"]
  restartPolicy: Always
```

---

### A5. Blue/Green Deployment with Traffic Split

**blue-apd:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue-apd
spec:
  replicas: 7
  selector:
    matchLabels:
      version: v1
      type-one: blue
  template:
    metadata:
      labels:
        version: v1
        type-one: blue
    spec:
      containers:
        - name: blue-apd
          image: kodekloud/webapp-color:v1
```

**green-apd:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: green-apd
spec:
  replicas: 3
  selector:
    matchLabels:
      version: v1
      type-two: green
  template:
    metadata:
      labels:
        version: v1
        type-two: green
    spec:
      containers:
        - name: green-apd
          image: kodekloud/webapp-color:v2
```

**Service:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: route-apd-svc
spec:
  type: NodePort
  ports:
    - port: 8080
      targetPort: 8080
  selector:
    version: v1
```

---

### A6. Fix Crashing Pod
```bash
kubectl config use-context cluster3
kubectl get all -n dev-001
kubectl describe pod <POD-NAME> -n dev-001
kubectl logs <POD-NAME> -n dev-001
kubectl edit deployment news-apd -n dev-001
```

---

### A7. Helm Chart Deployment
```bash
kubectl config use-context cluster2
kubectl get ns
kubectl create ns frontend-apd
cd /opt/webapp-color-apd
# Edit Chart.yaml, values.yaml as per spec
helm lint ./webapp-color-apd/
helm install webapp-color-apd -n frontend-apd ./webapp-color-apd
```

---

### A8. Create Service for Existing Pod
```bash
kubectl config use-context cluster3
kubectl expose pod pod22-ckad-svcn --name=svc22-ckad-svcn --port=6335
```

---

### A9. Network Policy Denying All Traffic
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all-svcn
  namespace: ckad12-svcn
spec:
  podSelector: {}
  policyTypes:
    - Ingress
    - Egress
```

---

### A10. Fix External Webserver Service
```bash
export IP_ADDR=$(ifconfig eth0 | grep inet | awk '{print $2}')
kubectl apply -f - <<EOF
apiVersion: v1
kind: Endpoints
metadata:
  name: external-webserver-ckad01-svcn
subsets:
  - addresses:
      - ip: $IP_ADDR
    ports:
      - port: 9999
EOF

kubectl run --rm -i test-curl-pod --image=curlimages/curl --restart=Never -- curl -m 2 external-webserver-ckad01-svcn
```

---

### A11. Internal Service for Web App
```bash
kubectl config use-context cluster2
kubectl run webapp-svcn --image=kodekloud/webapp-color -l tier=msg
kubectl expose pod webapp-svcn --port=6379 --name=webapp-service-svcn
```

---

### A12. ExternalName Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: ckad12-service
spec:
  type: ExternalName
  externalName: 8.8.8.8
  ports:
    - name: http
      port: 53
      targetPort: 53
```

---

### A13. Pod with Burstable QoS Class
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ckad17-qos-aecs-3
  namespace: ckad17-nqoss-aecs
spec:
  containers:
    - name: ckad17-qos-ctr-3-aecs
      image: nginx
      resources:
        limits:
          memory: "200Mi"
        requests:
          memory: "100Mi"
```
```bash
kubectl get pods -n ckad17-nqoss-aecs \
  --output=custom-columns="NAME:.metadata.name,QOS:.status.qosClass" > /root/qos_status_aecs
```

---

### A14. Custom Resource for Anime
```yaml
apiVersion: animes.k8s.io/v1alpha1
kind: Anime
metadata:
  name: my-anime
spec:
  animeName: "Death Note"
  episodeCount: 37
```

---

### A15. ConfigMap from Multiple Env Files
```bash
kubectl create configmap ckad04-config-multi-env-files-aecs \
  --from-env-file=/root/ckad04-multi-cm/file1.properties \
  --from-env-file=/root/ckad04-multi-cm/file2.properties
```

---

### A16. Use Secret for Env Vars
```bash
kubectl create secret generic ckad01-db-scrt-aecs \
  --namespace=ckad01-db-sec \
  --from-literal=DB_Host=sql01 \
  --from-literal=DB_User=root \
  --from-literal=DB_Password=password123
```

```yaml
# Pod YAML should include:
containers:
  envFrom:
    - secretRef:
        name: ckad01-db-scrt-aecs
```

---

### A17. ResourceQuota for Namespace
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: ckad16-rqc
  namespace: ckad16-rqc-ns
spec:
  hard:
    pods: "1"
```

---

### A18. Secret Volume Mount
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ckad18-secret-pod
  namespace: ckad18-secret
spec:
  containers:
    - name: pod-container
      image: busybox
      volumeMounts:
        - name: secret-volume
          mountPath: /etc/secret-volume
          readOnly: true
  volumes:
    - name: secret-volume
      secret:
        secretName: ckad18-secret-aecs
```

---

### A19. Add Readiness Probe
```yaml
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 10
```

---

### A20. Fix Busybox Manifest
```bash
# Edit the manifest if needed and then apply
kubectl apply -f /root/ckad-pod-busybox.yaml
```

---

### A21. Pod with Liveness Probe
```bash
kubectl run ckad-probe --image=redis --restart=Never \
  --dry-run=client -o yaml > liveness.yaml
```
```yaml
livenessProbe:
  exec:
    command:
    - ls
  initialDelaySeconds: 5
```
```bash
kubectl apply -f liveness.yaml
```

---


