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



**argocd login <instance\_public\_ip>:8080 \\**

&#x20; **--username admin \\**

&#x20; **--password <ADMIN\_PASSWORD> \\**

&#x20; **--insecure**





(To get account info ) argocd account get-user-info





**Create Application via CLI**



**argocd app create apache-app \\**

&#x20; **--repo https://github.com/url of the repo\\**

&#x20; **--path cli\_approach/apache \\**

&#x20; **--dest-server https://<your\_added\_cluster\_url> \\**

&#x20; **--dest-namespace default \\**

&#x20; **--sync-policy automated \\**

&#x20; **--self-heal \\**

&#x20; **--auto-prune**





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

&#x20;   **syncOptions:**

&#x20;     **- CreateNamespace=true       # Create namespace if not present**







**To Create a Project in Argocd (VIA YML)**



**apiVersion: argoproj.io/v1alpha1 # API version for ArgoCD Projects**

**kind: AppProject               # Kind is always AppProject for Projects**

**metadata:**

&#x20; **name: frontend-team          # Name of the Project (unique within ArgoCD)**

&#x20; **namespace: argocd            # Must always be created in ArgoCD's namespace**

**spec:**

&#x20; **description: Project for frontend team apps   # Optional description**

&#x20; **sourceRepos:                 # Which Git repos are allowed for this Project**

&#x20;   **- https://github.com/<your-username>/argocd-demos.git**

&#x20; **destinations:                # Which clusters/namespaces apps in this Project can target**

&#x20;   **- namespace: frontend**

&#x20;     **server: <added\_argocd\_cluster\_server\_url>  # Target cluster server url**

&#x20; **clusterResourceWhitelist:    # Which cluster-wide resources are allowed (e.g., CRDs)**

&#x20;   **- group: "\*"               # '\*' means allow all groups**

&#x20;     **kind: "\*"                # '\*' means allow all kinds**

&#x20; **namespaceResourceWhitelist:  # Which namespace-level resources are allowed**

&#x20;   **- group: "\*"               # Here also allowing everything**

&#x20;     **kind: "\*"**

&#x20; **roles:                       # Define roles for RBAC within this Project**

&#x20;   **- name: frontend-admins    # Role name**

&#x20;     **description: Admins for frontend team**

&#x20;     **policies:                # Policies associated with this role**

&#x20;       **# syntax: - p, proj:<project-name>:<role-name>, applications, <action>, <project>/<app-name>, permission(allow|deny)**

&#x20;       **- p, proj:frontend-team:frontend-admins, applications, \*, frontend-team/\*, allow  # Full access to all apps in this Project, Here p = policy, proj = project, applications = resource, \* = action (all actions), frontend-team/\* = resource name pattern, allow = effect (allow or deny)**







(To check number of project in argocd )  argocd proj list

(TO check the app list ) argocd app list

(TO delete app )argocd delete app <app name>





( To create list of app )



**apiVersion: argoproj.io/v1alpha1**

**kind: ApplicationSet**

**metadata:**

&#x20; **name: demo-list**

&#x20; **namespace: argocd**

**spec:**

&#x20; **# The list generator lets you enumerate a static set of elements (apps).**

&#x20; **generators:**

&#x20;   **- list:**

&#x20;       **elements:**

&#x20;         **- app: nginx**

&#x20;           **path: ui\_approach/nginx     # path in repo for this app where the k8s files are stored**

&#x20;         **- app: online-shop**

&#x20;           **path: multicluster/online-shop   # path in repo for this app where the k8s files are stored**

&#x20;         **- app: chaiapp**

&#x20;           **path: applicationsets/chai-app      # path in repo for this app where the k8s files are stored**

&#x20; **# Template that will be used to generate individual Application CRs**

&#x20; **template:**

&#x20;   **metadata:**

&#x20;     **# name template uses the element key 'app'**

&#x20;     **name: '{{app}}-list'**

&#x20;   **spec:**

&#x20;     **project: default**

&#x20;     **source:**

&#x20;       **repoURL: https://github.com/<your-username>/argocd-demos.git**

&#x20;       **targetRevision: main**

&#x20;       **path: '{{path}}'                # resolved from element**

&#x20;     **destination:**

&#x20;       **server: https://kubernetes.default.svc**

&#x20;       **namespace: default**

&#x20;     **syncPolicy:**

&#x20;       **automated:**

&#x20;         **prune: true**

&#x20;         **selfHeal: true**





(To check the app set list ) argocd appset list



(TO delete the app set) argocd appset delete <name of the app set>





##### Notification System in ARGOCD Via Email





Take Reference from https://github.com/LondheShubham153/argocd-in-one-shot/tree/main/06\_argocd\_notifications



Step 1 Install Triggers and Templates from the catalog



kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/notifications\_catalog/install.yaml



Step 2 Create a secret-smtp.yml 





**# secret-smtp.yaml**

**# This file defines a Kubernetes Secret for storing SMTP credentials used by ArgoCD Notifications.**



**apiVersion: v1  # Specifies the API version for the Kubernetes object.**

**kind: Secret    # Declares that this object is a Secret.**

**metadata:**

&#x20; **name: argocd-notifications-secret  # Name of the Secret resource.**

&#x20; **namespace: argocd                 # Namespace where the Secret will be created.**

**type: Opaque  # Generic secret type for arbitrary user-defined data.**

**stringData:   # Allows you to provide secret data as unencoded strings (Kubernetes will encode them).**

&#x20; **email-username: "your-email@example.com"   # SMTP sender email address (replace with your own).**

&#x20; **email-password: "your-smtp-password"       # SMTP password or app password (replace with your own).**



**# Note:**

**# - This Secret is used by ArgoCD Notifications to authenticate with your SMTP server for sending emails.**

**# - Replace the placeholder values with your actual SMTP credentials.**

**# - Using stringData is convenient for writing secrets in plain text; Kubernetes will convert them to base64.**





Step 3 Create Config Map 







**apiVersion: v1**

**kind: ConfigMap**

**metadata:**

&#x20; **name: argocd-notifications-cm**

&#x20; **namespace: argocd**

**data:**

&#x20; **# Context section: defines variables available in notification templates**

&#x20; **context: |**

&#x20;   **argocdUrl: "http://<your-argocd-server>:8080"  # Replace with your ArgoCD server URL**



&#x20; **# Email service configuration: SMTP settings for sending emails**

&#x20; **service.email: |**

&#x20;   **username: $email-username         # Email username (set as secret)**

&#x20;   **password: $email-password         # Email password (set as secret)**

&#x20;   **host: smtp.gmail.com              # SMTP server host**

&#x20;   **port: 465                         # SMTP server port (SSL)**

&#x20;   **from: $email-username             # Sender email address**



&#x20; **# Template for Degraded health: email content when app health is degraded**

&#x20; **template.email-health-degraded: |**

&#x20;   **email:**

&#x20;     **subject: "\[ArgoCD] {{.app.metadata.name}} health is {{.app.status.health.status}}"**

&#x20;   **message: |**

&#x20;     **🚨 Application: {{.app.metadata.name}}**

&#x20;     **📂 Namespace: {{.app.metadata.namespace}}**

&#x20;     **🔄 Sync status: {{.app.status.sync.status}}**

&#x20;     **📌 Revision: {{.app.status.sync.revision}}**

&#x20;     **❤️ Health status: {{.app.status.health.status}}**

&#x20;     **📝 Health message: {{.app.status.health.message}}**

&#x20;     **⚙️ Last operation: {{ if .app.operationState }}{{ .app.operationState.phase }}{{ else }}<none>{{ end }}**

&#x20;     **🔗 Details: {{.context.argocdUrl}}/applications/{{.app.metadata.name}}**



&#x20; **# Template for Deployed (synced + healthy): email content when app is successfully deployed**

&#x20; **template.email-deployed: |**

&#x20;   **email:**

&#x20;     **subject: "\[ArgoCD] {{.app.metadata.name}} successfully deployed 🎉"**

&#x20;   **message: |**

&#x20;     **✅ Application: {{.app.metadata.name}}**

&#x20;     **📂 Namespace: {{.app.metadata.namespace}}**

&#x20;     **🔄 Sync status: {{.app.status.sync.status}}**

&#x20;     **📌 Revision: {{.app.status.sync.revision}}**

&#x20;     **❤️ Health status: {{.app.status.health.status}}**

&#x20;     **⚙️ Last operation: {{ if .app.operationState }}{{ .app.operationState.phase }}{{ else }}<none>{{ end }}**

&#x20;     **Finished at: {{ if .app.operationState }}{{ .app.operationState.finishedAt }}{{ end }}**

&#x20;     **🔗 Details: {{.context.argocdUrl}}/applications/{{.app.metadata.name}}**



&#x20; **# Trigger for Degraded health: sends email when app health is degraded**

&#x20; **trigger.on-health-degraded: |**

&#x20;   **- when: app.status.health.status == 'Degraded'**

&#x20;     **send: \[email-health-degraded]**



&#x20; **# Trigger for Deployed: sends email when app is synced and healthy**

&#x20; **trigger.on-deployed: |**

&#x20;   **- when: app.status.operationState.phase == 'Succeeded' and app.status.health.status == 'Healthy'**

&#x20;     **send: \[email-deployed]**



**# This ConfigMap configures ArgoCD Notifications to send email alerts for application health changes.**

**# It defines SMTP settings, notification templates, and triggers for sending emails on specific events.**





**Step 4 Add Annotation in the yml file of the project** 



**In Metadata section add these annotation**



**annotations:**

&#x20;   **# The email notifier must be configured in argocd-notifications-cm.yaml, The `.email` is the name of the notifier, i.e, `service.email` in the argocd-notifications-cm.yaml.**

&#x20;   **# Subscribe to notifications when the app health is degraded.**

&#x20;   **notifications.argoproj.io/subscribe.on-health-degraded.email: "<receiver@example.com>"  # you can add multiple email ids separated by comma like "<receiver@example.com>, <receiver2@example.com>"**

&#x20;   **# Subscribe to notifications when the app is successfully deployed.**

&#x20;   **notifications.argoproj.io/subscribe.on-deployed.email: "<receiver@example.com>"**







##### Install Argo CD Image Updater





Step 1  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj-labs/argocd-image-updater/stable/config/install.yaml



Step 2 Verify pod is running or not kubectl -n argocd get pods -l app.kubernetes.io/name=argocd-image-updater



Step 3 login Docker via Personal Access Token



Step 4 Change / update the image tag



Step 5 push docker image to docker hub





Step 6  Configure Git credentials (for git write-back)



**apiVersion: v1 # Specifies the API version for Kubernetes resources**

**kind: Secret # Declares that this resource is a Secret**

**metadata:**

&#x20; **name: argocd-image-updater-git-creds # Name of the Secret object**

&#x20; **namespace: argocd # Namespace where the Secret will be created**

**stringData:**

&#x20; **username: "<github-username>" # Your GitHub username for authentication**

&#x20; **password: "<personal-access-token>" # Your GitHub personal access token for authentication**





Step 7 Add this annotation in the Declarative yml of the app 



&#x20;**annotations:**

&#x20;   **# Assign alias 'chai-app' to your image**

&#x20;   **argocd-image-updater.argoproj.io/image-list: chai-app=<your-dockerhub-username>/chai-devops # Maps the image alias 'chai-app' to your DockerHub image**



&#x20;   **# Use git write-back**

&#x20;   **argocd-image-updater.argoproj.io/write-back-method: git:secret:argocd/argocd-image-updater-git-creds # Configures image updater to write changes back to Git using provided secret**



&#x20;   **# Update strategy for chai-app image**

&#x20;   **argocd-image-updater.argoproj.io/chai-app.update-strategy: semver # Uses semantic versioning for image updates**







&#x20;

Step 8 To check the logs of image-updater-controller kubectl  logs deploy/argocd-image-updater-controller -f -n <name of the namespace>

&#x20;











