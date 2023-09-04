systemctl stop cascadiad
cd $HOME
rm -rf cascadiad
curl -L https://github.com/CascadiaFoundation/cascadia/releases/download/v0.1.4/cascadiad -o cascadiad
chmod +x cascadiad
sudo mv cascadiad $(which cascadiad)
sudo systemctl restart cascadiad

sudo journalctl -u cascadiad -fo cat
