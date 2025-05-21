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

![image alt](https://github.com/Dpk808/Argo-CI_CD/blob/main/Screenshots/2.2%20Creating%20Argo%20App%20from%20repo.png) 



![image alt](https://github.com/Dpk808/Argo-CI_CD/blob/main/Screenshots/2.3%20Argo%20App%202.png)


Click **Create**.

![image alt](https://github.com/Dpk808/Argo-CI_CD/blob/main/Screenshots/2.4%20App%20created.png)


### 2. Sync the Application

After creation:

- Click **Sync**

  
![image alt](https://github.com/Dpk808/Argo-CI_CD/blob/main/Screenshots/2.6%20App%20Synced.png) 



- The app will deploy
- You can port-forward the app service and verify it in the browser

```bash
kubectl port-forward svc/upload-server 8001:80
```

Open [http://localhost:8001](http://localhost:8001) to check if the app is working. 


![image alt](https://github.com/Dpk808/Argo-CI_CD/blob/main/Screenshots/2.6%20App%20Synced.png) 


And the app is working!

![image alt](https://github.com/Dpk808/Argo-CI_CD/blob/main/Screenshots/2.75%20uploadserver%20is%20working.png) 



---

## ♻️ Enable Auto-Sync, Prune & Self-Heal

For production best practices:

- Enable **Auto-Sync** (auto-deploy on Git change)
- Enable **Prune** (remove deleted resources)
- Enable **Self-Heal** (restore drifted resources)

![image alt](https://github.com/Dpk808/Argo-CI_CD/blob/main/Screenshots/2.8%20Auto%20Sync%2C%20prune%20and%20self-heal.png)


---

## ✅ Testing Auto-Sync

1. Changing the deployment manifest (e.g., replicas: `1` → `3`)
2. Commit and push to GitHub
3. Argo CD will auto-sync and apply the changes



![image alt](https://github.com/Dpk808/Argo-CI_CD/blob/main/Screenshots/3.%20Test%20auto-sync%20by%20changing%20replicas%20form%201%20to%203.png) 

And pushing it to the github repo: 

![image alt](https://github.com/Dpk808/Argo-CI_CD/blob/main/Screenshots/3.2%20After%20pushing%20the%20edited%20deployment%20yaml%20.png)



Verify:

```bash
kubectl get pods
```


![image alt](https://github.com/Dpk808/Argo-CI_CD/blob/main/Screenshots/3.3%20Three%20pods%20are%20created%20instantly.png) 



We can see 3 pods running, confirming auto-sync works.

---

## 🛠️ Tools Used

- Kubernetes
- Argo CD
- Flask (upload-server)
- GitHub
- GitOps Workflow

---

