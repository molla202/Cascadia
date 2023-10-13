`Upgrade to 0.1.6`

`height: 2,820,000`

### Manual Upgrade
```
sudo systemctl stop cascadiad
curl -L https://github.com/CascadiaFoundation/cascadia/releases/download/v0.1.6/cascadiad -o cascadiad
chmod +x cascadiad
sudo mv cascadiad $(which cascadiad)
sudo systemctl start cascadiad
sudo journalctl -u cascadiad -f --no-hostname -o cat
```

### Oto

### Güncelleme v0.1.6 Block=2,820,000
```
curl -sSL https://raw.githubusercontent.com/Core-Node-Team/Testnet-TR/main/Cascadia/update-version-0.1.7.sh | bash
```

### v0.1.7 cosmovisor upgrade
```
curl -L https://github.com/CascadiaFoundation/cascadia/releases/download/v0.1.7/cascadiad -o cascadiad
sudo chmod u+x cascadiad

mkdir -p $HOME/.cascadiad/cosmovisor/upgrades/v0.1.7/bin
mv cascadiad $HOME/.cascadiad/cosmovisor/upgrades/v0.1.7/bin/

```
