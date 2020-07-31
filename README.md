# AWS Exposable Resources

The goal of this repo is to maintain a list of all AWS resources that can be publicly exposed, and eventually, those that can be shared with untrusted accounts (that section is still in development and not included here yet).

The following concepts are applied in this list:
- Resources that could be indirectly exposed through another resource are not included. For example, CloudTrail logs can be sent to an S3 bucket that is public, but it is the S3 bucket that is misconfigured, so CloudTrail is not listed as a resource that can be made public.
- Some resources may require multiple things configured a certain way to be considered public. For example, a Secrets Manager secret that is encrypted with a KMS, would need both the Secret and KMS key to be public for access to the Secret. For the purposes of this list, I consider the Secret resource policy only.  Similarly, for Managed ElasticSearch clusters, you need both the resource policy to allow public access, and for it to have a non-VPC IP. I consider only the resource policy. For an EC2, you could create an EC2 with a public IP, but associate a restricted Security Group to it that perhaps later is opened up to allow public access. I view the creation of the EC2 with a public IP, and not the modification of the Securtiy Group to be the action of interest.


# Roadmap
I would like this repo to eventually contain the following:
- Sample CLI commands for creating both a private and public resource
- Associated CloudTrail logs for these two events so you can build and test monitoring solutions. For example, you can see sample CloudTrail events for StreamAlert [here](https://github.com/airbnb/streamalert/blob/master/rules/community/cloudwatch_events/cloudtrail_public_resources.json)
- Associated Describe calls on the resources to show what it looks like when these resources are public.  For example, you can see sample json responses in CloudMapper's test data [here](https://github.com/duo-labs/cloudmapper/blob/main/account-data/demo/us-east-1/s3-get-bucket-policy/cloudmapper_demo).

# Resources that can be made public through resource policies

## ECR Repository
Actions:
- ecr [set-repository-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ecr/set-repository-policy.html)

## Lambda
Allows invoking the function

Actions:
- lambda [add-permission](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/lambda/add-permission.html)

## Lambda layer
Actions:
- lambda [add-layer-version-permission](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/lambda/add-layer-version-permission.html)


## Serverless Application Repository

Actions:
- serverlessrepo [put-application-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/serverlessrepo/put-application-policy.html)


## Backup
[Docs](https://docs.aws.amazon.com/aws-backup/latest/devguide/creating-a-vault-access-policy.html)

Actions: 
- backup [put-backup-vault-access-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/backup/put-backup-vault-access-policy.html)


## EFS
TODO: Need to confirm this can actually be shared with other accounts. Some of the doc wording leads me to think this might only be shareable to principals within an account.

Actions:
- efs [put-file-system-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/efs/put-file-system-policy.html)

## Glacier
Actions: 
- glacier [set-vault-access-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/glacier/set-vault-access-policy.html)

## S3
S3 buckets can be public via policies and ACL. S3 objects can be public via ACL. ACLs can be set at bucket or object creation.

Actions:
- s3api [create-bucket](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3api/create-bucket.html)
- s3api [put-bucket-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3api/put-bucket-policy.html)
- s3api [put-bucket-acl](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3api/put-bucket-acl.html)
- s3api [put-object](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3api/put-object.html)
- s3api [put-object-acl](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3api/put-object-acl.html)

## IAM Role
Actions:
- iam [create-role](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iam/create-role.html)
- iam [update-assume-role-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iam/update-assume-role-policy.html)

## KMS Keys
Actions:
- kms [create-key](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/kms/create-key.html)
- kms [create-grant](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/kms/create-grant.html)
- kms [put-key-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/kms/put-key-policy.html)

## Secrets Managers
Actions:
- secretsmanager [put-resource-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/secretsmanager/put-resource-policy.html)


## CloudWatch Logs
Actions: 
- logs [put-resource-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/logs/put-resource-policy.html)
- logs [put-destination-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/logs/put-destination-policy.html)


## EventBridge
Only allows sending data into an account

Actions:
- events [put-permission](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/events/put-permission.html)


## MediaStore
[Docs](https://docs.aws.amazon.com/mediastore/latest/ug/policies-examples-cross-acccount-full.html)

Actions:
- mediastore [put-container-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/mediastore/put-container-policy.html)


## ElasticSearch
Actions: 
- es [create-elasticsearch-domain](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/es/create-elasticsearch-domain.html)
- es [update-elasticsearch-domain-config](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/es/update-elasticsearch-domain-config.html)

## Glue
Actions:
- glue [put-resource-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/glue/put-resource-policy.html)

## SNS
Actions:
- sns [create-topic](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sns/create-topic.html)
- sns [add-permission](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sns/add-permission.html)

## SQS
Actions:
- sqs [create-queue](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sqs/create-queue.html)
- sqs [add-permission](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sqs/add-permission.html)


## SES
[Docs](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization-policies.html)

Actions:
- ses [put-identity-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ses/put-identity-policy.html)

# Resource that can be made public through sharing APIs

## AMI
Actions:
- ec2 [modify-image-attribute](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/modify-image-attribute.html)

## FPGA image
Actions:
- ec2 [modify-fpga-image-attribute](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/modify-fpga-image-attribute.html)

## EBS snapshot
Actions:
- ec2 [modify-snapshot-attribute](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/modify-snapshot-attribute.html)

## RDS snapshot
Actions:
- rds [modify-db-snapshot](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/rds/modify-db-snapshot.html)

## RDS DB Cluster snapshot
Actions:
- rds [modify-db-cluster-snapshot-attribute](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/rds/modify-db-cluster-snapshot-attribute.html)

# Resources that can be made public through network access

## API Gateway
There are associated resource policies (see [here](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-control-access-policy-language-overview.html)) that may make this something that should be in multiple categories? 

Actions:
- apigateway [create-rest-api](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/apigateway/create-rest-api.html)
- apigateway [update-rest-api](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/apigateway/update-rest-api.html)
- apigateway [create-api](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/apigatewayv2/create-api.html)


## CloudFront
Actions:
- cloudfront [create-distribution](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudfront/create-distribution.html)
- cloudfront [create-distribution-with-tags](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudfront/create-distribution-with-tags.html)

## Redshift
Actions:
- redshift [create-cluster](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/redshift/create-cluster.html)
- redshift [modify-cluster](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/redshift/modify-cluster.html)


## RDS
Actions:
- rds [create-db-instance](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/rds/create-db-instance.html)
- rds [modify-db-instance](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/rds/modify-db-instance.html)

## EC2
Actions:
- ec2 [run-instances](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/run-instances.html)
- ec2 [run-scheduled-instances](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/run-scheduled-instances.html)

## Elastic IP
Actions:
- ec2 [allocate-address](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/allocate-address.html)

## ECS
Actions:
- ecs [create-service](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ecs/create-service.html)
- ecs [update-service](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ecs/update-service.html)
- ecs [create-task-set](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ecs/create-task-set.html)
- ecs [update-task-set](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ecs/update-task-set.html)

## Global Accelerator
Actions:
- globalaccelerator [create-accelerator](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/globalaccelerator/create-accelerator.html)

## ELB
Actions:
- elb [create-load-balancer](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/elb/create-load-balancer.html)
- elbv2 [create-load-balancer](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/elbv2/create-load-balancer.html)

## Lightsail
Actions:
- lightsail [allocate-static-ip](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/lightsail/allocate-static-ip.html)
- lightsail [create-distribution](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/lightsail/create-distribution.html)
- lightsail [create-relational-database](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/lightsail/create-relational-database.html)
- lightsail [update-relational-database](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/lightsail/update-relational-database.html)
- lightsail [create-load-balancer](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/lightsail/create-load-balancer.html)
- lightsail [create-instances](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/lightsail/create-instances.html)

## Neptune
Actions:
- neptune [create-db-instance](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/neptune/create-db-instance.html)

## ElasticCache
Actions:
- elasticcache [create-cache-cluster](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/elasticache/create-cache-cluster.html)

## EMR
Actions:
- emr [create-cluster](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/emr/create-cluster.html)
