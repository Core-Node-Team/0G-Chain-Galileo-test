
#### Durduralım
```
cd
systemctl stop 0gchaind
systemctl stop geth
```
#### Dosyaları çekelim
```
wget https://github.com/Core-Node-Team/0G-Chain-Galileo-test/raw/refs/heads/main/update/0gchaind.zip
wget https://github.com/Core-Node-Team/0G-Chain-Galileo-test/raw/refs/heads/main/update/geth.zip
```
#### Arşivleri aç
```
unzip -o 0gchaind.zip
unzip -o geth.zip
```
#### Binary dosyalarını taşı
```
mv -f 0gchaind $HOME/go/bin/
mv -f geth $HOME/go/bin/
```
#### İzinleri ayarla
```
chmod +x $HOME/go/bin/0gchaind
chmod +x $HOME/go/bin/geth
```
#### Başlatalım
```
systemctl restart 0gchaind
systemctl restart geth
```
#### Loglara bakalım
```
journalctl -u 0gchaind -fo cat
```
```
journalctl -u geth -fo cat
```

