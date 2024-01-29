Flux CD work:
=============
Flux CD is an open-source continuous delivery and GitOps tool designed to simplify and automate the deployment and lifecycle management of applications and infrastructure on Kubernetes. With Flux CD, developers, and operators can declaratively define the desired state of their applications and configurations as code stored in a Git repository.
Flux CD continuously monitors the repository for changes and automatically applies updates to the Kubernetes cluster, ensuring that the actual state matches the desired state. By adopting the GitOps approach, Flux CD enables teams to achieve a reliable and auditable deployment process while promoting collaboration and traceability across different environments. With its flexible architecture and robust feature set, Flux CD has gained popularity as a powerful tool for implementing GitOps workflows and achieving seamless application delivery in Kubernetes environments.

Features and Capabilities:
-------------------------
* Automated Deployments
* GitOps Workflow
* Progressive Delivery
* Secure by Design
* Compatible with all common tools
  
FluxCD: GitOps Toolkit Components:
---------------------------------
* Source controller:
* Kustomize Controller:
* Helm Controller
* Notification Controller
* mage Reflector and Automation Controller

FLUX CD WITH GITOPS TOOKIT – KUBERNETES DEPLOYMENNT AND SYNC MECHANISM :
======================================================================

Install AWS CLI Configuration:
------------------------------
The AWS CLI is used to configure your credentials, allowing you to interact with your Amazon EKS cluster. When you run commands like eksctl or kubectl to manage or interact with resources in your EKS cluster, the AWS CLI provides the necessary authentication and authorization.

* sudo apt install curl unzip

First, download the zip file using the curl command as shown:

* curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o                                                               
  "awscliv2.zip"

Now, use the unzip command to extract files:

* unzip awscliv2.zip

And finally, use the following command to install the AWS CLI:

* sudo ./aws/install

Once done, you can check the installed version using the following:
* aws --version
Connfigure AWS cli:

* aws configure

* Aceess key

* Secret key

Set up an Amazon EKS cluster :
------------------------------
with Amazon EKS – eksctl /

with Amazon EKS – AWS Management Console and AWS CLI

Iam role with required permissions

Amazon EKS Cluster:
-------------------

eksctl create cluster \
  --name flux-cd-cluster \
  --version 1.28 \
  --region ap-south-1 \
  --nodegroup-name flux-node-group \
  --node-type t2.medium \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 2 
 
Install Kubectl:
---------------
kubectl is used to interact with your Kubernetes cluster, including the EKS cluster. It also relies on the AWS CLI credentials for authentication when connecting to the EKS cluster

Download the client binaries for Linux and AMD64
 * wget https://dl.k8s.io/release/v1.28.0/bin/linux/amd64/kubectl

Make it executable
* chmod +x kubectl

Move it to a directory in your PATH
* sudo mv kubectl /usr/local/bin/

kubectl version --client

Install eksctl:
--------------

eksctl is a command-line tool for creating and managing Amazon EKS clusters. It uses the AWS CLI credentials to interact with the AWS APIs and provision the EKS cluster.

Setting up Flux:
----------------
Install Flux:
-------------
Install the Flux CLI on Ubuntu:
-------------------------------
* curl -s https://fluxcd.io/install.sh | sudo FLUX_VERSION=2.2.0 bash
* sudo chmod +x /usr/local/bin/flux

Create a Git Repository:
-------------------------
create a new repository with neccessary permissions.

Configure Flux with the Git Repository:
--------------------------------------
Use the following flux bootstrap command to configure Flux with your Git repository
flux bootstrap github \
  --owner=<user-name> \
  --repository=<repo-name>\
  --branch=<branch>\
  --path=<directory>\
  --namespace=<namespace-name>\
  --token=<github-token>\

Deploying an application:
-------------------------

flux create source git podinfo \
  --url=https://github.com/Meghana7777777/flux-cd-repo.git \
  --branch=deploy \
  --namespace=flux \
  --interval=30s \
  --export > /home/ubuntu/flux-cd-repo/clustersflux-manifests/app-podinfo/podinfo-source.yaml

Deploy the podinfo application:
-------------------------------

flux create kustomization podinfo \
--source=podinfo  \
--path="flux-cd-repo/clustersflux-manifests/app-podinfo" \
--prune=true \
--validation=client \
--namespace=flux \
--interval=5m \
--export > /home/ubuntu/flux-cd-repo/clustersflux-manifests/app-podinfo/podinfo-kustomization.yaml
