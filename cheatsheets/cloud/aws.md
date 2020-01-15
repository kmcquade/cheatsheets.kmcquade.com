# AWS

## Assessments

### Cloudmapper

```text
git clone https://github.com/duo-labs/cloudmapper.git
brew install autoconf automake libtool jq awscli python3 pipenv
cd cloudmapper/
pipenv install --skip-lock
pipenv shell
# Adjust your config.json
python cloudmapper.py collect --config config.json --account spaceteam
python cloudmapper.py weboftrust --account spaceteam 
python cloudmapper.py iam_report --accounts spaceteam
```

### Prowler

```text
git clone https://github.com/toniblyx/prowler
cd prowler
./prowler -sn | aha > prowler-report.htm
```

### PMapper

```text
https://github.com/nccgroup/PMapper.git
cd PMapper
pip install -r requirements.txt

# Use my automated PMapper script
# Create graph only
aws_privilege_audit.py graph
# Create graph and visualize
aws_privilege_audit.py start
# Identify privilege escalation opportunities
aws_privilege_audit.py escalate

# Create pretty graph
brew install librsvg
rsvg-convert -h 4096 pmapper-viz-acct-12345678910.svg > pmapper-viz-this-acct.png 
```

## Cartography

#### Internet accessible ALBs

```text
match (lb:LoadBalancer{scheme:"internet-facing"})-[:MEMBER_OF_EC2_SECURITY_GROUP]->(sb:EC2SecurityGroup)<-[:MEMBER_OF_EC2_SECURITY_GROUP]-(IP:IpPermissionInbound{protocol:"tcp"})<-[:MEMBER_OF_IP_RULE]-(rule:IpRange{range:"0.0.0.0/0"}) with lb, sb,IP,rule
match(aws:AWSAccount)-[:RESOURCE]->(lb) return aws.name,lb.dnsname,collect(IP.fromport)
```

#### Public Facing EC2 Instances with Instance Profiles attached

```text
MATCH (n:EC2Instance)
WHERE  (n.iaminstanceprofile) starts with 'arn' and (n.publicdnsname) contains '.'
RETURN n.region, n.instanceid, n.iaminstanceprofile, n.publicdnsname
```

## AWS Services

### EBS: Mounting disks for instances launched with same AMI

```text
# Mount with no uuid if the instance is launched with same AMI
mount -o nouuid /dev/xvdf1 /mnt
umount
lsblk

```

### EC2: List public facing instances with Instance Profiles

```text
aws ec2 describe-instances --profile default --region us-west-1 --query 'Reservations[*].Instances[?((IamInstanceProfile!=`null` && PublicIpAddress!=`null`))].[InstanceId,PublicIpAddress,IamInstanceProfile.Arn]' --output text
```

### ECS: Get container-instance ID

```
curl -s http://localhost:51678/v1/metadata | python -mjson.tool | jq '.ContainerInstanceArn' | sed -n -e 's/^.*\(container-instance\/\)/\1/p' | sed -e 's/container-instance\///g' | sed -e 's/"//g'
```

Same thing in a Bash script:

```text
container_instance_id=`curl -s http://localhost:51678/v1/metadata | python -mjson.tool | jq '.ContainerInstanceArn' | sed -n -e 's/^.*\(container-instance\/\)/\1/p' | sed -e 's/container-instance\///g' | sed -e 's/"//g'`
echo $container_instance_id
```

### ECR: Delete Untagged Images

```
aws ecr describe-repositories --output text | awk '{print $5}' | while read line; do  aws ecr list-images --repository-name $line --filter tagStatus=UNTAGGED --query 'imageIds[*]' --output text | while read imageId; do aws ecr batch-delete-image --repository-name $line --image-ids imageDigest=$imageId; done; done
```

### STS: AssumeRole

```text
aws sts assume-role \
    --role-arn arn:aws:iam::123456789012:role/xaccounts3access \
    --role-session-name s3-access-example
```

### IAM: Check for privilege escalation

```text
git clone https://github.com/RhinoSecurityLabs/Cloud-Security-Research.git
cd Cloud-Security-Research/AWS/aws_escalate/
chmod +x aws_escalate.py
./aws_escalate.py --all-users
```

### SSM: List AWS SSM Parameter Types in use

`aws ssm describe-parameters --region us-west-2 --query 'Parameters[*].{Name: Name, Type: Type}' | jq '.[] | select(.Type=="SecureString")'`

\`\`

