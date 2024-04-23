# Monitoring with Prometheus

## Prerrequisitos
- Docker
- Prometheus: **bitnami/prometheus:latest**
- Node Exporters: **bitnami/node-exporter**

## Iniciar servidores

Rede para comunicação entre os serviços:
```
docker network create monitor
```

Servidor 1:
```
docker run -d --name node-exporter1 -p 9101:9100 --network monitor bitnami/node-exporter:latest
```

Servidor 2:
```
docker run -d --name node-exporter2 -p 9102:9100 --network monitor bitnami/node-exporter:latest
```

Servidor 3:
```
docker run -d --name node-exporter3 -p 9103:9100 --network monitor bitnami/node-exporter:latest
```
