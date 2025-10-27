# Lab 03: Deployments – Production-Ready Apps ✓

**🎯 Goal:** Deploy apps with zero-downtime updates and rollbacks  
**⏱️ Time:** 60 minutes  
**📚 Full Guide:** [Reference PDF](./Guided-Lab-03-Deployments.pdf)

---

## Prerequisites Check

- [ ] Completed Lab 01 & 02
- [ ] Understand Pods and ReplicaSets
- [ ] kubectl ready

---

## Why Deployments?

**ReplicaSets are great, but missing:**
- ❌ Rolling updates (zero downtime)
- ❌ Rollback capability
- ❌ Version history

**Deployments add all of this! 🎉**

---

## Lab Steps - Follow in Order

### ☐ Step 1: Setup Workspace
```bash
cd ~/k8s-labs
mkdir -p deployments
cd deployments
```

---

### ☐ Step 2: Create Deployment YAML
```bash
cat > nginx-deployment.yaml << 'EOF'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
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

**New parts:**
- `strategy: RollingUpdate` - Update gradually
- `maxSurge: 1` - Add 1 extra Pod during update
- `maxUnavailable: 0` - Keep all Pods available

---

### ☐ Step 3: Apply Deployment
```bash
kubectl apply -f nginx-deployment.yaml
```

---

### ☐ Step 4: Check Everything
```bash
kubectl get deployments
kubectl get replicasets
kubectl get pods
```

**Notice:** Deployment → ReplicaSet → Pods

---

## Part 2: Rolling Update (Zero Downtime!)

### ☐ Step 5: Update to New Version
```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.23.1
```

**This updates NGINX from 1.21.1 → 1.23.1**

---

### ☐ Step 6: Watch the Rollout
```bash
kubectl rollout status deployment/nginx-deployment
```

**Expected:**
```
Waiting for deployment "nginx-deployment" rollout to finish...
deployment "nginx-deployment" successfully rolled out
```

---

### ☐ Step 7: Watch Pods Update
```bash
kubectl get pods --watch
```

**What you'll see:**
1. New Pod created (1.23.1 image)
2. New Pod becomes Ready
3. Old Pod terminates
4. Process repeats
5. **No downtime!**

Press `Ctrl+C` when done.

---

### ☐ Step 8: Check ReplicaSets
```bash
kubectl get replicasets
```

**You'll see 2 ReplicaSets:**
- Old one (1.21.1) - scaled to 0
- New one (1.23.1) - has 2 Pods

✅ Old ReplicaSet kept for rollback!

---

## Part 3: Rollout History

### ☐ Step 9: Check History
```bash
kubectl rollout history deployment/nginx-deployment
```

**Output:**
```
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
```

---

### ☐ Step 10: Add Change Description
```bash
kubectl annotate deployment/nginx-deployment kubernetes.io/change-cause="Updated to nginx 1.23.1"
```

---

### ☐ Step 11: Check History Again
```bash
kubectl rollout history deployment/nginx-deployment
```

**Now shows your description!**

---

## Part 4: Rollback

### ☐ Step 12: Rollback to Previous Version
```bash
kubectl rollout undo deployment/nginx-deployment
```

---

### ☐ Step 13: Watch Rollback
```bash
kubectl rollout status deployment/nginx-deployment
```

---

### ☐ Step 14: Verify Version
```bash
kubectl describe deployment nginx-deployment | grep Image
```

**Back to nginx:1.21.1! 🎉**

---

### ☐ Step 15: Check History
```bash
kubectl rollout history deployment/nginx-deployment
```

**Revision 3 created (rollback)**

---

## Part 5: Scaling

### ☐ Step 16: Scale Up
```bash
kubectl scale deployment nginx-deployment --replicas=5
```

---

### ☐ Step 17: Verify
```bash
kubectl get pods
```

**5 Pods running!**

---

### ☐ Step 18: Scale Down
```bash
kubectl scale deployment nginx-deployment --replicas=2
```

---

## Part 6: Advanced - Pause & Resume

### ☐ Step 19: Pause Rollout
```bash
kubectl rollout pause deployment/nginx-deployment
```

---

### ☐ Step 20: Make Multiple Changes
```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.24.0
kubectl set resources deployment/nginx-deployment -c=nginx --limits=cpu=200m,memory=512Mi
```

**Changes are staged but NOT applied yet!**

---

### ☐ Step 21: Check Pods
```bash
kubectl get pods
```

**No changes yet - deployment paused**

---

### ☐ Step 22: Resume Rollout
```bash
kubectl rollout resume deployment/nginx-deployment
```

**All changes applied in ONE rollout!**

---

### ☐ Step 23: Watch Combined Update
```bash
kubectl rollout status deployment/nginx-deployment
```

✅ Both changes deployed together!

---

## Part 7: Cleanup

### ☐ Step 24: Delete Deployment
```bash
kubectl delete deployment nginx-deployment
```

---

### ☐ Step 25: Verify All Gone
```bash
kubectl get deployments
kubectl get replicasets
kubectl get pods
```

✅ Everything cleaned up!

---

## ✅ Lab Complete!

### What You Learned:
- ✓ **Deployments** = Production standard
- ✓ **RollingUpdate** = Zero downtime
- ✓ **Rollback** = Instant undo
- ✓ **History** = Track all changes
- ✓ **Pause/Resume** = Batch updates
- ✓ Deployments → ReplicaSets → Pods

### Commands You Mastered:
- `kubectl apply` - Create/update Deployment
- `kubectl set image` - Update container image
- `kubectl rollout status` - Watch rollout
- `kubectl rollout history` - View changes
- `kubectl rollout undo` - Rollback
- `kubectl rollout pause/resume` - Control rollout
- `kubectl scale` - Change replicas

---

## 🎉 Congratulations!

You've completed all 3 core Kubernetes labs:
- ✅ Lab 01: Pods
- ✅ Lab 02: ReplicaSets
- ✅ Lab 03: Deployments

**You now understand K8s fundamentals!**

---

## 🎯 Share Your Work
```bash
mkdir -p solutions/YOUR-GITHUB-USERNAME/lab03
cp nginx-deployment.yaml solutions/YOUR-GITHUB-USERNAME/lab03/
git add solutions/YOUR-GITHUB-USERNAME/lab03/
git commit -m "solution(lab03): add my Deployment solution"
git push origin main
```

---

## 🆘 Need Help?

- [Check PDF guide](./Guided-Lab-03-Deployments.pdf)
- [Open an issue](../../issues/new/choose)

---

## 🚀 What's Next?

Continue your Kubernetes journey:
- Services & Networking
- ConfigMaps & Secrets
- Persistent Volumes
- Ingress Controllers
- Helm Charts

**Keep learning, keep building!** 💪
