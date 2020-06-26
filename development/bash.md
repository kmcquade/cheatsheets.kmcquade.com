# Bash



## Bash stuff <a id="bash-stuff"></a>

### For loop to execute scripts in a path:

```text
#!/bin/bash

readonly SCRIPTS_PATH='/tmp/scripts/hardening/'
chmod -R a+x ${SCRIPTS_PATH}
for file in ${SCRIPTS_PATH}/*.sh; do
    [ -f "$file" ] && [ -x "$file" ] && "$file";
done

```

### Wait for an HTTP endpoint to return 200 OK with Bash and curl

```text
bash -c 'while [[ "$(curl -s -o /dev/null -w ''%{http_code}'' localhost:9000)" != "200" ]]; do sleep 5; done'
```

### View history without numbers:

```text
history | cut -d' ' -f 6-
```

### **Bash history of all users**

```bash
getent passwd | 
cut -d : -f 6 | 
sed 's:$:/.bash_history:' | 
xargs -d '\n' grep -s -H -e "$pattern"
```

#### Sed automation script

[https://stackoverflow.com/questions/11245144/replace-whole-line-containing-a-string-using-sed/11245372](https://stackoverflow.com/questions/11245144/replace-whole-line-containing-a-string-using-sed/11245372)

The accepted answer did not work for me for several reasons:

my version of sed does not like -i with a zero length extension the syntax of the c\ command is weird and I couldn’t get it to work I didn’t realize some of my issues are coming from unescaped slashes So here is the solution I came up with which I think should work for most cases:

```bash
function escape_slashes {
    sed 's/\//\\\//g' 
}

function change_line {
    local OLD_LINE_PATTERN=$1; shift
    local NEW_LINE=$1; shift
    local FILE=$1

    local NEW=$(echo "${NEW_LINE}" | escape_slashes)
    sed -i .bak '/'"${OLD_LINE_PATTERN}"'/s/.*/'"${NEW}"'/' "${FILE}"
    mv "${FILE}.bak" /tmp/
}
```

So the sample usage to fix the problem posed:

```bash
change_line "TEXT_TO_BE_REPLACED" "This line is removed by the admin." yourFile
```

### grep check

```bash
#!/bin/bash

####
# https://github.com/immanuelpotter/harden/blob/master/grep_check.sh
# Written by Immanual Potter
# Usage:   grep_check "PASS_MAX_DAYS" /etc/login.defs "PASS_MAX_DAYS ${PASS_MAX_DAYS}"


grep_check(){
  string_to_find="$1"
  file_to_check="$2"
  expected="$3"

  # Potentially add logic to create file here
  found_line="$(egrep "${string_to_find}" ${file_to_check})"
  if [[ "$found_line" == "$expected" ]] ; then
    echo "$string_to_find found in $file_to_check and is as expected" 
  else
    if [[ ${#found_line} -gt 0 ]] ; then
       sed -i.${BACKUP} '/'${string_to_find}'/d' $file_to_check
    else
      echo "${string_to_find} not found, adding" 
      echo "$expected" >> "$file_to_check"
      echo "$expected added to file $file_to_check"
     fi
  fi
}
```

