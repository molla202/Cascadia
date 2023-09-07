# Cascadia

![image](https://github.com/molla202/Cascadia/assets/91562185/ad3e3dfe-b3dd-46a2-a49f-49a2409d603b)

Topluluk kanalları: - [Cascadia Discord](https://discord.gg/cascadia)

Exp. : - [Cascadia exp.](https://testnet.itrocket.net/cascadia)

```sh
# Upgrade ve kütüphane yüklemesi
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```
```sh
# go kurulumu yapalım
cd $HOME
! [ -x "$(command -v go)" ] && {
VER="1.19.3"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
}
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```
```sh
# varyasyonları ayarlayalım ( cüzdan adı ve moniker ismini giriniz)
echo "export WALLET="wallet"" >> $HOME/.bash_profile
echo "export MONIKER="test"" >> $HOME/.bash_profile
echo "export CASCADIA_CHAIN_ID="cascadia_6102-1"" >> $HOME/.bash_profile
echo "export CASCADIA_PORT="40"" >> $HOME/.bash_profile
source $HOME/.bash_profile
```
```sh
# binary indirelim
cd $HOME
rm -rf cascadia
git clone https://github.com/cascadiafoundation/cascadia
cd cascadia
git checkout v0.1.5
make install
```
```sh
# init işlemi
cascadiad config node tcp://localhost:${CASCADIA_PORT}657
cascadiad config keyring-backend os
cascadiad config chain-id cascadia_6102-1
cascadiad init "test" --chain-id cascadia_6102-1
```
```sh
# genesis ve addressbook indirelim
wget -O $HOME/.cascadiad/config/genesis.json https://testnet-files.itrocket.net/cascadia/genesis.json
wget -O $HOME/.cascadiad/config/addrbook.json https://testnet-files.itrocket.net/cascadia/addrbook.json
```
```sh
# seeds ve peers
SEEDS="42c4a78f39935df1c20b51c4b0d0a21db8f01c88@cascadia-testnet-seed.itrocket.net:40656"
PEERS="c6e3921222655345d8296353994e917f13a1b4a1@cascadia-testnet-peer.itrocket.net:40656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.cascadiad/config/config.toml
```
```sh
# port yapılandırması in app.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CASCADIA_PORT}317\"%;
s%^address = \":8080\"%address = \":${CASCADIA_PORT}080\"%;
s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CASCADIA_PORT}090\"%; 
s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CASCADIA_PORT}091\"%; 
s%^address = \"127.0.0.1:8545\"%address = \"127.0.0.1:${CASCADIA_PORT}545\"%; 
s%^ws-address = \"127.0.0.1:8546\"%ws-address = \"127.0.0.1:${CASCADIA_PORT}546\"%" $HOME/.cascadiad/config/app.toml
```
```sh
# port yapılandırması config.toml file
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CASCADIA_PORT}658\"%; 
s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://0.0.0.0:${CASCADIA_PORT}657\"%; 
s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CASCADIA_PORT}060\"%;
s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CASCADIA_PORT}656\"%;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${CASCADIA_PORT}656\"%;
s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CASCADIA_PORT}660\"%" $HOME/.cascadiad/config/config.toml
```
```sh
# pruning
sed -i -e "s/^pruning *=.*/pruning = \"nothing\"/" $HOME/.cascadiad/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.cascadiad/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"50\"/" $HOME/.cascadiad/config/app.toml
```
```sh
# gas ayarlaması ve prometeus
sed -i 's/minimum-gas-prices =.*/minimum-gas-prices = "0.025aCC"/g' $HOME/.cascadiad/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.cascadiad/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.cascadiad/config/config.toml
```
```sh
# servis dosyası
sudo tee /etc/systemd/system/cascadiad.service > /dev/null <<EOF
[Unit]
Description=Cascadia node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.cascadiad
ExecStart=$(which cascadiad) start --home $HOME/.cascadiad --chain-id cascadia_6102-1
Restart=on-failure
RestartSec=5
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```
```sh
# reset ve snapshot
cascadiad tendermint unsafe-reset-all --home $HOME/.cascadiad
if curl -s --head curl https://testnet-files.itrocket.net/cascadia/snap_cascadia.tar.lz4 | head -n 1 | grep "200" > /dev/null; then
  curl https://testnet-files.itrocket.net/cascadia/snap_cascadia.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.cascadiad
    else
  echo no have snap
fi
```
```sh
# servisleri başlatalım
sudo systemctl daemon-reload
sudo systemctl enable cascadiad
sudo systemctl restart cascadiad && sudo journalctl -u cascadiad -f
```


```sh
# cüzdan olusturma
cascadiad keys add $WALLET
```
```sh
# cüzdan import etme
cascadiad keys add $WALLET --recover
```
```sh
# validator olusturma
cascadiad tx staking create-validator \
--amount 1000000aCC \
--from $WALLET \
--commission-rate 0.1 \
--commission-max-rate 0.2 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--pubkey $(cascadiad tendermint show-validator) \
--moniker $MONIKER \
--identity "" \
--details "" \
--chain-id cascadia_6102-1 \
--gas auto --gas-adjustment 1.5 --gas-prices=7aCC \
-y
```
### Delege 
```
cascadiad tx staking delegate valoper-adresiniz 1000000000000000000aCC --from cüzdanadınız --chain-id cascadia_6102-1 --gas auto --gas-adjustment 1.5 --gas-prices=7aCC
```
