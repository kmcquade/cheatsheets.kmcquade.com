# Mac OS X

### Open up Google chrome from command line

```text
open -a "Google Chrome" report.html
```

### List members of a group

```text
#!/usr/bin/env bash
set -e

# members
#   List the members of a group on Mac
#   Usage: members mygroupname
members () { dscl . -list /Users | while read user; do printf "$user "; dsmemberutil checkmembership -U "$user" -G "$*"; done | grep "is a member" | cut -d " " -f 1; }; 

```

###  <a id="install-python-version-with-pyenv-on-mac"></a>

