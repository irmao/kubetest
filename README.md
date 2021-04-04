# kubetest
Kubernetes / GitOps experimentation

---

## Requirements
1. Install `kubectl`

## Instalation

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



