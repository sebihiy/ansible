# Automate WordPress Deployments in Multiple Linux Servers - Centos 7

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

### Check memory usage on all remote hosts.
$ ansible -m command -a "free -mt" web-servers
