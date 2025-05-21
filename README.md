# Argo CI CD


## Installation:

brew install argocd

kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl get svc

kubectl port-forward -n argocd svc/argocd-server 8080:443


## when logging in to Argo CD:

username is admin and password is created in a secret file “argocd-initial-admin-secret”


## To get the password:

kubectl get secret argocd-initial-admin-secret -n argocd -o yaml

in my case: dWFFSkRwTFJwUDlTYTVraw==



We need to decode that password:

echo dWFFSkRwTFJwUDlTYTVraw== | base64 --decode

which is : uaEJDpLRpP9Sa5kk



The ArgoCD is empty right now:


2.Creating an app on Argo CD from github repo:




And the app has ben created:



We then select the sync function in the app:




And its synced:






Then port forwarding the service:

To check if it is working:






And the upload-server app is working:


Also enabling auto sync, prune and auto-heal for best practice.




Now testing the Auto-Sync by changing the number of replicas from 1 to 3:



And pushing this changed code to the github:




Verifying:






And three pods are created instantly showing auto-sync is working.
