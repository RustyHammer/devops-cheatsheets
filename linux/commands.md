# Linux - Essential Commands

## Navigation and Files

```bash
pwd                     # Current directory
ls -la                  # List with details and hidden files
cd /path/to/dir         # Change directory
find / -name "*.log"    # Search for files
du -sh /var/log         # Directory size
df -h                   # Disk space
```

## Permissions

```bash
chmod 755 script.sh     # rwxr-xr-x (owner: all, group/others: read+exec)
chmod 600 secret.key    # rw------- (owner only)
chown user:group file   # Change owner
```

| Digit | Permission |
|-------|-----------|
| 7 | rwx (read + write + execute) |
| 6 | rw- (read + write) |
| 5 | r-x (read + execute) |
| 4 | r-- (read only) |
| 0 | --- (none) |

## Process Management

```bash
ps aux                  # All processes
top / htop              # Real-time monitoring
kill <PID>              # Stop a process (SIGTERM)
kill -9 <PID>           # Force stop (SIGKILL)
nohup command &         # Run in background (survives logout)
```

## Services (systemd)

```bash
systemctl start nginx       # Start
systemctl stop nginx        # Stop
systemctl restart nginx     # Restart
systemctl status nginx      # Status
systemctl enable nginx      # Start on boot
journalctl -u nginx -f      # Real-time logs
```

## User Management

```bash
useradd -m username         # Create a user with home directory
passwd username             # Set password
usermod -aG docker username # Add to a group
su - username               # Switch user
```

## Network

```bash
ip addr                 # Network interfaces
netstat -tlnp           # Listening ports
curl -v http://host     # HTTP request with details
ssh user@host           # SSH connection
scp file user@host:/path # Copy a file via SSH
```

## Package Management (yum / apt)

```bash
# Amazon Linux / CentOS (yum)
yum update -y
yum install -y package-name
yum list installed

# Ubuntu / Debian (apt)
apt update && apt upgrade -y
apt install -y package-name
apt list --installed
```
