
NOT:If you write all the information in config.toml, do the other option. Due to the installation differences, you can do it in 2 ways. Choose the one that suits you. If you installed from corenode documents, do the first one. If you installed from somewhere else, do the second option.
NOT: eğer tüm bilgileri config.toml yazıyorsanız diğer seçeneği yapın. kurulum farklarından ötürü 2 şekilde yapabilirsiniz. size uygun olanı seçin. eğer corenode dökumanalrından kurduysanız ilkini başka yerden kurduysanız 2.ci seçeneği yapın

```
systemctl stop zgsd
```
```
cp "$HOME/0g-storage-node/run/config-testnet-turbo.toml" "$HOME/0g-storage-node/run/config-testnet-turbo.toml.bak"
```
```
curl -o $HOME/0g-storage-node/run/config-testnet-turbo.toml https://raw.githubusercontent.com/Core-Node-Team/0G-Chain-Galileo-test/refs/heads/main/config-testnet-turbo.toml
```
```
rm -rf $HOME/0g-storage-node/run/db/data_db
```
```
systemctl start zgsd
```

# OR
```
systemctl stop zgsd
```
```
cp "$HOME/0g-storage-node/run/config-testnet-turbo.toml" "$HOME/0g-storage-node/run/config-testnet-turbo.toml.bak"
```
```
sed -i "s|log_contract_address = \".*\"|log_contract_address = \"0x5f1D96895e442FC0168FA2F9fb1EBeF93Cb5035e\"|" "$HOME/0g-storage-node/run/config-testnet-turbo.toml"
sed -i "s|mine_contract_address = \".*\"|mine_contract_address = \"0xB0F6c3E2E7Ada3b9a95a1582bF6562e24A62D334\"|" "$HOME/0g-storage-node/run/config-testnet-turbo.toml"
sed -i "s|reward_contract_address = \".*\"|reward_contract_address = \"0xdf758Bd14306482DeCbeF186eC6f18e4e79aaaE6\"|" "$HOME/0g-storage-node/run/config-testnet-turbo.toml"
```
```
rm -rf $HOME/0g-storage-node/run/db/data_db
```
```
systemctl start zgsd
```
