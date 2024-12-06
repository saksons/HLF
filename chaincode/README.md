```bash
    docker build -t poas-chaincode:test network/poas-chaincode-svc/

    tar cfz code.tar.gz connection.json

    tar cfz test-chaincode.tgz metadata.json code.tar.gz

    ./../bin/peer lifecycle chaincode calculatepackageid /home/soks/work/chaincode/test-chaincode.tgz

    cd ./chaincode/src

    docker cp test-chaincode.tar.gz cli.test.net:/opt/gopath/src/github.com/hyperledger/fabric/peer/

    cd ../

    docker-compose up -d

    docker exec -it cli.test.net bash

    CORE_PEER_ADDRESS=peer0.org1.test.net:7051
    CORE_PEER_LOCALMSPID=Org1MSP
    CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/server.crt
    CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/server.key
    CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/ca.crt
    CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/users/Admin@org1.test.net/msp

    peer lifecycle chaincode install test-chaincode.tar.gz

    CORE_PEER_ADDRESS=peer0.org2.test.net:7052
    CORE_PEER_LOCALMSPID=Org2MSP
    CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/server.crt
    CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/server.key
    CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/ca.crt
    CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/users/Admin@org2.test.net/msp
    
    peer lifecycle chaincode install test-chaincode.tar.gz

    CORE_PEER_ADDRESS=peer0.org1.test.net:7051
    CORE_PEER_LOCALMSPID=Org1MSP
    CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/server.crt
    CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/server.key
    CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/ca.crt
    CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/users/Admin@org1.test.net/msp

    peer lifecycle chaincode queryinstalled

    peer lifecycle chaincode approveformyorg -o orderer.test.net:7050 --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem -C test-net --name test-chaincode --package-id test-chaincode:9614bff32959aca7f253aea3e075f44983f09fcb568ea863346bda61597e11bf --sequence 1 -v 1.0 --signature-policy 'OR('\'Org1MSP.admin\'', '\'Org2MSP.admin\'')'

    peer lifecycle chaincode checkcommitreadiness -C test-net --name test-chaincode -v 1.0 --signature-policy 'OR('\'Org1MSP.admin\'', '\'Org2MSP.admin\'')' --sequence 1 --output json

    CORE_PEER_ADDRESS=peer0.org2.test.net:7052
    CORE_PEER_LOCALMSPID=Org2MSP
    CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/server.crt
    CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/server.key
    CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/ca.crt
    CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/users/Admin@org2.test.net/msp

    peer lifecycle chaincode queryinstalled

    peer lifecycle chaincode approveformyorg -o orderer.test.net:7050 --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem -C test-net --name test-chaincode --package-id test-chaincode:9614bff32959aca7f253aea3e075f44983f09fcb568ea863346bda61597e11bf --sequence 1 -v 1.0 --signature-policy 'OR('\'Org1MSP.admin\'', '\'Org2MSP.admin\'')'

    peer lifecycle chaincode checkcommitreadiness -C test-net --name test-chaincode -v 1.0 --signature-policy 'OR('\'Org1MSP.admin\'', '\'Org2MSP.admin\'')' --sequence 1 --output json


    peer lifecycle chaincode commit -o orderer.test.net:7050 --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/ordererOrganizations/test.net/tlsca/tlsca.test.net-cert.pem --signature-policy 'OR('\'Org1MSP.admin\'', '\'Org2MSP.admin\'')' -C test-net --name test-chaincode --peerAddresses peer0.org1.test.net:7051 --peerAddresses peer0.org2.test.net:7052 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/ca.crt --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/ca.crt -v 1.0 --sequence 1

    peer lifecycle chaincode querycommitted --channelID test-net --name test-chaincode

    peer chaincode invoke -o orderer.test.net:7050 --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/ordererOrganizations/test.net/msp/tlscacerts/tlsca.test.net-cert.pem --channelID test-net --name test-chaincode -c '{"Args":["Register", "{\"id\":\"237b7923-d57f-45e6-bbc4-7c8e44397511\",\"number\":\"8888\",\"organization_id\":\"7dc22d90-ee76-4774-9394-bcf5808a0240\",\"retrust\":true, \"reg_date\":\"15.01.2023\",\"xml_data\":\"ZWjb2RlZCB4bWwgZGF0YSBleGFtcGxl\",\"xml_hash\":\"f948bf53d294de59ca3cd8a9bd33849b1c254184af4feaabbca04473307a11b\"}"]}' --waitForEvent --peerAddresses peer0.org1.test.net:7051 --peerAddresses peer0.org2.test.net:7052 --tls --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org1.test.net/peers/peer0.org1.test.net/tls/ca.crt --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/organizations/peerOrganizations/org2.test.net/peers/peer0.org2.test.net/tls/ca.crt
```
