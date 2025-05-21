# 🚀 Argo CD CI/CD Setup with upload-server App

This project demonstrates setting up **Argo CD** on macOS and deploying a sample Flask app (`upload-server`) from a GitHub repository using GitOps principles, including **Auto-Sync**, **Prune**, and **Self-Heal** features.

---

## 📦 Step-by-Step Installation

### 1. Install Argo CD CLI

```bash
brew install argocd
```

### 2. Install Argo CD in Kubernetes

```bash
kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 3. Access the Argo CD UI

```bash
kubectl port-forward -n argocd svc/argocd-server 8080:443
```

Then open your browser and go to:

```
https://localhost:8080
```

---

## 🔐 Argo CD Login

- **Username:** `admin`
- **Password:** Extracted from the Kubernetes secret

Get the password:

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decode
```

Example output:

```
uaEJDpLRpP9Sa5kk
```

Use these credentials to log into the UI.

---

## 📁 Deploying the App from GitHub

### 1. Create a New Application in Argo CD UI

- **Application Name:** upload-server
- **Project:** default
- **Repository URL:** _your GitHub repo URL_
- **Path:** _path to your k8s manifests (e.g. `k8s/`)_
- **Cluster URL:** https://kubernetes.default.svc
- **Namespace:** default (or your custom namespace)

Click **Create**.

### 2. Sync the Application

After creation:

- Click **Sync**
- The app will deploy
- You can port-forward the app service and verify it in the browser

```bash
kubectl port-forward svc/upload-server 5000:5000
```

Open [http://localhost:5000](http://localhost:5000) to check if the app is working.

---

## ♻️ Enable Auto-Sync, Prune & Self-Heal

For production best practices:

- Enable **Auto-Sync** (auto-deploy on Git change)
- Enable **Prune** (remove deleted resources)
- Enable **Self-Heal** (restore drifted resources)

---

## ✅ Testing Auto-Sync

1. Change your deployment manifest (e.g., replicas: `1` → `3`)
2. Commit and push to GitHub
3. Argo CD will auto-sync and apply the changes

Verify:

```bash
kubectl get pods
```

You should see 3 pods running, confirming auto-sync works.

---

## 🛠️ Tools Used

- Kubernetes
- Argo CD
- Flask (upload-server)
- GitHub
- GitOps Workflow

---

## 📃 License

MIT License. See `LICENSE` file for details.
