## Create Network 
### first add iptables flase and bridge none into `/etc/docker/daemon.json`
```
{
	"iptables": false,
	"bridge": "none"
}
```
```
docker network create --subnet 192.168.2.0/24 --gateway 192.168.2.1 -d bridge -o com.docker.network.bridge.name=Test-net Test-net
```


## Run a container and set ip , use specific network , .... 

```
docker run -itd --rm --network Test-net --name C1 --ip 192.168.2.2 --dns 8.8.8.8 ubuntu:latest /bin/bash
```

```
iptables -t nat -I POSTROUTING -s 192.168.2.0/24 -j MASQUERADE 
```
