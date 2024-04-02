# 注意
## 1. 此代码拉取后，先把代码中的dev-bigdata-ai替换为新EKS集群的名字
## 2. 如主账号ID不为183593234327，则需替换
# 创建自定义策略（如已存在，则忽略）
aws iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam_policy.json

# 创建角色
eksctl create iamserviceaccount \
  --cluster=dev-bigdata-ai \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name dev-bigdata-ai-AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::183593234327:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve


kubectl apply -f cert-manager.yaml
kubectl apply -f v2_4_7_full.yaml
kubectl apply -f v2_4_7_ingclass.yaml

# 验证alb可用性
kubectl apply -f test.yaml
