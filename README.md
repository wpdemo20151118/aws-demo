# aws-demo-1
demo scripts &amp; artifacts for automated wp deployment

## Overview
1. Launch instance and configure baseline for Wordpress ec2 vm using Ansible. Ansible instance will be fired off by CloudFormation.
2. Configuration data and resources should be stored in version control (Git) and instance-specific information will be supplied during instantiation.
3. Wordpress and Ansible will exist on separate machine instances in the same new VPC.

## Open Issues
* **Q**: the password that's supposed to be entered upon use, is it for the wp box or wordpress or for ssh access to either servers or possibly for ansible's 'tower' web tool?
* **Q**: the response to my first questions read, in my mind, like "just solve the problem so we can see if you know what you're doing". I totally get that, but I'm still curious, why CF and Ansible? Is it to test the candidate on more services, or would you use CF only for baseline instance/iam/net/security and Ansible for software CM? (Or any other logical/arbitrary delineation)
* **T**: complete wp deployment playbook
* **T**: test/publish usage instructions
* **T**: bring server spec & configuration outlines below up to date
* **T**: add links to other stacks reviewed (samples mostly, ansible and wp have existing start-and-forget AMI builds ready, but I figured that would defeat the purpose of the exercise)
* **T**: review recipes for potential cleanup, optimization, configurable values

FYI: I've had various ansible and wp instances running while I was exploring, and I've been looking in to the configuration options pretty heavily - the CF template is just all I've published so far.

##Ansible Server Specifications
* AMI-5fb8c835 (Latest Amazon Linux PV)
* defaults
  * us-east-1b
  * t1.micro
* init
  * yum update -y
  * easy_install pip
  * pip install ansible
  * yum install git -y

##Web Server Specifications
* AMI: ami-8997afe0 (CentOS 6.5 Official)
* Default packages for apache, php, mysql
* Instance Type: t1.micro
* Availability Zone: usa-east-1b

##Ansible Server Setup
* yum update -y
* yum install epel-release -y
* yum install ansible -y

## References
* Ansible http://docs.ansible.com/ansible/playbooks_best_practices.html
* Chef http://my.safaribooksonline.com/book/web-development/web-services/9781782173632/9dot-bootstrapping-and-auto-configuration/ch09s03_html
* AWS Cloud Design Patterns  http://my.safaribooksonline.com/book/software-engineering-and-development/patterns/9781782177340/firstchapter
* CloudFormation http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/GettingStarted.html | http://my.safaribooksonline.com/book/web-development/web-services/9781782173632/4dot-web-application-and-batch-processing-architecture/ch04s03_html?query=%28%28cloudformation%29%29#snippet
* CloudFormation Parameters http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/best-practices.html#creds
* Wordpress http://codex.wordpress.org/Installing_WordPress
* EC2 + VPC http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-vpc.html
