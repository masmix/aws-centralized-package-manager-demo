# Reason

Demo for using AWS ECR registry with AWS EKS cluster 

# Prereqisites

aws configure

eksctl create cluster -f cluster-config.yaml 

aws eks --region us-west-2  update-kubeconfig --name eks-hpa


# Deploy fargate node EKS cluster

k apply -f deplyment.yaml

kubectl get pods -l 'app=nginx' -o wide | awk {'print $1" " $3 " " $6'} | column -t

Application load balancer

https://docs.aws.amazon.com/eks/latest/userguide/alb-ingress.html


https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html

https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html

# Install OIDC provider 

Check OIDC Provider is installed

export cluster_name=eks-fargate-hpa


oidc_id=$(aws eks describe-cluster --name $cluster_name --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5)


eksctl utils associate-iam-oidc-provider --cluster $cluster_name --approve


# Install IAM policy 

curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.4.7/docs/install/iam_policy.json

aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json


# Create IAM role

eksctl create iamserviceaccount \
  --cluster=$CLUSTER_NAME \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::792300178540:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve

  # Install AWS ALB Ingress Controller for Kubernetes

helm repo add eks https://aws.github.io/eks-charts

helm repo update eks

# Instal. AWS Load Balancer controller


helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=$CLUSTER_NAME \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller 







# Delete resources

Delete cluster

eksctl delete  cluster --name=eks-fargate-hpa

