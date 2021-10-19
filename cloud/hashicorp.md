---
description: Various HashiCorp tooling
---

# HashiCorp Terraform, Packer, Consul

## Terraform <a href="terraform" id="terraform"></a>

### Recursively remove .terraform folders

It may happen that Terraform working directory (`.terraform`) already exists but not in the best condition (eg, not initialized modules, wrong version of Terraform, etc). To solve this problem you can find and delete all `.terraform` directories in your repository using this command:

```
find . -type d -name ".terraform" -print0 | xargs -0 rm -r
```

### Using AWS SSM Parameter Store to stash terraform.tfvars variables

* Store a parameter at:
  * `/some-parameter-path/stage/1`
  * `/some-parameter-path/stage/2`

```
some_text="my_list = $(aws ssm get-parameters-by-path --output=json --with-decryption --path=/some-parameter-path/stage | jq -r '.Parameters | map(.Value)')"
echo "$some_text" > terraform.tfvars 
```

Then the contents of Terraform.tfvars will be:

```
my_list = [
  "stage1value",
  "stage2value"
]
```

#### Terraform Output: Conditional creation of resources in a module

```
output "ecs_service_name" {
  value = "${element(concat(aws_ecs_service.app.*.name, aws_ecs_service.app_no_load_balancer.*.name, list("")), 0)}"
  #value = "${aws_ecs_service.app.name}"
}
```

Or:

```
output "ssh_security_group_name" {
  description = "The name of the security group managing SSH traffic from on-premises to the VPC"
  value = "${element(concat(aws_security_group.ssh.*.name, list("")), 0)}"
  //value = "${aws_security_group.ssh.name}"
}

```

## Packer <a href="packer" id="packer"></a>

#### Login shells

```
    {
      "type": "shell",
      "execute_command": "sudo chmod +x {{ .Path }}; sudo -S -E -i env {{ .Vars }} {{ .Path }}",
      "scripts": [
        "scripts/install/install-base-aws.sh",
        "scripts/install/install-rbenv-systemwide.sh"
      ],
      "environment_vars": [
        "SSH_USERNAME={{ user `ssh_username` }}"
      ]
    },
```

#### Skipping post processors

```
jq 'del(."post-processors")' packer-local-virtualbox-troubleshooting.json | packer build -
```

## Consul

### Snapshots: List the most recent consul snapshot file in an S3 bucket folder

```
aws s3 ls s3://bucket-name/consul-snapshot-uat/ --recursive | sort | tail -n 1
```

## Vault

### Local Server

```
vault server -dev
export VAULT_ADDR='http://127.0.0.1:8200

```

* Launch new session

```
export VAULT_ADDR='http://127.0.0.1:8200'
export VAULT_TOKEN='root-token'
```

## Terraformer

```
# List supported resources
terraformer import aws list

# Initialize the directory
tfenv use 1.0.6
cat <<VERSIONS | tee versions.tf
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "3.63.0"
    }
  }
}
VERSIONS
terraform init

# Import ECR and SNS resources from us-east-1
terraformer import aws -r ecr,sns --regions us-east-1 --profile personal
# Import everything except for IAM in us-east-1
terraformer import aws --resources="*" --excludes="iam" --regions us-east-1 
```
