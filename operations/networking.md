# Networking

### Really useful curl command

```bash
curl -IkvL
```

### Check open ports

```bash
netstat -tulpn | grep LISTEN
lsof -i -P -n | grep LISTEN
```

### Show the whole routing table for the computer

```bash
netstat -r
```

### Tcpdump

```bash
sudo tcpdump -i eth0 -w client-dump.cap
```

### Get public IP address

```bash
ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
```
