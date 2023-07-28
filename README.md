# mpyl-example-argocd
Demonstrates the use of MPyL in cooperation with ArgoCD

## Usage
Log in on [https://argocd.kawalc1.org/](https://argocd.kawalc1.org/). User `readonly` with password `readonly`.

## Installing ArgoCD

Create cluster
```shell
k3d cluster create argo
kubectl cluster-info
```
Install ArgoCD
```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Get admin password
```shell
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath={.data.password} | base64 -d; echo
```

Create read only user
```shell
brew install argocd
argocd login localhost --username admin
kubectl get configmap argocd-cm -n argocd -o yaml > argocd-cm.yml
kubectl apply -f argocd-cm.yml
argocd argocd account update-password --account readonly --new-password readonly
```

Add appset for this repo
```shell
argocd login argocd.kawalc1.org:443 --username admin
argocd appset create manifests/monorepo_application.yaml
```