## Install EPS (Electrum Private Server)
https://armantheparman.com/bitcoin-core-and-electrum-personal-server-electrum-wallet-on-a-raspberry-pi/

https://armantheparman.com/electrum-server/

### Download EPS source code and signiture
1. Head over to [electrum-personal-server](https://github.com/chris-belcher/electrum-personal-server/releases) and make a note of the latest release version number. At time of writing it is ```0.2.4```, update the below command to the latest version number if this is now out of date. Run:
   ```bash copy
   cd ~/Downloads && wget https://github.com/chris-belcher/electrum-personal-server/archive/refs/tags/eps-v0.2.4.tar.gz
   ```
2. Download the file signiture
   ```bash copy
   wget https://github.com/chris-belcher/electrum-personal-server/releases/download/eps-v0.2.4/eps-v0.2.4.tar.gz.asc
   ```
3. Download Chris Belchers public key, there are multiple links on this [page](https://github.com/chris-belcher/electrum-personal-server/releases) for you to manually verify Chris Belchers public key. Run:
   ```bash copy
   wget https://github.com/chris-belcher/electrum-personal-server/raw/master/docs/pubkeys/belcher.asc
   ```
4. Import the key to your system keychain
   ```bash copy
   gpg --import belcher.asc
   ```
5. Verify the source file was signed by Chris Belcher
   ```bash copy
   gpg --verify eps-v0.2.4.tar.gz.asc
   ```
   In the Terminal output, you want to see ```Good Signiture from Chris Belcher```, ignore the warning.

### Prepare for the build
6. Extract the source file, rename and save onto your Desktop, change ```rez``` in the command to your system user name. Run:
   ```bash copy
   tar -xvf eps-v0.2.4.tar.gz -C /home/rez/Desktop
   ```
7. Navigate to Desktop and rename the extracted file to ```eps```. Remember to change the file version number if you downloaded a newer version of EPS.
   ```bash copy
   cd ~/Desktop && mv electrum-personal-server-eps-v0.2.4 eps
   ```
8. Update the package list
   ```bash copy
   sudo apt update
   ```
9. Install pip3
   ```bash copy
   sudo apt install python3-pip
   ```
10. Check pip3 was installed
   ```bash copy
   pip3 --version
   ```
11. Upgrade pip3 to the latest version
   ```bash copy
   sudo pip3 install --upgrade pip
   ```
### Complete EPS installation
12. Navigate into the EPS directory. Run:
   ```bash copy
   cd ~/Desktop/eps
   ```
13. Complete installation by running the below command. Note: do not use ```sudo``` and make sure to leave in the final ```.```  
   ```bash copy
   pip3 install --user . 
   ```
   Ignore the Terminal warning about PATH, a PATH veriable will be added the next time you log out and back into the laptop again.

14. Clean up Downloads folder, remove downloaded files
   ```bash copy
   cd ~/Downloads && rm belcher.asc eps-v0.2.4.tar.gz eps-v0.2.4.tar.gz.asc
   ```

## Don’t run EPS yet!
Follow the below instructions to Install Electrum and create a test wallet. After that we can edit EPS config.ini file and then run EPS for the first time.

===
### Create a Bitcoin Core wallet for EPS to use.
1. Start bitcoind
   ```bash copy
   bitcoind -datadir=/media/rez/T7\ Shield
   ```
2. Create a legacy bitcoin wallet called 'electrumpersonalserver'
   ```bash copy
   bitcoin-cli -datadir=/media/rez/T7\ Shield createwallet electrumpersonalserver true true "" false false true
   ```
3. You can now close Bitcoin Core by running the below command in any teminal window.
   ```bash copy
   bitcoin-cli -datadir=/media/rez/T7\ Shield stop
   ```

===
### Create a Test wallet
We will set up a test (hot) wallet in Electrum so we can make sure all of our config settings are correct before actually loading up one of our wallets that contains funds.
1. Run electrum. Run:
   ```bash copy
   electrum
   ```
2. At the welcome screen. Click ```Next```.
3. Rename ```default_wallet``` to ```test_wallet``` (I think this name is less confusing). Click ```Next```.                                                                        
4. Select ```I already have a seed```. Click ```Next```.
5. Head over to [CoinPlate](https://getcoinplate.com/bip39-seed-phrase-mnemonics-generator-offline-online-tool/?v=2a6039655313 "CoinPlate's Homepage") and generate a Bip39 24 word seed phrase .
   Copy and save the seed phrase to a note on your desktop. Don't worry about security as this is just a simple test wallet for testing configuration settings.
6. Copy and paste the seed phase into the Electrum box that is displayed.
7. Click ```Options```, and select ```Bip39 seed```. Click ```OK```. Ignore the warning and click ```Next```.
8. Keep ```native segwit (p2wpkh)``` selected and click ```Next```.
9. Leave the password fields blank, we wont encrypt the wallet data as this is just a test wallet, click ```Next```.
10. Electrum - enable update check. Click ```No```. We will always verify and update our software ourselves.

### Export your ```test_wallet``` public key
11. In Electrum, click on the ```Wallet``` tab, then ```Information```. Copy and save the long zpub key displayed. We will need to enter this public key into the EPS config.ini file later.


===
## Configure EPS 
Edit EPS config file:

1. Navigate to the EPS folder that we saved to Desktop earlier. Run:
   ```bash copy
   cd ~/Desktop/eps
   ```
2. Rename config.ini_sample to config.ini. Run:
   ```bash copy
   mv config.ini_sample config.ini
   ```
3. Open the config file with nano to edit. Run:
   ```bash copy
   nano config.ini
   ```
The EPS config.ini file will open and you will see that the file is divided into four sections with the following headings:
   - ```[master-public-keys]```
   - ```[bitcoin-rpc]```
   - ```[electrum-server]```
   - ```[watch-only-addresses]```
   - ```[logging]```

4. Under ```[master-public-keys]``` add any wallet public key (xpub, ypub, zpub) that you would like to have access too. EPS is Electrums link to your Bitcoin node, If you do not add the wallet public key (xpub, ypub, zpub) to this EPS config.ini file then Electrum will not use your node to pull transaction information from your node. Any wallet that you want to use with Electrum needs the public key added to this EPS config.ini file.

5. Under the ```[bitcoin-rpc]``` heading, add the path to your external SSD drive in the ```datadir``` section :

   ```datadir = /media/rez/T7 Shield```

6. Also under the ```[bitcoin-rpc]``` heading, add the name of the Bitcoin Core wallet we created specifically for EPS:

   ```wallet_filename = electrumpersonalserver```

7. Under the ```[watch-only-addresses]``` heading, add any bitcoin addresses that you would like to watch. A simple Google search can return some interesting famous bitcoin addresses. I added the following three addresses out of curiosty and fun.
   
   ```satoshi_address = 1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa```

   ```huobi_address = 35hK24tcLEWcgNA4JxpvbkNkoAcDGqQPsP```

   ```mr100_address = 1Ay8vMC7R1UbyCCZRVULMV7iQpHSAbguJP```

8. Save and exit out of the config.ini file.

## Run EPS for the first time
Before running EPS for the first time make sure Bitcoin Core is fully synced & running.

1. Start Bitcoin-Core, wait for it to fully sync
   ```bash copy
   bitcoin-qt
   ```
2. The first time EPS is run it will import all configured addresses as watch-only into the Bitcoin node, and then exit by itself.
   ```bash copy
   electrum-personal-server config.ini
   ```
3. If the wallets contain historical transactions you need to run a rescan to pull all of the transaction data.
   ```bash copy
   electrum-personal-server --rescan config.ini
   ```
   It will ask for the earliest wallet creation date. Enter 1 to start from block height 1, and y to confirm. The first initial scan can take a while, but a pop up box will be displayed with a % completion displayed.

   Once the server has finished scanning it will gracefully shut down on it's own, so you can leave the server to do it's job and come back in an hour or so.

===
### Edit Bitcoin core config file. Run:
Once Bitcoin core is fully synced you will be greeted with a welcome screen asking if you would like to ```Create a new wallet```.  We need to edit the config file first so ignore this prompt and open Bitcoin Core config file.

1. From the Bitcoin Core GUI click on ```Settings```, ```Options```, then ```Open Configuration File```.

2. Enter the below text, save and exit
   ```bash copy
   # server=1, this tells Bitcoin to accept JSON-RPC commands 
   # (such as ones from EPS). txindex=1 allows any transaction 
   # to be looked up by Bitcoin Core, not just your own wallet.
   server=1
   txindex=1
   listen=1

   proxy=127.0.0.1:9050
   bind=127.0.0.1

   # only connect to Tor hidden services, not even IPv4/IPv6 nodes
   onlynet=onion

   # If running tor, walletbroadcast=0 prevents the node from 
   # rebroadcasting transactions without tor.
   walletbroadcast=0

   # this allows old style bitcoin wallet to be created for eps 
   # using bitcoin-cli createwallet electrumpersonalserver
   deprecatedrpc=create_bdb
   ```
   
3. Now shut down Bitcoin Core so that the config changes we made can be applied. Click on the ```X``` in the top right hand corner. Or enter the below command in any Terminal window:
   ```bash copy
   bitcoin-cli -datadir=/media/rez/T7\ Shield stop
   ```
