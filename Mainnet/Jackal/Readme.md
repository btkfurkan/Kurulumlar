<p align="center">
  <img height="100" height="auto" src="https://raw.githubusercontent.com/Nodeist/Kurulumlar/main/logos/jackal.png">
</p>



# Jackal Node Installation Guide
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
cd $HOME
git clone https://github.com/JackalLabs/canine-chain
cd canine-chain
git checkout v1.2.0
make install
```

## Configure Node
### Initialize Node
Please replace `MONIKERNAME` with your own moniker.

```
canined init MONIKERNAME --chain-id jackal-1
```

### Download Genesis
The genesis file link below is Nodeist's mirror download. The best practice is to find the official genesis download link.

```
wget -O genesis.json https://snapshots.nodeist.net/jackal/genesis.json --inet4-only
mv genesis.json ~/.canine/config
```

### Configure Peers
Here is a script for you to update `persistent_peers` setting with these peers in `config.toml`.
```
PEERS=84abbfb6912262fa129ff65e4b56107820a00d65@135.181.214.121:31656,ad8afbc89ac64db1ee99fdd904cbd48876d44b7d@195.3.222.240:26256,2ec46ff04ebfafc19f505feaaf00943c15bb2757@185.16.38.149:26656,a877c11ecef83401dcc96c4499874ebc3f13367b@116.202.36.240:10756,8d59eb5f7ad207e59c06620f6e9e7b6760b56211@65.108.75.107:18656,0841db0ae5e5443905837e196d2e1ffd31f2e480@131.153.202.81:36656,26b6255375a592c3b0664bd474a6975f468c3785@88.99.164.158:11126,68eb09cb9c5a2b136e8c693a48bcb26d9108062f@157.90.2.254:26656,ecb163fca7436befa3a5694a7d558e89d3f04b2c@65.109.29.150:17656,e5a142be860ee9b2f5c71d813e39fceb12cbd218@78.46.78.83:26686,ff94a29e02de8369faf37c76d3c97684bbd51bd6@185.16.38.165:17556,a79da224ad9d4501dbf1d547986ebec55d56b951@135.181.128.114:17556,a13b5c78c65b785f4189a7873015c47217f2c83c@65.108.13.185:27565,544b3f5dbb772e086a7cceea376d19f9d31f4d91@15.204.143.216:26656,68205c025ec65bf4d4183691d19d15b0a72221ec@65.108.42.185:26656,753d35e39ad1f6f2fbf0f406a0c4f2bee3c4c7d0@135.181.153.228:56656,2a55d2e6cc5fa2dda8a484ab7d00f77f076d237f@141.95.47.216:26656,7751d16cfa48da0a5bea6f40e9bcc386b4c76c50@51.89.7.184:26638,f6aaf53be76e005f83376ceca6d26d30ac93d42c@46.4.81.204:33656,dbbd1e102b9d0cde827cd272205fa3a2886a6b2c@5.9.147.22:21656,fc905fe58d36875a833202ce53759d0ae6c11435@141.95.65.26:48656,83d66a37202785b09aee4e3ae1b50d2ddfbf860c@162.19.89.8:10856,108652f503665772ad024d9d2129a9f4fa9ffe9b@176.9.98.24:30536,ea35106e43dcec1e5c66319272da48df3dce7723@57.128.144.233:26656,4398bd773ac885b7365de3604eb487be10c54563@185.16.38.210:26906,b55e7c342620b73cbc9572604d1c2146892231d6@194.34.232.124:34656,9c149b35243970e1f8e0519f1f33f79f7d5bd91b@51.38.52.188:26638,ad41936e5f89b119fdaae25fef0652949770f06e@185.107.57.74:26656,0faa7f1099de2e02deebe09fcb52863056333265@144.202.72.17:26616,ef8c470a03f3753df53dad15a435f99d6869f6a7@51.81.107.95:10856,159834da1073b793a9f6730841d827802051ed75@198.244.178.213:26656,e98ed884751f26b98bc32d4469efd53b3507129f@15.235.114.194:10756,8be44995ab4eeafcde6e0a9e196c40d483ef6d2a@51.81.155.97:10556,55df88ae25223565af42ccd6b3b558b8e70bba31@213.239.216.252:26656,46d4495643f2579573a61e181a88de3b8f0acc4f@2.139.23.24:36656,0836e6f18a67cc6139e315f024189cb8a84f3121@95.217.0.158:26656,e2172f53b4c59ed157d97802dc6b5ae8b17d3bb1@109.236.81.221:46656,399068f8371dce4ae5d7cd7da2c965e765e68f4b@65.108.238.102:17556,db9c7d34cd04e155b3eed730f68fc9315245cf5c@65.108.124.219:30656,6cae04d59dedda3278abb0b31cccbc8bd73794c4@65.108.41.173:27186,e08efc0b0e15e4d8eacf0f4ed5e52f6e9bdc312d@144.76.97.251:36156,1f30e644ddd8edf310cbd9be4ac07b604eed581e@66.85.143.242:26676,588e509e3a8c1dc4ba938779bf569cd9f6f0f4be@212.23.222.109:26256,316864671ec9566a3d07b64040c45e3fc75ccf36@65.108.201.154:5020,f42498ca4d9e62f95115f04ae18fa5ec1c1487f1@65.108.141.109:18656,01ab8944f1d486f8b3682a457a020dd7c386cc16@185.215.166.126:26656,88130f394f62dc17b1960b5e2f50a0f18a7a7499@88.99.213.25:37656,637166728d6103ad4ec9fff97a321a024bff3e58@65.109.94.221:28656,519f2b648a2a8794ac33b195f39b6d836e09f8f2@131.153.154.13:26656,6852add4eaa027707a6000c78ea9e7cde81b058f@18.118.26.4:26656,4118b172fc2a45e0335c59641fd7c2e5e5e2c53c@65.108.238.203:28656,ebc272824924ea1a27ea3183dd0b9ba713494f83@95.214.52.139:26906,bc6ce122e5809b06dcf90742ee40091f3ee6bcee@142.132.248.253:42656,e272f855eb99975dbd23bfc52dce9ff9661596ff@65.109.60.54:37656,dc579f845ae894cdbe3ab19f1b52387f3d5b681d@23.88.69.167:27211,d39fecbc409541de13fa644d90066d4dabe08262@95.165.89.222:24475,c5b43622ecd7413dd41905f6f8f5b5befd299ced@65.109.65.210:32656,5a4d1a83c877dd5db378ef5f897824273c2d4beb@141.95.72.198:36656,d9abd1dd5bf7c57461f0476c61e28bac879430a2@141.94.109.71:10556,57d82676ab660e8e4471664d7fee18e3e2e3dd19@89.58.38.59:26656,e61861653d42ebe5d7bf46d4c61f3753091985cd@83.53.221.249:36656,c2842c76779913e05fa4256e3caab852e1782951@202.61.194.254:60756,24d557203af1734d8a9e94d1819f0920ee66845c@185.252.235.83:27656,039a1c4f438c1ecc2dd901e7316d16fdafadfdab@104.193.254.36:27656,f90a64a0a3f3c0480360e0fe5dd0f806d7741558@207.244.127.5:26656,a77da5b3ce86a5226bae6e7b87964dd4efe8fe46@65.21.170.3:31656,c0b6d010bb442ff6511bc6fdde1f319b8a3a3bdc@65.108.127.50:17556,271625e66eed066b35e8e7c84a0bf62c3b0429eb@155.133.22.8:23856,d0313585956c8e7969993c1577f4969739b19bb7@85.10.238.147:26656,9bcaee1ad957fa75f60a6dd9d8870e53220794a9@104.37.187.214:60756,289c3e984194ac2ccaa74e201147010648e90970@195.3.223.108:26656,0985977a794b298e7ef990fe344d572c60c453b1@172.105.72.158:26656,ff7ab7fdac43752163f141809b61c67eba837cb4@65.108.97.58:37656,8c6eae80747ae0a45befcece5170d23f432a2fb1@51.89.224.199:26656,dd7e72f0a71476e51c0a601a40d6fc02a1ae1a95@65.108.6.45:60856,4963e1c374624d2c625bdb89821ed0e7290c835b@152.32.185.156:26626,5745d29dd5b49009f405e21913a474a23f1e40ec@131.153.57.230:43656,fd249b5e15e8b89771c89f66c4dce4abcabfe331@136.244.29.116:27656,1f7506f1773de3bc12642f5760e016290384a16a@89.58.32.57:37656,173c43436e2287f3660c344a5fd2386da4a61968@65.109.92.241:11126,7574e0ab179fc6cc47ac89284f4641790218540e@18.163.165.245:26626,b3f167a06a8691d738de5fff2b3ba65053e0787d@65.21.183.76:26656,e1d47393788e5f82847e677af0ec5893ed1391aa@65.108.235.209:13656,460cf6a14f3fa0f3882400fbdcb80033105cac79@178.154.241.46:26656,6fb595ce8c3a58ce4498537ddfe5333f36a24957@38.242.250.7:26646,a2afb42b65da7013eca54778ce01dfb877c2a82a@154.12.227.132:37656,39b55b1c49ad0994bbead006be40d9c84b0bf2d4@78.107.253.133:28656,dd3cab79ffae0aed4f519503b66e9403c69eeb14@85.237.193.101:25565,8f68e41b8df40ea1f30ae2cae707bcc07f2da57f@51.79.27.21:14656,2747cd770717937021e66d3da8b730c666d74ae6@65.109.93.152:36156,f460d33619705cb145d88631115a0b5581515060@165.232.173.74:26656,ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@135.181.5.219:17556,400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@65.109.88.38:37659,94b63fddfc78230f51aeb7ac34b9fb86bd042a77@46.4.53.94:30561
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.canine/config/config.toml
```

## Launch Node
### Configure Cosmovisor Folder
Create Cosmovisor folders and load the node binary.

```
# Create Cosmovisor Folders
mkdir -p ~/.canine/cosmovisor/genesis/bin
mkdir -p ~/.canine/cosmovisor/upgrades

# Load Node Binary into Cosmovisor Folder
cp ~/go/bin/canined ~/.canine/cosmovisor/genesis/bin
```

### Create Service File
Create a `canined.service` file in the `/etc/systemd/system` folder with the following code snippet. Make sure to replace `USER` with your Linux user name. You need `sudo` previlege to do this step.

```
[Unit]
Description="canined node"
After=network-online.target

[Service]
User=USER
ExecStart=/home/USER/go/bin/cosmovisor start
Restart=always
RestartSec=3
LimitNOFILE=4096
Environment="DAEMON_NAME=canined"
Environment="DAEMON_HOME=/home/USER/.canine"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="UNSAFE_SKIP_BACKUP=true"

[Install]
WantedBy=multi-user.target
```

### Start Node Service
```
# Enable service
sudo systemctl enable canined.service

# Start service
sudo service canined start

# Check logs
sudo journalctl -fu canined
```

# Other Considerations
This installation guide is the bare minimum to get a node started. You should consider the following as you become a more experienced node operator.



> Configure firewall to close most ports while only leaving the p2p port (typically 26656) open

> Use custom ports for each node so you can run multiple nodes on the same server

> If you find a bug in this installation guide, please reach out to our [Discord Server](https://discord.gg/yV2nEunsTY) and let us know.
