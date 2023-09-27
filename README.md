# loki-setup

Copying Techno Tim's setup, but working in my environment

https://technotim.live/posts/grafana-loki/

## Docker driver for docker logs
docker plugin install grafana/loki-docker-driver:latest --alias loki --grant-all-permissions

## starting for debug
docker-compose up --force-recreate


## make sure they don't start on startup.
docker update --restart=no $(docker ps -a -q)
