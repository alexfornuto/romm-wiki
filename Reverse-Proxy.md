Here are some basic configurations for popular reverse proxies. Some installations may require modifications to config options not listed below.

## Caddy

```
http://romm.mysite.com {
  reverse_proxy romm:8080
}
```

### Caddy + TLS (HTTPS)

```
https://romm.mysite.com {
  tls mysite.com.crt mysite.com.key  # Certificate and key files

  encode zstd gzip

  header * {
    Strict-Transport-Security "max-age=31536000;"
    X-XSS-Protection "1; mode=block"
    X-Frame-Options "SAMEORIGIN"
    X-Robots-Tag "noindex, nofollow"
    -Server
    -X-Powered-By
  }

  reverse_proxy romm:8080
}
```

## NGINX

```
server {
  listen 80 default_server;
  server_name romm.mysite.com;
  client_max_body_size 0;

  location / {
    include /config/nginx/proxy.conf;
    include /config/nginx/resolver.conf;
    set $upstream_app romm;
    set $upstream_port 8080;
    set $upstream_proto http;
    proxy_pass $upstream_proto://$upstream_app:$upstream_port;
  }
}
```

### NGINX + TLS (HTTPS)

```
server {
  listen 80 default_server;
  server_name _;
  return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    server_name romm.mysite.com;
    include /config/nginx/ssl.conf;
    client_max_body_size 0;

    location / {
      include /config/nginx/proxy.conf;
      include /config/nginx/resolver.conf;
      set $upstream_app romm;
      set $upstream_port 8080;
      set $upstream_proto http;
      proxy_pass $upstream_proto://$upstream_app:$upstream_port;

      # Hide version
      server_tokens off;

      # Security headers
      add_header X-Frame-Options "SAMEORIGIN" always;
      add_header X-Content-Type-Options "nosniff" always;
      add_header X-XSS-Protection "1; mode=block" always;
      add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
      add_header Referrer-Policy "no-referrer-when-downgrade" always;
    }
}
```