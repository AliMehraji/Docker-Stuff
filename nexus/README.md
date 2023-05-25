# Nexus Registry Configuration

## For Interacting with http 

- add below contents in `/etc/docker/daemon.json`

  ```
  {
	  "insecure-registries" : ["nexus.example.com:8082"]
  }
  ```

- add below contents in `/etc/default/docker` to set Docker to use previous config file


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