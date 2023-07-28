# mpyl-example-argocd
Demonstrates the use of MPyL in cooperation with ArgoCD

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

Add appset for this repo
```shell
argocd login argocd.kawalc1.org:443 --username admin
argocd appset create manifests/monorepo_application.yaml
```
