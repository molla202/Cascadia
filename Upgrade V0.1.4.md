## Upgrade v0.1.4
Not: yedeklerinizi alın unutmayın.

## Blok Yüksekliği 1,774,000 geldiğinde yapılacak
```
systemctl stop cascadiad
cd $HOME
curl -L https://github.com/CascadiaFoundation/cascadia/releases/download/v0.1.4/cascadiad-v0.1.4-linux-amd64 -o cascadiad
chmod +x cascadiad
sudo mv cascadiad $(which cascadiad)
sudo systemctl restart cascadiad && sudo journalctl -u cascadiad -fo cat
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


