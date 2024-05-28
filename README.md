# minikube-installation

minikube installation on Ubuntu. CRI as docker.

**MINIKUBE ON UBUNTU 22.04 LTS**

![image](https://github.com/aicloudpost/minikube-installation/assets/166476986/fcb99c67-0c64-48bc-9b24-f8a51aacce3b)

Minikube is an open-source tool that facilitates the local deployment of Kubernetes clusters.

It is designed to simplify the process of learning, experimenting, developing, and testing applications. 

It is a lightweight single-node Kubernetes cluster that can run on a user’s local machine without needing a full-scale, production-grade Kubernetes cluster.

**Prerequisites**

•	Pre-Install Ubuntu 22.04 system

•	2 GB RAM or more

•	2 CPU or more		//otherwise the minikube installation will fail

•	20 GB free disk space

•	Sudo used with admin access

•	Reliable Internet Connectivity

•	Docker or VirtualBox or KVM

**Update Your System**

Before starting the minikube installation, you should install all available updates on your system. You can just run the following command.

$ sudo apt update

**Install Docker**

Minikube requires either docker or VirtualBox, in this post, we will be installing docker on the Ubuntu 22.04 system. 

Run the following set of commands one after the other to the docker apt repository.

$ sudo apt install ca-certificates curl gnupg wget apt-transport-https -y

$ sudo install -m 0755 -d /etc/apt/keyrings

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

$ sudo chmod a+r /etc/apt/keyrings/docker.gpg

$ echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
$ sudo apt update

**Next, install docker by running the following command.**

$ sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

Add your local user to the docker group so that your local user run docker commands without sudo.

**Add your user to the 'docker' group:**

$ sudo usermod -aG docker $USER && newgrp docker

Note: To make the above changes into the effect log out and log in.

**Check the docker running status**

$ service docker status 	OR 	$ systemctl status docker

**Download and Install Minikube Binary**

To download and install the minikube binary, run the following commands,

$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

$ sudo install minikube-linux-amd64 /usr/local/bin/minikube

**To verify the minikube version,** run

$ minikube version

![image](https://github.com/aicloudpost/minikube-installation/assets/166476986/7ebfc977-7cf9-4773-8cce-9d6526130bbb)

**Install kubectl tool**

Kubectl is a command line tool, used to interact with your Kubernetes cluster. So, to install kubectl run beneath the curl command.

$ curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl

Next, set the executable permission on it and move to /usr/local/bin

$ chmod +x kubectl

$ sudo mv kubectl /usr/local/bin/

Verify the kubectl version, run

$ kubectl version -o yaml

![image](https://github.com/aicloudpost/minikube-installation/assets/166476986/3d9d6343-88a0-464f-ae67-6c91fe0865f5)

**Start Minikube Cluster**

Now that Minikube is installed, start a Kubernetes cluster using the following command:

$ minikube start --driver=docker	#by default, it’s picking the driver whatever is installed in the server.

This command initializes a single-node Kubernetes cluster, and it might take a few minutes to download the necessary components.

Once the minikube has started, verify the status of your cluster, run

$ minikube status

![image](https://github.com/aicloudpost/minikube-installation/assets/166476986/ccfabcc7-20df-410f-8c13-d20c92b38391)

**Interact with Your Minikube Cluster**

Use kubectl to interact with your Minikube Kubernetes cluster. For example, you can check the nodes in your cluster:

$ kubectl get nodes

$ kubectl cluster-info

![image](https://github.com/aicloudpost/minikube-installation/assets/166476986/1ea1861a-1cd3-4804-870e-2c2b47fa6492)

Try to deploy a sample nginx deployment, and run the following set of commands.

$ kubectl create deployment nginx-web --image=nginx

check the deployment status,

$ kubectl get deployment,pod,svc

**Managing Minikube Add-ons**

If you want to add some additional functionality to the Kubernetes cluster like the Kubernetes dashboard, ingress controller, and more. 

You can enable these with add-ons. To view all the available add-ons, run

$ minikube addons list

To enable add-ons, run

$ minikube addons enable dashboard

$ minikube addons enable ingress

To start the Kubernetes dashboard run the below command, it will automatically launch the dashboard in the web browser as shown below:

$ minikube dashboard

**Managing Minikube Cluster**

To stop and start the minikube cluster, run beneath commands.

$ minikube stop

$ minikube start

$ minikube status

To delete the minikube cluster, run

$ minikube delete
