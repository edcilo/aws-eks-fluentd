## Requirements

* Cluster on EKS
* eksctl installed
* kubectl configured

## Instalation

1.  Create your IAM OIDC Identity Provider for your cluster

```
eksctl utils associate-iam-oidc-provider --cluster my-eks --approve
```

2. Create the service account for cloudwatch logs

```
eksctl create iamserviceaccount --name fluentd --namespace kube-system --cluster my-eks --attach-policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy --approve --override-existing-serviceaccounts
```

3. Create a Cluster Role, and Cluster RoleBinding

```
kubectl apply -f 00_clusterRole_roleBinding.yml
```

4. Create Kubernetes ConfigMap object for the Fluentd configuration

```
kubectl apply -f 01_fluentd_configMap.yml
```

5. Script to run the Fluentd container as a DaemonSet object.

You need to chance the value of the environment variables `AWS_REGION` and `AWS_EKS_CLUSTER_NAME`

```
kubectl apply -f 02_fleuntd_daemonset.yml
```

6. See if the DaemonSet is running or not.

```
kubectl get ds -n kube-system
```

7. Deploy samle pods to push logs.
8. Go to cloudwatch > Logs > Log group
