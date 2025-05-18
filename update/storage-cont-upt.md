
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
sed -i \
-e 's|^log_contract_address *= *".*"|log_contract_address = "0xbD75117F80b4E22698D0Cd7612d92BDb8eaff628"|' \
-e 's|^log_sync_start_block_number *= *[0-9]*|log_sync_start_block_number = 326165|' \
-e 's|^mine_contract_address *= *".*"|mine_contract_address = "0x3A0d1d67497Ad770d6f72e7f4B8F0BAbaa2A649C"|' \
-e 's|^reward_contract_address *= *".*"|reward_contract_address = "0xd3D4D91125D76112AE256327410Dd0414Ee08Cb4"|' \
"$HOME/0g-storage-node/run/config-testnet-turbo.toml"
```
```
rm -rf $HOME/0g-storage-node/run/db/data_db
```
```
systemctl start zgsd
```
