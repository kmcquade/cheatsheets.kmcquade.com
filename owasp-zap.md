# OWASP ZAP

## Config

### CLI Aliases

* The Desktop app:

```
alias zap="/Applications/OWASP\ ZAP.app/Contents/MacOS/OWASP\ ZAP.sh -cmd"

```

* The Docker container:

```
alias zap-docker="docker run -v $(pwd):/zap/wrk/:rw -v /tmp/zap:/tmp/zap/:rw -t owasp/zap2docker-weekly"
# Use it like this: 
# zap-docker zap-baseline.py -h
# zap-docker zap-full-scan.py -h
# zap-docker zap.sh -cmd -h
```

## Generate a config file

This will generate a file titled `zap-config.conf`. Unfortunately, you do have to supply a dummy target just to create the config file (example.com is sufficient), and the output is accompanied by a bunch of nonsense. But it will create a config file that you can use.

```
zap-docker zap-full-scan.py -t https://example.com -g zap-config.conf
```



## ZAP Docker

* Get interactive shell on ZAP Docker container while mounting local directory and /tmp/zap directory

```
docker run -it -v $(pwd):/zap/wrk/:rw -v /tmp/zap/:/tmp/zap/:rw --rm owasp/zap2docker-weekly /bin/sh
```

## Commands

### Quick scan

```
zap -cmd -quickurl http://testphp.vulnweb.com -quickprogress
```

### Install plugins

```
zap -cmd -addonupdate -addoninstall pscanrulesBeta -addoninstall ascanrulesBeta -addoninstall pscanrulesAlpha -addoninstall ascanrulesAlpha
```

## Other

### zap.sh location

{% embed url="https://github.com/zaproxy/zaproxy/blob/main/zap/src/main/dist/zap.sh" %}



## References

{% embed url="https://swingletree-oss.github.io/swingletree/blog/2020/06/24/zap/" %}

