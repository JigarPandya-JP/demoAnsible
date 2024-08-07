# Ansible Installation

Ansible is an open-source automation platform. It is very, very simple to set up and yet powerful. Ansible can help you with configuration management, application deployment, task automation.

### Pre-requisites

1. An AWS EC2 instance (on Control node)

### Installation steps:
#### on Amazon EC2 instance

   
1. Create a user called ansadmin (on Control node and Managed host)  
   ```sh
   useradd ansadmin
   passwd ansadmin
   
   visudo
   
   ansadmin ALL=(ALL) NOPASSWD: ALL  >> escape + :wq
   
   nano /etc/ssh/sshd_config
   #PasswordAuthentication no
   PasswordAuthentication yes		>>SAVE
   
   service sshd reload

   
2. Log in as a ansadmin user on master and generate ssh key (on Control node)
   ```sh 
   sudo su - ansadmin
   ssh-keygen
   ```
   
3.	Install ansible2
	```sh
	amazon-linux-extras install ansible2
	
	python --version
	ansible --version
