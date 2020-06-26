---
description: Commands specific to AWS Services
---

# AWS

## CloudWatch Insights

### Sort events by EventName and IAM Principal ARN

```text
stats count(*) by eventName, userIdentity.arn
```

## EBS 

### Mounting disks for instances launched with same AMI

```text
# Mount with no uuid if the instance is launched with same AMI
mount -o nouuid /dev/xvdf1 /mnt
umount
lsblk

```

## EC2

### List public facing instances with Instance Profiles

```text
aws ec2 describe-instances --profile default --region us-west-1 --query 'Reservations[*].Instances[?((IamInstanceProfile!=`null` && PublicIpAddress!=`null`))].[InstanceId,PublicIpAddress,IamInstanceProfile.Arn]' --output text
```

## ECR

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

```text
container_instance_id=`curl -s http://localhost:51678/v1/metadata | python -mjson.tool | jq '.ContainerInstanceArn' | sed -n -e 's/^.*\(container-instance\/\)/\1/p' | sed -e 's/container-instance\///g' | sed -e 's/"//g'`
echo $container_instance_id
```

### ECS: Cron expressions

[https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html)

```text
# Run every monday at 4am UTC (4 hours ahead of EST)
cron(0 04 ? * MON *)
```

## 

## IAM

### Check for privilege escalation

```text
git clone https://github.com/RhinoSecurityLabs/Cloud-Security-Research.git
cd Cloud-Security-Research/AWS/aws_escalate/
chmod +x aws_escalate.py
./aws_escalate.py --all-users
```

## SSM

### List AWS SSM Parameter Types in use

`aws ssm describe-parameters --region us-west-2 --query 'Parameters[*].{Name: Name, Type: Type}' | jq '.[] | select(.Type=="SecureString")'`

## STS

### AssumeRole

```text
aws sts assume-role \
    --role-arn arn:aws:iam::123456789012:role/xaccounts3access \
    --role-session-name s3-access-example
```

