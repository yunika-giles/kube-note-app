# 📦 Project Title: "Kube Notes App"

A basic Notes API app (Node.js or Python) that stores user notes in a file system and displays a welcome message (configured using a ConfigMap).

# 🧭 Full Project Workflow – Kube Notes App

This project demonstrates how to build a simple Flask-based notes app and deploy it on Kubernetes using:

- ✅ Volumes and Persistent Volumes
- ✅ ConfigMaps and environment injection
- ✅ RBAC (ServiceAccount, Role, RoleBinding)
- ✅ NodePort for browser access

---

## 📁 Directory Structure

```
kube-notes-app/
├── app/                    # Flask app code
│   ├── app.py
│   └── requirements.txt
├── Dockerfile              # For building the app image
├── configmap.yaml          # App config (welcome message)
├── pvc.yaml                # Persistent Volume Claim
├── deployment.yaml         # Deployment resource
├── service.yaml            # NodePort Service
├── rbac/                   # RBAC config files
│   ├── serviceaccount.yaml
│   ├── role.yaml
│   └── rolebinding.yaml
└── workflow.md             # Detailed project steps
```

---

## 🔹 Step 1: Build and Push Docker Image

(Optional if you're not using a local image)

```
docker build -t <your-username>/kube-notes-app:latest .
docker push <your-username>/kube-notes-app:latest
```

---

## 🔹 Step 2: Apply Kubernetes Resources

### ✅ Create ConfigMap
```
kubectl apply -f configmap.yaml
```

### ✅ Create Persistent Volume Claim
```
kubectl apply -f pvc.yaml
```

### ✅ Set up RBAC
```
kubectl apply -f rbac/serviceaccount.yaml
kubectl apply -f rbac/role.yaml
kubectl apply -f rbac/rolebinding.yaml
```

### ✅ Deploy the App
Update `deployment.yaml` to use your Docker image and set `serviceAccountName: notes-app-sa`.

```
kubectl apply -f deployment.yaml
```

### ✅ Expose the App
```
kubectl apply -f service.yaml
```

---

## 🔹 Step 3: Access the App

```
minikube service notes-service
```

Then visit:
```
http://<minikube-ip>:30036/
```

---

## 🔹 Step 4: Test Endpoints

- `GET /` → Displays welcome message (from ConfigMap)
- `POST /note` (form-data: `note=Hello`) → Saves a note
- `GET /notes` → Returns all saved notes

---

## 🔹 Step 5: Clean Up

```
kubectl delete -f service.yaml
kubectl delete -f deployment.yaml
kubectl delete -f pvc.yaml
kubectl delete -f configmap.yaml
kubectl delete -f rbac/rolebinding.yaml
kubectl delete -f rbac/role.yaml
kubectl delete -f rbac/serviceaccount.yaml
```

---

Happy Kube Coding! 🚀