NOT: kurulum corenode dokumanından yapmadıysanız zgsd yerine zgs yazınız.
```
systemctl stop zgsd
rm -rf $HOME/0g-storage-node/run/db/flow_db
wget https://snapshot.corenodehq.xyz/0g_testnet/flow_db.tar.gz -O $HOME/0g-storage-node/run/db/flow_db.tar.gz && tar -xzvf $HOME/0g-storage-node/run/db/flow_db.tar.gz -C $HOME/0g-storage-node/run/db/
```
