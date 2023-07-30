# mpyl-example-argocd
Demonstrates the use of MPyL in cooperation with ArgoCD

## Usage
Log in on [https://argocd.kawalc1.org/](https://argocd.kawalc1.org/). User `readonly` with password `readonly`.

## Installing ArgoCD

Create cluster
```shell
k3d cluster create --config cluster/k3d-argocd.yaml
kubectl cluster-info
```
If you get `The connection to the server host.docker.internal:6443 was refused`, edit `~/.kube/config` 
and replace `host.docker.internal` with `localhost`.

Install ArgoCD
```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
Configure load balancer
```shell
kubectl apply -f argo/argocd-cmd-params-cm.yaml
kubectl apply -f cluster/traefik-local.yml
kubectl rollout restart deployment/argocd-server -n argocd
```

Get admin password. Note: this could take a while to appear.
```shell
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath={.data.password} | base64 -d; echo
```

Create read only user
```shell
kubectl apply -f argo/argocd-cm.yaml
kubectl apply -f argo/argocd-cmd-params-cm.yaml
kubectl apply -f argo/argocd-rbac-cm.yaml
```

First time log in
```shell
argocd login localhost:9080 --username admin
```

Add appset for this repo
```shell
argocd login argocd.kawalc1.org --username admin
argocd appset create argo/monorepo_application.yaml
```

Add readonly user
```shell
argocd account update-password --account readonly --new-password readonly
```
Open [https://localhost:9080/](https://localhost:9080/) and log in with user `readonly` and password `readonly`.
