# Поднятие мониторинга
```bash
cd ./monitoring
docker-compose up -d
```

# Настройка мониторинга
## Node-exporter
**В prometheus.yml добавляем строки**
```yaml
scrape_configs:
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter.monitoring.test.net:9100']
        labels:
          instance: host
```

## Cadvisor
**В prometheus.yml добавляем строки**
```yaml
scrape_configs:
  - job_name: 'cadvisor'
    static_configs:
      - targets: ['cadvisor.monitoring.test.net:8080']
        labels:
          instance: containers
```

## Hyperledger Fabric
**Добавляем в переменные окружения контейнеров peer**
```bash
CORE_METRICS_PROVIDER=prometheus
CORE_OPERATIONS_LISTENADDRESS=0.0.0.0:8443
CORE_OPERATIONS_TLS_ENABLED=false
```
**Добавляем в переменные окружения контейнеров orderer**
```bash
ORDERER_METRICS_PROVIDER=prometheus
ORDERER_OPERATIONS_LISTENADDRESS=0.0.0.0:8443
ORDERER_OPERATIONS_TLS_ENABLED=false
```

**В prometheus.yml добавляем строки**
```yaml
scrape_configs:
  - job_name: 'hyperledger-fabric'
    static_configs:
      - targets: ['orderer.test.net:8443','peer0.org1.test.net:8443','peer0.org2.test.net:8443','peer0.org3.test.net:8443']
        labels:
          instance: HLF
```