#!/bin/bash
#Author:  elsa.kamga@bylight.com
#Date: Nov 26 2023
#Description: Installation of jenkins on Oracle/Centos/RedHat

#Not needed but I make sure of it
sudo yum install -y vim
sudo yum install -y wget
sudo yum install -y sshpass
sudo yum install -y gnupg2
sudo yum install -y openss1
sudo yum install -y curl

# Jenkins on CentOS requires Java, but it won't work with the default (GCJ) version of Java. So, let's remove it:
sudo yum remove -y java*
sudo yum install -y java-11*
sudo update-alternatives --set java /usr/lib/jvm/java-11-openjdk-11.0.16.0.8-1.el7_9.x86_64/bin/java
java -version
curl --silent --location http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo | sudo tee /etc/yum.repos.d/jenkins.repo
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
sudo yum install -y jenkins

#Update Server after Jenkins Installation
sudo yum update -y

#After the installation process is completed, start the Jenkins service with:
sudo systemctl start jenkins
#To check whether it started successfully run:
sudo systemctl status jenkins
#Finally enable the Jenkins service to start on system boot.
sudo systemctl enable jenkins
#Adjust the Firewall
sudo firewall-cmd --permanent --zone=public --add-port=8080/tcp
sudo firewall-cmd –reload

echo "END - install jenkins"
