# GitHub Advanced Searching

### By Extension

```
extension:hcl
extension:rego
extension:py
```

### With File Path

```
filename:Jenkinsfile path:/
filename:.gitlab-ci.yml path:/
packer filename:.gitlab-ci.yml path:/
```

### By Language

```
language:hcl
language:python
```

### In file

```
# matches code where "octocat" appears in the file contents.
octocat in:file 

# matches code where "octocat" appears in the file path.
octocat in:path

# matches code where "octocat" appears in the file contents or the file path.
octocat in:file,path
```
