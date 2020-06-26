---
description: Various HashiCorp tooling
---

# HashiCorp

## Terraform <a id="terraform"></a>

#### Terraform Output: Conditional creation of resources in a module

```text
output "ecs_service_name" {
  value = "${element(concat(aws_ecs_service.app.*.name, aws_ecs_service.app_no_load_balancer.*.name, list("")), 0)}"
  #value = "${aws_ecs_service.app.name}"
}
```

Or:

```text
output "ssh_security_group_name" {
  description = "The name of the security group managing SSH traffic from on-premises to the VPC"
  value = "${element(concat(aws_security_group.ssh.*.name, list("")), 0)}"
  //value = "${aws_security_group.ssh.name}"
}

```

## Packer <a id="packer"></a>

#### Login shells

```text
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

```text
jq 'del(."post-processors")' packer-local-virtualbox-troubleshooting.json | packer build -
```

## Consul

### Snapshots: List the most recent consul snapshot file in an S3 bucket folder

```text
aws s3 ls s3://bucket-name/consul-snapshot-uat/ --recursive | sort | tail -n 1
```

