# Configurations issues
install net-tools to check incoming/outgoing port traffic
```bash copy
sudo apt install net-tools
```
Start EPS
```bash copy
cd ~/Desktop/eps && electrum-personal-server config.ini
```
Open EPS config
```bash copy
cd ~/Desktop/eps && nano config.ini
```
Open Bitcoin config
```bash copy
cd /media/rez/T7\ Shield && nano bitcoin.conf
```

---
Check what IP and port Bitcoin is listening to
```bash copy
sudo netstat --ip -lpa|grep bitcoin
```
Start Bitcoind
```bash copy
bitcoind -datadir=/media/rez/T7\ Shield
```
Stop Bitcoind
```bash copy
bitcoin-cli -datadir=/media/rez/T7\ Shield stop
```
