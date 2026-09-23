# &#x20;                   Terraform Cheat Sheet



**To install terraform in the Linux**





wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

echo "deb \[arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU\_CODENAME=).\*' /etc/os-release || lsb\_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update \&\& sudo apt install terraform





* **To create a terraform env**



* terraform init



* **To validate terraform script**



* terraform validate



* **To see the planning  (Dry run the .tf file )**



* terraform plan



* **To apply the terraform file**



* terraform apply / terraform apply -auto-approve



* **To delete the terraform created file**



* terraform destroy / terraform destroy -auto-approve /  terraform destroy --target=<name of the state list> -auto-approve

&#x20;



##### &#x20;                                **State Management Command**





* **To get list of state**



* terraform state list

&#x20;

* **To Refresh the State of the Terraform**

&#x20;

* terraform refresh



* **To Remove the state link**



* terraform state rm <name of the resource>

&#x20;

* **To check the description of a particular state**

&#x20;

* terraform state show <name of the resource>



* **To import any thing from cloud to local**



* terraform import <name of the resource>

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_



* **To format all the .tf files**



* terraform fmt



* **To Create a workspace** 

&#x20;

* terraform workspace new <workspace name>



* **To list a workspaces**

&#x20;

* terraform workspace list



* **To select  a workspaces**

&#x20;

* terraform workspace select <workspace name>





