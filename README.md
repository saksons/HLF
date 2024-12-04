# Поднятие канала и добавление участников
## Создание сертификатов и первого блока
```bash
./fabric/bin/cryptogen generate --config=./configs/crypto-config-org1_2.yaml --output="organizations"

./fabric/bin/configtxgen -profile TestChannel -outputBlock ./channel-artifacts/test-net-genesis.block -channelID test-net -configPath ./configs/configtx-org1_2.yaml
```

## Поднятие контейнеров orderer, peer0.org1, peer0.org2, cli-org1_2
```bash
docker-compose up -d -f compose docker-compose-org1_2.yaml
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

<!-- ### Сбор конфигураций
```bash
    peer channel fetch config /opt/gopath/src/github.com/hyperledger/fabric/peer/channel-artifacts/config_block.pb -o orderer.test.net:7050 --ordererTLSHostnameOverride orderer.test.net -c test-net --tls --cafile "/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem"

    configtxlator proto_decode --input /opt/gopath/src/github.com/hyperledger/fabric/peer/channel-artifacts/config_block.pb --type common.Block --output /opt/gopath/src/github.com/hyperledger/fabric/peer/channel-artifacts/config_block.json

    jq .data.data[0].payload.data.config /opt/gopath/src/github.com/hyperledger/fabric/peer/channel-artifacts/config_block.json > "/opt/gopath/src/github.com/hyperledger/fabric/peer/channel-artifacts/Org1MSPconfig.json"

    jq .data.data[0].payload.data.config /opt/gopath/src/github.com/hyperledger/fabric/peer/channel-artifacts/config_block.json > "/opt/gopath/src/github.com/hyperledger/fabric/peer/channel-artifacts/Org2MSPconfig.json"

    jq .data.data[0].payload.data.config /opt/gopath/src/github.com/hyperledger/fabric/peer/channel-artifacts/config_block.json > "/opt/gopath/src/github.com/hyperledger/fabric/peer/channel-artifacts/Org3MSPconfig.json"
```  -->

```bash
export CORE_PEER_TLS_ENABLED=true
export ORDERER_CA=/root/test/organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem
export PEER0_ORG1_CA=/root/test/organizations/peerOrganizations/org1.test.net/tlsca/tlsca.org1.test.net-cert.pem
export PEER0_ORG2_CA=/root/test/organizations/peerOrganizations/org2.test.net/tlsca/tlsca.org2.test.net-cert.pem
export CORE_PEER_MSPCONFIGPATH=/root/test/organizations/peerOrganizations/org1.test.net/users/Admin@org1.test.net/msp
export CORE_PEER_ADDRESS=localhost:7051
export CORE_PEER_TLS_ROOTCERT_FILE=/root/test/organizations/peerOrganizations/org1.test.net/tlsca/tlsca.org1.test.net-cert.pem
export CORE_PEER_LOCALMSPID=Org1MSP
```

```bash
export PEER0_ORG3_CA=/root/test/organizations/peerOrganizations/org3.test.net/tlsca/tlsca.org3.test.net-cert.pem
```

```bash
/root/test/fabric/bin/peer chaincode invoke -o localhost:7050 --cafile /root/test/organizations/ordererOrganizations/test.net/msp/tlscacerts/tlsca.test.net-cert.pem --channelID test-net --name poas-chaincode -c '{"Args":["Register", "{\"id\":\"237b7923-d57f-45e6-bbc4-7c8e44397511\",\"number\":\"8888\",\"organization_id\":\"7dc22d90-ee76-4774-9394-bcf5808a0240\",\"retrust\":true, \"reg_date\":\"15.01.2023\",\"xml_data\":\"ZWjb2RlZCB4bWwgZGF0YSBleGFtcGxl\",\"xml_hash\":\"f948bf53d294de59ca3cd8a9bd33849b1c254184af4feaabbca04473307a11b\"}"]}' --waitForEvent --peerAddresses localhost:7051 --peerAddresses localhost:7052 --tls --tlsRootCertFiles /root/test/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/ca.crt --tlsRootCertFiles /root/test/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/ca.crt

peer chaincode invoke -o orderer.test.net:7050 --cafile ./organizations/ordererOrganizations/test.net/msp/tlscacerts/tlsca.test.net-cert.pem --channelID test-net --name poas-chaincode -c '{"Args":["Register", "{\"id\":\"237b7923-d57f-45e6-bbc4-7c8e44397511\",\"number\":\"8888\",\"organization_id\":\"7dc22d90-ee76-4774-9394-bcf5808a0240\",\"retrust\":true, \"reg_date\":\"15.01.2023\",\"xml_data\":\"ZWjb2RlZCB4bWwgZGF0YSBleGFtcGxl\",\"xml_hash\":\"f948bf53d294de59ca3cd8a9bd33849b1c254184af4feaabbca04473307a11b\"}"]}' --waitForEvent --peerAddresses peer0.org1.test.net:7051 --peerAddresses peer0.org2.test.net:7052 --tls --tlsRootCertFiles ./organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/ca.crt --tlsRootCertFiles ./organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/ca.crt
```