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

## docker compose

How to run -> in terminal, in directory where 'docker-compose.yml' is placed run this command:\
`docker compose -p "dev-containers" up`

You may have to setup the external network manually:\
`sudo docker network create -d bridge overlaynetwork`

To list networks use:\
`sudo docker network ls`

For elastic search, adjust memory\
`sudo sysctl -w vm.max_map_count=262144`

in file etc/sysctl.conf add line:\
`vm.max_map_count=262144`

to make it permanent: \
`sudo sysctl -p`

also for elastic, make sure it has proper permissions to volume:\
`sudo chown -R 1000:1000 /home/mirusser/docker/volumes/elasticsearch/data` 

remove any lingering node lock\
`sudo rm -f /home/mirusser/docker/volumes/elasticsearch/data/node.lock`

To delete all containers use:\
`docker container prune -f`

After starting containers make sure that ports are being forwarded (and are listening)
