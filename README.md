```bash
docker-machine create -d virtualbox domain
docker-machine create -d virtualbox domain001

docker-machine ls

docker-machine ssh domain "docker swarm init --listen-addr $(docker-machine ip domain) --advertise-addr $(docker-machine ip domain)"

export manager_token=$(docker-machine ssh domain "docker swarm join-token manager -q")

docker-machine ssh domain001 "docker swarm join --token=${manager_token} --listen-addr $(docker-machine ip domain001) --advertise-addr $(docker-machine ip domain001) $(docker-machine ip domain)"

eval $(docker-machine env domain)

docker network create -d overlay domain_world

docker stack deploy -c docker-compose-domain.yml domain

docker service ls

curl -H Host:domain.local http://192.168.99.111
```