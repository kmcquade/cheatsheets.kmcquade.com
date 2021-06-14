# CLI Aliases

## Miscellaneous

### Docker - Remove exited containers

```text
alias docker-rm-exited='docker rm "$(docker ps --filter status=exited -q)"'
```

### Trigger a build with a git empty commit

```text
alias git-empty-commit="git commit --allow-empty -m 'Trigger Build'."
# Open Sourcetree
alias sourcetree="open -a 'SourceTree' ."
# Open ZAP
alias zap="/Applications/OWASP\ ZAP.app/Contents/MacOS/OWASP\ ZAP.sh -cmd"
# Troubleshoot inside a docker container - supply a container name after this
alias shell-docker="docker run -it --rm -u root --entrypoint /bin/bash"

```

## Run CLI Tools

### asciicast2gif

```text
alias asciicast2gif='docker run --rm -v $PWD:/data asciinema/asciicast2gif'
```

### Azure CLI

```text
alias az-cli="docker run -it -v ~/.Azure/AzureRmContext.json:/root/.Azure/AzureRmContext.json -v ~/.Azure/TokenCache.dat:/root/.Azure/TokenCache.dat mcr.microsoft.com/azure-powershell pwsh"
```

### Burp Suite

```text
alias burp="/Applications/Burp\ Suite\ Professional.app/Contents/java/app/burpsuite_pro.jar -Djava.awt.headless=true"
```

### OWASP ZAP 

```text
alias zap="/Applications/OWASP\ ZAP.app/Contents/MacOS/OWASP\ ZAP.sh -cmd"
```

## Open Programs

### Firefox

```text
alias firefox='open -a "Firefox" '
```

### Google Chrome

```text
alias chrome='open -a "Google Chrome" '
```

### iTerm2

```text
alias iterm2='open -a iTerm .'
alias iterm='open -a iTerm .'
```

### Microsoft Excel

```text
alias excel='open -a "Microsoft Excel" '
```

### SourceTree

```text
alias sourcetree="open -a 'SourceTree' ."
```

