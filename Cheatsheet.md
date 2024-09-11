# Cheatsheet
## Start/Stop
Start Bitcoind
```bash copy
bitcoind -datadir=/media/rez/T7\ Shield
```
Start Bitcoind as daemon
```bash copy
bitcoind -datadir=/media/rez/T7\ Shield -server -daemon
```
Start Electra
```bash copy
electrs --log-filters INFO --network bitcoin --db-dir ./db --daemon-dir /media/rez/T7\ Shield
```
Start Electrum
```bash copy
electrum --oneserver --server 127.0.0.1:50002:s
```
Stop Bitcoind
```bash copy
bitcoin-cli -datadir=/media/rez/T7\ Shield stop
```
Check Bitcoind daemon
```bash copy
bitcoin-cli -datadir=/media/rez/T7\ Shield getblockchaininfo | head
```

---
## Configs
Open Bitcoin.conf
```bash copy
cd /media/rez/T7\ Shield && nano bitcoin.conf
```
Open Electrs config.toml
```bash copy
cd ~/.electrs && nano config.toml 
```
Open Electrum config
```bash copy
cd ~/.electrum && nano config
```

---
## Other settings
To search whole system directory
```bash copy
find ~/. -name bitcoin*
```

```bash copy
sudo shutdown -r now
```

