# Docker Network Containers

If you are familiar with Docker, and understand the concept of a 'network container', you can directly dive into the mechanics and go to the [Network Containers Github](https://github.com/jeroenpeeters/docker-network-containers "Docker Network Containers") repository.

## Audience

This article is written for people who have substantial experience with Docker. Dockerfiles, images, containers... it should all sound familiar. If not, [this](https://www.docker.com/tryit/ "Try Docker") is a great pointer to get you started.

## Docker Networking

If you have been working with Docker you know that Docker maps the container's private ports to a -sort of random- unique public port on the host machine. This way you can access the container's services from anywhere on your network. If you only expose one webserver, you can as well expose port 80 directly and this will work well. But if you want to expose two (or more) webservers from one host? Only one will be able to use port 80 directly. The other webservers will be available at awkard ports unless you use some kind of reverse proxy mechanism.

### Wouldn't it be nice...

-   If a container can get its own IP on the regular network?
-   If a container could get this IP from the regular DHCP server?
-   that all of this would happen without the container being aware?

At the time of writing, these scenario's are not cover by Docker. They might very well be in the (near) future. The following paragraph details how the scenario as detailed above can be accomplished.

## Network Containers: An Agnostic Approach

The goal should be clear by now: Starting multiple services on one Docker host that all can expose the same standard ports (for instance, port 80 for http). In order to achieve this each container should have its own IP on the network assigned by DHCP.

But how to assign an IP to a container dynamically without modifying the Docker image and having to install a DHCP client, let alone connecting the container to the network? It would be great if the containers would be unaware of the networking that's going on. This way images from Docker Hub could be used directly without modification.

### What is a Network Container?

The sole purpose of a Network Container is to add 'public' network capabilities to an existing service container. This is essentially what the network container does:

1. Get an IP from DHCP
2. Route public ports (e.g. 80) to the service container

Basically the Network Container is a simpel NAT-router.

#### Example

Suppose we have two containers called 'web1' and 'web2' running on a Docker host with IP 192.168.1. Both services run a webservice on port 80. Docker would route these ports to unique ports on the host so that both services are accessible from the host IP. Probably the following table makes it all clear.

| Service | Service Port | Accessible on  |
|---------|--------------|----------------|
| web1    | 80           | 192.168.1:5601 |
| web2    | 80           | 192.168.1:5602 |

It is clear that these services cannot be reached on their standard port. If we put a Network Container in front of each service it is assigned its own IP from the network DHCP server. See the following table.

| Service | Service Port | Accessible on  | Network Container Access |
|---------|--------------|----------------|--------------------------|
| web1    | 80           | 192.168.1:5601 | 192.168.1.50:80          |
| web2    | 80           | 192.168.1:5602 | 192.168.1.51:80          |

And here we have it, the services are both available directly on the host IP and from a DHCP assigned IP. This also opens the possibility of adding DNS capabilities to the set-up.

## Network Containers: A Scripted Approach

To automatically create a Network Container for a given Docker container I've created a script tha does all of steps outlined previously. The script checks a running container for the exposed ports, and generates a simple iptables script.

The script takes two parameters, the name of a running container and the name of the network interfaces. The latter is needed to correctly acquire an IP adress from DHCP. The script is simple as following:

```bash
create-network-container.sh web1 eth1
```

The script will block until the Network Container is fully up and has acquired an IP from DHCP. The output will look like this:

```
creating iptables route for port 80
containerid=63f967c4cc1e0cd166fffc6b469cc03190e9fd3b2ea86d290304b227459f5202
waiting for public ip to be bound
waiting for public ip to be bound
ip=192.168.1.50
```

The script, along with a more detailed explanation of its inner workings and installation requirements is available on the [Network Containers Github](https://github.com/jeroenpeeters/docker-network-containers "Docker Network Containers") repository.

## Whats next?

So now all your containers can expose standard ports to the network. That's great I hear you say, but what's next? This opens a world of possibilities, of which DNS is one. In my daily job I use it to run services within our department and have them available at their own hostnames. For this I use [SkyDNS](https://github.com/skynetservices/skydns "SkyDNS"), I'm planning to write an article about that soon.
