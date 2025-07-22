

```
sudo apt install liblz4-tool

systemctl stop 0gchaind geth

cp $HOME/.0gchaind/0g-home/0gchaind-home/data/priv_validator_state.json $HOME/.0gchaind/0g-home/0gchaind-home/priv_validator_state.json.backup

rm -rf $HOME/.0gchaind/0g-home/0gchaind-home/data
rm -rf $HOME/.0gchaind/0g-home/geth-home/geth/chaindata

curl -L https://snapshot.corenodehq.xyz/0g_testnet/0g_snap.tar.lz4 | tar -I lz4 -xf - -C $HOME/.0gchaind/0g-home/0gchaind-home
curl -L https://snapshot.corenodehq.xyz/0g_testnet/0g_snap_geth.tar.lz4 | tar -I lz4 -xf - -C $HOME/.0gchaind/0g-home/geth-home/geth

mv $HOME/.0gchaind/0g-home/0gchaind-home/priv_validator_state.json.backup $HOME/.0gchaind/0g-home/0gchaind-home/data/priv_validator_state.json

sudo systemctl restart geth
sleep 5
sudo systemctl restart 0gchaind

sudo journalctl -u 0gchaind -u geth -f -o cat

```

