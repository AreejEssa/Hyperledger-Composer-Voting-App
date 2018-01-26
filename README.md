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
### Instantiating your Business Blockchain with Composer
1. Create a new project directory
```
> mkdir app
> cd app
```
2. Instantiate a Business network by yo npm plugin
```
> yo hyperledger-composer:businessnetwork
> Business network name: voting
> Description: A simple blockchain network
> Author name:  saif
> Author email: s4saif.121@email.com
> License: Apache-2.0
> Namespace: org.acme.voting
```
3. Navigate to business network folder and install the dependencies
```
> cd my-network 
> npm install> cd my-network 
> npm install
```

.cto file code
```
/**
 * Write your model definitions here
 */
namespace org.acme.voting

participant voter identified by voterID {
  o String voterID
  o String fullName
  o ifVoted voted
}

asset ifVoted identified by voterID {
  o String voterID
  o Boolean votedOrNot
}

asset candidateVote identified by politicalParty {
  o String politicalParty
  o Integer totalVote
}
transaction vote {
  --> candidateVote candidateVoteAsset
  --> ifVoted ifVotedAsset
}
```
logic.js code
``` JavaScript
'use strict';
/**
 * Write your transction processor functions here
 */

/**
 * Sample transaction
 * @param {org.acme.voting.vote} vote
 * @transaction
 */
function vote(tx) {
    if (!tx.ifVotedAsset.votedOrNot) {
        tx.candidateVoteAsset.totalVote = tx.candidateVoteAsset.totalVote + 1;
        return getAssetRegistry('org.acme.voting.candidateVote')
            .then(function (assetRegistry) {
                return assetRegistry.update(tx.candidateVoteAsset);
            })
            .then(function () {
                return getAssetRegistry('org.acme.voting.ifVoted')
                    .then(function (assetRegistry) {
                        tx.ifVotedAsset.votedOrNot = true;
                        return assetRegistry.update(tx.ifVotedAsset);
                    })
            });
    }
}
```
4. Creating the .bna file

```
> mkdir dir
> composer archive create --sourceType dir --sourceName . -a ./dist/network.bna
```

5. Deploying the .bna file on the Hyperledger Fabric Blockchain
```
> composer runtime install --card PeerAdmin@hlfv1 --businessNetworkName voting
> composer network start --card PeerAdmin@hlfv1 -A admin -S adminpw -a network.bna -f networkadmin.card
> composer card import -f networkadmin.card
```

6. Interfacing you blockchain with Restfull CRUD Apis
```
> composer-rest-server
```

Authors
** [Saif Rehman](https://github.com/SaifRehman)
** [Areej Essa](https://github.com/AreejEssa/)