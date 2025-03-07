<p align="center">
  <img height="100" height="auto" src="https://raw.githubusercontent.com/Nodeist/Kurulumlar/main/logos/androma.png">
</p>



# Andromaverse Node Installation Guide
Feel free to skip this step if you already have Go and Cosmovisor.


## Install Go
We will use Go `v1.19.3` as example here. The code below also cleanly removes any previous Go installation.

```
sudo rm -rvf /usr/local/go/
wget https://golang.org/dl/go1.19.3.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.19.3.linux-amd64.tar.gz
rm go1.19.3.linux-amd64.tar.gz
```

### Configure Go
Unless you want to configure in a non-standard way, then set these in the `~/.profile` file.

```
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
```


### Install Cosmovisor
We will use Cosmovisor `v1.0.0` as example here.

```
go install github.com/cosmos/cosmos-sdk/cosmovisor/cmd/cosmovisor@v1.0.0
```

## Install Node
Install the current version of node binary.

```
git clone https://github.com/AndromaverseLabs/testnet androma
cd androma
git checkout v1
make install
```

## Configure Node
### Initialize Node
Please replace `MONIKERNAME` with your own moniker.

```
acred init MONIKERNAME --chain-id androma-1
```

### Download Genesis
The genesis file link below is Nodeist's mirror download. The best practice is to find the official genesis download link.

```
wget -O genesis.json https://snapshots.nodeist.net/t/androma/genesis.json --inet4-only
mv genesis.json ~/.androma/config
```

### Configure Peers
Here is a script for you to update `persistent_peers` setting with these peers in `config.toml`.
```
PEERS=4d6e5790be281a584c9226749a4d09ce14fabd02@65.108.194.40:32656,d7fc790e005d7cdc0700d7ea527a5ee5131a49aa@199.175.98.101:14656,0d14834f1f03b2193655e7c48512cc1a913e6778@65.109.85.170:36656,1a9279f3c3694a022b53de960db553509cb63016@65.108.75.32:31656,600410eead9d886603399808ed741ea03ee34c58@3.138.138.247:26656,f84b097aac2b96531f1fc77e1797ec08de31b70f@71.210.105.42:26656,4b503a2cb992ce83e8e2516fc59cafc662ec0566@97.70.169.114:26656,13732dc7250f22c5234fe751a49e65ccbf7d76b9@193.34.212.41:11564,6dbdf310876528a45e0f094df1160439f33a1bcf@65.109.87.135:10656,91efe71a306d0f16e51cca4ec5280bedde58a1e8@95.217.118.96:26989,152e12336f6b39ee9ce1bbb16edfe647ba4dd4d6@65.109.92.241:4176,7fd141d0e3dfd2a06adcf7b2679d7ce627ba2334@217.13.223.167:26656,62cc3e126e677c28f2f4ad7ae0d3523366f09ba8@185.237.253.211:26656,bbdc4d1e566a516e07b21b38ce43cf9e31026120@213.239.216.252:24656,a1658db741521f6af00d2f00073cb95ad732ba27@176.241.136.91:26656,401c3d7ce63bb8691d7a3ee2781fad96294bdca8@173.249.31.88:26656,e398049084b0e136c3bad7ad5f5b245bf216f385@161.97.109.47:26656,5ea3936c216086937677764fbf4a2326fdb7fc6f@185.182.184.200:36656,117950b24f349308840b245f62b9325cc09dcbc2@65.109.112.20:11024,44836c8b359b7cf14e8211b00afbb24e1f9d50c1@65.108.2.41:14656,b4dc2ce669832b3492b95a31ba6db1b4f1b26d8a@209.145.53.163:23656,cf99fc36374b48aad8c3e5acfd4e1963da853fae@65.109.17.86:38656,6abdc27d2ad8b4c600b58923985f133d7e66bf6d@65.108.126.46:37656,9d772cddf053b7e3ffe898e7d37be6aeb3e007db@94.130.16.254:60956,76b1343da5f76dcbef3c50c49f2811eab95129cf@65.108.195.235:23656,fc6f7914e4beb4b5278e7ba32ec2abde97cd8082@65.109.28.177:26656,93967b81ee0048c0979797068f4cc4f8944badac@95.217.207.236:27786,e46205b54bfe73483112624e6f8c8eb0bf4e8d69@149.102.147.157:56656,6ca9cc12c3448b22fc51f8ba11eb62b7cb667f04@65.108.132.239:26856,9943fed25f830a8c0eaa63efa9e637c1875bfdc8@38.242.219.158:26656,e2cd45459c798835d914dc68a166b3e18bff1bf7@198.244.179.62:27786,ce066724e8be693114dea2c1187786a4da62dd82@65.108.66.34:27786,5dff05a07a00df76f4632469942af5a4fb493a45@5.75.162.210:14656,753b41aa2817b0d26dcf063702588683ae6ce0cb@65.109.28.219:15056,ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@176.9.82.221:15056
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.androma/config/config.toml

```

## Launch Node
### Configure Cosmovisor Folder
Create Cosmovisor folders and load the node binary.

```
# Create Cosmovisor Folders
mkdir -p ~/.androma/cosmovisor/genesis/bin
mkdir -p ~/.androma/cosmovisor/upgrades

# Load Node Binary into Cosmovisor Folder
cp ~/go/bin/andromad ~/.androma/cosmovisor/genesis/bin
```

### Create Service File
Create a `andromad.service` file in the `/etc/systemd/system` folder with the following code snippet. Make sure to replace `USER` with your Linux user name. You need `sudo` previlege to do this step.

```
[Unit]
Description="andromad node"
After=network-online.target

[Service]
User=USER
ExecStart=/home/USER/go/bin/cosmovisor start
Restart=always
RestartSec=3
LimitNOFILE=4096
Environment="DAEMON_NAME=andromad"
Environment="DAEMON_HOME=/home/USER/.androma"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="UNSAFE_SKIP_BACKUP=true"

[Install]
WantedBy=multi-user.target
```

### Start Node Service
```
# Enable service
sudo systemctl enable androma.service

# Start service
sudo service andromad start

# Check logs
sudo journalctl -fu andromad
```

# Other Considerations
This installation guide is the bare minimum to get a node started. You should consider the following as you become a more experienced node operator.



> Configure firewall to close most ports while only leaving the p2p port (typically 26656) open

> Use custom ports for each node so you can run multiple nodes on the same server

> If you find a bug in this installation guide, please reach out to our [Discord Server](https://discord.gg/yV2nEunsTY) and let us know.
