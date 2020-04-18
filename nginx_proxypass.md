

create in sites available
``` servername ```

```
server {
    listen   80;
    server_name  dev.int.com;
    access_log off;
    location / {
        proxy_pass http://IP:8080;
        proxy_set_header    Host            $host;
        proxy_set_header    X-Real-IP       $remote_addr;
        proxy_set_header    X-Forwarded-for $remote_addr;
        port_in_redirect off;
        proxy_redirect   http://IP:8080/jira  /;
        proxy_connect_timeout 300;
    }

    location ~ ^/stash {
        proxy_pass http://IP:7990;
        proxy_set_header    Host            $host;
        proxy_set_header    X-Real-IP       $remote_addr;
        proxy_set_header    X-Forwarded-for $remote_addr;
        port_in_redirect off;
        proxy_redirect   http://IP:7990/  /stash;
        proxy_connect_timeout 300;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/local/nginx/html;
    }
}
```