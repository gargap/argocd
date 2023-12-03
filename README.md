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

```bash

Here's a breakdown of the key sections of Application.yaml:

metadata: Specifies metadata about the Argo CD Application, including its name (myapp-argo-application) and the namespace in which it resides (argocd).

spec.project: Specifies the Argo CD project to which this application belongs (default).

spec.source: Defines the source of the application, specifying the Git repository URL (https://github.com/gargap/argocd.git), target revision (HEAD), and the path within the repository (dev).

spec.destination: Specifies the destination where the application should be deployed. It includes the Kubernetes server URL (https://kubernetes.default.svc) and the target namespace (myapp). If the specified namespace doesn't exist, Argo CD will create it based on the syncPolicy.

syncPolicy: Configures the synchronization policy for the application.

syncOptions: Specifies synchronization options, and in this case, it includes CreateNamespace=true, indicating that Argo CD should create the namespace if it doesn't exist.

automated: Configures automated synchronization settings.

selfHeal: If set to true, the synchronization process will override any manual changes made to the cluster.

prune: If set to true, the automatic sync will delete resources that are not defined in the Git repository.

This YAML manifest indicates an Argo CD Application named myapp-argo-application that pulls Kubernetes manifests from a Git repository and deploys them to the specified namespace (myapp) in the Kubernetes cluster. The synchronization process is automated with options to self-heal and prune resources.


```
</br>

#### Links


* Install ArgoCD: [https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd](https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd)

* Login to ArgoCD: [https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli](https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli)

* ArgoCD Configuration: [https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/)