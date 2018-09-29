# hyperledgerlab2018

https://hyperledger.github.io/composer/latest/installing/development-tools.html

## Before you begin
Make sure you have installed the required pre-requisites, following the instructions in Installing pre-requisites.


## Step 1: Install the CLI tools

Essential CLI tools:


### npm install -g composer-cli@0.20
Utility for running a REST Server on your machine to expose your business networks as RESTful APIs:

### npm install -g composer-rest-server@0.20
Useful utility for generating application assets:

### npm install -g generator-hyperledger-composer@0.20
Yeoman is a tool for generating applications, which utilises generator-hyperledger-composer:

### npm install -g yo

## Step 2: Install Playground

Browser app for simple editing and testing Business Networks:

## npm install -g composer-playground@0.20

## Step 3: Set up your IDE

## Step 4 Install Hyperledger Fabric

In a directory of your choice (we will assume ~/fabric-dev-servers), get the .tar.gz file that contains the tools to install Hyperledger Fabric:

## mkdir ~/fabric-dev-servers && cd ~/fabric-dev-servers

## curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.tar.gz
tar -xvf fabric-dev-servers.tar.gz

A zip is also available if you prefer: just replace the .tar.gz file with fabric-dev-servers.zip and the tar -xvf command with a unzip command in the preceding snippet.

Use the scripts you just downloaded and extracted to download a local Hyperledger Fabric v1.2 runtime:

## cd ~/fabric-dev-servers
export FABRIC_VERSION=hlfv12
./downloadFabric.sh

