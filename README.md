
# Kujira Testnet harpoon-4 Validator Node Setup

## Install Ubuntu 20.04 on a new server and login as root

## Install ``ufw`` firewall and configure the firewall

```
apt-get update
apt-get install ufw
ufw default deny incoming
ufw default allow outgoing
ufw allow 22
ufw allow 26656
ufw enable
```

## Create a new User

```
# add user
adduser node

# add user to sudoers
usermod -aG sudo node

# login as user
su - node
```

## Install Prerequisites

```
sudo apt update
sudo apt install pkg-config build-essential libssl-dev curl jq git libleveldb-dev -y
sudo apt-get install manpages-dev -y

# install go
curl https://dl.google.com/go/go1.18.2.linux-amd64.tar.gz | sudo tar -C/usr/local -zxvf -

```

```
# Update environment variables to include go
cat <<'EOF' >>$HOME/.profile
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
EOF

```

```
source $HOME/.profile

# check go version
go version

```

## Install Kujira Testnet Node

```
git clone https://github.com/Team-Kujira/core
cd core
git checkout v0.4.0
make install
cd
```

## Initialise your Validator node
```
#Choose a name for your validator and use it in place of “<moniker-name>” in the following command:
kujirad init <moniker-name> --chain-id harpoon-4

#Example
kujirad init My_Node --chain-id harpoon-4
```

## Download genesis.json file
```
cd ~/.kujira/config
rm genesis.json
cd

wget https://raw.githubusercontent.com/Team-Kujira/networks/master/testnet/harpoon-4.json -O $HOME/.kujira/config/genesis.json
```

## Update configuration and peers list

```
cd

sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.00125ukuji\"/" $HOME/.kujira/config/app.toml

sed -i -e "s/^timeout_commit *=.*/timeout_commit = \"1500ms\"/" $HOME/.kujira/config/config.toml

PEERS="87ea1a43e7eecdd54399551b767599921e170399@52.215.221.93:26656,021b782ba721e799cd3d5a940fc4bdad4264b148@65.108.103.236:16656,c40ad9c0446d7ecb4e991a45cbfb81a685617a55@104.248.165.123:26656,ccd2861990a98dc6b3787451485b2213dd3805fa@185.144.99.234:26656,909b8da1ea042a75e0e5c10dc55f37711d640388@95.216.208.150:53756,235d6ac8aebf5b6d1e6d46747958c6c6ff394e49@95.111.245.104:26656,b525548dd8bb95d93903b3635f5d119523b3045a@194.163.142.29:26656,26876aff0abd62e0ab14724b3984af6661a78293@139.59.38.171:36346,21fb5e54874ea84a9769ac61d29c4ff1d380f8ec@188.132.128.149:25656,06ebd0b308950d5b5a0e0d81096befe5ba07e0b3@193.31.118.143:25656,f9ee35cf9aec3010f26b02e5b3354efaf1c02d53@116.203.135.192:26656,c014d76c1a0d1e0d60c7a701a7eff5d639c6237c@157.90.179.182:29656,0ae4b755e3da85c7e3d35ce31c9338cb648bba61@164.92.187.133:26656,202a3d8bd5a0e151ced025fc9cbff606845c6435@49.12.222.155:26656,843e2db960439151cd6b87d7c3bd7aea5e2d552f@95.216.7.241:40000,5334c2ad190a10b1e16165491903a7ce37659cd8@65.108.150.72:26656,24c582547690a2c529bef10d04ffb1acd5c30556@[2a01:4f9:1a:a718::5]:26656,e843509f244087fcd86f47dbf09fdc3b8983a8be@82.65.223.126:46656,d26a03f681850e679af0fdcf646c5d4d3435e607@206.189.15.47:36346,d317a5e54e886f1c721423359732b061d213f783@162.55.27.100:26612,362468ed96561770cd32fc0f7cbd45d2e5985d38@146.19.24.243:26656,cf445792afd448d5e79946226a8d86b3a29515c0@46.101.2.221:26656,0be7dfe405e364f87bda949a6692c24b2fcb66af@37.143.129.221:26656,32cadeecf6fe6ae152187b06ed7e093d19bd663e@88.208.57.200:26656,1b1ac7b7e957e9e2d5391a38b1ba3e4e5d2f3210@135.181.146.40:26656,9243239f47e55050afcb408e15bc1eb946003686@66.94.111.40:26656,32cadeecf6fe6ae152187b06ed7e093d19bd663e@88.208.57.200:26656,2b03400fd39304b04d8801de1d321d2a05e2cf0d@[2607:5300:205:300::11a6]:26666,9470c4e3c95ee574116b60c9d97f181c8906e9b3@135.181.209.51:26876,65dd5ec89379a88a87e096d6daf1f50cde12655e@94.103.92.39:26656,fbcb66dad0de1d857cd14e579e31f8f1e5b43f53@161.97.152.78:26656,714d5eac33a3a22fd264e8603f200c20d6bac34d@185.106.94.59:26656,ca7d24539da9f2dbd84e7f2991608910d1843d2d@65.21.134.202:26616,9831c8c1d9e924944a35ee0110345c4e5a69597d@65.21.131.215:26616,debe8f29511ba472314e597adc6ca79728b82659@135.181.129.86:10856,26c2c562d2e00247f462e5e9f170aecb415e9c28@54.36.109.62:26656,0bbe1fdcabb59e381fe0e461b04a1822fb1d656a@65.108.232.150:23095,4d70f900e7ab80bcdf76c2ceace5b6abc606b83d@37.143.129.221:26656,45520598b23d1eccc6d69a8975a77b32b1b9787a@65.108.228.227:26656,4bba4fc2d07b6f8b5aa0de8668e9d10cf22f1fe8@65.108.229.56:26656,030f65339defb01b0e3ddaeaa54cbeac00dd0c74@185.169.252.239:26656,f559d66add4e0821b1996b04f8e73a948d0757bd@161.97.74.88:46656,3f26aa959f2a0f16749747c63767c1e491275cae@88.198.242.163:26656,c4ddd1c13e5bde82768eebd567b58364d872c9a5@62.171.144.51:26656,d41e073286a0e7d6a647412c15e70457bbf306b3@161.97.109.47:26651,aa3643753c57f85148906a2f44fb85d3d583fe9a@49.12.224.69:26656,b47468f355cfa45dfaacfb4a2e1ab89b3a53d0b7@116.203.233.112:26656,20f0409359fa455d7ca11b81039782a260899579@195.201.164.223:26656,6266f0d84628f63418d857210bf4a0f20cc0bf1b@38.242.194.202:26656,118748411e54f3ebc82f8dc5339d58debc16c07a@209.145.57.201:26656,50327ddd0bb5f75c78164147957af60eee527c0a@95.31.16.222:26656,30ec2afd7674ebf8fab23e574e9f2da48dd15720@54.152.89.174:26656,5563a1953e39847ec9c77e8fd15b621ed960d34e@195.3.221.131:26656,0c899c47a1f0726a7edcc4a40d1681ffaecbcf3a@65.108.225.158:18656,107ab132ca930846d9e00bafa570e2481c103aae@217.125.110.171:26656,1dcfb8684153c711dcc4a7b65043f515d1293ce7@34.125.145.211:26656,8ab570a865fc3259a95b780e1af7d111685ca6af@149.102.145.138:26656,70fc0c23dac3788163e4b427bd42d66618c15fcd@135.181.249.202:26656,4a3a0e62750c959ff4e9569461d32a91d9fc5e04@65.108.233.175:26656,8a11c650e5233ae6a88c825a1ee6f8c46ff81a77@148.251.64.113:26656,5c11f15e62e408c6567847ad63891e81fa5ff03c@65.21.132.27:23357,25319f7aa3849f797c0aeef7e7406a3ddd4b50a9@198.244.200.116:26656,2cc485f4b9fa6651a828a6dd3373f0d2bc05b64f@65.21.201.244:26876,c57b8b5ce39260e75e38d048424c9417ac26c1cf@testnet.kujira.synergynodes.com:26656,4d35d83c24f231b18116820d3f955fc708205e47@173.249.1.72:26656,84439e87d049d8ad8eb59f30a68049ba46f6ac79@38.242.199.201:26656,9a3a074d6663b4bcb0b7ebfa78c6da15fd5a29e0@178.62.224.167:26656,ab3691fe63863f5575291a49da27f1070a08db0f@207.154.237.111:26656,86093241c8b102a6fe2f9a0836adab46bc078b6e@213.52.129.168:26656,6468b1b110212b1e4c848c2e0247046d890beeb9@165.22.122.161:26656,d12a0a634ba162e575faacbbc5955c94ba771df4@164.92.157.234:26656,0afe45bae237174154169965c3867a6934110926@138.68.83.32:26656,c20a6a1263693587ab6b6b5d19aafb71713dc1f2@46.101.75.158:26656,ac10457fcf3a4fea21d80d3a1e04093b33712dcc@146.190.21.32:26656,239b6d1544eca6129456f28ff47d824c297cbdf0@146.190.230.242:26656,b3e788a0761ef1dfb4969454062c3576de538259@148.251.64.113:26656,2381d4ba4d35b166cca9e0c5acf2bf6fef160ad2@34.91.242.163:26656,7868de11cd356bd31cfd8f944011f4a45e413a13@34.65.132.136:26656,c7c8068ffe6a07a303beac60ce0a976d3a0b195b@95.217.118.100:32656,e58ff20f91f84020329705bab366a96f9b8b42a1@161.97.183.184:26656,092617bcf66bc91118225b7794fd0acf4202d92a@107.181.244.202:26656,1611f897b14ac74a6a40d75317fdf7c90a8e8d39@74.207.244.63:26656,d708ca720ce30fe69f503d59e7466446a1b950e2@188.3.55.38:26656,72b46cc7e431996de21e7b2226177c10d5767d68@164.92.254.98:26656,91a4f32aa7e997b0d1a3a0b157db3c9523a4224e@188.166.7.27:26656,0fad9b2310f32735f1c8abeddd9570f9c383d86a@20.239.91.55:26656,ec737be748c5dcf89408be985ac32638e43c4a11@88.208.226.6:26656,cdaa8f8fd06d2f25127ddf6c43f9e14d6d7e1368@185.205.244.117:26656,2cf5e72aefb1389d5901597e7047705937cef57f@128.199.45.77:26656,0afe45bae237174154169965c3867a6934110926@138.68.83.32:26656,3d207046e1410f67852908b8a11585d463448f11@135.181.157.37:26656,a1bf10f41c330a16221f518512fcb34e76482295@148.251.64.113:26656,f275dd026e678e7151b86f1e6993681ecc225edb@5.189.144.237:26656,a20a7646e0124dbdf1ec4e4518d9e2cee33a77de@35.232.129.73:26656,920cb1518c5e1c692de9a48fc07828f5ded40da4@38.242.237.80:26656,c97a8014f0d8fd5f45862345ec0c3c52132bc8c8@167.172.173.147:26656,df9d1a9d42ccfed063af0f64d96b804436fc6a1e@188.166.60.128:26656,7e88c54dacbf76b907c6fa99612fb461fe2652fd@34.168.66.77:26656,7868de11cd356bd31cfd8f944011f4a45e413a13@34.65.132.136:26656,c4daa0be57a50e02741e6b0727ec25fa2b04dfcd@109.107.184.70:26656"

sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.kujira/config/config.toml
```

## Start the node and let it Sync
```
kujirad start

# Press Ctrl+c to exit

```

You should see the following output.

```
Genesis time is in the future. Sleeping until then... genTime=2022-06-13T12:00:00Z
```

## Running the validator as a systemd unit
```
cd /etc/systemd/system
sudo nano kujirad.service
```
Copy the following content into ``kujirad.service`` and save it.

```
[Unit]
Description=Kujirad Daemon
#After=network.target
StartLimitInterval=350
StartLimitBurst=10

[Service]
Type=simple
User=node
ExecStart=/home/node/go/bin/kujirad start
Restart=on-abort
RestartSec=30

[Install]
WantedBy=multi-user.target

[Service]
LimitNOFILE=1048576
```

```
sudo systemctl daemon-reload
sudo systemctl enable kujirad

# Start the service
sudo systemctl start kujirad

# Stop the service
sudo systemctl stop kujirad

# Restart the service
sudo systemctl restart kujirad


# For Entire log
journalctl -t kujirad

# For Entire log reversed
journalctl -t kujirad -r

# Latest and continuous
journalctl -t kujirad -f
```

## Execute the folloiwng command to get the node id

```
kujirad tendermint show-node-id
```

## Create a Wallet for your Validator Node

Make sure to copy the 24 words Mnemonics Phrase, save it in a file and store it on a safe location.

```
kujirad keys add <wallet-name>

#Example
kujirad keys add my_wallet

```

## The following applys after the network goes live on 13th June 2022

Get some KUJI tokens to the above created wallet.


## Create and Register Your Validator Node
```
kujirad tx staking create-validator -y \
  --chain-id harpoon-4 \
  --moniker <moniker-name> \
  --pubkey "$(kujirad tendermint show-validator)" \
  --amount 5000000ukuji \
  --identity "<Keybase.io ID>" \
  --website "<website-address>" \
  --details "Some description" \
  --from <wallet-name> \
  --gas-prices 1ukuji \
  --commission-rate=0.0 \
  --commission-max-rate=0.05 \
  --commission-max-change-rate=0.01 \
  --min-self-delegation 1
  
#Example

kujirad tx staking create-validator -y \
  --chain-id harpoon-4 \
  --moniker "My Node" \
  --pubkey "$(kujirad tendermint show-validator)" \
  --amount 5000000ukuji \
  --identity "D74433D32938F013" \
  --website "http://www.mywebsite.com" \
  --details "Some description" \
  --from my_wallet \
  --gas-prices 1ukuji \
  --commission-rate=0.0 \
  --commission-max-rate=0.05 \
  --commission-max-change-rate=0.01 \
  --min-self-delegation 1
  
```

## Get Validator Operator Address (Valapor Address)

Make sure to change ``<wallet-name>`` to correct values.

```
kujira keys show <wallet-name> --bech val --output json | jq -r .address

#Example
kujirad keys show my_wallet --bech val --output json | jq -r .address
```

## Delegate ``KUJI`` to Your Node
```
kujirad tx staking delegate <validator-address> 1000000ukuji --from <wallet-name> --fees 30000ukuji --chain-id harpoon-4 -y

#Example
kujirad tx staking delegate kujiravaloper1xesqr8vjvy34jhu027zd70ypl0nnev5esqejlw 1000000ukuji --from my_wallet --fees 30000ukuji --chain-id harpoon-4 -y
```
## Backup Validator node file

Take a backup of the following files after you have created and registered your validator node successfully.

```
/home/node/.kujira/config/node_key.json
/home/node/.kujira/config/priv_validator_key.json
/home/node/.kujira/data/priv_validator_state.json
```

## Withdraw Rewards

Make sure to change ``<validator-operator-address>``, ``<wallet-name>`` to correct values.

```
kujirad tx distribution withdraw-rewards <validator-address> --from <wallet-name> --chain-id=harpoon-4 --gas auto --fees 1ukuji --gas-adjustment 1.4 -y

#Example
kujirad tx distribution withdraw-rewards kujiravaloper1xesqr8vjvy34jhu027zd70ypl0nnev5esqejlw --from synergy --chain-id=harpoon-4 --gas auto --fees 1ukuji --gas-adjustment 1.4 -y
```

## Check Balance of an Address

```
kujirad query bank balances <wallet-address>

#Example:
kujirad query bank balances kujira1xesqr8vjvy34jhu027zd70ypl0nnev5eh42prp
```
