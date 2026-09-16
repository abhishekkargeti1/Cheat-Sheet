&#x09;	    ARGO CD Cheat Sheet





Step-1 For ARGO CD Installation Refer this (https://github.com/LondheShubham153/argocd-in-one-shot.git)

Step-2 03\_setup\_installation



Step-3 Setup ARGOCD using Helm (Recommended in Production )



helm repo add argo https://argoproj.github.io/argo-helm

helm repo update

kubectl create namespace <Namespace name>

helm install argocd argo/argo-cd -n <Namespace name>

kubectl get pods -n <Namespace name>

kubectl get svc -n <Namespace name>

kubectl port-forward svc/argocd-server -n argocd 8080:443 --address=0.0.0.0 \&

username = admin password = kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d \&\& echo





Step-4 SetUp ARGOCD using kubectl



kubectl create namespace <Namespace name>

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl get pods -n argocd

kubectl get svc -n argocd

kubectl port-forward svc/argocd-server -n argocd 8080:443 --address=0.0.0.0 \&

username = admin password = kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d \&\& echo



Step -5 SetUp ARGOCD CLI



curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64

&#x20;  sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd

&#x20;  rm argocd-linux-amd64

argocd version --client

argocd login <instance\_public\_ip>:8080 --username admin --password <initial\_password> --insecure







kubectl config get-contexts (To get the cluster info)

(To add cluster with ARGOCD) argocd cluster add <Name of the Cluster > --name <Name of the cluster> --insecure



(To get list of cluster added in argocd )argocd cluster list





CLI Command to login



argocd login <instance\_public\_ip>:8080 \\

&#x20; --username admin \\

&#x20; --password <ADMIN\_PASSWORD> \\

&#x20; --insecure





(To get account info ) argocd account get-user-info





Create Application via CLI



argocd app create apache-app \\

&#x20; --repo https://github.com/url of the repo\\

&#x20; --path cli\_approach/apache \\

&#x20; --dest-server https://<your\_added\_cluster\_url> \\

&#x20; --dest-namespace default \\

&#x20; --sync-policy automated \\

&#x20; --self-heal \\

&#x20; --auto-prune





Declarative Approach





apiVersion: argoproj.io/v1alpha1   # API group for ArgoCD resources

kind: Application                  # Resource type is "Application"

metadata:

&#x20; name:                            # Name of this ArgoCD application

&#x20; namespace: argocd                # Must be created in the 'argocd' namespace

spec:

&#x20; project: default                 # ArgoCD Project (logical grouping of apps)

&#x20; source:

&#x20;   repoURL: https://github.com/<your-username>/argocd-demos.git   # Git repo containing manifests

&#x20;   targetRevision: main           # Git branch or tag (e.g., main, dev, release-1.0)

&#x20;   path: declarative\_approach/online\_shop   # Path inside repo where manifests live

&#x20; destination:

&#x20;   server: <argocd\_cluster\_server\_url>   # Target cluster API Private ip

&#x20;   namespace: default             # Namespace in which to deploy the app

&#x20; syncPolicy:                      # Defines how ArgoCD syncs the app

&#x20;   automated:                     # Enable auto-sync

&#x20;     prune: true                  # Delete resources removed from Git

&#x20;     selfHeal: true               # Fix drift if resources are changed manually

&#x20;   syncOptions:

&#x20;     - CreateNamespace=true       # Create namespace if not present







To Create a Project in Argocd (VIA YML)



apiVersion: argoproj.io/v1alpha1 # API version for ArgoCD Projects

kind: AppProject               # Kind is always AppProject for Projects

metadata:

&#x20; name: frontend-team          # Name of the Project (unique within ArgoCD)

&#x20; namespace: argocd            # Must always be created in ArgoCD's namespace

spec:

&#x20; description: Project for frontend team apps   # Optional description

&#x20; sourceRepos:                 # Which Git repos are allowed for this Project

&#x20;   - https://github.com/<your-username>/argocd-demos.git

&#x20; destinations:                # Which clusters/namespaces apps in this Project can target

&#x20;   - namespace: frontend

&#x20;     server: <added\_argocd\_cluster\_server\_url>  # Target cluster server url

&#x20; clusterResourceWhitelist:    # Which cluster-wide resources are allowed (e.g., CRDs)

&#x20;   - group: "\*"               # '\*' means allow all groups

&#x20;     kind: "\*"                # '\*' means allow all kinds

&#x20; namespaceResourceWhitelist:  # Which namespace-level resources are allowed

&#x20;   - group: "\*"               # Here also allowing everything

&#x20;     kind: "\*"

&#x20; roles:                       # Define roles for RBAC within this Project

&#x20;   - name: frontend-admins    # Role name

&#x20;     description: Admins for frontend team

&#x20;     policies:                # Policies associated with this role

&#x20;       # syntax: - p, proj:<project-name>:<role-name>, applications, <action>, <project>/<app-name>, permission(allow|deny)

&#x20;       - p, proj:frontend-team:frontend-admins, applications, \*, frontend-team/\*, allow  # Full access to all apps in this Project, Here p = policy, proj = project, applications = resource, \* = action (all actions), frontend-team/\* = resource name pattern, allow = effect (allow or deny)







(To check number of project in argocd )  argocd proj list

(TO check the app list ) argocd app list

(TO delete app )argocd delete app <app name>





( To create list of app )



apiVersion: argoproj.io/v1alpha1

kind: ApplicationSet

metadata:

&#x20; name: demo-list

&#x20; namespace: argocd

spec:

&#x20; # The list generator lets you enumerate a static set of elements (apps).

&#x20; generators:

&#x20;   - list:

&#x20;       elements:

&#x20;         - app: nginx

&#x20;           path: ui\_approach/nginx     # path in repo for this app where the k8s files are stored

&#x20;         - app: online-shop

&#x20;           path: multicluster/online-shop   # path in repo for this app where the k8s files are stored

&#x20;         - app: chaiapp

&#x20;           path: applicationsets/chai-app      # path in repo for this app where the k8s files are stored

&#x20; # Template that will be used to generate individual Application CRs

&#x20; template:

&#x20;   metadata:

&#x20;     # name template uses the element key 'app'

&#x20;     name: '{{app}}-list'

&#x20;   spec:

&#x20;     project: default

&#x20;     source:

&#x20;       repoURL: https://github.com/<your-username>/argocd-demos.git

&#x20;       targetRevision: main

&#x20;       path: '{{path}}'                # resolved from element

&#x20;     destination:

&#x20;       server: https://kubernetes.default.svc

&#x20;       namespace: default

&#x20;     syncPolicy:

&#x20;       automated:

&#x20;         prune: true

&#x20;         selfHeal: true





(To check the app set list ) argocd appset list



(TO delete the app set) argocd appset delete <name of the app set>





Notification System in ARGOCD Via Email





Take Reference from https://github.com/LondheShubham153/argocd-in-one-shot/tree/main/06\_argocd\_notifications



Step 1 Install Triggers and Templates from the catalog



kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/notifications\_catalog/install.yaml



Step 2 Create a secret-smtp.yml





\# secret-smtp.yaml

\# This file defines a Kubernetes Secret for storing SMTP credentials used by ArgoCD Notifications.



apiVersion: v1  # Specifies the API version for the Kubernetes object.

kind: Secret    # Declares that this object is a Secret.

metadata:

&#x20; name: argocd-notifications-secret  # Name of the Secret resource.

&#x20; namespace: argocd                 # Namespace where the Secret will be created.

type: Opaque  # Generic secret type for arbitrary user-defined data.

stringData:   # Allows you to provide secret data as unencoded strings (Kubernetes will encode them).

&#x20; email-username: "your-email@example.com"   # SMTP sender email address (replace with your own).

&#x20; email-password: "your-smtp-password"       # SMTP password or app password (replace with your own).



\# Note:

\# - This Secret is used by ArgoCD Notifications to authenticate with your SMTP server for sending emails.

\# - Replace the placeholder values with your actual SMTP credentials.

\# - Using stringData is convenient for writing secrets in plain text; Kubernetes will convert them to base64.





Step 3 Create Config Map







apiVersion: v1

kind: ConfigMap

metadata:

&#x20; name: argocd-notifications-cm

&#x20; namespace: argocd

data:

&#x20; # Context section: defines variables available in notification templates

&#x20; context: |

&#x20;   argocdUrl: "http://<your-argocd-server>:8080"  # Replace with your ArgoCD server URL



&#x20; # Email service configuration: SMTP settings for sending emails

&#x20; service.email: |

&#x20;   username: $email-username         # Email username (set as secret)

&#x20;   password: $email-password         # Email password (set as secret)

&#x20;   host: smtp.gmail.com              # SMTP server host

&#x20;   port: 465                         # SMTP server port (SSL)

&#x20;   from: $email-username             # Sender email address



&#x20; # Template for Degraded health: email content when app health is degraded

&#x20; template.email-health-degraded: |

&#x20;   email:

&#x20;     subject: "\[ArgoCD] {{.app.metadata.name}} health is {{.app.status.health.status}}"

&#x20;   message: |

&#x20;     🚨 Application: {{.app.metadata.name}}

&#x20;     📂 Namespace: {{.app.metadata.namespace}}

&#x20;     🔄 Sync status: {{.app.status.sync.status}}

&#x20;     📌 Revision: {{.app.status.sync.revision}}

&#x20;     ❤️ Health status: {{.app.status.health.status}}

&#x20;     📝 Health message: {{.app.status.health.message}}

&#x20;     ⚙️ Last operation: {{ if .app.operationState }}{{ .app.operationState.phase }}{{ else }}<none>{{ end }}

&#x20;     🔗 Details: {{.context.argocdUrl}}/applications/{{.app.metadata.name}}



&#x20; # Template for Deployed (synced + healthy): email content when app is successfully deployed

&#x20; template.email-deployed: |

&#x20;   email:

&#x20;     subject: "\[ArgoCD] {{.app.metadata.name}} successfully deployed 🎉"

&#x20;   message: |

&#x20;     ✅ Application: {{.app.metadata.name}}

&#x20;     📂 Namespace: {{.app.metadata.namespace}}

&#x20;     🔄 Sync status: {{.app.status.sync.status}}

&#x20;     📌 Revision: {{.app.status.sync.revision}}

&#x20;     ❤️ Health status: {{.app.status.health.status}}

&#x20;     ⚙️ Last operation: {{ if .app.operationState }}{{ .app.operationState.phase }}{{ else }}<none>{{ end }}

&#x20;     Finished at: {{ if .app.operationState }}{{ .app.operationState.finishedAt }}{{ end }}

&#x20;     🔗 Details: {{.context.argocdUrl}}/applications/{{.app.metadata.name}}



&#x20; # Trigger for Degraded health: sends email when app health is degraded

&#x20; trigger.on-health-degraded: |

&#x20;   - when: app.status.health.status == 'Degraded'

&#x20;     send: \[email-health-degraded]



&#x20; # Trigger for Deployed: sends email when app is synced and healthy

&#x20; trigger.on-deployed: |

&#x20;   - when: app.status.operationState.phase == 'Succeeded' and app.status.health.status == 'Healthy'

&#x20;     send: \[email-deployed]



\# This ConfigMap configures ArgoCD Notifications to send email alerts for application health changes.

\# It defines SMTP settings, notification templates, and triggers for sending emails on specific events.





Step 4 Add Annotation in the yml file of the project



In Metadata section add these annotation



annotations:

&#x20;   # The email notifier must be configured in argocd-notifications-cm.yaml, The `.email` is the name of the notifier, i.e, `service.email` in the argocd-notifications-cm.yaml.

&#x20;   # Subscribe to notifications when the app health is degraded.

&#x20;   notifications.argoproj.io/subscribe.on-health-degraded.email: "receiver@example.com"  # you can add multiple email ids separated by comma like "receiver@example.com, receiver2@example.com"

&#x20;   # Subscribe to notifications when the app is successfully deployed.

&#x20;   notifications.argoproj.io/subscribe.on-deployed.email: "receiver@example.com"







Install Argo CD Image Updater





Step 1  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj-labs/argocd-image-updater/stable/config/install.yaml



Step 2 Verify pod is running or not kubectl -n argocd get pods -l app.kubernetes.io/name=argocd-image-updater



Step 3 login Docker via Personal Access Token



Step 4 Change / update the image tag



Step 5 push docker image to docker hub





Step 6  Configure Git credentials (for git write-back)



apiVersion: v1 # Specifies the API version for Kubernetes resources

kind: Secret # Declares that this resource is a Secret

metadata:

&#x20; name: argocd-image-updater-git-creds # Name of the Secret object

&#x20; namespace: argocd # Namespace where the Secret will be created

stringData:

&#x20; username: "<github-username>" # Your GitHub username for authentication

&#x20; password: "<personal-access-token>" # Your GitHub personal access token for authentication





Step 7 Add this annotation and kustomization in the Declarative yml of the app



&#x20;annotations:

&#x20;   # Assign alias 'chai-app' to your image

&#x20;   argocd-image-updater.argoproj.io/image-list: chai-app=<your-dockerhub-username>/chai-devops # Maps the image alias 'chai-app' to your DockerHub image



&#x20;   # Use git write-back

&#x20;   argocd-image-updater.argoproj.io/write-back-method: git:secret:argocd/argocd-image-updater-git-creds # Configures image updater to write changes back to Git using provided secret



&#x20;   # Update strategy for chai-app image

&#x20;   argocd-image-updater.argoproj.io/chai-app.update-strategy: semver # Uses semantic versioning for image updates



\---------------------------------------------------------------------------------------------------------------------

spec:

&#x20; project: default              # Associates this Application with the 'default' ArgoCD project

&#x20; source:

&#x20;   repoURL: https://github.com/abhishekkargeti1/Argo\_CD\_Demo.git # Git repository containing the app manifests

&#x20;   targetRevision: main        # Branch or tag to track in the repository

&#x20;   path: AuthServer

&#x20;   kustomize: {}    # Path within the repo where the manifests(kustomization) are located





Step 8 Create CRD of Image Updater



apiVersion: argocd-image-updater.argoproj.io/v1alpha1

kind: ImageUpdater



metadata:

&#x20; name: auth-server-image-updater

&#x20; namespace: argocd



spec:

&#x20; applicationRefs:

&#x20;   - namePattern: "auth-server-app"



&#x20;     images:

&#x20;       - alias: auth-server-image

&#x20;         imageName: abhishekkargeti/authserviceimages:>=1.0.0



&#x20;         commonUpdateSettings:

&#x20;           updateStrategy: semver



&#x20; writeBackConfig:

&#x20;   method: git:secret:argocd-image-updater-git-creds



&#x20;   gitConfig:

&#x20;     branch: main

&#x20;     writeBackTarget: kustomization



Step 9 Create a kustomization.yml (Push this file along with all the manifest file on GitHub repo)



apiVersion: kustomize.config.k8s.io/v1beta1

kind: Kustomization



resources:

&#x20; - auth-server-database-deployment.yml

&#x20; - auth-server-deployment.yml

&#x20; - auth-server-service.yml

&#x20; - ingress.yml

&#x20; - config-file.yml

&#x20; - secret-config-file.yml

&#x20; - mysl\_service.yml

&#x20; - namespace.yml

&#x20; - persistentVolume.yml

images:

&#x20;- name: abhishekkargeti/authserviceimages

&#x20;  newTag: v1.0.3





&#x20;

Step 10 To check the logs of image-updater-controller  kubectl logs -n <name of the namespace> deployment/argocd-image-updater-controller -f







Monitoring ArgoCD





Prerequisites

Kubernetes cluster (kind, minikube, etc.)

ArgoCD Server installed \& running

ArgoCD CLI Installed \& Logged in

kubectl configured

Helm 3.x installed





Step 1: Verify Metrics Endpoints



kubectl get svc -n argocd





Step 2: Install Prometheus \& Grafana



helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

kubectl create namespace monitoring

helm install kube-prometheus-stack prometheus-community/kube-prometheus-stack -n monitoring



username = admin | password =  kubectl --namespace monitoring get secrets kube-prometheus-stack-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo



Step 3 Create ServiceMonitors



\# argocd-service-monitors.yaml

\# This file defines ServiceMonitor resources for monitoring ArgoCD components with Prometheus Operator.



apiVersion: monitoring.coreos.com/v1 # API version for ServiceMonitor

kind: ServiceMonitor                 # Resource type

metadata:

&#x20; name: argocd-metrics               # Name of the ServiceMonitor

&#x20; namespace: argocd                  # Namespace where ServiceMonitor is deployed

&#x20; labels:

&#x20;   release: kube-prometheus-stack   # Label to associate with Prometheus release

spec:

&#x20; selector:

&#x20;   matchLabels:

&#x20;     app.kubernetes.io/name: argocd-metrics # Selects services with this label, must match with ArgoCD metrics service, check with: kubectl get svc -n argocd

&#x20; endpoints:

&#x20; - port: metrics                    # Monitors the 'metrics' port



\---

apiVersion: monitoring.coreos.com/v1 # API version for ServiceMonitor

kind: ServiceMonitor                 # Resource type

metadata:

&#x20; name: argocd-server-metrics        # Name of the ServiceMonitor

&#x20; namespace: argocd                  # Namespace where ServiceMonitor is deployed

&#x20; labels:

&#x20;   release: kube-prometheus-stack   # Label to associate with Prometheus release

spec:

&#x20; selector:

&#x20;   matchLabels:

&#x20;     app.kubernetes.io/name: argocd-server-metrics # Selects services with this label, must match with ArgoCD server metrics service, check with: kubectl get svc -n argocd

&#x20; endpoints:

&#x20; - port: metrics                    # Monitors the 'metrics' port



\---

apiVersion: monitoring.coreos.com/v1 # API version for ServiceMonitor

kind: ServiceMonitor                 # Resource type

metadata:

&#x20; name: argocd-repo-server-metrics   # Name of the ServiceMonitor

&#x20; namespace: argocd                  # Namespace where ServiceMonitor is deployed

&#x20; labels:

&#x20;   release: kube-prometheus-stack   # Label to associate with Prometheus release

spec:

&#x20; selector:

&#x20;   matchLabels:

&#x20;     app.kubernetes.io/name: argocd-repo-server # Selects services with this label, must match with ArgoCD repo server metrics service, check with: kubectl get svc -n argocd

&#x20; endpoints:

&#x20; - port: metrics                    # Monitors the 'metrics' port







kubectl apply -f argocd-service-monitors.yaml





kubectl port-forward service/kube-prometheus-stack-grafana -n monitoring 3000:80 --address=0.0.0.0 \& (For Grafana dashboard)



kubectl port-forward service/kube-prometheus-stack-prometheus  -n monitoring 9090:9090 --address=0.0.0.0 \& (For Prometheus)



Refer to chat gpt pin chat





RBAC in Argocd







Step 1 Creating Local Users



apiVersion: v1

kind: ConfigMap

metadata:

&#x20; name: argocd-cm

&#x20; namespace: argocd

&#x20; labels:

&#x20;   app.kubernetes.io/name: argocd-cm # standard label for ArgoCD config maps, used for selection by ArgoCD components

&#x20;   app.kubernetes.io/part-of: argocd # It is useful to identify all resources that are part of ArgoCD

data:

&#x20; # Add local users with capabilities

&#x20; accounts.alice: apiKey, login  # Can generate tokens and login to UI

&#x20; accounts.bob: login            # Can only login to UI

&#x20; accounts.ci-user: apiKey       # Can only generate tokens (for automation)







kubectl apply -f argocd-user-cm.yaml





argocd account update-password --account <user name>

argocd account update-password --account <user name>





Resource\\Action	get	create	update	delete	sync	action	override	invoke

applications	✅	✅	✅	✅	✅	✅	✅		❌

applicationsets	✅	✅	✅	✅	❌	❌	❌		❌

clusters	✅	✅	✅	✅	❌	❌	❌		❌

projects	✅	✅	✅	✅	❌	❌	❌		❌

repositories	✅	✅	✅	✅	❌	❌	❌		❌

accounts	✅	❌	✅	❌	❌	❌	❌		❌

certificates	✅	✅	❌	✅	❌	❌	❌		❌

gpgkeys		✅	✅	❌	✅	❌	❌	❌		❌

logs		✅	❌	❌	❌	❌	❌	❌		❌

exec		❌	✅	❌	❌	❌	❌	❌		❌

extensions	❌	❌	❌	❌	❌	❌	❌		✅







(To check the access of any user)     argocd admin settings rbac can <user name> update application "<project name>/\*" -n <namespace name>





(To validate the policy file)  argocd admin settings rbac validate --policy-file <policy file name>







###### EKS 



(Take reference from https://github.com/LondheShubham153/argocd-in-one-shot/tree/main/Bonus\_https\_hosting\_argocd)



Step-1  create a new i am role and user 



Step -2 install aws cli 



Step -3 aws configure

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Step 4 - eksctl installed (For linux)



\# Linux/WSL 

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl\_$(uname -s)\_amd64.tar.gz" | tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin



\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Step 5 eksctl installed (For Windows)



Invoke-WebRequest `

&#x20; -Uri "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl\_Windows\_amd64.zip" `

&#x20; -OutFile "eksctl.zip"





Expand-Archive .\\eksctl.zip -DestinationPath .





.\\eksctl.exe version



mkdir C:\\eksctl



Move-Item .\\eksctl.exe C:\\eksctl\\





\[Environment]::SetEnvironmentVariable(

&#x20;   "Path",

&#x20;   \[Environment]::GetEnvironmentVariable("Path", "User") + ";C:\\eksctl",

&#x20;   "User"

)



eksctl version

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_







Step 1 Create EKS Cluster



\# Create EKS Cluster without node group

eksctl create cluster --name argocd-cluster --region eu-west-1 --without-nodegroup





Step 2  Verify Cluster Creation

eksctl get clusters --region eu-west-1



(To delete eks cluster) eksctl delete cluster --name argocd-cluster --region us-east-1



Step 3: Associate IAM OIDC Provider

eksctl utils associate-iam-oidc-provider --region=eu-west-1 --cluster=argocd-cluster --approve



Step 4: Create Node Group

eksctl create nodegroup \\

\--cluster=argocd-cluster \\

\--region=eu-west-1 \\

\--name=argocd-ng \\

\--node-type=t3.medium \\

\--nodes=2 \\

\--nodes-min=1 \\

\--nodes-max=3 \\

\--node-volume-size=20 \\

\--managed



Step 5: Verify Cluster Access



Update kubeconfig



aws eks update-kubeconfig --region eu-west-1 --name argocd-cluster



kubectl get nodes



Step 6: Install ArgoCD

Create namespace



kubectl create namespace argocd

Install ArgoCD



kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

Wait for pods to be ready



kubectl wait --for=condition=ready pod --all -n argocd --timeout=300s





Step 7: Install NGINX Ingress Controller

Add Helm repository



helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

helm repo update



Install ingress controller



helm install my-ingress-nginx ingress-nginx/ingress-nginx \\

&#x20; --namespace ingress-nginx \\

&#x20; --create-namespace \\

&#x20; --set controller.enableSSLPassthrough=true





Step 8: Install cert-manager for SSL Certificates

Install cert-manager



kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.0/cert-manager.yaml

Wait for cert-manager to be ready



kubectl wait --for=condition=ready pod -l app.kubernetes.io/instance=cert-manager -n cert-manager --timeout=300s



Step 9: Configure Let's Encrypt and HTTPS



apiVersion: cert-manager.io/v1

kind: ClusterIssuer

metadata:

&#x20; name: letsencrypt-prod

spec:

&#x20; acme:

&#x20;   server: https://acme-v02.api.letsencrypt.org/directory

&#x20;   email: <your-email@example.com> # Replace with your email

&#x20;   privateKeySecretRef:

&#x20;     name: letsencrypt-prod

&#x20;   solvers:

&#x20;   - http01:

&#x20;       ingress:

&#x20;         class: nginx

\---

apiVersion: cert-manager.io/v1

kind: ClusterIssuer

metadata:

&#x20; name: letsencrypt-staging

spec:

&#x20; acme:

&#x20;   server: https://acme-staging-v02.api.letsencrypt.org/directory

&#x20;   email: <your-email@example.com>  # Replace with your email

&#x20;   privateKeySecretRef:

&#x20;     name: letsencrypt-staging

&#x20;   solvers:

&#x20;   - http01:

&#x20;       ingress:

&#x20;         class: nginx









Step 10 Create argocd-ingress.yaml with your domain (replace argocd.yourdomain.com with your actual domain).



**# argocd-ingress.yaml**

**apiVersion: networking.k8s.io/v1**

**kind: Ingress**

**metadata:**

&#x20; **name: argocd-server-ingress**

&#x20; **namespace: argocd**

&#x20; **annotations:**

&#x20;   **nginx.ingress.kubernetes.io/ssl-passthrough: "true" # Enable SSL passthrough, meaning the ingress will forward HTTPS traffic directly to the backend service without terminating SSL.**

&#x20;   **nginx.ingress.kubernetes.io/backend-protocol: "HTTPS" # Specify that the backend service uses HTTPS**

&#x20;   **cert-manager.io/cluster-issuer: letsencrypt-prod # Switch to production for trusted certificate**

**spec:**

&#x20; **ingressClassName: nginx**

&#x20; **rules:**

&#x20; **- host: argocd.yourdomain.com  # Replace with your domain**

&#x20;   **http:**

&#x20;     **paths:**

&#x20;     **- path: /**

&#x20;       **pathType: Prefix**

&#x20;       **backend:**

&#x20;         **service:**

&#x20;           **name: argocd-server**

&#x20;           **port:**

&#x20;             **name: https**

&#x20; **tls:**

&#x20; **- hosts:**

&#x20;   **- argocd.yourdomain.com  # Replace with your domain**

&#x20;   **secretName: argocd-server-tls # Name of the TLS secret to be created by cert-manager**



&#x09;Apply ArgoCD ingress with SSL



&#x09;kubectl apply -f argocd-ingress.yaml





Step 11: Update DNS and Access ArgoCD





kubectl get svc -n ingress-nginx



**Note Certificate will take time to get successfully issued**



Step 12  Verify SSL Certificate Creation

After applying the ingress, cert-manager will automatically request a Let's Encrypt certificate:



Check certificate request status



kubectl get certificate -n argocd



