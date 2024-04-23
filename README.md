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

## Iniciar Prometheus

```
docker run -d --name prometheus -p 9090:9090 --network monitor \
-v $(pwd)/prometheus.yml:/opt/bitnami/prometheus/conf/prometheus.yml \
bitnami/prometheus:latest
```

**Prometheus UI:**
```
http://localhost:9090/targets/
```

## Consultas

A partir da página inicial **Graph**:

- node_cpu_seconds_total
- node_cpu_seconds_total{instance="node-exporter2:9100"} // filtragem por instância específica
- node_ipvs_connections_total

## Bônus: Instrumentalizando uma aplicação Python Flask

Através do pacote Prometheus Flask exporter

Build:
```
docker build -t pythonserver .
```

Iniciar aplicação:
```
docker run -d --name pythonserver -p 8081:8080 --network monitor pythonserver
```

Simular tráfico de rede:
```
curl localhost:8081
curl localhost:8081/home
curl localhost:8081/contact
```

Consultas:
- flask_http_request_duration_seconds_bucket
- flask_http_request_total
- process_virtual_memory_bytes
