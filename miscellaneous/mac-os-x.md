# Mac OS X

### Open up Google chrome from the command line

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

### Hex Codes for GitHub gists <a id="install-python-version-with-pyenv-on-mac"></a>

* Blue: \#0F305F
* Green: \#438343
* Grey: \#6C737C
* Example: [https://gist.github.com/kmcquade/5f08120d20de12669e55ee1f565ce1b2\#file-crud-input-yml](https://gist.github.com/kmcquade/5f08120d20de12669e55ee1f565ce1b2#file-crud-input-yml)

