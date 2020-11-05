# Simple config traefik on docker-compose
Traefik is a modern HTTP reverse proxy and load balancer that makes deploying microservices easy. Traefik integrates with your existing infrastructure components (Docker, Swarm mode, Kubernetes, Marathon, Consul, Etcd, Rancher, Amazon ECS, ...) and configures itself automatically and dynamically. Pointing Traefik at your orchestrator should be the only configuration step you need.

## Traefik V2 config 
- in the v2 traffic config already using the cloudflare provider validation DNS when generating ssl from letsencrypt. 
- if you use a cloudflare provider, don't forget to adjust the CF token and create an .env file, for example, there is an env.example file. 
```
# How to Generate Token https://www.cf-purge.com/token-setup.html 
#
CF_ZONE_API_TOKEN=Lq4lTB-21sksdkjsakk
CF_DNS_API_TOKEN=kk7RHdBxkuh777gadgah
CF_API_EMAIL=admin@example.com
```
- If you don't want to use Cloudflare's DNS validation you can replace it with this:
```
    command:
      - "--certificatesresolvers.le.acme.httpChallenge.entryPoint=http"
      - "--certificatesresolvers.le.acme.email=${CF_API_EMAIL}"
      - "--certificatesresolvers.le.acme.storage=/letsencrypt/cloudflare.json"
```

## Example run config worker / service:
### Traefik V1
- add labels run command 
> docker-compose 
```
    labels:
      - traefik.backend=example
      - traefik.frontend.rule=Host:example.com
      - traefik.port=80
```
### Traefik V2
Before doing this you have created an .env file with an example in env.example
> docker-compose 
```
    labels:
      - "traefik.http.routers.webserver.rule=Host(`example.com`)"
      - "traefik.enable=true"
      - "traefik.http.routers.webserver.entrypoints=https"
      - "traefik.http.routers.webserver.tls.certresolver=cloudflare"
      - "traefik.http.routers.webserver.tls=true"
```
### Traefik V1
> docker run 
```
docker run -d \
--name nginx \
-l traefik.backend=example \
-l traefik.frontend.rule=Host:example.com \
-l traefik.port=80  nginx
```
### Traefik V2
> docker run 
```
docker run -d \
--name nginx \
-l "traefik.http.routers.webserver.rule=Host(`example.com`)" \
-l "traefik.http.routers.webserver.entrypoints=https" \
-l "traefik.http.routers.webserver.tls.certresolver=cloudflare" \
-l "traefik.http.routers.webserver.tls=true" nginx
```

**Thanks**, Detail config you check link below. \
Refrence : [link](https://docs.traefik.io)
