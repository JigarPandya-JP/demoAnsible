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
   
3. Install ansible2
   ```sh
	amazon-linux-extras install ansible2
	
	python --version
	ansible --version

   Alternate way using this link
   	(https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-and-upgrading-ansible-with-pip
   	after installation run $ ansible-config init --disabled > ansible.cfg
   	)
   ```

4.	An AWS EC2 instance (on Managed node : RHEL)
	```sh
   	hostname rhel-managed-node
	useradd ansadmin
	passwd ansadmin
	   
	visudo
	   
  	 add following text at last line(shift + g), press escape, enter :wq

	ansadmin ALL=(ALL) NOPASSWD: ALL  
	   
	vi /etc/ssh/sshd_config
	#PasswordAuthentication no
	PasswordAuthentication yes		>>SAVE

   	following step is only for RHEL
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
         ssh-copy-id [IP of your managed node :172.31.35.135]

   Try to login
         ssh  [IP of your managed node :172.31.35.135]


   ```
6. Run Adhoc Commands
   ```sh
      
   Ping		:	ansible all -m ping
      
   Command	:	ansible all -m command -a "uptime" /  "date" / "who"
      
   Stat		:	ansible all -m stat -a "path=/etc/hosts"
      
   Yum		:	 ansible all -m yum -a "name=git" –b
      
   User		:	ansible all -m user –a "name=scott" -b
      
   Setup	   :	ansible all -m setup  
      

   ```

7. Invenotry Files
   ```sh
   Host Files :

         ansible all -m ping
         ansible all -m ping -i hosts
         cat /etc/ansible/ansible.cfg
            
         Create Group like
         
         [rehl]
         172.31.35.135
         [ubuntu]
         172.31.35.136
            ansible rehl -m ping -i hosts 
      

   ```

8. Config File Demo
   ```sh
      Create ansible.config file under home and add following
         [privilege_escalation]
         become=False

      ansible all -m yum -a "name=tree"


   ```

9. Modules
   ```sh

      ansible-doc -l 
      ansible-doc -l | wc

   ```

10. Playbooks
   ```sh
   HelloWorld.yml

   - name: My first play
   hosts: all
   tasks:
      - name: Ping all
      ansible.builtin.ping:

      - name: Print message
      ansible.builtin.debug:
         msg: Hello world

   ansible-playbook -i hosts HelloWorld.yml
   
   ```
