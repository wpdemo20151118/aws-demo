# aws-demo-1
demo scripts &amp; artifacts for automated wp deployment

## Overview
1. Launch instance and configure baseline for Wordpress instance using ansible. Ansible instance will be fired off by CloudFormation.
2. Configuration data and resources should be stored in version control (Git) and instance-specific information will be supplied during instantiation.
3. Wordpress instance will exist in an isolated VPC.

##Ansible Server Specifications
* AMI: ami-b27067da (Latest Ansible Tower
* -ami-5fb8c835 (Amazon Linux PV)-

##Web Server Specifications
* AMI: ami-8997afe0 (CentOS 6.5 Official)
* Default packages for apache, php, mysql
* Instance Type: t1.tiny
* Availability Zone: usa-east-1b

##Ansible Server Setup
* yum update -y
* yum install epel-release -y
* yum install ansible -y

##Web Server Configuration
* yum update -y
* yum install php-mysql mysql-server -y
* service httpd start
* service mysqld start
* mysql_secure_install
* mysql> CREATE USER 'wordpress-user'@'localhost' IDENTIFIED BY 'your_strong_password';
* mysql> CREATE DATABASE `wordpress-db`;
* mysql> GRANT ALL PRIVILEGES ON `wordpress-db`.* TO "wordpress-user"@"localhost";
* mysql> FLUSH PRIVILEGES;
* mysql> exit
* wget https://wordpress.org/latest.tar.gz
* tar -xzf latest.tar.gz
* mv ./wordpress/* /var/www/html/
* rm ./wordpress
* usermod -a -G www apache
* chown -R apache /var/www
* chgrp -R www /var/www
* sudo chmod 2775 /var/www
* find /var/www -type d -exec sudo chmod 2775 {} +
* find /var/www -type f -exec sudo chmod 0664 {} +
* service httpd restart
* chkconfig httpd on
* chkconfig mysqld on
* 

## References
* Ansible http://docs.ansible.com/ansible/playbooks_best_practices.html
* Chef http://my.safaribooksonline.com/book/web-development/web-services/9781782173632/9dot-bootstrapping-and-auto-configuration/ch09s03_html
* AWS Cloud Design Patterns  http://my.safaribooksonline.com/book/software-engineering-and-development/patterns/9781782177340/firstchapter
* CloudFormation http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/GettingStarted.html | http://my.safaribooksonline.com/book/web-development/web-services/9781782173632/4dot-web-application-and-batch-processing-architecture/ch04s03_html?query=%28%28cloudformation%29%29#snippet
* CloudFormation Parameters http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/best-practices.html#creds
* Wordpress http://codex.wordpress.org/Installing_WordPress
* EC2 + VPC http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-vpc.html
