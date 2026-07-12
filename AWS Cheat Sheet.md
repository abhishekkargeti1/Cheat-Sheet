# &#x09;			AWS Cheat Sheet







1. I am  - AWS Identity \& Access Management

&#x09;1.1 IAM is a service that helps you securely control access to AWS resource.

&#x09;1.2 Its allow you to mange users , roles and permissions to define who can access what within your AWS environment.

&#x09;1.3 Its is global service.

&#x09;1.4 Root account created by default ,shouldn't be used or shared.

&#x09;



2\. AWS CLI Commands
2.1 AWS IAM list-users (To get number of users)

&#x09;2.2 AWS configure (To setup cli in local system )



3\. AWS EC2 (Amazon Elastic Compute Cloud)

&#x09;It is a cloud service that provides resizable virtual server called instances

&#x09;which you can use to run applications

&#x09;

* &#x09;Instance Type - Select the hardware capacity
* &#x09;AMI (Amazon Machine Image) - Choose the operating system and software
* &#x09;Storage - Configure the type and size of storage (Example EBS Volume)
* &#x09;Security Group -Set up the firewall rules to control inbound/outbound traffic
* &#x09;Key Pair - Create or use an existing key pair for SSH access
* &#x20; 	Network Settings - Configure VPC ,subnet and assign public or private IP addresses
* &#x20;   	IAM Roles - Attach an IAM role for permission to access other AWS resource
* &#x20;   	User Data - Add script to executed when the instance starts
* &#x20;   	Elastic IP Optionally associate a static IP Address for consistent public access.



4\. Security Group

* &#x09;Network Firewall rules that control inbound and outbound traffic for instance.
* &#x09;Region Specific
* &#x09;Only Allow rules (but no deny rule)
* &#x09;All inbound traffic blocked and outbound allowed by default
* &#x09;You Define rule for specific :
* &#x09;	Protocol (Like HTTP ,SSH etc )
* &#x09;	Port Network (e.g. :- port 80 for HTTP, port 22 for SSH)
* &#x09;	IP Address or ranges (e.g. :- allow traffic from specific IP or range of IPS)
* &#x09;If you allow incoming traffic on a specific port the outgoing response traffic in automatically allowed without an explicit outbound rule



5\. AWS EBS (Elastic Block Store)



* &#x09;It is a cloud based storage service that provide durable high performance block 

&#x09;storage for use with Amazon EC2 instances.

&#x09;		

* &#x09;It works like a virtual hard drive allowing you to store and access data even

&#x09;when your EC2 instances are stopped or terminated.



* &#x09;Region \& AZ specific 
* &#x09;Build in Redundancy 
* &#x09;	EBS volumes are automatically replicated within the same Availability
* &#x09;	Zone to prevent data loss due to hardware failures.
* &#x09;Different Volume Types
* &#x09;	gp2/3,io1 etc
* &#x09;Allow Encryption \& Snapshot for backup
* &#x09;Scalable (Volume can be resizable) 
* &#x09;	No Data loss will occur during resizing
* &#x09;	No need to restart the EC2 instance during the process



6\. AWS EBS Snapshot

&#x09;What if we want copy our data to 

&#x09;	New AZ

&#x09;	New Region

