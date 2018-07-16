# Hyperledger-Fabric-Install
Steps I took to install Hyperledger Fabric on Ubuntu, which may one day become automated... maybe.

## I am using **Ubuntu 16.04 LTS**

```
### Update the package index
sudo apt-get update

### Install curl and packages to setup Docker repository for the usual Docker install method see https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce
sudo apt-get install curl apt-transport-https ca-certificates software-properties-common

#### Add Docker gpg key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

#### Verify the gpg key
sudo apt-key fingerprint 0EBFCD88

#### Add Docker repository
sudo add-apt-repository   "deb [arch=amd64] https://download.docker.com/linux/ubuntu    $(lsb_release -cs)  stable"

#### Update the package index again
sudo apt-get update

#### Install the latest stable version of Docker (note: Hyperledger requires 17.06.2-ce or newer)
sudo apt-get install docker-ce

#### Check Docker version to ensure it's installed
docker --version

#### Download Docker Compose
sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose

#### Apply executable permission to the Docker Compose binary
sudo chmod +x /usr/local/bin/docker-compose

#### Check Docker Compose version to ensure it's installed
docker-compose --version

### Download and setup Go programming language (check for latest stable release on https://golang.org/dl/ and edit the *.tar.gz bit accordingly on the commands below
curl -O https://storage.googleapis.com/golang/go1.10.3.linux-amd64.tar.gz
sha256sum go1.10.3.linux-amd64.tar.gz
tar -xvf go1.10.3.linux-amd64.tar.gz
sudo mv go /usr/local

#### Open profile to setup Go related paths for the user profile
sudo vim ~/.profile
 
#### Add following lines to ~/.profile file
export GOPATH=$HOME/go
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
 
#### Reload the profile file
source ~/.profile

### Install Node.js (here we install required Ubuntu packages and build Node.js from source since Hyperledger 1.2 only supports Node.js versions 8.9+ but < 9)

#### Update the package index again and install necessary Ubuntu packages for Node.js
sudo apt-get update
sudo apt-get install build-essential openssl libssl-dev pkg-config

#### Go to the src directory, create a node directory, and go into it
cd /usr/local/src
sudo mkdir node
cd node
#### Download Node.js (here we download version 8.9.3 due to limitations of Hyperledger 1.2)
sudo wget https://nodejs.org/dist/v8.9.3/node-v8.9.3.tar.gz

#### Extract the tar file and go into the newly created node-v*.*.* directory, configure the directory, run make, and make install
sudo tar zxvf node-v8.9.3.tar.gz
cd node-v8.9.3
sudo ./configure
sudo make
sudo make install

#### Check Node.js and NPM versions to ensure they're good and correct
node --version
npm --version

#### Upgrade NPM version as per instructions on http://hyperledger-fabric.readthedocs.io/en/release-1.2/prereqs.html
sudo npm install npm@5.6.0 -g

### Python 2.7.12 should already be installed on a fresh Ubuntu 16.04 LTS, but just in case, install Python (ensure we're only using version 2.7x and not Python 3)
sudo apt-get install python

## Install Fabric Samples
### Personally I create a hyperledger directory in /usr/local/ and within that a fabric-samples directory and install them there
cd /usr/local
sudo mkdir hyperledger
cd hyperledger
curl -sSL http://bit.ly/2ysbOFE | sudo bash -s 1.2.0

### You may want to add that to your PATH environment variable so that these can be picked up without fully qualifying the path to each binary. e.g.: export PATH=<path to download location>/bin:$PATH so in this case export PATH=/usr/local/hyperledger/fa
bric-samples/bin:$PATH

# (If you want) Create a genesis block for your network
cd /path/to/fabric-samples/first-network
sudo ./byfn.sh generate
## See http://hyperledger-fabric.readthedocs.io/en/release-1.2/build_network.html for more
```
