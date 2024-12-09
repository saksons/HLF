# Поднятие канала и добавление участников
## Создание сертификатов и первого блока
```bash
./fabric/bin/cryptogen generate --config=./crypto-config.yaml --output="organizations"

./fabric/bin/configtxgen -profile TestChannel -outputBlock ./channel-artifacts/test-net-genesis.block -channelID test-net
```

## Поднятие контейнеров orderer, peer0.org1, peer0.org2, cli-org1_2
```bash
docker-compose up -d
```

## Присоединение участников к каналу
```bash
docker exec -it cli.test.net /bin/bash

osnadmin channel join --channelID test-net --config-block ./channel-artifacts/test-net-genesis.block -o orderer.test.net:7054 --ca-file "./organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem" --client-cert "./organizations/ordererOrganizations/test.net/orderers/orderer.test.net/tls/server.crt" --client-key "./organizations/ordererOrganizations/test.net/orderers/orderer.test.net/tls/server.key"

peer channel join -b channel-artifacts/test-net-genesis.block

CORE_PEER_ADDRESS=peer0.org2.test.net:7052
CORE_PEER_LOCALMSPID=Org2MSP
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/server.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/server.key
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/users/Admin@org2.test.net/msp

peer channel join -b channel-artifacts/test-net-genesis.block
```

# Установка чейнкода, мониторинга и 3 организации
[Установка чейнкода](./chaincode/README.md)  
[Установка мониторинга](./monitoring/README.md)  
[Добавление 3 организации в канал](./addOrg3/README.md)