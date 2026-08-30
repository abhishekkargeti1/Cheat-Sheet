# &#x09;	Kubernetes Cheat Sheet





**Types to make Kubernetes Cluster**

&#x09;**Kubeadm  (Create 3 more different ec2 Instance add all in one cluster)**

&#x09;**Minikube  (Create in single ec2 or a local machine)**

&#x09;**KIND Cluster (Kubernetes in Docker)**





**Kind Cluster Installation (Take Reference From this GitHub https://github.com/LondheShubham153/kubestarter/tree/main/kind-cluster)**





**To Create a Cluster with a Config File (kind create cluster --name=<Name of the cluster> --config=<Config file Path>)**



**(To check the cluster info  ) kubectl cluster-info --context kind-<cluster name>**



**(To get Number of Nodes ) kubectl get nodes**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



**Minikube Installation (Take Reference From this GitHub https://github.com/LondheShubham153/kubestarter/blob/main/minikube\_installation.md)**



**(To start Minikube ) minikube start --driver=docker --vm=true**



**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**





kubectl get nodes --context <cluster name> (To change the context )



(To set default cluster) kubectl config use-context <cluster name>



(To get all context nodes name) kubectl config get-contexts



(To get default node) kubectl get nodes



(To get namespaces list ) kubectl get ns / namespace



(To create new namespace ) kubectl create ns <new namespace name>



(To create a pod ) kubectl run <name of the pod > --image=<image name> /kubectl run <name of the pod > --image=<image name> -n <namespace name>



(To get list of pods ) kubectl get pods



(To get pod from any namespace ) kubectl get pod -n <namespace name>



(To delete any pod ) kubectl delete pod <Pod name>





(To create Ns with manifest file )

1. Create One yml file
kind: Namespace

&#x09;apiVersion : v1

&#x09;metadata:

&#x20; 		name: nginx



2\. kubectl apply -f <name of the yml file>





(To create Pod with manifest file )



1\. Create One yml file



kind: Pod

apiVersion: v1

metadata:

&#x20; name: nginx

&#x20; namespace: nginx

spec:

&#x20; containers:

&#x20; - name: nginx

&#x20;   image: nginx:latest

&#x20;   ports:

&#x20;     - containerPort: 80



2\. kubectl apply -f <name of the yml file>



(To get inside the pod)



kubectl exec -it <pod name> -n <namespace name> -- bash



(To get Details of pod)



kubectl describe pod/<pod name> -n <name of the namespace>



(To delete any pod with manifest file)



kubectl delete -f <deployment filename>

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_



**Deployed Auth Service**

ubuntu@ip-172-31-29-124:\~/authserver$ cat mysl\_service.yml



apiVersion: v1

kind: Service

metadata:

&#x20; name: mysqlcontainer

&#x20; namespace: authserver

spec:

&#x20; selector:

&#x20;   app: mysql

&#x20; ports:

&#x20;   - port: 3306

&#x20;     targetPort: 3306



ubuntu@ip-172-31-29-124:\~/authserver$ cat auth-server-deployment.yml



kind: Deployment

apiVersion: apps/v1

metadata:

&#x20; name: auth-server-deployment

&#x20; namespace: authserver

spec:

&#x20; replicas: 3

&#x20; selector:

&#x20;   matchLabels:

&#x20;     app: auth-server-app



&#x20; template:

&#x20;   metadata:

&#x20;     name: auth-server

&#x20;     labels:

&#x20;       app: auth-server-app



&#x20;   spec:

&#x20;     containers:

&#x20;       - name: auth-server-app

&#x20;         image: abhishekkargeti/authserviceimages

&#x20;         ports:

&#x20;           - containerPort: 8080

&#x20;         env:

&#x20;           - name: DB\_URL

&#x20;             value: "jdbc:mysql://mysqlcontainer:3306/employees"

&#x20;           - name: DB\_USERNAME

&#x20;             value: "root"

&#x20;           - name: DB\_PASSWORD

&#x20;             value: "1808"



ubuntu@ip-172-31-29-124:\~/authserver$ cat auth-server-database-deployment.yml



apiVersion: apps/v1

kind: Deployment

metadata:

&#x20; name: mysqlcontainer

&#x20; namespace: authserver

spec:

&#x20; replicas: 1

&#x20; selector:

&#x20;   matchLabels:

&#x20;     app: mysql

&#x20; template:

&#x20;   metadata:

&#x20;     labels:

&#x20;       app: mysql

&#x20;   spec:

&#x20;     containers:

&#x20;       - name: mysql

&#x20;         image: mysql:latest

&#x20;         ports:

&#x20;           - containerPort: 3306



&#x20;         env:

&#x20;           - name: MYSQL\_ROOT\_PASSWORD

&#x20;             value: "1808"



&#x20;           - name: MYSQL\_DATABASE

&#x20;             value: "employees"



(To get info about pods) kubectl get pods -n <namespace> -o wide



(TO create multiple pods )kubectl scale deployment/<Deployment Name> -n <namespace> --replicas=90



(To set new image version on running pods ) kubectl set image deployment/<deployment-name> <container-name>=<new-image> -n namespace



(To get everything which inside the namespace) kubectl get all -n <namespace name>



(To get the ip and port of the pod )  kubectl get svc -n <namespace>



(Port Forwarding Command) kubectl port-forward service/<name of the ingress service controller > -n namespace name  <Port binding> --address =0.0.0.0



\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

(Ingress setup in kind cluster)



Step 1. kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml



\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_







### **Example of Stateful set with MySQL**



ubuntu@ip-172-31-29-124:\~/authserver$ cat mysl\_service.yml

apiVersion: v1

kind: Service

metadata:

&#x20; name: mysqlcontainer

&#x20; namespace: authserver

spec:

&#x20; clusterIP: None

&#x20; selector:

&#x20;   app: mysql

&#x20; ports:

&#x20;   - port: 3306

&#x20;     name: mysqlcontainer1

&#x20;     targetPort: 3306

ubuntu@ip-172-31-29-124:\~/authserver$ cat auth-server-database-deployment.yml

apiVersion: apps/v1

kind: StatefulSet

metadata:

&#x20; name: mysqlcontainer

&#x20; namespace: authserver

spec:

&#x20; replicas: 1

&#x20; selector:

&#x20;   matchLabels:

&#x20;     app: mysql

&#x20; template:

&#x20;   metadata:

&#x20;     labels:

&#x20;       app: mysql

&#x20;   spec:

&#x20;     containers:

&#x20;       - name: mysql

&#x20;         image: mysql:latest

&#x20;         ports:

&#x20;           - containerPort: 3306



&#x20;         env:

&#x20;           - name: MYSQL\_ROOT\_PASSWORD

&#x20;             value: "1808"



&#x20;           - name: MYSQL\_DATABASE

&#x20;             value: "employees"

&#x20;         volumeMounts:

&#x20;           - name: mysql-storage

&#x20;             mountPath: /var/lib/mysql

&#x20;     volumes:

&#x20;       - name: mysql-storage

&#x20;         persistentVolumeClaim:

&#x20;           claimName: mysql-pvc

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_



### Example of Ingress



ubuntu@ip-172-31-29-124:\~/authserver$ cat ingress.yml

apiVersion: networking.k8s.io/v1

kind: Ingress

metadata:

&#x20; name: auth-server-ingress

&#x20; namespace: authserver

&#x20; annotations:

&#x20;   nginx.ingress.kubernetes.io/rewrite-target: /$2

spec:

&#x20; rules:

&#x20; - http:

&#x20;     paths:

&#x20;     - pathType: ImplementationSpecific

&#x20;       path: /authservice(/|$)(.\*)

&#x20;       backend:

&#x20;         service:

&#x20;           name: auth-server-service

&#x20;           port:

&#x20;             number: 80

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_



### Resource Allocation



&#x20;spec:

&#x20;     containers:

&#x20;       - name: auth-server-app

&#x20;         image: abhishekkargeti/authserviceimages

&#x20;         ports:

&#x20;           - containerPort: 8087

&#x20;         resources:

&#x20;           requests:

&#x20;             memory: "200Mi"

&#x20;             cpu: "250m"

&#x20;           limits:

&#x20;             memory: "300Mi"

&#x20;             cpu: "500m"



\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_





(Taint and Tolerance)



Taint - It means when we don't want to create any pod a particular node (For that we use Taint)





(This command is to taint the node ) kubectl taint node <cluster node name > <any key example prod=true>:NoSchedule



(This command is to untaint the node ) kubectl taint node <cluster node name > <any key example prod=true>:NoSchedule-





Tolerance - It means when in special case if we have to run the pod on tainted node (For the we use Tolerance)



&#x20; spec:

&#x20;     tolerations:

&#x20;       - key: "prod"

&#x20;         operator: "Equal"

&#x20;         value: "true"

&#x20;         effect: "NoSchedule"





(To run Load generator) kubectl run -it --tty load-generator --image=busybox -n apache-namespace /bin/sh







(To get VPA we have to get files from github) 



Step -1 git clone https://github.com/kubernetes/autoscaler.git

Step -2 cd autoscaler/vertical-pod-autoscaler

Step -3 install VPA (./hack/vpa-up.sh)

Ste - 4 Create vpa.yml 



kind: VerticalPodAutoscaler

apiVersion: autoscaling.k8s.io/v1

metadata:

&#x20; name: apache-vpa

&#x20; namespace: apache-namespace

spec:

&#x20; targetRef:

&#x20;   apiVersion: apps/v1

&#x20;   kind: Deployment

&#x20;   name: apache-server-pod

&#x20; updatePolicy:

&#x20;   updateMode: "Auto"



(To select a particular node where your pod should deploy ) we use the concept of node affinity 


spec:

&#x20; affinity:

&#x20;   nodeAffinity:

&#x20;     requiredDuringSchedulingIgnoredDuringExecution:

&#x20;       nodeSelectorTerms:

&#x20;       - matchExpressions:

&#x20;         - key: topology.kubernetes.io/zone

&#x20;           operator: In

&#x20;           values:

&#x20;           - antarctica-east1

&#x20;           - antarctica-west1

put this above config in pod spec. 





(To check whoami ) kubectl auth whoami





(To check other user access) kubectl auth can-i get pods -n <namespace name> --as=<other user name>

(To check the other user access) kubectl auth can-i get pods -n <namespace name> --as=<other user name service account name>

&#x20; 

(RBAC in namespace)



Step1 create a role

Step2 create service account 

Step3 create a role binding 





(RBAC in Cluster)



**(Take Reference From this GitHub https://github.com/LondheShubham153/kubestarter/tree/main/kind-cluster)**



Step 1 **kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml**





Step 2  dashboard-admin-user.yml



Step 3 apply dashboard.yml



Step 4 Create a token 

kubectl -n kubernetes-dashboard create token admin-user



Step 5 Expose Proxy 

kubectl proxy --port=8001 --address=0.0.0.0 --accept-hosts='.\*'



Step 6 hit on this url



http://<Public Ip>:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/ 



This above link is not going to working on your localmachine browser



To get access of the Admin Dashboard on Local System  

Follow these steps



Step 1 kubectl get svc -n kubernetes-dashboard
Step 2 kubectl -n kubernetes-dashboard port-forward svc/<Service name> 8443:443

Step 3 ssh -i "C:\\path\\to\\your-key.pem" -L 8443:localhost:8443 ubuntu@YOUR\_EC2\_PUBLIC\_IP
Step 4 https://localhost:8443 



(Helm)



Step 1 



curl -fsSL -o get\_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4

chmod 700 get\_helm.sh

./get\_helm.sh



Step 2 helm create authserver-helm

Step 3 vim value.yaml  (This is user to put values in the deployment,Service etc )

Step 4 helm package .

Step 4 helm install <Any Name you can give here> package helm file name -n <namespace name> --create-namespace. 



(To upgrade any existing image )helm upgrade apache-dev <name of the helm packaged folder> -n <namespace>



(To uninstall existing helm chart) helm uninstall <name of the helm deployment>



(TO rollback to previous version through helm) helm rollback <helm-name> -n <namespace> (To find helm name use helm list)



&#x09;				Use **Artifact Hub** to install any image by using helm 

