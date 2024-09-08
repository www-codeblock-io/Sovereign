# Cheatsheet
## Start/Stop
Start Bitcoind
```bash copy
bitcoind -datadir=/media/rez/T7\ Shield
```
Start Electra
   ```bash copy
   ./target/release/electrs --log-filters INFO --network bitcoin --db-dir ./db --daemon-dir /media/rez/T7\ Shield
   ```
Start Electrum
```bash copy
electrum --oneserver --server 127.0.0.1:50002:s
```
Stop Bitcoind
```bash copy
bitcoin-cli -datadir=/media/rez/T7\ Shield stop
```
Start Bitcoind as daemon
```bash copy
bitcoind -datadir=/media/rez/T7\ Shield -server -daemon
```
Check Bitcoind daemon
```bash copy
bitcoin-cli -datadir=/media/rez/T7\ Shield getblockchaininfo | head
```
```bash copy
sudo shutdown -r now
```
---
## Configs
Open Bitcoin config
```bash copy
cd /media/rez/T7\ Shield && nano bitcoin.conf
```
Open EPS config
```bash copy
cd ~/Desktop/eps && nano config.ini
```
Open Electrum config
```bash copy
cd ~/.electrum && nano config
```

---
## Other settings
Install net-tools to check incoming/outgoing port traffic
```bash copy
sudo apt install net-tools
```
Check what IP and port Bitcoin is listening to
```bash copy
sudo netstat --ip -lpa|grep bitcoin
```
Create a wallet dedicated to EPS
```bash copy
bitcoin-cli -datadir=/media/rez/T7\ Shield createwallet electrumpersonalserver true true "" false false true
```
To add deprecated wallet functionality
```bash copy
deprecatedrpc=create_bdb
```
To search whole system directory
```bash copy
find ~/. -name bitcoin*
```
Start EPS
```bash copy
cd ~/Desktop/eps && electrum-personal-server config.ini
```
