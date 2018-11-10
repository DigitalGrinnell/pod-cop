# Pod-Cop - A Utility for Directing Traffic and Controlling Docker Containers

**Pod-Cop** pairs *Traefik* with *Portainer* to provide scalable reverse-proxy routing of external traffic 
to Docker containers, and to provide a dashboard for management of said containers.

## This is the Digital-Ocean Branch of https://github.com/DigitalGrinnell/pod-cop

This technique was lifted largely from https://www.digitalocean.com/community/tutorials/how-to-use-traefik-as-a-reverse-proxy-for-docker-containers-on-ubuntu-16-04

## Cloning This Repository

It's assumed that **Pod-Cop** will reside on your host in a directory named `Stacks` under your home directory.  So, to clone this repo it's recommended that you do the following:

```
mkdir ~/Stacks
cd ~/Stacks
git clone https://github.com/DigitalGrinnell/pod-cop.git -b digital-ocean
```

## To launch Traefik... 

```
cd ~/Stacks/pod-cop/traefik
docker network create proxy
docker run -d  -v /var/run/docker.sock:/var/run/docker.sock  -v $PWD/traefik.toml:/traefik.toml  \
  -v $PWD/acme.json:/acme.json  -p 80:80  -p 443:443  \
  -l traefik.frontend.rule=Host:traefik.mcfate.family  \
  -l traefik.port=8080   --network proxy  --name traefik  traefik:1.3.6-alpine --docker
```
On my DO Docker droplet you can reach the *Traefik* dashboard by visiting [https://traefik.mcfate.family](https://traefik.mcfate.family).


## To launch Portainer...

```
cd ~/Stacks/pod-cop/portainer
docker-compose up -d
```
On my DO Docker droplet you can reach the *Portainer* dashboard by visiting [https://portainer.mcfate.family](https://portainer.mcfate.family).

