<h1 align="center"> Autonity - Piccadilly Games R4 Guide </h1>

![image](https://user-images.githubusercontent.com/106930902/233866088-85068fd1-a996-499e-9737-51cab182046b.png)

Start: December 13th 2023 00:00:00 UTC,<br>
End: January 31st 2024 00:00:00 UTC (7 week duration)7 weeks<br>
Theme: Node infrastructure<br>
In order to get more info about tasks, visit: [https://game.autonity.org/](https://game.autonity.org/round-4/)

# Requirements

## Hardware 

To run an Autonity Go Client node, Autonity recommends using a host machine (physical or virtual) with the following minimum specification:

Requirement	At least	              Recommended<br>
OS	        Ubuntu 20.04 LTS	      Ubuntu 20.04 LTS<br>
CPU	        1.90 GHz with 4 CPU’s	  1.90 GHz with 4 CPU’s<br>
RAM	        4GB	                    8GB<br>
Storage	    40GB free storage       60GB free storage<br>

## Network 
A public-facing internet connection with static IP is required. Incoming traffic must be allowed on the following:

TCP, UDP 30303 for node p2p (DEVp2p) communication.
TCP 8545 to make http RPC connections to the node.
TCP 8546 to make WebSocket RPC connections to the node.
TCP 6060 to export Autonity metrics (recommended but not required)

Check if the port is already open by running the following command(Replace <port_number> with the number of the port you want to check.):
sudo netstat -tuln | grep <port_number>

If the port is not already open, you can open it by modifying the firewall settings using iptables. For example, to open port 80 for HTTP traffic, you can run the following command:
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT

If you have a firewall management tool such as ufw (Uncomplicated Firewall) installed, you can use it to open the port instead. For example, to open port 80 for HTTP traffic using ufw, you can run the following commands:
sudo ufw allow 80/tcp
sudo ufw reload

Finally, you may need to restart any services that are using the port for the changes to take effect. For example, if you opened port 80 for HTTP traffic, you would need to restart your web server.
In order to open all related ports you can run below snippet;
```
sudo iptables -A INPUT -p tcp --dport 30303 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 30303 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8545 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8546 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 6060 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

sudo ufw allow 30303/tcp
sudo ufw allow 30303/udp
sudo ufw allow 8545/tcp
sudo ufw allow 8546/tcp
sudo ufw allow 6060/tcp
sudo ufw allow 22/tcp
sudo ufw reload
```

## Update the Server
```
sudo apt-get update && sudo apt-get upgrade
```

## Install the necessary packages to allow apt to use a repository over HTTPS:
```
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```

## install git 
```
sudo apt-get install git
```

## install golang
```
wget https://golang.org/dl/go1.17.4.linux-amd64.tar.gz
```
```
sudo tar -C /usr/local -xzf go1.17.4.linux-amd64.tar.gz
```
```
export PATH=$PATH:/usr/local/go/bin
```
```
source ~/.profile
```
```
go version
```

## install c compiler
```
sudo apt-get install build-essential
```
```
gcc --version
```


## Installing the Docker image
```
sudo apt install docker.io -y
```
```
sudo systemctl enable --now docker
```
```
systemctl restart docker.service
```

## Setup Python and Pip
```
sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev wget
```
```
wget https://www.python.org/ftp/python/3.9.6/Python-3.9.6.tgz
```
```
tar -xf Python-3.9.6.tgz
```
```
cd Python-3.9.6/
```
```
./configure --enable-optimizations
```
```
apt install python3-pip
```
```
pip install pipx
```
```
python3 -m pip install --user pipx
```
```
python3 -m pipx ensurepath
```
```
python3 -m pip install --user --upgrade pipx
```
```
cd
```
```
apt install python3.8-venv
```
```
cd
```

## Once the installation is complete, start the Docker service and enable it to start at boot:
```
sudo systemctl start docker
```
```
sudo systemctl enable docker
```

## Verify that Docker is installed correctly by running the hello-world container:
```
sudo docker run hello-world
```

## To limit the size of the log files, add the following to the file /etc/docker/daemon.json

```
nano /etc/docker/daemon.json
```
```
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "500m",
    "max-file": "20"
  }
}
```
Press CTRL + X Then Y to save and quit.

## Restart the Docker service to ensure the change is reflected:

```
sudo systemctl restart docker
```

## We have to reboot in order to run pipx command. 

```
sudo reboot
```

## Ensure your aut tool is installed and up to date (0.3.0.dev3):

```
pipx install --force https://github.com/autonity/aut/releases/download/v0.3.0.dev3/aut-0.3.0.dev3-py3-none-any.whl
```

## Edit file .autrc with below command. Change keystorenameofyours with your info;

```
nano /root/.autrc
```

```
[aut]
rpc_endpoint = https://rpc1.piccadilly.autonity.org/
nodekey=autonity-chaindata/autonity/nodekey
keyfile=.autonity/keystore/keystorenameofyours.key
```

## Create keystore file and create account(Change keystorenameofyours with your info). You will have your autonity address when you have created the account.
<br> if you have already created your key in previous games. Change the key file in folder. 

```
cd
mkdir .autonity/keystore
aut account new --keyfile .autonity/keystore/keystorenameofyours.key
```

##Then we have to sign-message in the form with below code in order to get private key. Change the keystorenameofyours with your specific file name. When you signed you will have signature hash.

```
aut account sign-message "I have read and agree to comply with the Piccadilly Circus Games Competition Terms and Conditions published on IPFS with CID QmVghJVoWkFPtMBUcCiqs7Utydgkfe19wkLunhS5t57yEu" --keyfile /root/.autonity/keystore/keystorenameofyours
```

##Register to the game with the following link by using your autonity key address and signature hash.

```
https://game.autonity.org/getting-started/register.html
```

##After registration you should have atn and ntn balance in your account. Check with below commands.

```
aut account balance
```

```
aut account balance -ntn
```

## pull latest docker

```
docker pull ghcr.io/autonity/autonity:latest
```

## Run Autonity (binary or source code install)

```
mkdir autonity-client
cd autonity-client
mkdir autonity-chaindata
```

## Find your IP Address

```
curl ifconfig.me
```

## First create a new screen
```
apt install screen
```
```
screen -S node
```

```
docker run \
    -t -i -d\
    --volume $(pwd)/autonity-chaindata:/autonity-chaindata \
    --publish 8545:8545 \
    --publish 8546:8546 \
    --publish 30303:30303 \
    --publish 30303:30303/udp \
    --publish 6060:6060 \
    --name autonity \
    --rm \
    ghcr.io/autonity/autonity:latest \
        --datadir ./autonity-chaindata  \
        --piccadilly \
        --http  \
        --http.addr 0.0.0.0 \
        --http.api aut,eth,net,txpool,web3,admin  \
        --http.vhosts \* \
        --ws  \
        --ws.addr 0.0.0.0 \
        --ws.api aut,eth,net,txpool,web3,admin  \
        --nat extip:<IP_ADDRESS>
```
<br> docker ps to check whether your docker running and docker logs [Container ID] to check logs.<br>

exit with CTRL + A + D ( Dont use CTRL + C)

## in order to get back to node logs, you should enter below code: 
```
screen -r node
```

## If you are able to write below code your setup is completed.
```
aut node info
```

## Install Autonity Oracle Server in your environment

```
cd
mkdir autonity-oracle && cd autonity-oracle
docker pull ghcr.io/autonity/autonity-oracle-piccadilly:latest
```

## Create account for your oracle server.

```
aut account new -k .autonity/keystore/dacxysoracle.key
```
```
cd autonity-oracle
```

##Create oracle server account keys:

## Open https://github.com/autonity/autonity-oracle/blob/master/config/plugins-conf.yml and copy them into plugins-conf.yml file. 

```
nano plugins-conf.yml
```

<br> After nano *.yml paste all from the above link. 
<br> Uncomment name: forex_currencyfreaks and 2 of the below lines. 
<br> change key by getting API key from currencyfreaks.com
<br> Press CTRL + X then Y to save your changes.

##Run Oracle Server
<br>Write docker ps to find container ID
<br>Then write docker inspect that includes your container ID to find WS address.

```
docker ps
```

```
docker inspect -f '{{.NetworkSettings.IPAddress}}' [Container_ID]
```

<br> change dacxysoracle with your oracle key.
<br> 
```
docker run \
     -t -i -d \
     --volume $(pwd)/keystore/dacxysoracle.key:/autoracle/oracle.key \
     --volume $(pwd)/autonity-oracle/plugins-conf.yml:/autoracle/plugins-conf.yml \
     --name oracle-server-piccadilly \
     --rm \
     ghcr.io/autonity/autonity-oracle-piccadilly:latest \
     -key.file="/autoracle/oracle.key" \
     -key.password="yourpassword" \
     -ws="ws://172.17.0.2:8546" \
     -plugin.dir="/usr/local/bin/plugins/" \
     -plugin.conf="/autoracle/plugins-conf.yml"
```


##where:

<ORACLE_KEYFILE> specifies the path to your oracle server key file. E.g. ../aut/keystore/oracle.key
<PLUGINS_CONF_FILE> is the path to the oracle server configuration file plugins-conf.yml. E.g. ./plugins-conf.yml.
<ORACLE_CONTAINER_NAME> is the name you are specifying for the container, i.e. oracle-server-bakerloo or oracle-server-piccadilly
<DOCKER_IMAGE> is the Docker image name, i.e. ghcr.io/autonity/autonity-oracle-bakerloo or ghcr.io/autonity/autonity-oracle-piccadilly
<PWD> is the password to your oracle server key file
<WS_ADDRESS> is the WebSocket IP Address of your connected Autonity Go Client node (e.g. “ws://172.17.0.2:8546”, see install Autonity, networks ).
See the Autonity Oracle Server command-line reference for the full set of available flags.

# Onboard validator<br>
# Register as a Validator<br>

## Step 1. Generate a cryptographic proof of node ownership 
This must be performed on the host machine running the Autonity Go Client, using the autonity genEnodeProof command:<br>
<TREASURY_ACCOUNT_ADDRESS> should be changed with your account address <br>
Get your signature hex with below command

```
SIGN_ADDR=$(autonity genEnodeProof --nodekey autonity-chaindata/autonity/nodekey <TREASURY_ACCOUNT_ADDRESS> | awk '{print $3}')
```
```
echo "Signature Address: " $SIGN_ADDR
```

## Step 2. Determine the validator enode and address 
```
ENODEURL=$(aut node info -r http://127.0.0.1:8545 | grep enode | awk '{print $2}' | tr -d ,'"')
```
```
echo $ENODEURL
```
The url is returned in the admin_enode field with echo line.
```
VALIDATOR=$(aut validator compute-address $ENODEURL)
```

Make a note of this identifier that return to you. This is the unique code for your validator. 
![image](https://user-images.githubusercontent.com/106930902/233868590-7a9c2c15-a421-4837-993c-7d87bde03b2e.png)

## Step 3. Submit the registration transaction. 

<ENODE_URL>: the enode url returned in Step 2.<br>
<PROOF>: the proof of enode ownership generated in Step 1.<br>

```
REGS_VALI=$(aut validator register $ENODEURL $SIGN_ADDR | aut tx sign - | aut tx send -)
```
```
echo "Validator Registration TX : " $REGS_VALI
```
  
Once the transaction is finalized (use aut tx wait <txid> to wait for it to be included in a block and return the status), the node is registered as a validator in the active state. It will become eligible for selection to the consensus committee once stake has been bonded to it.

## Step 4. Confirm registration
```  
aut validator list
```
  
Confirm the validator details using:

```
aut validator info --validator $VALIDATOR
```

## Step 5. Create new keyfile from nodekey to sign new message validator onboarded. Second step change newkeyfilename with the filename created by first step below. Use created signature in the form.
```
aut account import-private-key autonity-chaindata/autonity/nodekey
```
  ```
  aut account sign-message "validator onboarded" --keyfile .autonity/keystore/<newkeyfilename>
  ```








## Create a working directory for installing Autonity.

```
mkdir autonity-go-client
cd autonity-go-client
```

## Download latest stable release version of the client
```
wget https://github.com/autonity/autonity/releases/download/v0.12.2/autonity-linux-amd64-0.12.2.tar.gz
```

## Extract the file after download is completed.
```
tar -xzvf autonity-linux-amd64-0.12.2.tar.gz
```

## (Optional) Copy the binary to /usr/local/bin so it can be accessed by all users, or other location in your PATH :
```
sudo cp -r autonity /usr/local/bin/autonity
```

## Verify the installation

```
./autonity version
```
# You should have the following output
```
Autonity
Version: 0.12.2
Git Commit: 2495b37ae4aacc6505f6287cafe19cfbb0b7f17b
Git Commit Date: 20231128
Architecture: amd64
Protocol Versions: [66]
Go Version: go1.20.4
Operating System: linux
GOPATH=
GOROOT=
```


 
## Get the block number:
```
aut block height
```

## Check the auton balance of an account:
```
aut account balance <_addr>
```

## Check the newton balance of an account: 
```
aut account balance --ntn <_addr>
```

## Create account using aut. This will create key file and give you your address. 
```
aut account new
```

##Then we have to sign-message in the form with below code in order to get private key. Change the filename with your specific file name.
```
aut account sign-message "I have read and agree to comply with the Piccadilly Circus Games Competition Terms and Conditions published on IPFS with CID QmVghJVoWkFPtMBUcCiqs7Utydgkfe19wkLunhS5t57yEu" --keyfile /root/.autonity/keystore/filename
```

## Fund the account
https://faucet.autonity.org/
![image](https://user-images.githubusercontent.com/106930902/233856072-0cbeafb5-bd48-4b1a-b092-0a5d2c458346.png)

## Register to game from the below link;
https://game.autonity.org/getting-started/register.html


## Continue to create Validator after you registered for the game;
https://github.com/dacyxs/Autonity/blob/main/On_chain_tasks.md

## Original document: 
https://docs.autonity.org/
https://game.autonity.org/

## Official links of the project: 
https://discord.gg/autonity
https://twitter.com/autonity_
https://autonity.org/

## Block Explorer
https://piccadilly.autonity.org/

## Dashboard
https://validators.game.autonity.org/dashboards/?query=

## Follow me @
https://twitter.com/Dacxys

