# loki-setup

Copying Techno Tim's setup, but working in my environment

https://technotim.live/posts/grafana-loki/

## update APT
```
sudo apt update && sudo apt upgrade -y
sudo apt autoremove
```
## Install docker and docker-compose
```
sudo apt install docker docker-compose -y
```
## Add user to docker group
```
sudo usermod -a -G docker jfmatth
```
- Logout and back in to take effect

## rsyslog - https://alexandre.deverteuil.net/post/syslog-relay-for-loki/

- Create file /etc/rsyslog.d/00-promtail.conf
```
ruleset(name="remote"){
  # https://www.rsyslog.com/doc/v8-stable/configuration/modules/omfwd.html
  # https://grafana.com/docs/loki/latest/clients/promtail/scraping/#rsyslog-output-configuration
  action(type="omfwd" Target="localhost" Port="1514" Protocol="tcp" Template="RSYSLOG_SyslogProtocol23Format" TCP_Framing="octet-counted")
}

module(load="imudp")
input(type="imudp" port="514" ruleset="remote")

module(load="imtcp")
input(type="imtcp" port="514" ruleset="remote")
```
- Restart rsyslog service

## Docker driver for docker logs
```
docker plugin install grafana/loki-docker-driver:latest --alias loki --grant-all-permissions
```

## Create grafana folder
```
mkdir grafana
```

## starting for debug
```
docker-compose up --force-recreate
```
- add -d to run in background and on restart

## Login to Grafana UI
- ```http://<ip>:3000```
- Add Loki datasource
    ```http://loki:3100```

## Point syslog generating devices to <ip>:514 - Enjoy

## DEBUG - To keep the containers from starting on reboot
docker update --restart=no $(docker ps -a -q)
