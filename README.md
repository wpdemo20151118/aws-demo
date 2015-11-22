# aws-demo-1
demo scripts &amp; artifacts for automated wp deployment

## Overview
1. Launch instance and configure baseline for Wordpress ec2 vm using Ansible. Ansible instance will be fired off by CloudFormation.
2. Configuration data and resources should be stored in version control (Git) and instance-specific information will be supplied during instantiation.
3. Wordpress and Ansible will exist on separate machine instances in the same new VPC.

##Comments
* Using public IPs assigned to instances vs nat
* Using security groups without network access lists

## Open Issues

* **Q**: ~~should I put the ansible box in a separate subnet from the wp box?~~ (done)
* **Q**: the password that's supposed to be entered upon use, is it for the wp box or wordpress or for ssh access to either servers or possibly for ansible's 'tower' web tool?
* **Q**: ~~the response to my initial emailed questions read, in my mind, like "just solve the problem so we can see if you know what you're doing". I totally get that, but I'm still curious, why CF and Ansible? Is it to test the candidate on more services, or would you use CF only for baseline instance/iam/net/security and Ansible for software CM? (Or any other logical/arbitrary delineation)~~
* **T**: add output to CF stack to display web server address and/or send notification
* **T**: complete wp deployment playbook
* **T**: test/publish usage instructions
* **T**: bring server spec & configuration outlines below up to date
* **T**: add links to other stacks reviewed (samples mostly, ansible and wp have existing start-and-forget AMI builds ready, but I figured that would defeat the purpose of the exercise)
* **T**: review recipes for potential cleanup, optimization, configurable values

## Powershell
    PS C:\WINDOWS\system32> New-CFNStack -StackName "ansible-ps-deploy-1" -TemplateURL "[S3URL]" -Parameters @(@{ParameterKey="KeyPair";ParameterValue="rean"},@{ParameterKey="Password";ParameterValue="0a2z45b67y8"})

## Instance Configuration

Role | AMI | Type | Zone | Subnet | Ports
---|---|---|---|---|---
CM (Ansible) | ami-5fb8c835 (AZL) PV 64| t1.micro | us-east-1b|10.0.1.0/24|22
Web (Wpress) | ami-8997afe0 (Centos) PV 64|t1.micro|us-east-1b|10.0.11.0/24|22, 80, 443

###Ansible Server Initialization
  * yum update -y
  * easy_install pip
  * pip install ansible
  * yum install git -y

###Web Server Initialization
    yum update -y
    yum install httpd git wget mysql-server php-mysql -y
    wget  https://raw.githubusercontent.com/wpdemo20151118/aws-demo/master/sys/iptables-web.sh
    chmod 700 iptables-web.sh
    ./iptables-web.sh
    service iptables restart
    rm iptables-web.sh -f
    chkconfig httpd on
    chkconfig mysqld on
    service httpd stop
    service httpd start
    service mysqld stop
    service mysqld start
    [secure mysql]
    service mysqld stop
    service mysqld start
    [install wp]
    

## Resources
* Ansible http://docs.ansible.com/ansible/playbooks_best_practices.html
* Chef http://my.safaribooksonline.com/book/web-development/web-services/9781782173632/9dot-bootstrapping-and-auto-configuration/ch09s03_html
* AWS Cloud Design Patterns  http://my.safaribooksonline.com/book/software-engineering-and-development/patterns/9781782177340/firstchapter
* CloudFormation http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/GettingStarted.html | http://my.safaribooksonline.com/book/web-development/web-services/9781782173632/4dot-web-application-and-batch-processing-architecture/ch04s03_html?query=%28%28cloudformation%29%29#snippet
* CloudFormation Parameters http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/best-practices.html#creds
* Wordpress http://codex.wordpress.org/Installing_WordPress
* EC2 + VPC http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario2.html |  http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-vpc.html | http://harish11g.blogspot.com/2014/01/Amazon-Virtual-Private-Cloud-VPC-best-practices-tips-for-architecture-migration.html
* VPC/Networking http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Networking.html
* Boto http://answersforaws.com/statics/presentations/Ansible-and-AWS.pdf | http://boto.readthedocs.org/en/latest/boto_config_tut.html
