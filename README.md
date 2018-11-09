# Pod-Cop - A Utility for Directing Traffic and Controlling Docker Containers

Pod-Cop pairs *Traefik* with *Portainer* to provide scalable reverse-proxy routing of external traffic 
to Docker containers, and to provide a dashboard for management of said containers.

This technique was lifted largely from https://www.digitalocean.com/community/tutorials/how-to-use-traefik-as-a-reverse-proxy-for-docker-containers-on-ubuntu-16-04

## To launch Traefik... 

```
cd ~/Docker/pod-cop
docker network create proxy
docker run -d  -v /var/run/docker.sock:/var/run/docker.sock  -v $PWD/traefik.toml:/traefik.toml  \
  -v $PWD/acme.json:/acme.json  -p 80:80  -p 443:443  \
  -l traefik.frontend.rule=Host:traefikx.grinnell.edu  \
  -l traefik.port=8080   --network proxy  --name traefik  traefik:1.3.6-alpine --docker
```

## To launch Portainer...

```
cd ~/Docker/pod-cop/portainer
docker-compose up -d
```

