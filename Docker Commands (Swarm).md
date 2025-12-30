Docker Basic commands 

| Command                                                   | Purpose                                                                | Example                                     |
| --------------------------------------------------------- | ---------------------------------------------------------------------- | ------------------------------------------- |
| docker --version                                          | Show Docker client version.                                            | `docker --version`                          |
| docker info                                               | Display system-wide Docker info (daemon, containers, storage).         | `docker info`                               |
| docker images                                             | List local images.                                                     | `docker images`                             |
| docker pull <image>                                       | Download image from registry.                                          | `docker pull nginx:latest`                  |
| docker rmi <image>                                        | Remove one or more images.                                             | `docker rmi myimage:tag`                    |
| docker ps                                                 | List running containers.                                               | `docker ps`                                 |
| docker ps -a                                              | List all containers (running + stopped).                               | `docker ps -a`                              |
| docker run <image>                                        | Run a container from an image (interactive/default).                   | `docker run nginx`                          |
| docker run -d -p <host>:<container> --name <name> <image> | Run detached, map ports, and name the container.                       | `docker run -d -p 8080:80 --name web nginx` |
| docker build -t <name:tag> <path>                         | Build an image from a Dockerfile.                                      | `docker build -t myapp:1.0 .`               |
| docker exec -it <container> <cmd>                         | Run a command inside a running container (interactive).                | `docker exec -it myapp /bin/bash`           |
| docker logs -f <container>                                | Stream container logs (follow).                                        | `docker logs -f web`                        |
| docker stop <container>                                   | Stop a running container gracefully.                                   | `docker stop web`                           |
| docker rm <container>                                     | Remove a stopped container.                                            | `docker rm web`                             |
| docker-compose up -d                                      | Start services defined in `docker-compose.yml` (detached).             | `docker-compose up -d`                      |
| docker compose up -d                                      | Newer CLI (Compose v2) — start services detached.                      | `docker compose up -d`                      |
| docker network ls                                         | List Docker networks.                                                  | `docker network ls`                         |
| docker system prune -a                                    | Clean up unused data (images, containers, networks). Use with caution. | `docker system prune -a`                    |


### 📦  1. Basic Management & Image Operations

These commands handle the Docker daemon's status and the lifecycle of images.

|Command|Purpose|Example|
|:--|:--|:--|
|`docker --version`|Show Docker client version.|`docker --version`|
|`docker info`|Display system-wide info (daemon, containers, storage).|`docker info`|
|`docker images`|List all local images.|`docker images`|
|`docker pull`|Download an image from a registry.|`docker pull nginx:latest`|
|`docker build -t`|Build an image from a **Dockerfile**.|`docker build -t myapp:1.0 .`|
|`docker rmi`|Remove one or more local images.|`docker rmi myimage:tag`|
|**Import Image**|Create an image from a tarball.|`tar -C focal -c . \|

---

### 🚀  2. Container Lifecycle & Resource Control

Commands for running, managing, and restricting container instances.

|Operation|Command / Pattern|Key Details|
|:--|:--|:--|
|**Start (Foreground)**|`docker run <image>`|Runs a container from an image.|
|**Start (Detached)**|`docker run -d -p 80:80 --name web <img>`|Runs in background, maps ports, and assigns a name.|
|**Start (Interactive)**|`docker run -it ubuntu:latest`|Launches an interactive terminal session.|
|**Resource Limits**|`--cpus=0.5` / `--memory=500m`|Limits container to 50% CPU or 500MB RAM (Conversation History).|
|**Execute Command**|`docker exec -it <id> /bin/bash`|Runs a command inside a running container.|
|**Monitoring**|`docker ps -a` / `docker logs -f`|`ps -a` lists all containers; `logs -f` streams output.|
|**Stop & Remove**|`docker stop` / `docker rm`|`stop` shuts down gracefully; `rm` deletes the container.|

---

### 🌐  3. Networking Comparison

Docker networking enables isolated or shared communication between containers.

|Feature|Bridge Network (Default)|Overlay Network (Swarm)|
|:--|:--|:--|
|**Scope**|Single host.|Multi-host/Swarm cluster.|
|**Creation**|`docker network create mynetwork`.|`docker network create --driver=overlay my_ov`.|
|**Communication**|Containers on the same bridge can talk internally.|Connects services across different physical nodes.|
|**Management**|`docker network ls` / `inspect bridge`.|`docker stack deploy` uses these via compose files.|
|**Example Use**|Local development and isolated web/db pairs.|Production microservices across a cluster.|

---

### 💾  4. Volume Management & Persistence

Volumes ensure data survives container deletion and provides backup capabilities.

|Action|Command / Usage|
|:--|:--|
|**Management**|`docker volume create mydata` / `docker volume ls` / `inspect`.|
|**Standard Usage**|`docker run -v mydata:/var/opt/project bash:latest`.|
|**Backup**|`docker run --rm -v my_volume:/data -v ~/backups:/backup busybox tar cvf /backup/backup.tar /data`.|
|**Restore**|`docker run --rm -v my_volume:/data -v ~/backups:/backup busybox sh -c "cd /data && tar xvf /backup/backup.tar --strip 1"`.|
|**Cleanup**|`docker system prune -a --volumes` (⚠️ **Danger**: Deletes all unused images and volumes).|

---

### 📄  5. Dockerfile & Best Practice Comparison

Optimized patterns for building secure and efficient images.

|Category|Instruction / Pattern|Professional Detail|
|:--|:--|:--|
|**Base Image**|`FROM node:alpine`|Use lightweight (Alpine) images to reduce size and attack surface.|
|**Security**|`USER myuser`|Run as a non-root user to prevent container-breakout vulnerabilities.|
|**Startup**|`CMD ["npm", "start"]`|Use JSON format for better signal handling (Conversation History).|
|**Work Context**|`WORKDIR /app`|Set an explicit working directory for subsequent instructions.|
|**Optimization**|`RUN apt update && apt install...`|Combine `RUN` commands to minimize image layers (Conversation History).|

---

### 🐝  6. Docker Swarm Orchestration

Commands for clustering multiple engines and managing services.

|Role / Feature|Commands|Description|
|:--|:--|:--|
|**Initialize**|`docker swarm init --advertise-addr`|Sets up the current node as a manager.|
|**Node Status**|`docker node ls`|Lists all nodes currently in the swarm.|
|**Scale Service**|`docker service scale web=5`|Adjusts the number of replicas for a service.|
|**Secrets (Create)**|`docker secret create sec1 ./sec.txt`|Securely stores sensitive data in the swarm.|
|**Secrets (Use)**|`cat /run/secrets/sec1`|Secrets are mounted at this path inside the container.|
|**Security**|`docker swarm update --cert-expiry 168h`|Rotates/updates swarm certificates for security.|

---

### 🛠️  7. Professional Maintenance & Productivity

Tools to keep the environment clean and your workflow fast.

|Category|Item|Description / Command|
|:--|:--|:--|
|**Cleanup**|`docker system prune -a`|Removes unused images, containers, and networks.|
|**Security**|`export DOCKER_CONTENT_TRUST=1`|Enforces image signing and verification.|
|**Aliases**|`alias d='sudo docker'`|Speed up CLI usage: `dps` (ps), `di` (images), `dlogs` (logs).|
|**Compose**|`docker compose up -d`|V2 CLI for starting multi-container applications in background.|
|**Certificates**|`openssl x509...`|Used to view node certificates in `/var/lib/docker/swarm/`.|

---

### 🧠  8. Comparison & Common Mistake Recall

Critical distinctions for high-level understanding and troubleshooting.

|Topic|Comparison A|Comparison B|
|:--|:--|:--|
|**Infrastructure**|**VMs**: Complete Guest OS per instance; heavy footprint (Conversation History).|**Containers**: Share one Host OS kernel; very lightweight (Conversation History).|
|**Networking**|**IP Addressing**: Hardcoding internal IPs is a common senior failure (Conversation History).|**DNS Resolution**: Use User-defined networks for container name resolution (Conversation History).|
|**Port Mapping**|`-p <HostPort>:<ContainerPort>`|Mistake: Getting the order backward leads to "Bind failures" (Conversation History).|
|**Data Safety**|`docker rm <container>`|Warning: Containers are ephemeral, but volumes persist unless explicitly deleted.|

---

**Analogy for Recall**: Think of Docker like a **Standardized Shipping Port**. The **Image** is the blueprint for a crate; the **Registry** is the library of blueprints; the **Container** is the physical crate moved by the crane (**Docker Engine**); and the **Volume** is a permanent warehouse on the dock where contents are stored so they aren't lost when the crate is recycled.


## 9. Network
sudo docker network create mynetwork   
	- if mynetwork is used for containers then they can communicate with each other internally.
## Volume
```
docker volume create mydata
docker volume ls
docker volume inspect mydata
docker run -c mydata:/var/opt/project bash:latest bash -c "echo firstcontainer > /var/opt/project/first.txt"
docker run -c mydata:/var/opt/project bash:latest bash -c "ls /var/opt/project"
```

docker system prune -a --volumes  %% danger for cleanup of volumes %%

## 10. Run container 
```
sudo docker run -it ubuntu:latest     %% ineractive terminal  -it %%
echo "This is my first ubuntu container" > index.txt
ls
sudo docker rm <containerid>
docker run -d --name firstcontainer -p 80:80 httpd
curl http://localhost:80
```
Docker Content Trust
export DOCKER_CONTENT_TRUST=1
##  11. **What should my Dockerfile look like to run as a non-root user?**
```
FROM ubuntu:22.04

# Create a non-root user and group
RUN groupadd -r myuser && useradd -r -g myuser myuser

# Set the user
USER myuser

# Your app logic here
WORKDIR /app
COPY . /app
CMD ["./run.sh"]

```


## 12. Docker Swarm
```
sudo docker swarm init
sudo docker node ls
sudo docker info
sudo echo "secrets are important" > sec.txt   %% create secret %%
sudo docker secret create sec1 ./sec.txt   %% create secret in docker from root %%
sudo docker secret ls     %% lists secrets %%
sudo docker secret inspect sec1
sudo docker service create --name sec-test --secret="sec1" redis:alpine      %% create service with secret %%
sudo docker ps --filter name=sec-test
sudo docker exec -it <CONTAINER_ID> sh       %% sec-test container id %%
cat /run/secrets/sec1     %% secret is stored in /run/secret/ %%
sudo docker service ls -q                 %% list service id %%
sudo docker service rm <SERVICE-ID>
sudo docker secret rm <SECRET-ID>
```
## 13. Docker swarm security 
	Docker swarm Security need 4 ec2s
```
	sudo docker swarm init
	sudo docker swarm join-token manager      %% save the token and add that in another node %%
	sudo docker node ls                      %% test node is joined to swarm %%
	sudo docker swarm join-token worker
	sudo docker swarm join-token --rotate worker     %% to remove old worker token %%
	sudo docker swarm leave --force
	sudo openssl x509 -in /var/lib/docker/swarm/certificates/swarm-node.crt -text     %% to view certificates %%
	sudo docker info 
	sudo docker swarm update --cert-expiry 168h	        %% changes cert expiration %%
```

## 14. Node app
```
# Dockerfile   for nodejs app
	FROM node:alpine
	
	WORKDIR /app
	
	COPY package.json .
	
	RUN npm install
	
	COPY .  .
	
	CMD ["npm", "start"]
```

## Docker swarm
```
sudo docker swarm init --advertise-addr <manager-node-public-ip>
sudo docker service create --replicas 3 --name my_web_service -p 8080:80 nginx:alpine
sudo docker service ls
sudo docker service scale my_web_service=5
```

## 15. Build and manage bridge network
```
sudo docker network ls
brctl show
ip a show docker0
docker run -dt ubuntu sleep infinity
docker network inspect bridge
ping <container_ip>
docker exec -it <container_id> /bin/bash
apt-get update
apt-get install iputils-ping
ping www.google.com
docker run --name web1 -d -p 8080:80 nginx
docker ps
curl 127.0.0.1:8080
```

##  16. Overlay network
	sudo docker swarm init --advertise-addr <manager-node-public-ip>
	sudo docker network create --driver=overlay my_overlay_network
	nano docker-compose.yml
```
version: '3'

services:
  web:
    image: nginx:alpine
    networks:
      - my_overlay_network
    deploy:
      replicas: 3
    ports:
      - "8080:80"

networks:
  my_overlay_network:
    external: true
```
	sudo docker stack deploy -c docker-compose.yml my_overlay_stack
	sudo docker stack services my_overlay_stack
	public-ip:8080 in webpage

### 17. Own base image creation
```
sudo su
sudo debootstrap focal focal > /dev/null
sudo tar -C focal -c . | docker import - focal
docker run focal cat /etc/lsb-release
```

### 18. Docker service on overlay network
```
sudo su
docker swarm init
docker network create --driver overlay myoverlay0
docker service create --name testWeb -p 80:80 --network=myoverlay0 --replicas 3 httpd
docker service inspect --format='{{.ID}}' testWeb
docker service create --name testApp -p 8081:80 --replicas 3 nginx:alpine
docker service inspect --format='{{.ID}}' testApp
docker service update --network-add myoverlay0 testApp
docker service inspect --format='{{range .Endpoint.Ports}}{{.PublishedPort}}{{end}}' testApp
docker service update --network-rm myoverlay0 testApp
```
# 19. Docker Aliases
```
alias d='sudo docker'
alias dc='sudo docker-compose'
alias dps='sudo docker ps'
alias di='sudo docker images'
alias drm='sudo docker rm'
alias drmi='sudo docker rmi'
alias dlogs='sudo docker logs -f'
```

# 20. Backup Volume
```
sudo docker run --rm -v my_volume:/data -v ~/docker-backups:/backup busybox tar cvf /backup/my_volume_backup.tar /data
sudo docker run --rm -v my_volume:/data -v ~/docker-backups:/backup busybox sh -c "cd /data && tar xvf /backup/my_volume_backup.tar --strip 1"

```

