Jenkins installation on AWS-EC2

> Pre-requisites:
  > Existance or create an AWS account, with root creds, create an IAM account.
    That account must have IAM policies attaches to deploy EC2 instances, autoscaling group(Adavanced)
    Create Secret key and Secret access key to log in via local computer terminal to interact or ssh into running aws resources.
    In instances console create a .pem ssh key from web ui, security group with open ports of ssh(22), http(80), and for jenkins port (8080).
    Have an ssm role for ec2 instance to login via local terminal aws cli, and install aws cli, 
    configure cli profile with aws configure, account secret key, secret access key from *.csv file that we created, 
    for region choose your own accord, and output format will be in JSON.
  
  > Launch ec2 instanc with ubuntu-22 ami, or amazon linux ami, 
    Min-15GIB EBS vol root required as o9s is of 8 gb , and jenkins needs additional space.
    Attach security groups, add profile role, configure ec2 with delte root vol on termination.
    and lauch instance.
  
  > Refer Jenkins instaltion on ec2 page offical doc.
    Either use EC2 connect from web or use ssh -i <*.pem>@<instance_public_ip> or use aws ssm start-session instance-id to login to ec2.
    
    
    update the os
    sudo apt/yum update && sudo apt/yum upgrade

     add Jenkins repo on ec2
    [ec2-user ~]$ sudo wget -O /etc/yum.repos.d/jenkins.repo \
                https://pkg.jenkins.io/rpm-stable/jenkins.repo
    
    Import a key file from Jenkins-CI to enable installation from the package:
    [ec2-user ~]$ sudo rpm --import https://pkg.jenkins.io/rpm-stable/jenkins.io-2026.key

    Install java

    [ec2-user ~]$ sudo yum install java-21-amazon-corretto -y

    Install jenins

    [ec2-user ~]$ sudo yum install jenkins -y

    Enable jenkins to start boot
    [ec2-user ~]$ sudo systemctl enable jenkins

    Start jenkins as Service

    [ec2-user ~]$ sudo systemctl start jenkins

    Check jenkins service status

    sudo systemctl status jenkins

  > Configure Jenkins
    
    Use <instance_public_ip>:8080/ to acces jenkins web ui, make sure ec2-sg has incoming traffic allowed on port 8080.

    initialAdminPassword is is in var/lib/jenkins/secretes/initialAdminPassword dir

    sudo   cat  var/lib/jenkins/secretes/initialAdminPassword

    Create aadmin/user

    Install necessary pluggins to jenkins to work properly, 

    Advanced-to login jenkins via console from *.pem key of ec2
    Under Manage Jenkins  , nodes > configure cloud --> slect Amazon ec2.

    Configure clouds:
    add ec2 credentials

    Kind: Aws creds
    Access key, Secret Acces key, Description, region, *.pem key, username: ec2-user or hostname add public key and private key in kid: ssh with username and private key

