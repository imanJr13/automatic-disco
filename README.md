## Install loki and promtail to monitor server terminal and journal

  1. install loki and promtail using docker compose and use below docker file
      ```
     version: "3.4"

     networks:
     loki:

     services:
     loki:
     image: grafana/loki:2.8.0
     mem_limit: 3G
     mem_reservation: 128M
     cpus: 0.5

     volumes: 
      - "/etc/localtime:/etc/localtime:ro"
     ports:
      - "10.19.143.65:3100:3100"
     command: -config.file=/etc/loki/local-config.yaml
     networks:
      - loki

     promtail:
     image: grafana/promtail:2.8.0
     mem_limit: 3G
     mem_reservation: 128M
     cpus: 0.5
     volumes:
      - "/etc/localtime:/etc/localtime:ro"
      - /var/log/journal:/var/log/journal
      - /var/run/docker.sock:/var/run/docker.sock 
      - /data/users/appuser/loki-promtail/config.yml:/etc/promtail/config.yml
      - /data/docker/containers:/var/lib/docker/containers
     command: -config.file=/etc/promtail/config.yml
     networks:
      - loki

     cadvisor:
     image: gcr.io/cadvisor/cadvisor:latest
     mem_limit: 2G
     mem_reservation: 128M
     cpus: 0.5

     container_name: cadvisor
     privileged: true
     volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /data/docker:/var/lib/docker:ro
      - /dev/disk:/dev/disk/:ro

     networks:
      - loki  
      ```
    
the run below comand in a directory which docker-compose.yml exist

```
docker compose up -d
