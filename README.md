# bastion-playbook
Ansible Playbook for creating Bastion Architecture on AWS

Prerequisites

- Ansbible playbook is written in version "2.9.7"
- Needs to install python boto library in order to interact with AWS
- If you're using default python interpreter the boto will work 
- if you're using python interpreter of version 3 or above then you'll need boto3 library
- encrypt the variable files in bastionhost/vars/main.yml as AWS access key and secret access key is declare in that

For encrypting use the following command

ansible-vault encrypt bastionhost/vars/main.yml

- set the password for encryption

For editing this file use the following command 

ansible-vault edit bastionhost/vars/main.yml

And for running play book use the following command 

ansible-playbook --ask-vault-pass site.yml




Following tasks been performed with this ansible playbook bastionhost/tasks/main.yml

- Custom VPC creation (10.0.0.0/16)
- IGW creation
- Public Subnet in us-east-1a (For Bastion/Jump Box public zone) (10.0.1.0/24)
- Private subnet App in us-east-1a (For Private Application private zone) (10.0.4.0/24)
- Private subnet DB1 in us-east-1a (For Database RDS as RDS required two zones for it's subnet group) (10.0.7.0/24)
- Private Subnet DB2 in us-east-1b (For Database RDS as RDS required two zones for it's subnet group) (10.0.9.0/24)
- NGW creation with EIP
- Route table For public subnet with IGW and with cidr of vpc
- Route table For private subnet APP with NGW and with cidr of vpc
- Route table For private subnet DB1 with only cidr of VPC
- Route table For private subnet DB2 with only cidr of VPC
- Create security group of Bastion/jump box which will be in public subnet with port 22,80,443 opens for 0.0.0.0/0
- Create security group for private Applications which will be in private subnet with port 22 opens only for Bastion Security group
- Create Security group for RDS which will be in private subnet with port 3306 opens only for private applications server
- Create a Bastion key and saving it in current bastion host directory with name bastion.pem and setting permission "400" for the key as well
- Create a Private Application server key and saving it in current bastion host directory with name private_app.pem and setting permission "400" for the key as well
- Saving key file in bastion/bastionhost/ directory adjust the path a/c to the directory structure you have
- Create Bastion/Jumpbox server in public subnet with tag "Bastion" and assign EIP as well, with key bastion.pem, with security group of Bastion
- Create two App01 and App02 in private subnet with tag "Private-App01" and "Private-App02", with key private_app.pem and with security group of private security group
- All instance volume size are 10 GB and ami is of Ubuntu 18
- Create RDS subnet group with two zones of subnet Private subnet DB1 and Private Subnet DB2
- Create RDS with this DB subnet group with zone us-east-1a, MYSQL 8.0.17, size 20 GB, Backup retention of 7 days
- RDS username - admin, Pass - 1nsecure 




