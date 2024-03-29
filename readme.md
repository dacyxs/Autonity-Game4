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
pipx install --force git+https://github.com/autonity/aut
pipx ensurepath
```

## We have to reboot in order to run aut command. 

```
sudo reboot
```

## Edit file .autrc with below command. Change keystorenameofyours with your info;

```
nano /root/.autrc
```

```
[aut]
rpc_endpoint = https://rpc1.piccadilly.autonity.org/
keyfile=.autonity/keystore/keystorenameofyours.key
```

## Create keystore file and create account(Change keystorenameofyours with your info). You will have your autonity address when you have created the account.
<br> if you have already created your key in previous games. Change the key file in folder. 

```
cd
mkdir .autonity
mkdir .autonity/keystore
aut account new --keyfile .autonity/keystore/keystorenameofyours.key
```

OR import

```
cd
mkdir .autonity
mkdir .autonity/keystore
aut account import-private-key ./alice.priv
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
     --volume $(pwd)/.autonity/keystore/dacxysoracle.key:/autoracle/oracle.key \
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

<br><ORACLE_KEYFILE> specifies the path to your oracle server key file. E.g. ../aut/keystore/oracle.key
<br><PLUGINS_CONF_FILE> is the path to the oracle server configuration file plugins-conf.yml. E.g. ./plugins-conf.yml.
<br><ORACLE_CONTAINER_NAME> is the name you are specifying for the container, i.e. oracle-server-bakerloo or oracle-server-piccadilly
<br><DOCKER_IMAGE> is the Docker image name, i.e. ghcr.io/autonity/autonity-oracle-bakerloo or ghcr.io/autonity/autonity-oracle-piccadilly
<br><PWD> is the password to your oracle server key file
<br><WS_ADDRESS> is the WebSocket IP Address of your connected Autonity Go Client node (e.g. “ws://172.17.0.2:8546”, see install Autonity, networks ).
<br>See the Autonity Oracle Server command-line reference for the full set of available flags.

## Add autonity to metamask to send atn to oracle account. Send 0.01 ATN. Then reveal private key of your oracle account.
<br>Network Name: Autonity Piccadilly (Barada) Testnet
<br>New RPC URL: https://rpc1.piccadilly.autonity.org/
<br>Chain ID: 65100001
<br>Currency symbol: ATN
<br>Block explorer URL: https://piccadilly.autonity.org/


```
cd .autonity/keystore
nano oracle.key
```
Write you private key in file. 

#Change your autrc as below.

```
[aut]
rpc_endpoint = http://0.0.0.0:8545/
keyfile=.autonity/keystore/keystorenameofyours.key
```

# Onboard validator<br>
# Register as a Validator<br>

<TREASURY_ACCOUNT_ADDRESS> is address of your first key created. Change it below. 

```
cd

docker run -t -i \
  --volume /root/autonity-chaindata:/autonity-chaindata \
  --volume $(pwd)/autonity-chaindata/keystore/oracle.key:/oracle.key \
  --name autonity-proof --rm ghcr.io/autonity/autonity:latest \
  genOwnershipProof \
  --nodekey /autonity-chaindata/autonity/nodekey \
  --oraclekey /oracle.key \
  <TREASURY_ACCOUNT_ADDRESS>
```
Output will be your signature hex. Save it to use later.

# Get your enode. by running below command.

```
aut node info
```

aut validator register ENODE ORACLE_KEY Signature_Hex | aut tx sign - | aut tx send -

  
Once the transaction is finalized (use aut tx wait <txid> to wait for it to be included in a block and return the status), the node is registered as a validator in the active state. It will become eligible for selection to the consensus committee once stake has been bonded to it.

## Confirm registration
```  
aut validator list
```
  
Confirm the validator details using:

```
aut validator info --validator $VALIDATOR
```

## Create new keyfile from nodekey to sign new message validator onboarded. Second step change newkeyfilename with the filename created by first step below. Use created signature in the form.
```
aut account import-private-key autonity-chaindata/autonity/nodekey
```
  ```
  aut account sign-message "validator onboarded" --keyfile .autonity/keystore/<newkeyfilename>
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

##Bond your ntn(change <amount> with the desired amount to bond.

```
ENODEURL=$(aut node info -r http://127.0.0.1:8545 | grep enode | awk '{print $2}' | tr -d ,'"')
VALIDATOR=$(aut validator compute-address $ENODEURL)
aut validator bond --validator $VALIDATOR <AMOUNT> | aut tx sign - | aut tx send -
```

## If your point is not increasing. Check your status. Your status should be 0. If not your jail release block should be passed. 
```
aut validator info --validator $VALIDATOR
```

Write below command in order to avtivate your validator.
```
aut validator activate --validator <VALIDATOR_IDENTIFIER_ADDRESS> | aut tx sign - | aut tx send -
```



## Fund the account
https://faucet.autonity.org/
![image](https://user-images.githubusercontent.com/106930902/233856072-0cbeafb5-bd48-4b1a-b092-0a5d2c458346.png)

## Register to game from the below link;
https://game.autonity.org/getting-started/register.html

## Onboard Validaotr from the below link;
https://game.autonity.org/awards/register-validator.html

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

## Youtube video 
https://www.youtube.com/watch?v=cJJS0KDwyBo

## Follow me @
https://twitter.com/Dacxys

