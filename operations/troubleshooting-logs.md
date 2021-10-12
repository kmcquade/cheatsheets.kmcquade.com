# Troubleshooting: Logs

## General

### Actively follow the logs as they are being written

```bash
journalctl -f
```

### Send command to background while logging

```bash
# Start a program as a background job with an "&" on the command line.
myprogram &
# In the above example, # note that output (both stdout and stderr) will still go to the current tty, 
# it's generally a good idea to redirect to /dev/null or to a log file, like so:
myprogram > ~/program.log 2>&1 &
```

## Systemd

### List systemd

```bash
systemctl list-unit-files --type=service
```

### Service status

```bash
systemctl status service-name
```

### Service status with more lines

```bash
systemctl status service-name -n50
```

### Full log for service

```bash
journalctl -u service-name.service
```

### Full log for service for current boot

```bash
journalctl -u service-name.service -b
```

### List services in failed state

```bash
systemctl list-units --state=failed
```
