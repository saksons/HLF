# Подключение 3-ий организации к каналу
## Создание крипто материалов
```bash
cd ./addOrg3
../fabric/bin/cryptogen generate --config=crypto-config-org3.yaml --output="../organizations"
../fabric/bin/configtxgen -printOrg Org3 > ../organizations/peerOrganizations/org3.test.net/org3.json
```

## Поднятие сервисов
```bash
docker-compose up -d
docker exec -it cli-org3.test.net bash
```

## Изменение конфигураций канала
```bash
CORE_PEER_ADDRESS=peer0.org1.test.net:7051
CORE_PEER_LOCALMSPID=Org1MSP
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/server.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/server.key
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/users/Admin@org1.test.net/msp

peer channel fetch config config_block.pb -o orderer.test.net:7050 -c test-net --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem

configtxlator proto_decode --input config_block.pb --type common.Block | jq .data.data[0].payload.data.config > config.json

jq -s '.[0] * {"channel_group":{"groups":{"Application":{"groups": {"Org3MSP":.[1]}}}}}' config.json /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org3.test.net/org3.json > modified_config.json

configtxlator proto_encode --input config.json --type common.Config --output config.pb

configtxlator proto_encode --input modified_config.json --type common.Config --output modified_config.pb

configtxlator compute_update --channel_id test-net --original config.pb --updated modified_config.pb --output org3_update.pb

configtxlator proto_decode --input org3_update.pb --type common.ConfigUpdate | jq . > org3_update.json

echo '{"payload":{"header":{"channel_header":{"channel_id":"test-net", "type":2}},"data":{"config_update":'$(cat org3_update.json)'}}}' | jq . > org3_update_in_envelope.json

configtxlator proto_encode --input org3_update_in_envelope.json --type common.Envelope --output org3_update_in_envelope.pb


peer channel signconfigtx -f org3_update_in_envelope.pb

CORE_PEER_ADDRESS=peer0.org2.test.net:7052
CORE_PEER_LOCALMSPID=Org2MSP
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/server.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/server.key
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/users/Admin@org2.test.net/msp

peer channel update -f org3_update_in_envelope.pb -c test-net -o orderer.test.net:7050 --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem
```

## Подключение 3-ий организации к каналу
```bash
CORE_PEER_ADDRESS=peer0.org3.test.net:7053
CORE_PEER_LOCALMSPID=Org3MSP
CORE_PEER_TLS_ENABLED=true
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org3.test.net/peers/peer0.org3.test.net/tls/server.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org3.test.net/peers/peer0.org3.test.net/tls/server.key
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org3.test.net/peers/peer0.org3.test.net/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org3.test.net/users/Admin@org3.test.net/msp

peer channel fetch 0 mychannel.block -o orderer.test.net:7050 -c test-net --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem

peer channel join -b mychannel.block
```

# Установка чейнкода
[Установка чейнкода для 3-ий организации](../chaincode/README.org3.md)
