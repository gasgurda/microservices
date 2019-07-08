# Microservices
# Deploy to swarm from 2 compose files (docker-compose.yml and docker-compose.infra.yml)
docker stack deploy --compose-file=<(docker-compose -f docker-compose.infra.yml -f docker-compose.yml config 2>/dev/null)  DEV

# Check swarm nodes (from master node)
docker node ps

# Check status all containers in DEV env swarm cluster
docker stack services DEV

# Check topology DEV env
docker stack ps DEV

# Scale container replicas
docker service scale DEV_ui=3
#or
docker service update --replicas 3 DEV_ui

# Disable all replicas
docker service update --replicas 0 DEV_ui

# Set reliability label to node master-1 
docker node update --label-add reliability=high master-1

# Show reliability label from node 
docker node ls -q | xargs docker node inspect  -f '{{ .ID }} [{{ .Description.Hostname }}]: {{ .Spec.Labels }}'
