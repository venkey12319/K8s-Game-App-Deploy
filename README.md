# K8s-Game-App-Deploy

I deployed a game application in the Mumbai region. Below are the commands I used to execute the EKS Fargate cluster

Commands to create EKS Cluster

**command 1:** eksctl create cluster --name shivani --region ap-south-1 --fargate ( To create fargate clsuter with desired region) 

**command 2:** aws eks update-kubeconfig --name shivani --region ap-south-1 ( adding kubectl commands with cluster name & region)

**command 3:** eksctl create fargateprofile --cluster shivani --region ap-south-1 --name Venkat-alb-sample-app --namespace game-2048 ( To create a new fargate profile for nee namespace)

**command 4:** kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/examples/2048/2048_full.yaml (To deploy deployment file & serive file and the ingress at a time)

**command 5:** eksctl utils associate-iam-oidc-provider --cluster shivani --region ap-south-1 --approve (this is pre-requisites to install alb ingress controller)

**command 6:** curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json (To grant all permisiion woth policies)

**command 7:** aws iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam_policy.json ( To create aws I AM policy)

**command 8:** eksctl create iamserviceaccount --cluster=shivani --namespace=kube-system --name=aws-load-balancer-controller --role-name AmazonEKSLoadBalancerControllerRole --attach-policy-arn=arn:aws:iam::905418019946:policy/AWSLoadBalancerControllerIAMPolicy --approve --region=ap-south-1 ( To create a IAM role)

**Optional Command 13:** helm delete aws-load-balancer-controller -n kube-system ( To delete helm load balancer controller)

**command 9:** helm repo add eks https://aws.github.io/eks-charts ( To add helm to your cluster)

**command 10**: helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=shivani --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller --set region=ap-south-1 --set vpcId=vpc-0cd2f0a5f1d8daf79 (To create AWS Load Balancer controller)

**command 14:** Optional Command: helm repo update ( To update helm)

**command 11:** kubectl get deployment -n kube-system aws-load-balancer-controller (To install AWS Load Balancer controller)

**command 12**: eksctl delete cluster --name shivani --region ap-south-1 ( To delete the cluster)

**command 15** KUBE_EDITOR="vim" kubectl edit deploy/aws-load-balancer-controller -n kube-system (To dubug the issue)

**command 16** aws cloudformation delete-stack --stack-name eksctl-Venkat-EKS-2-cluster (To delte CFT stack)
