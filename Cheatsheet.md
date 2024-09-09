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
cd /usr/local/bin/electrs && ./target/release/electrs --log-filters INFO --network bitcoin --db-dir ./db --daemon-dir /media/rez/T7\ Shield
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
# Prometheus
Restart prometheus
```bash copy
sudo systemctl restart prometheus
```
Check collected metrics
```bash copy
brave-browser 'http://localhost:9090/graph?g0.range_input=1h&g0.expr=index_height&g0.tab=0'
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
```bash copy
sudo shutdown -r now
```
