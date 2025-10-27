# Lab 02: ReplicaSets – Keeping Your Pods Alive ✓

**🎯 Goal:** Create a ReplicaSet that maintains 3 Pods and auto-heals failures  
**⏱️ Time:** 45 minutes  
**📚 Full Guide:** [Reference PDF](./Guided-Lab-02-ReplicaSets.pdf)

---

## Prerequisites Check

- [ ] Completed Lab 01
- [ ] Kubernetes cluster running
- [ ] kubectl ready

---

## The Problem

**In Lab 01:** If your Pod crashed → it's gone forever 😢

**Today's Solution:** ReplicaSets keep N copies running automatically!

---

## Lab Steps - Follow in Order

### ☐ Step 1: Setup Workspace
```bash
cd ~/k8s-labs
mkdir -p replicasets
cd replicasets
```

---

### ☐ Step 2: Create ReplicaSet YAML
```bash
cat > nginx-replicaset.yaml << 'EOF'
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21.1
        ports:
        - containerPort: 80
EOF
```

**Key parts:**
- `replicas: 3` - Keep 3 Pods running
- `selector.matchLabels` - Which Pods to manage
- `template` - Pod definition

---

### ☐ Step 3: Apply ReplicaSet
```bash
kubectl apply -f nginx-replicaset.yaml
```

---

### ☐ Step 4: Verify ReplicaSet
```bash
kubectl get replicaset
```

**Expected:**
```
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   3         3         3       10s
```

---

### ☐ Step 5: Check the Pods
```bash
kubectl get pods
```

**You should see 3 Pods!**

---

## Part 2: Test Auto-Healing 🔥

### ☐ Step 6: Delete One Pod
```bash
kubectl get pods
kubectl delete pod nginx-replicaset-XXXXX
```

**Note:** Replace XXXXX with actual pod name

---

### ☐ Step 7: Watch the Magic
```bash
kubectl get pods --watch
```

**What happens:**
1. Deleted Pod terminates
2. New Pod created immediately
3. Total stays at 3!

🎉 **Auto-healing in action!**

Press `Ctrl+C` to stop.

---

## Part 3: Scaling

### ☐ Step 8: Scale to 5 Pods
```bash
kubectl scale replicaset nginx-replicaset --replicas=5
```

---

### ☐ Step 9: Verify
```bash
kubectl get pods
```

**Now 5 Pods!**

---

### ☐ Step 10: Scale Back Down
```bash
kubectl scale replicaset nginx-replicaset --replicas=3
```

---

### ☐ Step 11: Cleanup
```bash
kubectl delete replicaset nginx-replicaset
kubectl get pods
```

✅ Everything cleaned up!

---

## ✅ Lab Complete!

### What You Learned:
- ✓ ReplicaSets maintain N Pod replicas
- ✓ **Auto-healing:** Failed Pods replaced automatically
- ✓ **Scaling:** Easy with one command
- ✓ **Labels:** How ReplicaSets track Pods

### Commands You Mastered:
- `kubectl get replicaset`
- `kubectl scale`
- `kubectl describe replicaset`

---

## 🎯 Share Your Work
```bash
mkdir -p solutions/YOUR-GITHUB-USERNAME/lab02
cp nginx-replicaset.yaml solutions/YOUR-GITHUB-USERNAME/lab02/
git add solutions/YOUR-GITHUB-USERNAME/lab02/
git commit -m "solution(lab02): add my ReplicaSet solution"
git push origin main
```

---

## 🆘 Need Help?

- [Check PDF guide](./Guided-Lab-02-ReplicaSets.pdf)
- [Open an issue](../../issues/new/choose)

---

## ⏭️ Next Lab

[Lab 03: Deployments →](../03-deployments/README.md)

---

**Great job! 🚀**
