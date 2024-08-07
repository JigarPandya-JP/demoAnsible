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
   ```

 4	create RHEL Server  on EC2 and repeat following task
	   ```sh

      hostname rhel-managed-node

	   useradd ansadmin
	   passwd ansadmin
	   
	   visudo
	   
      #add following text at last line(shift + g), press escape, enter :wq

	   ansadmin ALL=(ALL) NOPASSWD: ALL  
	   
	   vi /etc/ssh/sshd_config
	   #PasswordAuthentication no
	   PasswordAuthentication yes		>>SAVE

      vi /etc/ssh/sshd_config.d/50-cloud-init.conf
      PasswordAuthentication yes    >>SAVE
       
	   service sshd reload


      ```
5. Add RHEL Managed node to control node
      ```sh
      login to ansible server with ansadmin
      
      Add Private IP address of RHEL EC2 to following
         nano /etc/ansible/hosts

      Copy ssh id
         ssh-copy-id 172.31.35.135

      Try to login
         ssh  172.31.35.135


      ```
6. Run Adhoc Commands
      ```sh
      
      Ping		:	ansible all -m ping
      
      Command	:	ansible all -m command -a "uptime“ /  “date” / “who”
      
      Stat		:	ansible all -m stat -a "path=/etc/hosts"
      
      Yum		:	 ansible all -m yum -a "name=git" –b
      
      User		:	ansible all -m user –a “name=scott” -b
      
      Setup	   :	ansible all -m setup  
      

      ```

7. Config File Demo
      ```sh
      Create ansible.config file under home and add following
         [privilege_escalation]
         become=False

      ansible all -m yum -a "name=tree"


      ```

8. Modules
      ```sh

      ansible-doc -l 
      ansible-doc -l | wc

      ```

9. Playbooks
      ```sh

      ansible-doc -l 
      ansible-doc -l | wc

      ```