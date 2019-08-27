# Simple config traefik on docker-compose
Traefik is a modern HTTP reverse proxy and load balancer that makes deploying microservices easy. Traefik integrates with your existing infrastructure components (Docker, Swarm mode, Kubernetes, Marathon, Consul, Etcd, Rancher, Amazon ECS, ...) and configures itself automatically and dynamically. Pointing Traefik at your orchestrator should be the only configuration step you need.

## example run config worker / service:
how to create external network
- `docker network create web`
- add labels run command \
>docker-compose
```
    labels:
      - traefik.backend=example
      - traefik.frontend.rule=Host:example.com
      - traefik.docker.network=web
      - traefik.port=80
```
>> docker run 
```
docker run -d \
--name nginx \
-l traefik.backend=example \
-l traefik.frontend.rule=Host:example.com \
-l traefik.docker.network=web \
-l traefik.port=80  nginx
```

### thanks
Refrence : [link](https://docs.traefik.io)
