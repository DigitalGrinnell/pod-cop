# Pod-Cop - A Utility for Directing Traffic and Controlling Docker Containers

**Pod-Cop** pairs *Traefik* with *Portainer* to provide scalable reverse-proxy routing of external traffic 
to Docker containers, and to provide a dashboard for management of said containers.

This technique was lifted largely from https://www.digitalocean.com/community/tutorials/how-to-use-traefik-as-a-reverse-proxy-for-docker-containers-on-ubuntu-16-04

## Cloning This Repository

It's assumed that **Pod-Cop** will reside on your host in a directory named `Stacks` under your home directory.  So, to clone this repo it's recommended that you do the following:

```
mkdir ~/Stacks
cd ~/Stacks
git clone https://github.com/DigitalGrinnell/pod-cop.git
```

## To launch Traefik... 

```
cd ~/Stacks/pod-cop
docker network create proxy
docker run -d  -v /var/run/docker.sock:/var/run/docker.sock  -v $PWD/traefik.toml:/traefik.toml  \
  -v $PWD/acme.json:/acme.json  -p 80:80  -p 443:443  \
  -l traefik.frontend.rule=Host:traefikx.grinnell.edu  \
  -l traefik.port=8080   --network proxy  --name traefik  traefik:1.3.6-alpine --docker
```
On DGDockerX you can reach the *Traefik* dashboard by visiting [https://traefikx.grinnell.edu](https://traefikx.grinnell.edu).


## To launch Portainer...There are two ways to create links.

```
cd ~/Stacks/pod-cop/portainer
docker-compose up -d
```
On DGDockerX you can reach the *Portainer* dashboard by visiting [https://portainerx.grinnell.edu](https://portainerx.grinnell.edu).

