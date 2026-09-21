# &#x09;	    Terraform Cheat Sheet



**To install terraform in the Linux**





* wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
* echo "deb \[arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU\_CODENAME=).\*' /etc/os-release || lsb\_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
* sudo apt update \&\& sudo apt install terraform





* **To create a terraform env**



&#x20;   terraform init



* **To validate terraform script**



&#x20;   terraform validate



* **To see the planning  (Dry run the .tf file )**



* terraform plan



* **To apply the terraform file**



* terraform apply / terraform apply -auto-approve



* **To delete the terraform created file**



* terraform destroy / terraform destroy -auto-approve

