# EKS Spot NodeGroup

## Create a group with Mixed ASG Spot instances

```
cat eks-spot-group.yaml
```

```
eksctl create nodegroup --config-file=eks-spot-group.yaml
```

```
kubectl get nodes --show-labels --selector=lifecycle=Ec2Spot
```


