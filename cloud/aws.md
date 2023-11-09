---
description: Commands specific to AWS Services
---

# AWS

## AWS SSO

Log in to all SSO profiles using [aws-sso-util](https://github.com/benkehoe/aws-sso-util)

```
aws-sso-util login --all
```

Use [aws-export-credentials](https://github.com/benkehoe/aws-export-credentials) to AssumeRole and save creds to \~/.aws/credentials anyway:

```
pipx install aws-export-credentials

aws-export-credentials --profile default --credentials-file-profile default
```

&#x20;

## Misc

### Get my IP Address according to Amazon for use in security groups

```
http://checkip.amazonaws.com/
```

## CloudWatch Logs Filters

### Filter for any JSON event that has the key "message"

```
# In the console
{ $.message = "*"}

# Using cw: https://github.com/lucagrulla/cw
cw tail -f --grep '{$.message="*"}' /aws/lambda/my-function
```





## CloudWatch Insights

### Sort events by EventName and IAM Principal ARN

```
stats count(*) by eventName, userIdentity.arn
```

## CodeBuild

### CodeBuild Docker Images

[https://hub.docker.com/r/amazon/aws-codebuild-local](https://hub.docker.com/r/amazon/aws-codebuild-local)

## EBS&#x20;

### Mounting disks for instances launched with same AMI

```
# Mount with no uuid if the instance is launched with same AMI
mount -o nouuid /dev/xvdf1 /mnt
umount
lsblk

```

## EC2

### Export AWS Access keys from metadata IP

```
echo "export AWS_ACCESS_KEY=$(curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/$(curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/) | jq -r .AccessKeyId)"
echo "export AWS_SECRET_ACCESS_KEY=$(curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/$(curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/) | jq -r .SecretAccessKey)"
echo "export AWS_SESSION_TOKEN=$(curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/$(curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/) | jq -r .Token)"
```

### List public facing instances with Instance Profiles

```
aws ec2 describe-instances --profile default --region us-west-1 --query 'Reservations[*].Instances[?((IamInstanceProfile!=`null` && PublicIpAddress!=`null`))].[InstanceId,PublicIpAddress,IamInstanceProfile.Arn]' --output text
```

## ECR

### Create an ECR Repository if it doesn't exist

```
aws ecr describe-repositories --repository-names ${REPO_NAME} || aws ecr create-repository --repository-name ${REPO_NAME}
```

### Delete Untagged Images

```
aws ecr describe-repositories --output text | awk '{print $5}' | while read line; do  aws ecr list-images --repository-name $line --filter tagStatus=UNTAGGED --query 'imageIds[*]' --output text | while read imageId; do aws ecr batch-delete-image --repository-name $line --image-ids imageDigest=$imageId; done; done
```

## ECS

### Get container-instance ID

```
curl -s http://localhost:51678/v1/metadata | python -mjson.tool | jq '.ContainerInstanceArn' | sed -n -e 's/^.*\(container-instance\/\)/\1/p' | sed -e 's/container-instance\///g' | sed -e 's/"//g'
```

Same thing in a Bash script:

```
container_instance_id=`curl -s http://localhost:51678/v1/metadata | python -mjson.tool | jq '.ContainerInstanceArn' | sed -n -e 's/^.*\(container-instance\/\)/\1/p' | sed -e 's/container-instance\///g' | sed -e 's/"//g'`
echo $container_instance_id
```

### ECS: Cron expressions

[https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html)

```
# Run every monday at 4am UTC (4 hours ahead of EST)
cron(0 04 ? * MON *)
```

## IAM

### Check for privilege escalation

```
git clone https://github.com/RhinoSecurityLabs/Cloud-Security-Research.git
cd Cloud-Security-Research/AWS/aws_escalate/
chmod +x aws_escalate.py
./aws_escalate.py --all-users
```

## Organizations

### Brick an AWS Account

```
aws organizations attach-policy \
  --policy-id $(aws organizations create-policy --name pwn \
     --type SERVICE_CONTROL_POLICY \
     --description "pwn" 
     --content '{"Version": "2012-10-17","Statement": [{"Effect": "Deny", "Action": "*", "Resource": "*"}]}' \
    | jq ".Policy.PolicySummary.Id"\
    ) \
  --target-id $(aws organizations list-roots | jq ".Roots | .[0].Id")
```

## SSM

### List AWS SSM Parameter Types in use

`aws ssm describe-parameters --region us-west-2 --query 'Parameters[*].{Name: Name, Type: Type}' | jq '.[] | select(.Type=="SecureString")'`

## STS

### AssumeRole

```
aws sts assume-role \
    --role-arn arn:aws:iam::123456789012:role/xaccounts3access \
    --role-session-name s3-access-example
    
```

### Get current AWS account ID

```
aws sts get-caller-identity | jq -r .Account
```
