

```
eksctl create cluster --version=1.13 --name=eks-spot-cluster --nodes=1 --node-ami=auto --region=eu-west-1
```

STACK_NAME=$(eksctl get nodegroup --cluster eks-spot-cluster -o json | jq -r '.[].StackName')
INSTANCE_PROFILE_ARN=$(aws cloudformation describe-stacks --stack-name $STACK_NAME | jq -r '.Stacks[].Outputs[] | select(.OutputKey=="InstanceProfileARN") | .OutputValue')
ROLE_NAME=$(aws cloudformation describe-stacks --stack-name $STACK_NAME | jq -r '.Stacks[].Outputs[] | select(.OutputKey=="InstanceRoleARN") | .OutputValue' | cut -f2 -d/)
