---
description: Useful scripts and commands during image baking / server building
---

# Server Cleanup

## Logging Related

### Filter out useless messages from logs

```text
cat <<EOF > /etc/rsyslog.d/01_filters.conf

if \$programname == 'systemd' and \$msg contains "Started Session" then stop
if \$programname == 'systemd' and \$msg contains "Starting Session" then stop
if \$programname == 'systemd' and \$msg contains "Created slice" then stop
if \$programname == 'systemd' and \$msg contains "Starting user-" then stop
if \$programname == 'systemd' and \$msg contains "Stopping user-" then stop
if \$programname == 'systemd' and \$msg contains "Removed slice" then stop
EOF
```

### Clean old kernels, but keep the most recent 2

Clean out all old kernels except most recent 2, that way you can quickly revert to a stable one if needed

```bash
sudo package-cleanup --oldkernels --count=2
```

### 

