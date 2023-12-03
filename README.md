#### Commands to configure argocd

```bash
# install ArgoCD in k8s
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

#install via Helm
kubectl create ns argocd
helm install argocd argo/argo-cd --namespace argocd --set meta.helm.sh/release-name="argocd"

# access ArgoCD UI
kubectl get svc -n argocd
kubectl port-forward svc/argocd-server 8080:443 -n argocd

#Expose the service
kubectl patch svc argocd-server --type='json' -p '[{"op":"replace","path":"/spec/type","value":"LoadBalancer"}]'  -n argocd

#you can also create an ingress to exoise the argocd-server service

# login with admin user and below token (as in documentation):
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode && echo

# you can change and delete init password

#command to check the api version

$ kubectl api-versions | grep argo
argoproj.io/v1alpha1

```
</br>

#### Links


* Install ArgoCD: [https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd](https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd)

* Login to ArgoCD: [https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli](https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli)

* ArgoCD Configuration: [https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/)