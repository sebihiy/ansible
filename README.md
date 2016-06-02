## Automate WordPress Deployments in Multiple Linux Centos 7

### 1./ How to Install and Configure ‘Ansible’ :
Ansible is an open source, powerful automation software for configuring, managing and deploying software applications on the nodes without any downtime just by using SSH.

#### My Environment Setup :
Master : 192.168.1.54

#### Remote Nodes :
Node 1 : 192.168.1.51
Node 2 : 192.168.1.52
Node 3 : 192.168.1.53

### Step 1: Installing Controlling Machine – Ansible
$ sudo yum install ansible -y
$ ansible --version

### Step 2: Preparing SSH Keys to Remote Hosts
$ ssh-keygen -t rsa -b 4096 -C "sebihiy@tyahoo.fr"

After creating SSH Key successfully, now copy the created key to all three remote server’s.

$ ssh-copy-id root@192.168.1.51
$ ssh-copy-id root@192.168.1.52
$ ssh-copy-id root@192.168.1.53

### Step 3: Creating Inventory File for Remote Hosts
$ sudo vim /etc/ansible/hosts

[web-servers]
192.168.1.51
192.168.1.52
192.168.1.53

#### check our all 3 server by just doing a ping from my ansible server
$ ansible -m ping web-server

#### check the partitions on all remote hosts
$ ansible -m command -a "df -h" web-servers

#### Check memory usage on all remote hosts.
$ ansible -m command -a "free -mt" web-servers

### 2./  Introducing Ansible Playbooks :

Playbooks are plain text files written in the YAML format, and contain a list with items with one or more key/value pairs (also known as a “hash” or a “dictionary”). Inside each Playbook you will find one or more group of hosts (each one of these groups is also called a play) where the desired tasks are to be performed.

##### example : 

$ ansible-playbook /home/sebihiy/ansible/playbooks/apache.yml
you find apache.yaml  it in : https://github.com/sebihiy/ansible/blob/master/apache.yaml

### 3./ Introducing Ansible Roles

As you start adding more and more tasks to plays, your Playbooks can become increasingly difficult to handle. For that reason, the recommended approach in those situations (actually, in all cases) is to use a directory structure that contains the directives for each group of tasks in distinct files.

This approach allows us to re-use these configuration files in separate projects further down the road. Each of these files define what is called in the Ansible ecosystem a role.

#### Step 4: Creating Ansible Roles : 

$ mkdir /home/sebihiy/ansible/playbooks
$ cd /home/sebihiy/ansible/playbooks
$ ansible-galaxy init wp-dependencies
$ ansible-galaxy init wp-install-config

Next confirms the newly created roles.

$ ls -R /home/sebihiy/ansible/playbooks

We will begin by editing the following configuration files as indicated:

1 - edit file  /home/sebihiy/ansible/playbooks/wp-dependencies/tasks/main.yml : 
$ nano /home/sebihiy/ansible/playbooks/wp-dependencies/tasks/main.yml

2 - edit file /home/sebihiy/ansible/playbooks/wp-dependencies/defaults/main.yml
$ nano /home/sebihiy/ansible/playbooks/wp-dependencies/defaults/main.yml

3- edit file /home/sebihiy/ansible/playbooks/wp-install-config/tasks/main.yml:
$ nano /home/sebihiy/ansible/playbooks/wp-install-config/tasks/main.yml

4- create file playbook.yml
$ nano  playbook.yml
5- For new database server installations where the root password is empty, such as in this case, unfortunately we need to setup the password for user root individually in every machine through mysql_secure_installation.

Finally, it’s time to run these tasks by invoking our playbook:
$ ansible-playbook playbook.yml

#####Important: 
Please note that the value for variables DB_NAME, DB_USER, and DB_PASSWORD are the same as in /home/sebihiy/ansible/playbooks/wp-dependencies/defaults/main.yml:










