# &#x09;	    ARGO CD Cheat Sheet





**Step-1** For ARGO CD Installation Refer this (https://github.com/LondheShubham153/argocd-in-one-shot.git)

**Step-2** 03\_setup\_installation 



**Step-3** Setup ARGOCD using Helm (Recommended in Production )



* helm repo add argo https://argoproj.github.io/argo-helm
* helm repo update	 
* kubectl create namespace <Namespace name>
* helm install argocd argo/argo-cd -n <Namespace name>
* kubectl get pods -n <Namespace name>
* kubectl get svc -n <Namespace name>
* kubectl port-forward svc/argocd-server -n argocd 8080:443 --address=0.0.0.0 \&
* username = admin password = kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d \&\& echo





**Step-4** SetUp ARGOCD using kubectl 



* kubectl create namespace <Namespace name>
* kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
* kubectl get pods -n argocd
* kubectl get svc -n argocd
* kubectl port-forward svc/argocd-server -n argocd 8080:443 --address=0.0.0.0 \&
* username = admin password = kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d \&\& echo



Step -5 SetUp ARGOCD CLI



* curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64

&#x20;  sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd

&#x20;  rm argocd-linux-amd64

* argocd version --client
* argocd login <instance\_public\_ip>:8080 --username admin --password <initial\_password> --insecure







kubectl config get-contexts (To get the cluster info)

(To add cluster with ARGOCD) argocd cluster add <Name of the Cluster > --name <Name of the cluster> --insecure



(To get list of cluster added in argocd )argocd cluster list





**CLI Command to login**



argocd login <instance\_public\_ip>:8080 \\

&#x20; --username admin \\

&#x20; --password <ADMIN\_PASSWORD> \\

&#x20; --insecure





(To get account info ) argocd account get-user-info





**Create Application via CLI**



argocd app create apache-app \\

&#x20; --repo https://github.com/url of the repo\\

&#x20; --path cli\_approach/apache \\

&#x20; --dest-server https://<your\_added\_cluster\_url> \\

&#x20; --dest-namespace default \\

&#x20; --sync-policy automated \\

&#x20; --self-heal \\

&#x20; --auto-prune





**Declarative Approach**





**apiVersion: argoproj.io/v1alpha1   # API group for ArgoCD resources**

**kind: Application                  # Resource type is "Application"**

**metadata:**

&#x20; **name:                            # Name of this ArgoCD application**

&#x20; **namespace: argocd                # Must be created in the 'argocd' namespace**

**spec:**

&#x20; **project: default                 # ArgoCD Project (logical grouping of apps)**

&#x20; **source:**

&#x20;   **repoURL: https://github.com/<your-username>/argocd-demos.git   # Git repo containing manifests**

&#x20;   **targetRevision: main           # Git branch or tag (e.g., main, dev, release-1.0)**

&#x20;   **path: declarative\_approach/online\_shop   # Path inside repo where manifests live**

&#x20; **destination:**

&#x20;   **server: <argocd\_cluster\_server\_url>   # Target cluster API Private ip**

&#x20;   **namespace: default             # Namespace in which to deploy the app**

&#x20; **syncPolicy:                      # Defines how ArgoCD syncs the app**

&#x20;   **automated:                     # Enable auto-sync**

&#x20;     **prune: true                  # Delete resources removed from Git**

&#x20;     **selfHeal: true               # Fix drift if resources are changed manually**





















