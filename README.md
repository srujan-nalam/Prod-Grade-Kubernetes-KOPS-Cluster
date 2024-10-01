# Prod-Grade-Kubernetes-KOPS-Cluster
Creating a Production Grade Kubernetes cluster with KOPS

Add a DNS NAME using a domain and Route 53 AWS service
Create a S3 BUCKET with your Domain Name(srujannalam.xyz) 
IAM ROLE with administrator Access AND ASSIGN IT TO EC2 
CREATE A EC2 INSTANCE AND GENERATE SSH ROLE
download Kops and Kubectl to usr/local/bin and change permissions.

# Download Kubectl and give permissions.
# edit .bashrc and add all the env variables 

export NAME=srujannalam.xyz
export KOPS_STATE_STORE=s3://srujannalam.xyz
export AWS_REGION=us-east-1
export CLUSTER_NAME=srujannalam.xyz
export EDITOR='/usr/bin/nano'

# After copying the above files to .bashrc run “ source .bashrc ”.

# Create a Cluster using Kops and generate a cluster file and save it carefully and do neccessary changes

kops create cluster --name=srujannalam.xyz \
--state=s3://srujannalam.xyz --zones=us-east-1a,us-east-1b \
--node-count=2 --control-plane-count=1 --node-size=t3.medium --control-plane-size=t3.medium \
--control-plane-zones=us-east-1a --control-plane-volume-size 10 --node-volume-size 10 \
--ssh-public-key ~/.ssh/id_ed25519.pub \
--dns-zone=srujannalam.xyz --dry-run --output yaml

# One done run below commands to create the cluster 

"""
kops create -f cluster.yml
kops update cluster --name srujannalam.xyz --yes --admin
kops validate cluster --wait 10m
kops delete -f cluster.yml  --yes
"""

# Some Smoke Testing to test Our KUBERNETES CLUSTER

kubectl get pods
kubectl get nodes
kubectl get nodes -owide
kubectl cluster-info
kubectl get ns
kubectl get pods -n kube-system
kubectl get pods -n kube-system -o wide | grep -i api
kubectl get pods -n kube-system -o wide | grep -i control
kubectl get pods -n kube-system -o wide | grep -i scheduler

# Running a test pod

kubectl run testpod1 --image nginx:latest

kubectl get pods -owide

