

```
systemctl stop 0gchaind geth
```
```
wget https://github.com/0glabs/0gchain-NG/releases/download/v2.0.2/galileo-v2.0.2.tar.gz
tar -xzf galileo-v2.0.2.tar.gz
```
```
cp galileo-v2.0.2/bin/0gchaind $HOME/go/bin/0gchaind
```
```
systemctl start geth
sleep 5
systemctl start 0gchaind
```
```
sudo journalctl -u 0gchaind -u geth -f
```
