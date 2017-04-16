# Some NGINX examples
### Force SSL to nonstandard port
http requests at port 80 will redirect to port 443 and serve web application running at port 3000.
```
ssl_certificate /path/to/mywebsite.crt;
ssl_certificate_key /path/to/mywebsite.key;

server {
  server_name my.website.com;
  return 301 https://$server_name$request_uri;
}

server {
  listen 443 ssl;
  server_name my.website.com;
  location / {
    if ($http_x_forwarded_proto != 'https') {
      rewrite ^ https://$server_name$request_uri? permanent;
    }
    proxy_pass http://127.0.0.1:3000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header X-Forwarded-For $remote_addr;
  }
}
```
