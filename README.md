# Steps to Deploy ArgoCD
1. Install ArgoCD into the cluster
  ```bash
  # With helm
  helm install argocd

  # With kubectl
  kubectl create namespace argocd
  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/{version}/manifests/install.yaml
  ```
2. Get the default password:
    ```bash
    kubectl -n argocd get secrets argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
    ```
3. Port Forward (for development)
    ```bash
    kubectl port-forward -n argocd services/argocd-server 8080:443
    ```
4. Apply the remaining manifests
    ```bash
    # Apply the projects (RBAC)
    kubectl apply -f argocd/projects

    # Apply the repositories (Helm Credentials)
    kubectl apply -f argocd/repositories

    # Register clusters
    kubectl apply -f argocd/clusters

    # Create ApplicationSets (GitOps)
    kubectl apply -f argocd/applicationSets

    # ArgoCD Self Management
    kubectl apply -f argocd/applications/argocd-self.yaml 
    ```