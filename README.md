# Wordpress Two Tier App Template on a Load Balance Webserver.

<img alt="Wordpress" src="https://raw.githubusercontent.com/tso-ansible/wordpress/master/wordpress.jpg" width="600px">


This is a two tier app for Wordpress app, one for webserver and the database/mysql.<br>
You need to have at two webserver vm and one database vm.<br>
The two webserver is intended for Load Balancing.<br>

On the group_vars/all,<br>

---<br>
selinux: disabled<br>
web1name: vm0<br>
web2name: vm1<br>
mysqlname: vmdb<br>      
web1ip: 10.0.10.6<br>
web2ip: 10.0.10.5<br>
mysqlip: 10.0.10.4<br>
dbname: wordpressdb<br>
dbuser: wordpress<br>
dbpassword: password<br>

Modify the ip address according to your vm.  The web1ip should be the ip of your first webserver vm.<br>  
The script is designed for two webserver only you have to modify according to your requirements.
The mysqlip should have the ip of your database vm and it's designed for one database.<br>

On your ansible-playbook/hosts,

[db]<br>
10.0.10.4

[webserver]<br>
10.0.10.5<br>
10.0.10.6<br>

It's better to play with ip when you don't have DNS server.

You can use the database vm to act as the manager to install this playbook, so you need to execute these on it,<br>

sh -c 'echo -e "[centos]\nname=CentOS $releasever - $basearch\nbaseurl=http://mirror.centos.org/centos/7/os/\$basearch/\nenabled=1\ngpgcheck=1\ngpgkey=http://mirror.centos.org/centos/7/os/\$basearch/RPM-GPG-KEY-CentOS-7" > /etc/yum.repos.d/centos.repo'<br>
wget https://archive.fedoraproject.org/pub/epel/7Server/x86_64/Packages/e/epel-release-7-11.noarch.rpm<br>
rpm -ivh epel-release-7-11.noarch.rpm<br>

Also better use ssh login only and enable sudo no passwd (otherwise you have to use root login on ansible),<br>

sshpass ssh-copy-id -o StrictHostKeyChecking=no 10.0.10.4<br>
sshpass ssh-copy-id -o StrictHostKeyChecking=no 10.0.10.5<br>
sshpass ssh-copy-id -o StrictHostKeyChecking=no 10.0.10.6<br>

The connection of the wordpress app and the database is already done, you may check the wordpress/template/wp-config.j2.<br>
But you still have to create the Title and Login for the Wordpress App on the first connection.<br>

This is intended to be use with the Terraform Loab Balance template.<br>

http://gitlab-scm.52.235.39.185.nip.io/tso-mto-gitlab/terraform-lb.git
