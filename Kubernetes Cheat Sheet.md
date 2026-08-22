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



(TO create multiple pods )kubectl scale deployment/<Pod Name> -n <namespace> --replicas=90



(To set new image version on running pods ) kubectl set image deployment/<deployment-name> <container-name>=<new-image> -n namespace



(To get everything which inside the namespace) kubectl get all -n <namespace name>



(To get the ip and port of the pod )  kubectl get svc -n <namespace>



(Port Forwarding Command) kubectl port-forward service/<name of the service yml file> -n namespace name  <Port binding> --address =0.0.0.0



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





