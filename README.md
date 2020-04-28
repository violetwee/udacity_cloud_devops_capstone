## Project Overview

This is the capstone project for Udacity's Cloud DevOps Nanodegree program.

In this project, we build a pipeline that starts from a Git commit, which triggers the Jenkins pipeline below to run. 

The Jenkins pipeline that looks like this:
1. Lint HTML
2. Build Docker images for Blue and Green deployments
3. Push Docker images to Docker Hub (Docker Hub account required)
4. Set cluster as current context (Amazon EKS cluster required)
5. Deploy blue container 
6. Deploy green container
7. Create service in the cluster, redirect to Blue
8. Prompt for traffic redirection (Proceed/Abort)
9. If Proceed, we create service in the cluster and redirect to Green

## Setup the Environment

1. Install Jenkins on an EC2 server
sudo apt update
sudo apt upgrade
sudo apt install default-jdk
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

Fix for: The following signatures couldn't be verified because the public key is not available
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys <PUBKEY>
sudo apt-get update

sudo apt install jenkins
sudo systemctl status jenkins

2. Install Jenkins Plugins
- Blue Ocean
- Pipeline AWS
- Docker
Then, add credentials for AWS and Docker Hub.

3. Install and configure AWS CLI
4. Install AWS IAM Authenticator
5. Install eksctl
6. Install linter
    sudo apt install tidy
7. Install and configure kubectl
8. Install Docker

### Create an Amazon EKS Cluster
eksctl create cluster --name capstone --without-nodegroup
Note: --name param does not support underscore (ie. capstone_cluster is not allowed)

eksctl create nodegroup --cluster capstone --name capstone-nodes --node-type t3.small --node-ami auto --nodes 3 --nodes-min 1 --nodes-max 3

### Things to note
1. ssh to server logs in as ubuntu user. However, Jenkins pipeline runs commands as jenkins user. Make sure packages are installed for jenkins user. Otherwise, may encounter "aws" not found or "kubectl" config as empty. To switch to jenkins user, run "sudo -i -u jenkins"
2. Installation of plugins on Jenkins may fail with timeout error. This could be due to some mirrors malfunctioning. Try again after a while. 
3. Git repo root needs to have a Jenkinsfile in order for Jenkins pipeline to run
4. 

### References
https://bogotobogo.com/DevOps/AWS/aws-EKS-Elastic-Container-Service-Kubernetes.php
https://medium.com/@andresaaap/jenkins-pipeline-for-blue-green-deployment-using-aws-eks-kubernetes-docker-7e5d6a401021
https://medium.com/@andresaaap/jenkins-pipeline-for-blue-green-deployment-using-aws-eks-kubernetes-docker-7e5d6a401021
https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html
https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04

