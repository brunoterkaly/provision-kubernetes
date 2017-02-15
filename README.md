# Efficient Provisioning of a Kubernetes Cluster

## Using Azure Container Service

How to:

1. Provision a Linux VM
1. Install Microsoft Azure Cross Platform Command Line 
1. Azure SDK for Python
1. Provision a Kubernetes Cluster


## Provisioning a Linux VM In Azure

This post is about provisioning a Kubernetes cluster in the Azure container service. We will begin by provisioning a Linux VM in Azure. This Linux VM will be  set up to include the Azure cross platform client library, as well as the Python Azure SDK. once we get this VM configured, it can be used to perform the provisioning using a very abbreviated syntax.

## Provisioning the Linux VM

Login to your Windows Azure subscription and navigate to the portal.

- http://portal.azure.com



1. In the upper left corner of the portal, select "+ New." 
2. A search text box will appear into which you should type in "Ubuntu."
3. You will be asked to select from a list of Ubunutu VMs. Select version 16.10.
4. Next you will configure the virtual machine, starting with the "Basics". Accept most of the defaults and fill in the following information
	1. VM Name
	2. Authentication type = Password
	3. A strong password
	4. A resource group
4. For the VM type select "DS1_V2."
5. Accept the default settings
6. In the "summary" section select "OK" to continue
7. A few moments later a VM will be available to SSH into

## Remoting into the newly created virtual machine

Now that the VM is provisioned, we can use some tooling from our Windows or MacOS computer to remote in. You can use "Putty" but I'm using "Moba".

There is a good example of using Putty.

- https://mediatemple.net/community/products/dv/204404604/using-ssh-in-putty-

Once we have SSH'd into the VM, we can begin the process of setting up the VM to be able to provision a Kubernetes cluster.


#### Getting the IP address

You will notice that a public IP address has already been provisioned. Write this down as you will need to SSH into the VM.

Follow the directions here to be able to SSH into the Linux VM.


## Setting up the Azure/Python SDK

	apt-get update
	apt-get install npm
	npm install -g azure-cli

Let's do some file linking.

	ln -s /usr/bin/nodejs /usr/bin/node

Install the Python utility, pip

	apt install python-pip

Install the Python/Azure SDK

	pip install --pre azure
	pip install --upgrade pip

Some final commands. **You will need to hit [enter] a couple of times and answer YES.**

	curl -L https://aka.ms/InstallAzureCli | bash
	source ~/.bashrc

You are now ready to test

	az acs -h

## Logging In

Log into Azure

	az login

## Create a RESOURCE GROUP and the Kubernetes Cluster

The resource group can be thought of a container (along with all the lifecycle) of the infrastructure.

	RESOURCE_GROUP_NAME=ascend-k8s-rg
	DNS_PREFIX=ascend-k8s
	SERVICE_NAME=ascend-k8s-svc
	az group create \
	    --name $RESOURCE_GROUP_NAME \
	    --location "centralus"

Create a cluster in ACS (Orchestrator = kubernetes)

	RESOURCE_GROUP_NAME=ascend-k8s-rg
	DNS_PREFIX=ascend-k8s
	SERVICE_NAME=ascend-k8s-svc	az acs create --orchestrator-type=kubernetes        \
		--generate-ssh-key                              \
		--resource-group $RESOURCE_GROUP_NAME           \
		--name=$SERVICE_NAME --dns-prefix=$DNS_PREFIX
	
## You are finished

You can now return back to the portal and see all the infrastructure that got provisioned:

- Availability Sets
- Container Service
- Load Balancer
- NIC Cards
- Network Security Groups
- Route Tables
- Public IPs
- Storage Accounts
- Virtual Machines

## In the next lab...

What you will learn in the Kubernets session:
- How to provision a VM with the Azure / Python CLI
- How to provsion K8s Cluster using Python
- Implementing 2 pods in cluster (Web+Caching and DB)
- Technologies include Python, Flask, Redis, MySQL
- Understanding how to implement RESTful API




