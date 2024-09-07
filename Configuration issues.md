# Configurations issues
Start Bitcoind
```bash copy
bitcoind -datadir=/media/rez/T7\ Shield
```
Start EPS
```bash copy
cd ~/Desktop/eps && electrum-personal-server config.ini
```
Stop Bitcoind
```bash copy
bitcoin-cli -datadir=/media/rez/T7\ Shield stop
```
```bash copy
sudo shutdown -r now
```
Install net-tools to check incoming/outgoing port traffic
```bash copy
sudo apt install net-tools
```
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
cd ~/.elctrum && nano config
```

---
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
