# Open Policy Agent

## Commands

### OPA

#### Unit Testing

```
# OPA basic unit tests
opa test policy -v
# Conftest alternative (nicer on the eyes)
conftest verify
```

#### Format

```text
opa fmt policy/*.rego -w
```

#### Create a bundle

```text
tar --exclude "*_test.rego" --exclude "*.md" -zcvf policy.tar.gz ./policy/*
```

#### Run REPL on a JSON file

```text
opa run ./test/terraform/case1/plan.json
```

### Conftest

```text
# Terraform
go get github.com/tmccombs/hcl2json
# See how it would be rendered in json
conftest parse s3_public_acl.tf -i hcl2

# Note: It doesn't do variable expansion which kinda stinks.
```

## Makefile

#### Makefile snippet for conftest testing

```text
TF_MODULE_SUBFOLDER="./terraform/$(TF_MODULE_NAME)"
PLAN_FILE = plan.tfplan
SHOW_FILE = plan.json
ROOT_DIR = $(shell pwd)
NAMESPACE=${CONFTEST_NAMESPACE}

conftest: plan conftest-test

plan:
	pushd $(TF_MODULE_SUBFOLDER) && \
	 	terraform init && \
		pwd && \
		terraform plan -out=$(PLAN_FILE) && \
		tfjson2 --plan $(PLAN_FILE) | jq . > $(SHOW_FILE) && \
		popd

conftest-test:
	conftest test $(TF_MODULE_SUBFOLDER)/$(SHOW_FILE) -p policy --namespace $(CONFTEST_NAMESPACE)

conftest-test-trace:
	conftest test --trace $(TF_MODULE_SUBFOLDER)/$(SHOW_FILE) -p policy --namespace $(CONFTEST_NAMESPACE
```

## **OPA** Code - Tips and Tricks

To be added...

