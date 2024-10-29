## Setup Linux VM

1) Login into AWS Cloud account
2) Create Linux VM with Ubuntu AMI (t2.medium or t3.medium)
2) Create Linux VM and connect to it using SSH Client

## Install Docker In Ubuntu VM

```
sudo apt update
curl -fsSL get.docker.com | /bin/bash
sudo usermod -aG docker ubuntu 
exit
```
## Updating system packages and installing Minikube dependencies

```
sudo apt update
sudo apt install -y curl wget apt-transport-https

```

## Installing Minikube

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube

minikube version
```

## Install Kubectl (Kubernetes Client)

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version -o yaml
```

## Start MiniKube Server

```
minikube start — driver=docker
```

## Check MiniKube status

```
minikube status
```

## Access K8S Cluster

```
kubectl cluster-info
```

## Access K8S Nodes

```
kubectl get nodes
```



