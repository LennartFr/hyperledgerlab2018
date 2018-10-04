<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">

# News Updates

<img src="https://www.hyperledger.org/wp-content/uploads/2018/10/EEA-Hyperledger-02-1.png"><p>

<a href="https://www.hyperledger.org/blog/2018/10/01/growing-the-enterprise-blockchain-ecosystem-through-open-standards-and-open-source-code">Growing the Enterprise Blockchain Ecosystem Through Open Standards and Open Source Code</a>

# Blockchain Usecases
[Blockchain usecases from IBM](https://www.ibm.com/blockchain/use-cases/)

<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">

# Hyperledger Components

<img src="https://farm1.staticflickr.com/960/41055079635_d00c82c7dd_z.jpg" width="640" height="203" alt="fabric">

https://www.hyperledger.org/projects/fabric

[Hyperledger Fabric Documentation](https://hyperledger-fabric.readthedocs.io/en/release/)

Hyperledger, an open source collaborative effort to advance cross-industry blockchain technologies, 
is hosted by The Linux Foundation®. 

Deployed in Docker images.

<img src="https://farm1.staticflickr.com/968/27085403057_c8a2ccd0cc_z.jpg" width="640" height="202" alt="composer">

https://www.hyperledger.org/projects/composer

[Composer Playground](https://composer-playground.mybluemix.net/login)

<img src="https://hyperledger.github.io/composer/v0.19/assets/img/ComposerArchitecture.svg">


<img src="https://hyperledger.github.io/composer/v0.19/assets/img/Composer-Diagram.svg">


<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">

# hyperledgerlab2018

https://hyperledger.github.io/composer/latest/installing/development-tools.html

## Before you begin
Make sure you have installed the required pre-requisites, following the instructions in Installing pre-requisites.

<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">

### Step 1: Install the CLI tools

Essential CLI tools:

#### npm install -g composer-cli@0.20
Utility for running a REST Server on your machine to expose your business networks as RESTful APIs:

#### npm install -g composer-rest-server@0.20
Useful utility for generating application assets:

#### npm install -g generator-hyperledger-composer@0.20


Yeoman is a tool for generating applications, which utilises generator-hyperledger-composer:

#### npm install -g yo

### Step 2: Install Playground

Browser app for simple editing and testing Business Networks:

#### npm install -g composer-playground@0.20

### Step 3: Set up your IDE

### Step 4 Install Hyperledger Fabric

In a directory of your choice (we will assume ~/fabric-dev-servers), get the .tar.gz file that contains the tools to install Hyperledger Fabric:

## mkdir ~/fabric-dev-servers && cd ~/fabric-dev-servers

## curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.tar.gz
tar -xvf fabric-dev-servers.tar.gz

A zip is also available if you prefer: just replace the .tar.gz file with fabric-dev-servers.zip and the tar -xvf command with a unzip command in the preceding snippet.

Use the scripts you just downloaded and extracted to download a local Hyperledger Fabric v1.2 runtime:

## cd ~/fabric-dev-servers
export FABRIC_VERSION=hlfv12
./downloadFabric.sh

## Controlling your dev environment
## Starting and stopping Hyperledger Fabric

    cd ~/fabric-dev-servers
    export FABRIC_VERSION=hlfv12
    ./startFabric.sh
    ./createPeerAdminCard.sh


##  Start the web app ("Playground")
  To start the web app, run:
  composer-playground
  
  
  Appendix: destroy a previous setup
If you've previously used an older version of Hyperledger Composer and are now setting up a new install, you may want to kill and remove all previous Docker containers, which you can do with these commands:

Copy
    docker kill $(docker ps -q)
    docker rm $(docker ps -aq)
    docker rmi $(docker images dev-* -q)

<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">

# Developer tutorial for creating a Hyperledger Composer solution

# Step One: Creating a business network structure
https://hyperledger.github.io/composer/latest/tutorials/developer-tutorial.html

##   Create a skeleton Business Network Archive with Yeoman    

Enter tutorial-network for the network name, and desired information for description, author name, and author email.

Select Apache-2.0 as the license.

Select org.example.mynetwork as the namespace.

Select No when asked whether to generate an empty network or not.

# Step Two: Defining a business network

## Open the org.example.mynetwork.cto model file.
Replace the contents with the following:
~~~~
/**
 * My commodity trading network
 */
namespace org.example.mynetwork
asset Commodity identified by tradingSymbol {
    o String tradingSymbol
    o String description
    o String mainExchange
    o Double quantity
    --> Trader owner
}
participant Trader identified by tradeId {
    o String tradeId
    o String firstName
    o String lastName
}
transaction Trade {
    --> Commodity commodity
    --> Trader newOwner
}

~~~~

Save your changes to org.example.mynetwork.cto

## Adding JavaScript transaction logic

Open the logic.js script file.

Replace the contents with the following:

~~~~/**
 * Track the trade of a commodity from one trader to another
 * @param {org.example.mynetwork.Trade} trade - the trade to be processed
 * @transaction
 */
async function tradeCommodity(trade) {
    trade.commodity.owner = trade.newOwner;
    let assetRegistry = await getAssetRegistry('org.example.mynetwork.Commodity');
    await assetRegistry.update(trade.commodity);
}

~~~~

Save your changes to logic.js.

## Adding Access Control

~~~~/**
 * Access control rules for tutorial-network
 */
rule Default {
    description: "Allow all participants access to all resources"
    participant: "ANY"
    operation: ALL
    resource: "org.example.mynetwork.*"
    action: ALLOW
}

rule SystemACL {
  description:  "System ACL to permit all access"
  participant: "ANY"
  operation: ALL
  resource: "org.hyperledger.composer.system.**"
  action: ALLOW
}

~~~~

## Save your changes to permissions.acl.

## Step Three: Generate a business network archive

Now that the business network has been defined, it must be packaged into a deployable business network archive (.bna) file.

Using the command line, navigate to the tutorial-network directory.

From the tutorial-network directory, run the following command:

~~~~
composer archive create -t dir -n .
~~~~

After the command has run, a business network archive file called tutorial-network@0.0.1.bna has been created in the tutorial-network directory

## Step Four: Deploying the business network

~~~~

composer network install --card PeerAdmin@hlfv1 --archiveFile tutorial-network@0.0.1.bna
~~~~

~~~~
composer network start --networkName tutorial-network --networkVersion 0.0.1 --networkAdmin admin --networkAdminEnrollSecret adminpw --card PeerAdmin@hlfv1 --file networkadmin.card
~~~~

~~~~
composer card import --file networkadmin.card
~~~~

~~~~
composer network ping --card admin@tutorial-network
~~~~

## Step Five: Generating a REST server

~~~~
composer-rest-server

~~~~
Enter admin@tutorial-network as the card name.

Select never use namespaces when asked whether to use namespaces in the generated API.

Select No when asked whether to secure the generated API.

Select Yes when asked whether to enable event publication.

Select No when asked whether to enable TLS security.~~~~


    
    
