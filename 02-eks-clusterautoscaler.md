# Install the cluster autoscaler

```
eksctl get nodegroup --cluster eksworkshop-eksctl -o json | jq -r '.[].StackName'

aws cloudformation describe-stacks --stack-name $STACK_NAME | jq -r '.Stacks[].Outputs[] | select(.OutputKey=="InstanceProfileARN") | .OutputValue'

#ROLE_NAME=

aws cloudformation describe-stacks --stack-name $STACK_NAME | jq -r '.Stacks[].Outputs[] | select(.OutputKey=="InstanceRoleARN") | .OutputValue' | cut -f2 -d/
```


## Attach role to instance group
```
aws iam put-role-policy --role-name $ROLE_NAME --policy-name ASG-Policy-For-Worker --policy-document file://./k8s-asg-policy.json
```

# Get ASG Name
```
eksctl get nodegroup --cluster=eks-spot-cluster mixed-spot-node-grp -o json  | jq -r '.[].StackName' 
```

```
kubectl apply -f cluster_autoscaler.yml
```

```
kubectl logs -f deployment/cluster-autoscaler -n kube-system
```

## Deploy sample app and scale it 

```
kubectl apply -f nginx.yaml 
```

```
kubectl scale --replicas=10 deployment/nginx-to-scaleout
```
