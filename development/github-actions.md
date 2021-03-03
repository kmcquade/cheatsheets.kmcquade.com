---
description: Tips and tricks
---

# GitHub Actions

### Use \`act\` to test GitHub actions locally without having to make new commits

{% embed url="https://github.com/nektos/act" %}

> Caveat: this will require downloading a large docker image; proceed carefully if you are on capped internet GB usage

### To trigger new builds without modifying the files

Use `git commit --allow-empty` to make a new commit without having to modify files.

```
git commit --allow-empty
# No more random whitespace changes to trigger new builds! 
```





