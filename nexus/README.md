# Nexus Registry Configuration

## In RedHat Based Linux first of all

> Set the `httpd_can_network_connect` SELinux boolean parameter to `1` to configure that SELinux allows NGINX to forward traffic.

```
sudo setsebool -P httpd_can_network_connect 1
```
> See [Setting up and configuring NGINX](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/deploying_different_types_of_servers/setting-up-and-configuring-nginx_deploying-different-types-of-servers)

## Nginx Config 

- Add Below Contents in `/etc/nginx/conf.d/nexus.conf`

  ```
  server {
      listen 443 ssl http2;
      listen [::]:443 ssl http2 ;
      server_name nexus.example.com;
  
      ssl_certificate /etc/nginx/ssl/nexus.example.com.pem;
      ssl_certificate_key /etc/nginx/ssl/nexus.example.com.key;
  
      access_log /var/log/nginx/nexus-access.log;
      error_log /var/log/nginx/nexus-error.log error;
  
      location / {
          proxy_pass http://localhost:8081;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection "Upgrade";
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto https;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header Host $http_host;
      }
  }
  ```
- `nginx -t ` 
- `systemctl restart nginx`
--- 

</br></br>

## For Interacting with Nexus vi HTTP 

> First You need To open HTTP Port in Nexus UI with creating Repository.

> The Below Config file Tells Docker Daemon , Do Not Check The Security (HTTPS) of this URL.

- Add below contents in `/etc/docker/daemon.json`

  ```
  {
	  "insecure-registries" : ["nexus.example.com:8082"]
  }
  ```

- Add below contents in `/etc/default/docker` to set Docker to use previous config file


  ```
  # Docker Upstart and SysVinit configuration file
  
  #
  # THIS FILE DOES NOT APPLY TO SYSTEMD
  #
  #   Please see the documentation for "systemd drop-ins":
  #   https://docs.docker.com/engine/admin/systemd/
  #
  
  # Customize location of Docker binary (especially for development testing).
  #DOCKERD="/usr/local/bin/dockerd"

  # Use DOCKER_OPTS to modify the daemon startup options.
  #DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4"

  # If you need Docker to use an HTTP proxy, it can also be specified here.
  #export http_proxy="http://127.0.0.1:3128/"

  # This is also a handy place to tweak where Docker's temporary files go.
  #export DOCKER_TMPDIR="/mnt/bigdrive/docker-tmp"

  DOCKER_OPTS="--config-file=/etc/docker/daemon.json"
  ```

- `systemctl daemon-reload`
- `systemctl restart docker`