## Upgrade v0.1.4
Not: yedeklerinizi alın unutmayın.

## Blok Yüksekliği 1,774,000 geldiğinde yapılacak
```
systemctl stop cascadiad
cd $HOME
wget -O cascadiad https://github.com/CascadiaFoundation/cascadia/releases/download/v0.1.4/cascadiad
chmod +x $HOME/cascadiad
sudo mv $HOME/cascadiad $(which cascadiad)
sudo systemctl restart cascadiad && sudo journalctl -u cascadiad -f
```
## ardından snap atın bazen sorun çıkıyor
```
sudo systemctl stop cascadiad

cp $HOME/.cascadiad/data/priv_validator_state.json $HOME/.cascadiad/priv_validator_state.json.backup

rm -rf $HOME/.cascadiad/data 
curl https://testnet-files.itrocket.net/cascadia/snap_cascadia.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.cascadiad

mv $HOME/.cascadiad/priv_validator_state.json.backup $HOME/.cascadiad/data/priv_validator_state.json
wget -O $HOME/.cascadiad/config/addrbook.json https://testnet-files.itrocket.net/cascadia/addrbook.json
sudo systemctl restart cascadiad && sudo journalctl -u cascadiad -f
```
## olmaz state deneyin

```
sudo systemctl stop cascadiad

cp $HOME/.cascadiad/data/priv_validator_state.json $HOME/.cascadiad/priv_validator_state.json.backup
cascadiad tendermint unsafe-reset-all --home $HOME/.cascadiad

peers="c6e3921222655345d8296353994e917f13a1b4a1@cascadia-testnet-peer.itrocket.net:40656,192731fe8a88b7923869511dc2f8ee11b58c56d1@31.220.85.212:56656,b19ac2a44da617ee60920fe9cecd2fee1595f0cf@46.4.101.89:26680,1ecb3286ed1e3ef898459f55948234014b3be6f7@46.4.68.113:21456,1d61222b7b8e180aacebfd57fbd2d8ab95ebdc4c@65.109.93.152:35656,3bfc33673d8fcfee0bd392113b68ce758003a5cc@54.175.24.20:26656,3ae7f63ee1cf8ea10a4596c4d5d46bb2074500fb@65.109.99.123:26656,dd225f803eb3ae4bba2eef4628bebd6fc52092c2@65.108.97.111:36656,46b554c4cddbde4adb7b10e7b6c66e46845ffc33@135.181.116.109:22796,5126c2904cf4d9ed9b2c6cd203fccbe3983229da@23.88.5.169:22656,c01101d842b95cfc3b8510a523b3f5aec60d59f0@208.73.234.229:26656,f78611ffa950efd9ddb4ed8f7bd8327c289ba377@65.109.108.150:46656,16b622800dedb46f764eb052bd9596b762a05bfd@49.12.84.248:15656,80dc7406f7ec05f26cd4e529208fc911f75923b0@65.21.235.172:39656,e25bf22448e62faca2359985303ec4557f662444@95.217.11.20:26656,3afe6df94dc385efa85aef823e038c76147e4c99@95.217.35.111:26656,b08c31354f72aabc769abc8bfd0a8ee7cd0201e5@38.242.238.112:26656,1fa47c9fa214bcd7b6c2f900b11f831371c411e3@31.220.87.33:26656,288b101225b97a3b4c7e642e70bf8bcde962c247@185.215.164.114:26656"  
SNAP_RPC="https://cascadia-testnet-rpc.itrocket.net:443"

sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.cascadiad/config/config.toml 

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height);
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000));
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash) 

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH && sleep 2

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ;
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ;
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ;
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ;
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.cascadiad/config/config.toml

mv $HOME/.cascadiad/priv_validator_state.json.backup $HOME/.cascadiad/data/priv_validator_state.json

sudo systemctl restart cascadiad && sudo journalctl -u cascadiad -f
```
