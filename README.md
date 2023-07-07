# Reason

Mastering AWS skills. 
Exercise for using AWS ECR registry with AWS EKS cluster and test rolling update.

# Prereqisites

- aws cli configured
- eksctl command installed
- kubectl command installed

# Aws cli configuration

`aws configure`

# Cluster creation

`eksctl create cluster -f cluster-config.yaml`

# Kubectl configuration update

`aws eks --region us-west-2  update-kubeconfig --name eks-hpa`

# Apply k8s deployment

`kubectl apply -f deployment.yaml`

# Apply k8s service

`kubectl apply -f service.yaml`

# Test pods

`kubectl get pods`

check if status is `RUNNING'

`kubectl describe pods` 

check image version

# Update image version

Edit and save deployment.yaml

# Apply updated manifest

`kubectl apply -f deployment.yaml`

# Observe pods behaviour

`kubectl get pods`

check what's happen with a pods

`kubectl describe pods` 

check image version

# Delete resources

Delete cluster

eksctl delete  cluster --name=eks-fargate-hpa

