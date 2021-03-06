# IAM Roles

Two IAM roles are created for the cluster: one for the masters, and one for the nodes.
The permissions are kept to the minimum required to setup and maintain the cluster.

Master permissions:

```
ec2:*
route53:*
elasticloadbalancing:*
ecr:GetAuthorizationToken
ecr:BatchCheckLayerAvailability
ecr:GetDownloadUrlForLayer
ecr:GetRepositoryPolicy
ecr:DescribeRepositories
ecr:ListImages
ecr:BatchGetImage

// The following permissions are only created if you are using etcd volumes with "encrypted: true" and a custom kmsKeyId.
// They are scoped to the kmsKeyId that you are using.
kms:Encrypt
kms:Decrypt
kms:ReEncrypt*
kms:GenerateDataKey*
kms:DescribeKey
kms:CreateGrant
kms:ListGrants
kms:RevokeGrant
```

Node permissions:

```
ec2:Describe*
route53:*
ecr:GetAuthorizationToken
ecr:BatchCheckLayerAvailability
ecr:GetDownloadUrlForLayer
ecr:GetRepositoryPolicy
ecr:DescribeRepositories
ecr:ListImages
ecr:BatchGetImage
```

## Adding Additional Policies

Sometimes you may need to extend the kops IAM roles to add additional policies. You can do this
through the `additionalPolicies` spec field. For instance, let's say you want
to add DynamoDB and Elasticsearch permissions to your nodes.

Edit your cluster via `kops edit cluster ${CLUSTER_NAME}` and add the following to the spec:

```
  additionalPolicies:
    node: |
      [
        {
          "Effect": "Allow",
          "Action": ["dynamodb:*"],
          "Resource": ["*"]
        },
        {
          "Effect": "Allow",
          "Action": ["es:*"],
          "Resource": ["*"]
        }
      ]
```

After you're finished editing, your cluster spec should look something like this:

```
metadata:
  creationTimestamp: "2016-06-27T14:23:34Z"
  name: ${CLUSTER_NAME}
spec:
  cloudProvider: aws
  networkCIDR: 10.100.0.0/16
  networkID: vpc-a80734c1
  nonMasqueradeCIDR: 100.64.0.0/10
  zones:
  - cidr: 10.100.32.0/19
    name: eu-central-1a
  additionalPolicies:
    node: |
      [
        {
          "Effect": "Allow",
          "Action": ["dynamodb:*"],
          "Resource": ["*"]
        },
        {
          "Effect": "Allow",
          "Action": ["es:*"],
          "Resource": ["*"]
        }
      ]
```

Now you can update to have the changes take effect:

```
kops update cluster ${CLUSTER_NAME} --yes
```
