# Установка чейнкода для 3-ий организации
> Вполнять если чейнкод установлен на другие организации 

## Копируем пакет чейнкода на контейнер cli-org3
```bash
cd ./chaincode/src
docker cp test-chaincode.tar.gz cli-org3.test.net:/opt/gopath/src/github.com/hyperledger/fabric/peer/
```

## Переходим в контейнер
```bash
docker exec -it cli-org3.test.net bash
```

## Установка чейнкода на peer0.org3.test.net
```bash
peer lifecycle chaincode install test-chaincode.tar.gz
peer lifecycle chaincode queryinstalled --output json
```

## Апрув чейнкода на peer0.org1.test.net
```bash
CORE_PEER_ADDRESS=peer0.org1.test.net:7051
CORE_PEER_LOCALMSPID=Org1MSP
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/server.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/server.key
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/users/Admin@org1.test.net/msp
peer lifecycle chaincode approveformyorg \
-o orderer.test.net:7050 \
--tls \
--cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem \
--channelID test-net \
--name test-chaincode \
--version 1.0 \
--package-id test-chaincode:9614bff32959aca7f253aea3e075f44983f09fcb568ea863346bda61597e11bf \
--sequence 2

peer lifecycle chaincode checkcommitreadiness --channelID test-net --name test-chaincode --version 1.0 --sequence 2 --output json
```

## Апрув чейнкода на peer0.org2.test.net
```bash
CORE_PEER_ADDRESS=peer0.org2.test.net:7052
CORE_PEER_LOCALMSPID=Org2MSP
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/server.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/server.key
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/users/Admin@org2.test.net/msp

peer lifecycle chaincode approveformyorg \
-o orderer.test.net:7050 \
--tls \
--cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem \
--channelID test-net \
--name test-chaincode \
--version 1.0 \
--package-id test-chaincode:9614bff32959aca7f253aea3e075f44983f09fcb568ea863346bda61597e11bf \
--sequence 2

peer lifecycle chaincode checkcommitreadiness --channelID test-net --name test-chaincode --version 1.0 --sequence 2 --output json
```

## Апрув чейнкода на peer0.org3.test.net
```bash
CORE_PEER_ADDRESS=peer0.org3.test.net:7053
CORE_PEER_LOCALMSPID=Org3MSP
CORE_PEER_TLS_ENABLED=true
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org3.test.net/peers/peer0.org3.test.net/tls/server.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org3.test.net/peers/peer0.org3.test.net/tls/server.key
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org3.test.net/peers/peer0.org3.test.net/tls/ca.crt
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org3.test.net/users/Admin@org3.test.net/msp

peer lifecycle chaincode approveformyorg \
-o orderer.test.net:7050 \
--tls \
--cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem \
--channelID test-net \
--name test-chaincode \
--version 1.0 \
--package-id test-chaincode:9614bff32959aca7f253aea3e075f44983f09fcb568ea863346bda61597e11bf \
--sequence 2

peer lifecycle chaincode checkcommitreadiness --channelID test-net --name test-chaincode --version 1.0 --sequence 2 --output json
```

## Коммит чейнкода
```bash
peer lifecycle chaincode commit \
-o orderer.test.net:7050 \
--tls \
--cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem \
--channelID test-net \
--name test-chaincode \
--peerAddresses peer0.org1.test.net:7051 \
--tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/tlsca/tlsca.org1.test.net-cert.pem \
--peerAddresses peer0.org2.test.net:7052 \
--tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/tlsca/tlsca.org2.test.net-cert.pem \
--peerAddresses peer0.org3.test.net:7053 \
--tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org3.test.net/tlsca/tlsca.org3.test.net-cert.pem \
--version 1.0 \
--sequence 2
```

## Выполнение чейнкода из контейнера cli
```bash
peer chaincode invoke \
-o orderer.test.net:7050 \
--tls \
--cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem \
--channelID test-net \
--name test-chaincode \
--waitForEvent \
--peerAddresses peer0.org1.test.net:7051 \
--peerAddresses peer0.org2.test.net:7052 \
--peerAddresses peer0.org3.test.net:7053 \
--tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/ca.crt \
--tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/ca.crt \
--tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org3.test.net/peers/peer0.org3.test.net/tls/ca.crt \
-c '{"Args":["Register", "{\"id\":\"237b7923-d57f-45e6-bbc4-7c8e44397511\",\"number\":\"8885\",\"organization_id\":\"7dc22d90-ee76-4774-9394-bcf5808a0240\",\"retrust\":true, \"reg_date\":\"15.01.2023\",\"xml_data\":\"ZWjb2RlZCB4bWwgZGF0YSBleGFtcGxl\",\"xml_hash\":\"f948bf53d294de59ca3cd8a9bd33849b1c254184af4feaabbca04473307a11b\"}"]}'
```

## Выполнение чейнкода из хоста
```bash
cd ../../fabric/config

export CORE_PEER_ADDRESS=localhost:7051
export CORE_PEER_LOCALMSPID=Org1MSP
export CORE_PEER_TLS_CERT_FILE=/home/soks/test/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/server.crt
export CORE_PEER_TLS_KEY_FILE=/home/soks/test/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/server.key
export CORE_PEER_TLS_ROOTCERT_FILE=/home/soks/test/organizations/peerOrganizations/org1.test.net/tlsca/tlsca.org1.test.net-cert.pem
export CORE_PEER_MSPCONFIGPATH=/home/soks/test/organizations/peerOrganizations/org1.test.net/users/Admin@org1.test.net/msp
export CORE_PEER_TLS_ENABLED=true
export ORDERER_CA=/home/soks/test/organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem
export PEER0_ORG1_CA=/home/soks/test/organizations/peerOrganizations/org1.test.net/tlsca/tlsca.org1.test.net-cert.pem
export PEER0_ORG2_CA=/home/soks/test/organizations/peerOrganizations/org2.test.net/tlsca/tlsca.org2.test.net-cert.pem
export PEER0_ORG3_CA=/home/soks/test/organizations/peerOrganizations/org3.test.net/tlsca/tlsca.org3.test.net-cert.pem

../bin/peer chaincode invoke \
-o localhost:7050 \
--ordererTLSHostnameOverride orderer.test.net \
--tls \
--cafile ../../organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem \
--channelID test-net \
--name test-chaincode \
--waitForEvent \
--peerAddresses localhost:7051 \
--peerAddresses localhost:7052 \
--peerAddresses localhost:7053 \
--tlsRootCertFiles ../../organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/ca.crt \
--tlsRootCertFiles ../../organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/ca.crt \
--tlsRootCertFiles ../../organizations/peerOrganizations/org3.test.net/peers/peer0.org3.test.net/tls/ca.crt \
-c '{"Args":["Register", "{\"id\":\"247b7923-d57f-45e6-bbc4-7c8e44397511\",\"number\":\"8886\",\"organization_id\":\"1dc22d90-ee76-4774-9394-bcf5808a0240\",\"retrust\":true, \"reg_date\":\"15.01.2023\",\"xml_data\":\"ZWjb2RlZCB4bWwgZGF0YSBleGFtcGxl\",\"xml_hash\":\"f948bf53d294de59ca3cd8a9bd33849b1c254184af4feaabbca04473307a11b\"}"]}'
```

## Чтение данных из контейнера cli
```bash
peer chaincode query -C test-net -n test-chaincode -c '{"Args":["GetByNumber", "8885"]}'
```

## Чтение данных из хоста
```bash
../bin/peer chaincode query -C test-net -n test-chaincode -c '{"Args":["GetByNumber", "8886"]}'
```