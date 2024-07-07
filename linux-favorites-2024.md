# Linux Favorites - 2024

Thought I'd start keeping a new list of Linux favorites that I use frequently.

### Kill processes using port

Kill all processes using port 4000

```bash
kill -9 $(lsof -t -i:4000)
```

### Tail Cloud-init logs in AWS

```
tail -F /var/log/cloud-init-output.log
```

### Create Python virtual environment

```
python3 -m venv ./venv && source venv/bin/activate
python3 -m pip install -r requirements.txt
```
