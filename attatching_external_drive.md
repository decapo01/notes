run this command from sshuser

```
sshfs user@ipaddress:/home/user /home/user/backup_server -o allow_other
```

where `/home/user/` on the server is the full path to the directory you want to
mount and `/home/user/backup_server` is the local path that you want to mount
to