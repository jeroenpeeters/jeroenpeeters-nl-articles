# SkyDNS and Docker

In my previous post I introduced a method for assigning an IP to each Docker container. This way all containers can expose standard ports (e.g. 80 for http) over the network. This can be really handy if you need to expose several services within your department and don't want people to connect to non-standard Docker exposed ports. But still, having an IP for each Docker container is nice, yet doesn't change much since you still need to tell people the IP of a particular service. Furthermore, this IP will change when you restart the container since it comes from DHCP. In an environment where you can spawn and delete containers at will, some will live shorter than other's. You don't want to manage static IP's, but just allocate a dynamic IP using DHCP. Know, wouldn't it be nice to connect to a service using a well known, user friendly domain name?

## SkyDNS
