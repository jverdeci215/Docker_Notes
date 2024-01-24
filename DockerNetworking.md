### Docker Networking
There are a many types of networks we can go over when discussing docker, we will discuss a few here:
1.	Bridge Networking (default):
-	The bridge network is the default network driver used by Docker. It allows containers on the same host to communicate with each other.
-	Containers connected to the bridge network can communicate via IP addresses or container names.
-	Containers on the bridge network can expose ports to the host or to other containers.
```
docker run --name bridgecontainer -d nginx
docker inspect <container name>
```
2.	Host Networking:
-	With host networking, containers use the network stack of the host directly, rather than running in their own network namespace.
-	This mode allows containers to bypass Docker's network isolation and have full access to the host network interfaces.
-	Host networking can be useful when performance is a critical factor or when you need to access network services running on the host directly.
```
docker run --name hostnetcontainer --network host -d nginx
docker inspect <container name>
```
3.	Custom Networking:
-	Docker allows you to create custom networks to meet specific requirements.
-	Custom networks can be created using the docker network create command and can be either bridge networks or overlay networks.
-	Custom networks provide isolation, control over IP addressing, and connectivity between containers within the same network.
Create the network and containers
```
docker network create mynetwork
docker run --network mynetwork --name netcontainer -d nginx
```
4.	Service Discovery and DNS:
-	Docker provides a built-in DNS service that allows containers to resolve each other's hostnames.
-	Containers within the same network can refer to each other using their container names instead of IP addresses.
-	Docker's embedded DNS server automatically resolves container names to their IP addresses within the network.
Connect our ```bridgecontainer``` to the network and install ping on the first container
```
docker network connect mynetwork bridgecontainer
docker exec -it netcontainer bash
apt-get update
apt-get install inetutils-ping
exit
```
We can ping the container that is in our network
```
docker exec -it netcontainer ping bridgecontainer
```
But we cannot ping the container that is on the default network
```
docker exec -it netcontainer ping hostnetcontainer
```
5.	Container Port Mapping:
-	Docker enables port mapping, allowing you to expose container ports to the host or to other containers.
-	The -p or --publish flag is used when running a container to specify the port mapping.
-	Port mapping enables external access to containerized applications by mapping container ports to host ports.
```
docker run -d -p <host_port>:<container_port> <image_name>
docker run -d -p 5000:5000 <flask app container name>
```
Valuable commands:
```
docker network ls
docker network remove <NetworkID>
```
