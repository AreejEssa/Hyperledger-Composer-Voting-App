### Usuage

## Prerequisites
1. Install Dockers
2. Install VSCode, and VSCode Hyperledger Composer Plugin
3. Install required npm node packages
``` 
> npm install -g composer-cli
> npm install -g generator-hyperledger-composer
> npm install -g composer-rest-server
> npm install -g yo
```
4. Setting and running Hyperledger Fabric
```
> mkdir fabric-tools
> cd fabric-tools
> curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.zip
> unzip fabric-dev-servers.zip
> ./downloadFabric.sh
> ./startFabric.sh
> ./createPeerAdminCard.sh
```
