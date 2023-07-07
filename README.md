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

# Apply k8s deployment - revison 1

Image version 2.0.0

`kubectl apply -f deployment.yaml`

# Apply k8s service 

`kubectl apply -f service.yaml`

# Test pods

`kubectl get pods`

check if status is `RUNNING'

`kubectl describe pods` 

check image version

# Update image version - revison 2

Edit and save deployment.yaml
Image version 2.0.1


# Apply updated manifest

`kubectl apply -f deployment.yaml`

# Observe pods behaviour

`kubectl get pods`

check what's happen with a pods

`kubectl describe pods` 

check image version

# Update image version - revison 3

Edit and save deployment.yaml
Image version 2.0.2

# Apply updated manifest

`kubectl apply -f deployment.yaml`

# Observe pods behaviour

`kubectl get pods`

check what's happen with a pods

`kubectl describe pods` 

check image version

# Check service from pod level

kubectl exec -it pod/flask-demo-cdf9564c7-4c8bq -- /bin/bash
curl 

# Check rollout history

kubectl rollout history deployment/flask-demo

kubectl rollout undo deployment/flask-demo --to-revision=4

# Delete resources

Delete cluster

eksctl delete  cluster --name=eks-fargate-hpa

