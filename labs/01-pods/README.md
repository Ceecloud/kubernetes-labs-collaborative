# Lab 01: Creating Your First Pod ✓

**🎯 Goal:** Create and manage your first Kubernetes Pod  
**⏱️ Time:** 30 minutes  
**📚 Full Guide:** [Reference PDF](./Guided-Lab-01-Creating-Your-First-Pod.pdf)

---

## Prerequisites Check

- [ ] Kubernetes cluster running (`kubectl cluster-info`)
- [ ] kubectl installed (`kubectl version --client`)
- [ ] Terminal/PowerShell open

---

## Lab Steps - Follow in Order

### ☐ Step 1: Setup Your Workspace
```bash
mkdir -p ~/k8s-labs/pods
cd ~/k8s-labs/pods
```

---

### ☐ Step 2: Create Pod (Imperative Method)
```bash
kubectl run nginx-pod --image=nginx
```

**What this does:** Creates a Pod named `nginx-pod` using NGINX image

---

### ☐ Step 3: Verify Pod is Running
```bash
kubectl get pods
```

**Expected output:**
```
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   1/1     Running   0          10s
```

✅ If you see "Running", continue!

---

### ☐ Step 4: Inspect the Pod
```bash
kubectl describe pod nginx-pod
```

**Look for:**
- Pod IP address
- Node where it's running
- Container image
- Events section

---

### ☐ Step 5: View Pod Logs
```bash
kubectl logs nginx-pod
```

---

### ☐ Step 6: Delete the Pod
```bash
kubectl delete pod nginx-pod
```

**Verify it's gone:**
```bash
kubectl get pods
```

---

## Part 2: Declarative Method (Production Way)

### ☐ Step 7: Create YAML File
```bash
cat > nginx-pod.yaml << 'EOF'
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx-container
    image: nginx
    ports:
    - containerPort: 80
EOF
```

---

### ☐ Step 8: Apply the YAML
```bash
kubectl apply -f nginx-pod.yaml
```

---

### ☐ Step 9: Verify Again
```bash
kubectl get pods
```

✅ Pod created from YAML!

---

## Bonus Steps: Explore Your Pod

### ☐ Step 10: Get Inside the Container
```bash
kubectl exec -it nginx-pod -- /bin/bash
```

**Inside the container, try:**
```bash
cat /usr/share/nginx/html/index.html
exit
```

---

### ☐ Step 11: Access from Browser
```bash
kubectl port-forward nginx-pod 8080:80
```

**Open browser:** http://localhost:8080  
**Stop:** Press `Ctrl+C`

---

### ☐ Step 12: Cleanup
```bash
kubectl delete pod nginx-pod
```

---

## ✅ Lab Complete!

### What You Learned:
- ✓ Pods are the smallest unit in Kubernetes
- ✓ **Imperative:** Quick commands (`kubectl run`)
- ✓ **Declarative:** YAML files (production way)
- ✓ How to inspect, debug, and access Pods

---

## 🎯 Share Your Work
```bash
mkdir -p solutions/YOUR-GITHUB-USERNAME/lab01
cp nginx-pod.yaml solutions/YOUR-GITHUB-USERNAME/lab01/
git add solutions/YOUR-GITHUB-USERNAME/
git commit -m "solution(lab01): add my Pod solution"
git push origin main
```

---

## 🆘 Stuck?

- [Check PDF guide](./Guided-Lab-01-Creating-Your-First-Pod.pdf)
- [Open an issue](../../issues/new/choose)

---

## ⏭️ Next Lab

[Lab 02: ReplicaSets →](../02-replicasets/README.md)

---

**Good luck! 🚀**
