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
sudo tee /etc/systemd/system/cascadiad.service > /dev/null <<EOF
[Unit]
Description=Cascadia Node
After=network.target
[Service]
User=$USER
Type=simple
ExecStart=$(which cascadiad) start --home $HOME/.cascadiad --chain-id cascadia_6102-1
Restart=on-failure
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
```
```
sudo systemctl restart cascadiad && sudo journalctl -u cascadiad -f
```
