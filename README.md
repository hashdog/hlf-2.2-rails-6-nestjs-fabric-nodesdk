# HyperLedger Fabric 2.2 + TODO APP Rails 6 + Fabric-sdk-nestjs chaincode restapi

## Hello
![network](https://miro.medium.com/max/700/1*nz4hX8J2DII6fcXzJap37Q.png "network")

We are going to create a HLF 2.1 network with name ApNetwork, consisting of one consortium (a collection of organizations) named ApConsortium. This consortium includes two organizations: Org1 (red) and Org2 (blue). Both organizations provide each one peer (peer0.org1.ap.com for Org1 and peer0.org2.ap.com for Org2). We will join those peers to a so called channel. The channel allows sending transactions between those two organizations and has the name apchannel. The network and channel are supported by one orderer organization with name OrdererOrg (green) which provides the transaction ordering service with one single orderer node: orderer0.ap.com. This is the minimum viable setup for a functional HLF blockchain network and introduces all of the crucial components and concepts of the HLF framework.
After Create HLF network, a new rest-api service  and todo app will be available for interacts with the chaincode installed

## Structure folder

**Let’s examine the folder structure inside the tutorial project root folder:**

- **channel-artifacts**: Here we are going to store all artifacts which we will generate in order to setup an application channel.
- **config**: In the config folder we have two files: core.yaml and configtx.yaml. While core.yaml has some default configuration values for our peers, configtx.yaml holds information about our network and application channel structure we intend to create.
- **crypto-material:** In this folder all identity related files (often called crypto-material) will be stored after being generated by a tool called cryptogen. Since identity is one important key aspect in a HLF network, this is one of the first folders we will fill with life. But besides a .gitignore file, this folder is empty for now.
- **cryptogen-input**- : While crypto-material is the place where our generated identity related files go, the folder cryptogen-input holds information about how we would like to generate our “blockchain identities”. Since we plan to create a network consisting of three participants (namely peer0.org1.ap.com, peer0.org2.ap.com, orderer0.ap.com), we need to provide three input files, each for one of those elements: crypto-config-org1.yaml, crypto-config-org2.yaml and crypto-config-orderer.yaml. All those files lay within the cryptogen-input folder.
- **docker:** The easiest (and for development most preferred) way to instantiate peers, orderers and to create networks in HLF, is to use docker containers. In the docker folder we have three docker-compose files, each for one of our network components: docker-compose-org1.yaml for peer0.org1.ap.com (our peer in Org1) , docker-compose-org2.yaml for peer0.org2.ap.com (our peer in Org2) and docker-compose-orderer.yaml for orderer.ap.com (our orderer).
- **system-genesis-block:** A (system) genesis block is the very first (configuration) block of a blockchain network. We are going to create a genesis block for our network and store it in the system-genesis block folder. Our orderer is going to need this block to start the network. This folder is empty for now.
- **.env:** Holds some basic information about which HLF docker images we will use and how our docker network will be called. This file is used by docker-compose and we will have a look into it later.
- **start.sh:** This script runs the whole tutorial. What exactly happens, will be covered in the remaining sections. Actually this is the result of all the steps mentioned in this article. If you don’t plan to read all of the text and prefer just to see some colorful terminal outputs: There you go! ;-) Start the network by executing the script from the project root directory:
- **services:** In this folder we have the app chaincode rest-api service
- **stop.sh:** Stops all network relevant docker containers and does some docker clean up jobs. In case you started the tutorial network earlier, execute this script to shut it down and to remove any created project artifacts:
- **deploy_cc.sh:** This script deploy the chaincode store in chaincode/source folder to org1 and org2 and start a rest api for access to the chaincode deployed

## Usage

    ./stop.sh && ./start.sh && ./deploy_cc.sh todo apchannel


## Fabric-sdk NestJS
Now you can start chaincode-rest-api service

    docker-compose -f ./docker/docker-compose-chaincode-rest-api.yaml up

then

    open localhost:8080/api-docs

now you try enrollAdmin user like this:

    curl -X GET "http://localhost:8080/enrollAdmin" -H "accept: */*" -H "Authorization: Basic YWRtaW46YWRtaW5wdw=="


## TODO App

After run `chaincode-rest-api service`, and `enrollAdmin` user  you can start todo rails app 

    docker-compose -f ./docker/docker-compose-todo-app.yaml up

then

    open localhost:3000

all request going to send directly to HYPERLEDGER FABRIC 


## Acknowledgment

This project was based heavy on

- this article https://medium.com/@Arphil/setup-a-hyperledger-fabric-2-1-network-from-scratch-with-less-than-20-lines-of-shell-script-3bb75ef477f9
**thanks @Arphil**  

- fabric-sdk-nestjs