# Docker Network Containers

If you are familiar with Docker, and understand the concept of a 'network container', you can directly dive into the mechanics and go to the [Network Containers Github](https://github.com/jeroenpeeters/docker-network-containers) repository.

## Audience

This article is written for people who have substantial experience with Docker. Dockerfiles, images, containers... it should all sound familiar. If not, [this](https://www.docker.com/tryit/ "Try Docker") is a great pointer to get you started.


## Docker Networking

If you have been working with Docker you know that Docker maps the container's private ports to a -sort of random- unique public port on the host machine. This way you can access the container's services from anywhere on your network. If you only expose one webserver, you can as well expose port 80 directly and this will work well. But if you want to expose two (or more) webservers from one host? Only one will be able to use port 80 directly. The other webservers will be available at awkard ports unless you use some kind of reverse proxy mechanism.

### Wouldn't it be nice...

-   If a container can get its own IP on the regular network?
-   If a container could get this IP from the regular DHCP server?
-   that all of this would happen without the container being aware?

At the time of writing, these scenario's are not cover by Docker. They might very well be in the (near) future.

## Network Containers: An Agnostic Approach
