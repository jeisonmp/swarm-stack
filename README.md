

### Config a first Swarm node
`sudo docker swarm init`

### Create a network "router-net"
`docker network create -d overlay --subnet 10.1.0.0/16 router-net`
# Configure /etc/hosts in all nodes. Change <ROUTER_IP>
`echo "127.0.0.1       portainer.local.com.br" >> /etc/hosts`
`echo "127.0.0.1       traefik.local.com.br" >> /etc/hosts`
`echo "127.0.0.1       mariadb.local.com.br" >> /etc/hosts`
`echo "127.0.0.1       postgress.local.com.br" >> /etc/hosts`
# If not works, change 127.0.1 to ip inside router-net.Peers.IP ###
`docker network inspect frontend-net`

### Deploy Stack "Traefik"
`DNS=traefik.local.com.br USER=admin HASHED_PASSWORD=$(openssl passwd -apr1 admin123) docker stack deploy -c docker-compose-traefik.yml traefik`
### Deploy Stack "Portainer-ce"
`DNS=portainer.local.com.br docker stack deploy -c docker-compose-portainer.yml portainer`

### Testing your DOMAINS ###
`curl -vvv traefik.local.com.br`
`curl -vvv portainer.local.com.br`
`curl -vvv mariadb.local.com.br`
`curl -vvv redis.local.com.br`

### Now testing the DOMAINS in your Browser ###
# Open your browser and access "traefik.vm.com.br" and "portainer.vm.com.br" 

### Deploy the Stacks ###
`DNS=traefik.local.com.br USER=admin HASHED_PASSWORD=$(openssl passwd -apr1 admin123) docker stack deploy -c docker-compose-traefik.yml traefik`
`DNS=portainer.local.com.br docker stack deploy -c docker-compose-portainer.yml portainer`
`DNS=mariadb.local.com.br docker stack deploy -c docker-compose-dbs.yml dbs`
`DNS=redis.local.com.br docker stack deploy -c docker-compose-redis.yml redis`
`POSTGRES_USER=<user> POSTGRES_PASS=<pass> POSTGRES_DB=<database-name> docker stack deploy -c docker-compose-postgress.yml postgress`
`docker stack deploy -c docker-compose-sqlexpress.yml sqlexpress --detach=false`
`DNS=wordpress.local.com.br MYSQL_HOST=mariadb.local.com.br:3306 MYSQL_USER=admin MYSQL_PASSWORD=admin123 MYSQL_DATABASE=wordpress docker stack deploy -c docker-compose-wordpress.yml wordpress`

### In your browser ###
# https://mariadb.local.com.br => Server=mariadb.local.com.br, User=admin, Pass=admin123
# https://redis.local.com.br => Host=redis.local.com.br, Port=6379, Name=graphlinq-cache

###  ###