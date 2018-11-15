# Pod-Cop - A Utility for Directing Traffic and Controlling Docker Containers

**Pod-Cop** pairs *Traefik* with *Portainer* to provide scalable reverse-proxy routing of external traffic 
to Docker containers, and to provide a dashboard for management of said containers.

This technique was lifted largely from https://www.digitalocean.com/community/tutorials/how-to-use-traefik-as-a-reverse-proxy-for-docker-containers-on-ubuntu-16-04

## Cloning This Repository

It is suggested/assumed that **Pod-Cop** will reside on your host in a directory named `Stacks` under your home directory.  So, to clone this repo it's recommended that you do the following:

```
mkdir ~/Stacks
cd ~/Stacks
git clone https://github.com/DigitalGrinnell/pod-cop.git
```

## To launch Traefik... 

This assumes that your host has a working DNS entry as specified in `DNS_NAME=sub.domain.top`.  My example is `static.grinnell.edu` so my corresponding line reads:  

  - DNS_NAME=static.grinnell.edu
  
Other instances, like yours, will of course need to modify `sub.domain.top`.

```
cd ~/Stacks/pod-cop
DNS_NAME=sub.domain.top      # <<--- Modify this to match your DNS entry!
docker network create proxy
docker run -d  -v /var/run/docker.sock:/var/run/docker.sock  -v $PWD/traefik/traefik.toml:/traefik.toml  \
  -v $PWD/traefik/acme.json:/acme.json  -p 80:80  -p 443:443  \
  -l traefik.port=8080   --network proxy  \
  -l traefik.frontend.rule=Host:${DNS_NAME} --name traefik  traefik:1.3.6-alpine --docker
```
I can now reach the *Traefik* dashboard on my `static` server by visiting [https://static.grinnell.edu](https://static.grinnell.edu).


## To launch Portainer...

```
cd ~/Stacks/pod-cop/portainer
docker-compose up -d
```
On 'static.grinnell.edu` I can subsequently reach the *Portainer* dashboard by visiting [https://static.grinnell.edu/portainer](https://static.grinnell.edu/portainer).

Modify the `Host:` and `PathPrefixStrip:` values in following line from `docker-compose.yml` in order to specify a different address for your service:

        - "traefik.frontend.rule=Host:static.grinnell.edu;PathPrefixStrip:/portainer"

