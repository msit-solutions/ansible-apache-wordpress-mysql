# ansible-apache-wordpress-mysql

Installation of the ansible on the amazon-linux

Go to aws managemet console 
create two ec2 instance of amazon-linux ,instance-type with t2.micro,edit the security groups with ports ssh-22,http-80
connect the two instance to the terminal and follow he below steps

on master amazon-linux  
==========================
 sudo yum update
 sudo amazon-linux-extras install ansible2 -y 
 sudo vi /etc/ansible/hosts [ enter in the file the node_private ip ---> ex: 172.0.30.26  [node1]
 ssh-keygen 
 ssh-copy-id amazonlinux@private_ip_of_managed node 



==============================================
on node amazon-linux 
======================
sudo passwd ec2-user         [ give the password with 8 charcters]
sudo vi /etc/ssh/sshd_config [ password authentiction as yes]
sudo systemctl restart sshd

after install check version 
ansible --version

add the private of the node to the /etc/ansible/hosts

1 creation of apache webserver and edit the page content
 before creation edit the file /tmp/index.html
=============================
---
 - name: congiuring Apache web server
   hosts: all
   tasks:
     - name: Install apache2 package
       yum:
         name: httpd
         state: present
     - name: start apache2 service
       service:
         name: httpd
         state: started
     - name: copy index file to apache2 directory
       copy:
         src: /tmp/index.html
         dest: /var/www/html/
     - name: pause for 1 min
       pause:
         minutes: 1  
     - name: restart apache2 service
       service:
         name: httpd
         state: reloaded
...
======================================
for sytnax check 
ansible-playbook apache.yaml  --syntax-check

for running of the playbook 

ansible-playbook apache.yaml -b


 Install wordpress on ansible 
after installation go to the loction /var/www/html ( see which version has been download)

=====================
---
 hosts: all
  remote_user: ec2-user
  become: true
  become_user: root
  tasks:
   - name: 1. download wordpress I
     get_url:
      url: http://wordpress.org/latest.tar.gz
      dest: /var/www/html/
   - name: 2. extract wordpress
     unarchive:
      src: /var/www/html/latest.tar.gz
      dest: /var/www/html/
      copy: no
...
======================================

for sytnax check 
ansible-playbook wordpress.yaml  --syntax-check

for running of the playbook 

ansible-playbook wordpress.yaml -b

===========================================

need to install docker on the node machine 
==============
sudo yum update
sudo yum search docker 
sudo yum info docker 
sudo yum install docker
sudo usermod -a -G docker ec2-user
 ===========
---
- name: Running MYSQL Database as Docker Container using Ansible
  hosts: testservers
  remote_user: root
  vars:
   db_volume: db_data
  tasks:
   - name: Launch mysql database container
     docker_container:
      name: db
      image: mysql:5.7
      volumes:
        - "{{ db_volume }}:/var/lib/mysql"
      restart: true
      env:
        MYSQL_ROOT PASSWORD: password
        MYSQL_DATABASE: test
        MYSQL_USER: wordpress
        MYSQL_PASSWORD: wordpress
      ports:
       - "33306:3306"
...

=========================================================
for sytnax check 
ansible-playbook mysql.yml  --syntax-check

for running of the playbook 

ansible-playbook mysql.yml -b
