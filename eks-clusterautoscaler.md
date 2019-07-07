eksctl get nodegroup --cluster eksworkshop-eksctl -o json | jq -r '.[].StackName'

aws cloudformation describe-stacks --stack-name $STACK_NAME | jq -r '.Stacks[].Outputs[] | select(.OutputKey=="InstanceProfileARN") | .OutputValue'

ROLE_NAME=

aws cloudformation describe-stacks --stack-name $STACK_NAME | jq -r '.Stacks[].Outputs[] | select(.OutputKey=="InstanceRoleARN") | .OutputValue' | cut -f2 -d/



F > ./k8s-asg-policy.json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "autoscaling:DescribeAutoScalingGroups",
        "autoscaling:DescribeAutoScalingInstances",
        "autoscaling:SetDesiredCapacity",
        "autoscaling:TerminateInstanceInAutoScalingGroup"
      ],
      "Resource": "*"
    }
  ]
}
EoF

aws iam put-role-policy --role-name eksctl-eksworkshop-eksctl-nodegro-NodeInstanceRole-1UEDPMM8Y6K74 --policy-name ASG-Policy-For-Worker --policy-document file://./k8s-asg-policy.json


kubectl apply -f cluster_autoscaler.yml

kubectl logs -f deployment/cluster-autoscaler -n kube-system


## Deploy sample app

kubectl apply -f nginx.yaml 
