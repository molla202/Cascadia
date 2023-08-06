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


