## Upgrade v0.1.5 
Not: 2,229,000 blok yükseliğine gelince

```
systemctl stop cascadiad
cd $HOME
rm -rf cascadiad
wget -O cascadiad https://github.com/CascadiaFoundation/cascadia/releases/download/v0.1.4/cascadiad
chmod +x $HOME/cascadiad
sudo mv $HOME/cascadiad $(which cascadiad)
```
```
sudo systemctl restart cascadiad && sudo journalctl -u cascadiad -f
```