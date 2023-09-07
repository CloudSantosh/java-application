# Address Book Application using Jenkins and Github webhook on Tomcat Server

**Table of Contents:**

1. [Introduction](#Introduction)
2. [Prerequisites](#prerequisites)
3. [Setting up Jenkins](#setting-up-jenkins)
4. [Configuring GitHub Webhook](#configuring-github-webhook)
5. [Configuring Tomcat Server](#configuring-tomcat-server)
6. [Automating Deployment](#automating-deployment)
7. [Conclusion](#conclusion)

## Introduction

Deploying a Java Address Book application using Jenkins and GitHub Webhook on a Tomcat Server. In this guide, we will walk you through the process of automating the deployment of a Java-based Address Book application. By leveraging Jenkins, a popular continuous integration and continuous deployment (CI/CD) tool, along with GitHub Webhooks, we will streamline the deployment process and ensure that your application is always up-to-date.

## Prerequisites

Before we dive into the deployment process, make sure you have the following prerequisites in place:

- A Java Address Book application with a GitHub repository.
- A Jenkins server installed and configured.
- Access to a Tomcat Server where you want to deploy your application.
- Knowledge of basic Git and Jenkins concepts.

If you have these prerequisites ready, let's proceed to set up Jenkins, configure GitHub Webhook, and automate the deployment of your Java Address Book application to the Tomcat Server. This guide will help you achieve a seamless and efficient deployment process, ensuring your application is always accessible and up-to-date.

## Setting up Jenkins

In order to set up jenkins, you need to make ready with jenkins server. Here is the github link where terraform code is used to make jenkins server ready i.e. [link](https://github.com/CloudSantosh/Jenkins_tomcat_deployment)

## Script for jenkins server and Apache webserver

#### Install jenkins on jenkin-server node

```bash

#!/bin/bash
#################################
# Author: Santosh
# Date: 8th-August-2023
# version 1
# This code install jenkins in the ubuntu instances
##################################

sudo apt update -y
sudo apt install openjdk-17-jre -y
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
 /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
 https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
 /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
sudo systemctl enable jenkins
sudo systemctl start jenkins

```

(This script is on jenkins-server as data.sh and renders on EC2 instance when terraform deploy via command.)

##### Access URL

```bash
http://<public IP of Jenkins server>:8080
```

In order to access the default admin password we need to login to the jenkins server and run the command

```bash
sudo cat /var/lib/jenkins/secrets/intialAdminPassword
```

and copy & paste in the windows.

#### Create the job

steps

- Click New item
- Type name of the item
- choose freestyle project
- Click Ok
- Select github project and paste urls of repository
- Select git
- Paste xxxxxx.git from github
- Select github hook trigger from GITSCM pooling for webhook
- Select build steps
- Select send files and execute command over ssh

![App Screenshot](images/ssh-server-publisher.png)

# For CI/CD we use web-hook for automatic build trigger

steps

- Browse github repository
- Click at settings
- Click at webhook

![App Screenshot](images/webhook.png)

# Apache Webserver

The folder /var/www/html/ has a default user as root. Therefore, it is supposed to be changed into normal user and in our case, it is ubuntu.
![App Screenshot](images/website.png)

# Build is triggered.

![App Screenshot](images/final-result.png)

# Final Result

![App Screenshot](images/output.png)
