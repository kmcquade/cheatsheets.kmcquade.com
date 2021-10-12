# Parsing and Searching



**Find: List all files with the extension .txt**

```
find . -type f -name "*.txt"
```

**Grep: List all files within the current directory containing the string "foo"**

```
grep -rnw ./* -e "foo"
```

#### Sed: Delete lines matching a pattern

```
sed -i "s/text_to_delete//g" /path/to/file
```
