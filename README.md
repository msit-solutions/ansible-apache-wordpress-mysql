# Installation of the ansible on the amazon-linux

* Go to aws managemet console 
* create two ec2 instance of amazon-linux ,instance-type with t2.micro,edit the security groups with ports ssh-22,http-80
* connect the two instance to the terminal and follow he below steps

# on master machine or controller  amazon-linux  

 * sudo yum update
 * sudo amazon-linux-extras install ansible2 -y 
 *  sudo ssh-keygen
 * ssh-copy-id amazonlinux@private_ip_of_managed node 
 * sudo vi /etc/ansible/hosts [ enter in the file the node_private ip ---> ex: 172.0.30.26  [node1]


# on node amazon-linux 

* sudo passwd ec2-user         [ give the password with 8 charcters]
* sudo vi /etc/ssh/sshd_config [ password authentiction as yes]
* sudo systemctl restart sshd

* after install check version 
-->>ansible --version
* add the private of the node to the /etc/ansible/hosts

# install-docker-on-ec2-not-using-ansible.sh

* sudo yum install -y amazon-linux-extras
* sudo amazon-linux-extras enable python3.8 ansible2 docker
* sudo yum clean metadata
* sudo yum install -y python3.8 ansible docker
* sudo python3.8 -m pip install pip --upgrade
* sudo /usr/local/bin/pip3.8 install boto boto3 docker-compose
* sudo systemctl enable docker.service
* sudo systemctl start docker.service

# Create the requirment.yml and install all the requirments using playbook

# 1. creation of apache webserver and edit the page content
   
 before creation playbook the create /tmp/index.html
  on master 
  * echo "Welcomw to webserver" > /tmp/index.html ( create file and add the content )
  * ls -l /tmp/index.html ( view file has created or not)
  * cat /tmp/index.html ( view the data in index.html)


# 2.Install wordpress on ansible 
  after installation go to the loction /var/www/html ( see which version has been download)

# 3. Running MYSQL Database as Docker Container using Ansible

need to install docker on the node machine 
==================================
* sudo yum update
* sudo yum search docker 
* sudo yum info docker 
* sudo yum install docker
* sudo usermod -a -G docker ec2-user

install on master machine
======================== 
* sudo pip3 install ansible
* ansible --version | grep "python3 version"


*for sytnax check 
-->> ansible-playbook [play-book_name] --syntax-check

*for running of the playbook 
-->>ansible-playbook [play-book_name] -b

===========
