# Repo for my dev containers

## General docker commands:

Install docker (on arch linux):
```bash
sudo pacman -S docker
```

Enable Docker’s systemd service so that it starts on boot, and then start it right away:
```bash
sudo systemctl enable --now docker.service
```

Just start docker:
```bash
sudo systemctl start docker.socket docker.service
```

To stop docker:
```bash
sudo systemctl stop docker.socket docker.service
```

Verify it runs:
```bash
systemctl status docker.service
```

Add your user to the docker group:
```bash
sudo usermod -aG docker $USER
```

Disable on startup
```bash
sudo systemctl disable --now docker.socket
sudo systemctl disable --now docker.service
```

Find docker0 IP:
```bash
ip -4 addr show docker0
```

## docker compose

How to run -> in terminal, in directory where file: 'docker-compose.yml' is placed run this command:
```bash
docker compose -p "dev-containers" up
```

You may have to setup the external network manually:
```bash
sudo docker network create -d bridge sws-containers-bridge-network
```

To list networks use:
```bash
sudo docker network ls
```

For elastic search, adjust memory:
```bash
sudo sysctl -w vm.max_map_count=262144
```

in file etc/sysctl.conf add line:
```bash
vm.max_map_count=262144
```

to make it permanent:
```bash
sudo sysctl -p
```

also for elastic, make sure it has proper permissions to volume:
```bash
sudo chown -R 1000:1000 /home/mirusser/docker/volumes/elasticsearch/data
```

remove any lingering node lock:
```bash
sudo rm -f /home/mirusser/docker/volumes/elasticsearch/data/node.lock`
```

To delete all containers use:
```bash
docker container prune -f`
```

Mongo, initialize the replica set (one-time):
```bash
docker exec -it mongo mongosh --eval "rs.initiate({_id:'rs0', members:[{_id:0, host:'mongo:27017'}]})"
```

After enabling replica set, you can verify transactions are supported:
```bash
docker exec -it mongo mongosh --eval "rs.status().ok"
```
Should output `1`

When running apps locally you may need to associate localhost with 'mongo':
```bash
sudo sh -c 'echo "127.0.0.1 mongo" >> /etc/hosts'
```

---

After starting containers make sure that ports are being forwarded (and are listening)
