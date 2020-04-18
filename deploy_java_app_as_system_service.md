

```sudo groupadd -r appmgr```

```sudo useradd -r -s /bin/false -g appmgr jvmapps```

```sudo vim /etc/systemd/system/myapp.service```

```
[Unit]
Description=Manage Java service

[Service]
WorkingDirectory=/opt/prod
# ExecStart=/usr/bin/java -Xms128m -Xmx256m -jar myapp.jar
ExecStart=/usr/bin/java -jar myapp.jar
User=jvmapps
Type=simple
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```

`sudo chown -R jvmapps:appmgr /opt/prod`

`sudo systemctl daemon-reload`

`sudo systemctl start myapp.service`

`sudo systemctl enable myapp`
