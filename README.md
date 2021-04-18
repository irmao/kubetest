# kubetest
Kubernetes / GitOps experimentation

---

## Requirements
1. Install `kubectl`

## Instalation

### Configure application
1. Create application namespace
```
$ kubectl create namespace application
```

### Install argo-cd

1. Install
```
$ kubectl create namespace argocd
$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

2. Change the default password
```
kubectl patch secret -n argocd argocd-secret \
  -p '{"stringData": { "admin.password": "'$(htpasswd -bnBC 10 "" newpassword | tr -d ':\n')'"}}'
```

Or retrieve the default password for using (not recommended):
```
$ kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2
```

3. Port-forward argocd server to access the UI via browser

```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

4. Access the UI via browser at `localhost:8080` and login

5. Connect to the repository
  - Side menu, 'wheel' icon
  - Repositories
  - Connect repo using HTTPS
  - Fill the fields:
    - Type: git
    - Repostory URL: https://github.com/irmao/kubetest.git
    - Username: [username]
    - Password: [password]

5. Click `New App` and fill the application info
  - Application Name: kubetest
  - Project: default
  - Sync Policy: Automatic
  - Prune Resources: checked
  - Repository URL: https://github.com/irmao/kubetest.git
  - Path: application
  - Cluster URL: cluster name ('in-cluster', if using minikube)
  - Namespace: application
