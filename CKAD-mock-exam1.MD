## CKAD Practice Tasks - Questions Only

### Q1. Pod with Multiple Containers
Create a pod `tres-containers-pod` in `ckad-multi-containers` namespace with:
- `primero`: busybox:1.28 with env `ORDER=FIRST`
- `segundo`: nginx:1.17 on port 8080
- `tercero`: busybox:1.31.1 with env `ORDER=THIRD`

---

### Q2. Create Storage Class
Create `banana-sc-ckad08-str` with:
- Provisioner: `kubernetes.io/no-provisioner`
- Volume binding: `WaitForFirstConsumer`
- Volume expansion: Enabled

---

### Q3. CronJob with Node Image
In `ckad-job` namespace, create a CronJob `simple-node-job` that runs every 30 minutes, listing processes using `ps -eaf`.

---

### Q4. Ubuntu Pod
In `ckad-pod-design` namespace, create `ckad-ubuntu-qwfefefwe` with container `ubuntu-server` running Ubuntu and sleeping for 3600s.

---

### Q5. Blue/Green Deployment with Traffic Split
Deploy `blue-apd` and `green-apd` with:
- blue: 7 replicas, label `type-one: blue`, image v1
- green: 3 replicas, label `type-two: green`, image v2
- Service `route-apd-svc` with selector `version: v1`, type `NodePort`, port 8080

---

### Q6. Fix Crashing Pod
Inspect `dev-001` namespace on `cluster3` and fix the crashing `news-apd` pod.

---

### Q7. Helm Chart Deployment
Deploy Helm chart in `/opt/webapp-color-apd` with:
- Release name: `webapp-color-apd`
- Namespace: `frontend-apd`
- Service type: `NodePort`
- Replicas: 3
- App version: 1.20.0

---

### Q8. Create Service for Existing Pod
Expose pod `pod22-ckad-svcn` on port 6335 with service name `svc22-ckad-svcn`.

---

### Q9. Network Policy Denying All Traffic
Create namespace `ckad12-svcn` and add `deny-all-svcn` network policy to deny all ingress and egress.

---

### Q10. Fix External Webserver Service
Fix service `external-webserver-ckad01-svcn` so pods in `cluster3` can access external webserver on port 9999.

---

### Q11. Internal Service for Web App
Deploy pod `webapp-svcn` with image `kodekloud/webapp-color` and expose it on port 6379 using a ClusterIP service.

---

### Q12. ExternalName Service
Create ExternalName service `ckad12-service` that points to IP `8.8.8.8` on port 53.

---

### Q13. Pod with Burstable QoS Class
Create pod `ckad17-qos-aecs-3` in `ckad17-nqoss-aecs` namespace with Burstable QoS and export all pod QoS to `/root/qos_status_aecs`.

---

### Q14. Custom Resource for Anime
Using CRD with `anime`, create custom resource `my-anime` with name `Death Note` and 37 episodes.

---

### Q15. ConfigMap from Multiple Env Files
Create ConfigMap `ckad04-config-multi-env-files-aecs` from two env files in `/root/ckad04-multi-cm`.

---

### Q16. Use Secret for Env Vars
Create secret `ckad01-db-scrt-aecs` and configure pod `ckad01-mysql-server` to load env vars from it.

---

### Q17. ResourceQuota for Namespace
Create `ckad16-rqc` ResourceQuota in namespace `ckad16-rqc-ns`.

---

### Q18. Secret Volume Mount
Create pod `ckad18-secret-pod` using template that mounts secret `ckad18-secret-aecs` as read-only volume.

---

### Q19. Add Readiness Probe
Add readiness probe to pod `simple-webapp-aom` on path `/ready` and port 8080.

---

### Q20. Fix Busybox Manifest
Fix the manifest `/root/ckad-pod-busybox.yaml` and apply it.

---

### Q21. Pod with Liveness Probe
Create pod `ckad-probe` with image `redis` and a liveness probe using command `ls` and `initialDelaySeconds: 5`.

---
