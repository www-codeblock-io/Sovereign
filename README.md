# Sovereign - Bitcoin Personal Node
last updated: 8th September 2024.

# Project goal
A secure and sovereign Bitcoin node using software self-verified (non branded/containerised), enabling the use of hardware wallets and preserving the privacy of personal addresses and balances while interacting with the Bitcoin network.

I recommend in the first instance to always consult a software projects official installation instructions. This will have the most queries and bugs already logged by users and answered by the project maintainers. External guides can be out-of-date and not actively maintained. This guide has a subheading stating the last update and a link to each official software project page.

Think of this as a general guide with some trouble shooting tips. I have included all the Terminal commands that I personally ran to achieve the above goal.


---
## SOFTWARE INSTALLED
- Ubuntu 22.04 LTS
- Bitcoin-Core
- Tor
- ProtonVPN
- Electra
- Electrum wallet
- Specter wallet
- Sparrow wallet

(optional)
- Brave Browser
- Mullvad VPN
- Telegram

---
## HARDWARE

Bitcoin Core [minimum requirements](https://bitcoin.org/en/bitcoin-core/features/requirements)
- Ram: 1GB
- CPU: Some ARM chipsets >1.GHz
- HDD: 650GB (however large the Bitcoin blockchain currently is.)
- Download: 500 MB/day (15 GB/month)
- Upload: 5 GB/day (150 GB/month)

Most entry level laptops should meet the above specifications. Just add an external 2T SSD drive with an unlimited broadband connection.

I purchased this second hand [HP portable laptop](https://www.amazon.com/gp/aw/d/B0C9N3H8F2/ref=ox_sc_act_image_1?smid=AYY6S2IYYQJGA "Amazon HP Laptop") from Amazon.com for $140.

- Ram: 16GB
- CPU: Intel celeron 1.1 GHz
- HDD: 64GB

I then added this external [2T SSD drive](https://a.co/d/itXEh1A) from Amazon.com for $140.

___
## VERIFY & INSTALL SOFTWARE

For all Bitcoin software, it’s a particularly important security step to verify the release. This is done to ensure the installation file you download has not been compromised. Follow download instructions and use gpg in the terminal as instructed.

___
## Install Linux Ubuntu 22.04 LTS
I installed Ubuntu 22.04 LTS because at the time of writing the Tor project have not released a package for Ubuntu 24.04.4 LTS.

### Deactivate Secure-Boot
Secure boot signing authorities have made mistakes in the past ([section 3, page 2 under 'Disadvantages'](https://www.nsa.gov/portals/75/documents/what-we-do/cybersecurity/professional-resources/BootSecurityModesAndRec_20190522.pdf)). I will verify all the software myself and not trust a third-party to do this for me.

1. Enter into the laptops Bios menu:
Press the laptops power-on button then immediatly press the Function key that enters the Bios menu. For the HP Laptop I'm using it was the F2 key. [Bios key by manufacturer. ](https://gadgetmates.com/bios-key-by-manufacturer)
2. Once the system Bios has loaded, proceed to turn-off ```Secure Boot``` (usually located under the ```security``` tab), save and exit. Then turn-off the laptop.

3. On a seperate PC head over to [Ubuntu Releases](https://releases.ubuntu.com/22.04/ "Ubuntu releases") and download the following three files: 
   - ```Ubuntu-22.04.4-desktop-amd64.iso```
   - ```SHA256SUMS```
   - ```SHA256SUMS.gpg``` 

   Save all three files to your Downloads folder.


### Verify download signitures
Note, the slightly different commands, depending if you are on Windows or Linux.
[Reference Link](https://ubuntu.com/tutorials/how-to-verify-ubuntu#2-necessary-software "Ubuntu tutorial")

#### Windows10
1. Verify signitures. Open a Terminal window (CTRL+ALT+T). Run:
   ```bash copy
   gpg --keyid-format long --verify SHA256SUMS.gpg SHA256SUMS.txt
   ```
2. Verify ISO file
   ```bash copy
   certutil -hashFile <ISO_file_path> SHA256
   ```
Now open the SHA256SUMS.txt file in your Downloads folder and compare the SHA256 checksum inside the file with the checksum that was printed to the Terminal. It will be a long hash, verify both hashes/numbers match, if so, your ISO download is authentic.

#### Linux/MacOS
1. Verify signitures. Open a Terminal window (CTRL+ALT+T). Run:
   ```bash copy
   gpg --keyid-format long --verify SHA256SUMS.gpg SHA256SUMS.txt
   ```
2. Verify ISO file
   ```bash copy
   sha256sum -c SHA256SUMS 2>&1 | grep OK
   ```
   The output you want will look similar to the following:
      ```ubuntu-22.04.4-desktop-amd64.iso: OK```


### Install OS:
Read all of the below instructions (including links) before proceeding.

[General installation instructions](https://ubuntu.com/tutorials/install-ubuntu-desktop#3-create-a-bootable-usb-stick "https://ubuntu.com/tutorials/install-ubuntu-desktop#3-create-a-bootable-usb-stick")

[Create partition table](https://ubuntuhandbook.org/index.php/2022/04/install-ubuntu-2204-step-by-step/amp/ "Ubuntuhandbook.org") not always required.

Note: Flashing a USB drive with an ISO image will place a write protection on the USB drive, to prevent accidental deletion of your bootable USB drive data. If you make a mistake and need to reformat the USB drive then use something like [Aomei Partition Assistant](https://www.aomeitech.com/download.html#pa "Aomitech.com") to remove the write protection so you can reformat the drive and start again (or to reclaim the USB drive to be used for something else).


### Once Installation has finished
1. Update package manager. Run:
   ```bash copy
   sudo apt update && sudo apt upgrade
   ```

### Adjust the power settings
Because you will want to run your node for 6+ hours a day (24hrs is better) you will need to adjust the power & lid closure settings to prevent the laptop entering into a low power mode, slowing or halting network traffic.

#### Navigate to the Ubuntu power settings menu:
Show Apps > Settings > Power Settings.

Make the following adjustments:
- Power Mode = Balanced
- Automatic Power Saver = Off
- Automatic suspend = Off

Then close the settings menu.

### Laptop lid, do nothing when closed
1. Navigate to systemd directory. Run:
   ```bash copy
   cd /etc/systemd
   ```
2. Open logind configuration file 
   ```bash copy
   sudo nano logind.conf
   ```
3. Uncomment HandleLidSwitch and make it equal to inore, save and exit
   ```HandleLidSwitch=ignore```

4. Restart the systemd daemon (be aware that this will log you out of Ubuntu) with this command
   ```bash copy
   sudo systemctl restart systemd-logind
   ```

___
## Install Proton VPN
1. Download repo. Run:
   ```bash copy
   wget https://repo.protonvpn.com/debian/dists/stable/main/binary-all/protonvpn-stable-release_1.0.4_all.deb
   ```

2. Install repo
   ```bash copy
   sudo dpkg -i ./protonvpn-stable-release_1.0.4_all.deb && sudo apt update
   ```

3. Check the repo package integrity by checking its checksum
   ```bash copy
   echo "62a9d849835de8a5664cf95329458bf1966780b15cec420bf707b5f7278b9027  protonvpn-stable-   release_1.0.4_all.deb" | sha256sum --check -
   ```

4. Install ProtonVPN desktop app
   ```bash copy
   sudo apt install proton-vpn-gnome-desktop
   ```

6. Check for updates to ensure you’re running the latest version
   ```bash copy
   sudo apt update && sudo apt upgrade
   ```

7. Linux system tray icon (optional)
   By default, GNOME doesn’t support tray icons. Enable this functionality
   ```bash copy
   sudo apt install libayatana-appindicator3-1 gir1.2-ayatanaappindicator3-0.1 gnome-shell-extension-appindicator
   ```
9. Headover to [ProtonVPN](https://account.protonvpn.com/signup "ProtonVPN.com") and Sign-up to a free account.


**_Connect to ProtonVPN whenever accessing a web browser on your node._**

---
## Install Mullvad VPN (optional) 
[Mullvad](https://mullvad.net/en/download/vpn/linux "Mullvad.net") recommended by the official Tor website. It's a paid service but has no email requirement and can use BTC to pay for the service. The internet speed should also be faster than a free ProtonVPN account.

___
## Install Brave Browser (optional)
1. Enter the following commands one at a time. Run:
   ```bash copy
   sudo apt install curl
   ```
2. Download keys to keychain
   ```bash copy
   sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-      release.s3.brave.com/brave-browser-archive-keyring.gpg
   ```
3. Verify  
   ```bash copy
   echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg] https://brave-browser-apt-release.s3.brave.com/ stable main"|sudo tee /etc/apt/sources.list.d/brave-browser-release.list
   ```
4. Update and install 
   ```bash copy
   sudo apt update && sudo apt install brave-browser
   ```

---
## Install Telegram Desktop (optional)
A quick solution for encrypted files/messages.
1. Install Telegram from apt repo. Run:
   ```bash copy
   sudo apt install telegram-desktop
   ```

---
## Install Bitcoin Core
Install using a binary package. Follow the links below to official websites, check your URL to make sure you are on the correct site and be sure to verify your download before installing.

Download Bitcoin Core
[Link](https://bitcoincore.org/en/download/ "Bitcoincore.org")
Head over to bitcoincore.org and:
- Download Linux(tgz) 64 bit
- Click on ```Linux verification instructions``` follow theses instructions to verify your download.

Unzip the bitcoin folder
```bash copy
tar xzf bitcoin-27.1-x86_64-linux-gnu.tar.gz
```
Install bitcoin core
```bash copy
sudo install -m 0755 -o root -g root -t /usr/local/bin bitcoin-27.1/bin/*
```

Install instructions
[Link](https://bitcoin.org/en/full-node#linux-instructions "Bitcoin.org")


---
## Run Bitcoin Core (start initial blockchain download)
Plug in your external 2T SSD drive.

1. Then start Bitcoin Core. Run:
   ```bash copy
   bitcoin-qt
   ``` 
   You will be greated with the Bitcoin Core Welcome screen, it is here that we will let Bitcoin Core know where our external SSD drive is located.

2. Select 'Use a custom data directory'.
3. Click the three dots to the right and navigate to your external SSD drive location. Mine was located at ```/media/rez/T7 Shield```.
4. Uncheck ```Limit block chain storage```. We want to download the complete Bitcoin blockchain from the genisis block.
5. Click ```OK```.

Bitcoin Core will now create the main 'data-directory' inside your external SSD drive. It is here that all Bitcoin Core configuaration files will be stored along with the complete blockchain data. Get familiar with this 'data-directory' (SSD Drive) location as we will be accessing it later.

If you have trouble locating your external SSD Drive, try the following, open a new Terminal. Run:
   ```bash copy
   cd /media && dir
   ```
   You should see your user directory listed, ```cd``` into this dir which should contain your external drive.

6. Leave the laptop power plugged in and wait for the Bitcoin blockchain to download, Bitcoin Core will show a % complete and a rough estimate of how much time remains until completion.


---
## Configure Bitcoin Core
1. Once the download has completed you will be greated with a Welcome screen and a prompt to create a new wallet. We will create a wallet using bitcoind with bitcoin-cli commands so shut down for now. Run:
   ```bash copy
   bitcoin-cli -datadir=/media/rez/T7\ Shield stop
   ```

### Edit Bitcoin core config file. Run:
Once Bitcoin core is fully synced you will be greeted with a welcome screen asking if you would like to ```Create a new wallet```.  We need to edit the config file first so ignore this prompt and open Bitcoin Core config file.

1. From the Bitcoin Core GUI click on ```Settings```, ```Options```, then ```Open Configuration File```.

2. Enter the below text, save and exit
   ```bash copy
   # server=1, this tells Bitcoin to accept JSON-RPC commands 
   # (such as ones from EPS or Electrs etc.). txindex=1 allows any transaction 
   # to be looked up by EPS, this flag is not required for Electrs.
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
   
3. Now shut down Bitcoin Core so that the config changes we made can be applied. Click on the ```X``` in the top right hand corner. Or enter the below command in any Terminal window:
   ```bash copy
   bitcoin-cli -datadir=/media/rez/T7\ Shield stop
   ```
### Allow incoming connections (optional but recommended)
Configure your WIFI router to allow Bitcoin core incoming connections on port:8332. Make a note of both your MAC and IP address then follow the instructions in this [Link](https://bitcoin.org/en/full-node#network-configuration)

Retrieve your Laptops MAC address and IP address from the Terminal. 

1. To print your MAC address to Terminal. Run:
   ```bash copy
   ip link
   ```
   You can find the MAC address of your device at the last line after ```link/ether=##:##:##:##:##:##```.

2. Print your IP address to Terminal
   ```bash copy
   hostname -I
   ```

===
## Install Tor

[Official website](https://support.torproject.org/apt/tor-deb-repo/)

1. Verify your CPU architecture. Run:
   ```bash copy
   dpkg --print-architecture
   ```
2. Install apt-transport-https
   ```bash copy
   sudo apt install apt-transport-https
   ```
3. Check what Linux Distribution you have installed
   ```bash copy
   lsb_release -c
   ```
4. Create a new file in /etc/apt/sources.list.d/ named tor.list
   ```bash copy
   cd /etc/apt/sources.list.d/
   ```
5. Open the file
   ```bash copy
   sudo nano tor.list
   ```
6. Add the following entries inside the file, save and close
   ```bash copy
      deb     [arch=amd64 signed-by=/usr/share/keyrings/deb.torproject.org-keyring.gpg] https://deb.torproject.org/torproject.org jammy main
      deb-src [arch=<amd64 signed-by=/usr/share/keyrings/deb.torproject.org-keyring.gpg] https://deb.torproject.org/torproject.org jammy main
   ```
7. Add Tor gpg key used to sign the packages
   ```bash copy
   sudo wget -qO- https://deb.torproject.org/torproject.org/A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89.asc | gpg --dearmor | sudo tee /usr/share/keyrings/deb.torproject.org-keyring.gpg >/dev/null
   ```
9. Update apt
   ```bash copy
   sudo apt update
   ```
10. Install Tor
   ```bash copy
   sudo apt install tor deb.torproject.org-keyring
   ```
11. Check Tor is installed
   ```bash copy
   tor --version
   ```

### Configure Tor to start at system Boot (optional but recommended)
1. Enable Tor to start automatically on boot, this is your sovereign bitcoin node, don't use it to surf the web aimlessly. Run:
   ```bash copy
   sudo systemctl enable tor
   ```

### Controlling Tor
1. Manage Tor using systemd
   ```bash copy
   sudo systemctl restart tor
   ```
2. To check the status
   ```bash copy
   sudo systemctl status tor
   ```

---
## Set up Tor hidden service
Configure Bitcoin Core to only connect using Tor. [Reference link](
https://en.bitcoin.it/wiki/Setting_up_a_Tor_hidden_service)

Additional video tutorial, for further reference: [Canadian Bitcoiners](
https://www.youtube.com/watch?v=goTkOt8Rr1Q&list=PLZqQw4mLW-r3UEyL12FG4koZq6lOLsURF&index=3)

1. Make changes to Tor configuration file. Run:
   ```bash copy
   cd /etc/tor
   ```
2. Open torrc file   
   ```bash copy
   sudo nano torrc
   ```
3. Enter the below text at the bottom of the file, save and exit
   ```bash copy
   ControlPort 9051
   CookieAuthentication 1
   CookieAuthFileGroupReadable 1
   ```
4. Check what your Bitcoin Core user name is
   ```bash copy
   ps -eo user,group,comm |egrep 'bitcoind|bitcoin-qt' |awk '{print "Bitcoin user: " $1}'
   ```
5. Now add your Bitcoin Core user name to your Tor group. For example my command would be ```sudo usermod -a -G debian-tor rez``` as my Bitcoin user name is rez.
   ```bash copy
   sudo usermod -a -G debian-tor <user name found from above command>
   ```
6. If the above command did not work, double check your Tor user group name, run this command for a list of user groups, look for a user group with Tor in it's name.
   ```bash copy
   getent group
   ```
   
The above configuration sets up an automatic hidden service that is initiated by Bitcoin Core. On the next startup of bitcoin, Bitcoin Core will generate a file called ```onion_private_key``` in the data directory. KEEP THIS SAFE. If someone copies this file they can run a server with your .onion address. While a malicious party cannot necessarily associate the server with you as a person, as long as your server has the same xxxx.onion address they will know it is run by the same person.

If you delete this file, the next time bitcoind loads it will generate a new key file and xxxxxxxx.onion address.  For absolute security delete your ```onion_private_key``` file at each reboot or some frequent interval.

7. Do a system Restart
   ```bash copy
   sudo shutdown -r now
   ```

### Verify Bitcoin Core is running behind Tor, option: 1
1. Start Bitcoin Core GUI.
2. Once loaded enter the ```Network``` screen by pressing CTL+N.
3. Click on the ```Peers``` tab.
   You should now see a list of all the Bitcoin peers that your node is connected too. Under the ```Network``` column all the row values should say ```Onion```, meaning you are only connected to    peers over the onion ntwork. 

### Verify Bitcoin Core is running behind Tor, option: 2
1. Check Bitcoin Core is now running behind Tor, from the Bitcoin Core GUI click on 'Window', 'Consol', then type the below command into the consol shell after the > and press enter. Run:
   ```bash copy
   getnetworkinfo
   ```
2. Scroll up until you see the networks section. Your node is running behind Tor if the 'ipv4' and 'onion' networks have the below settings (there will also be other settings in each network group but they can be ignored):
   ```
   "name": "ipv4",
         "limited": true,
         "reachable": false

   And

   "name": "onion",
         "limited": false,
         "reachable": true
   ```

### Verify Bitcoin Core is running behind Tor, option: 3
1. For the extra security concious, you can also check the debug.log file in the Bitcoin data-directory.
   At some point during startup in the bitcoin/debug.log file you will see:
   ```tor: Got service ID XXXXXXXXXXX, advertising service XXXXXXXXXXX.onion:8333```

### Verify Bitcoin Core is running behind Tor, option: 4
You will learn a lot if you ```cd``` into your bitcoin data-directory delete the 'debug.log' file (don't worry bitcoin core just creates a new one at startup), restart Tor, and then Bitcoin Core. Then shut down Bitcoin core again once the application has completly loaded. Bitcoin Core will have created a new debug.log file and printed some debug statements during shut down.

1. Take a look at the new debug.log file. Run:
   ```bash copy
   nano debug.log
   ```
   You will see a lot of the actions listed that the bitcoin core software completed during start-up and shut-down, including starting a ```Tor control thread```, opening a ```Tor connection``` and finally connecting to the Tor netork using your Tor encryption key.


===
## Install Electrs
Why Electrs: https://blog.casa.io/electrum-server-performance-report/

https://github.com/romanz/electrs/tree/master?tab=readme-ov-file
I installed the ```static``` vaersion.

During installation build the below warning was thrown, I installed as the warning suggested:

warning: electrs (lib) generated 1 warning (run cargo fix --lib -p electrs to apply 1 suggestion)
    Finished release [optimized] target(s) in 5m 53s

I ran this command:
$ cargo fix --lib -p electrs

===
## Install Electrum
[Resource link](https://electrum.org/#download)
1. Download ThomasV public key and import into keychain. Run:
   ```bash copy
   cd ~/Downloads && wget https://raw.githubusercontent.com/spesmilo/electrum/master/pubkeys/ThomasV.asc && gpg --import ThomasV.asc
   ```
2. Install dependencies. Run:
   ```bash copy
   sudo apt-get install python3-pyqt5 libsecp256k1-dev python3-cryptography
   ```
3. Download package
   ```bash copy
   wget https://download.electrum.org/4.5.5/Electrum-4.5.5.tar.gz
   ```
4. Verify signitures
   ```bash copy
   wget https://download.electrum.org/4.5.5/Electrum-4.5.5.tar.gz.asc && gpg --verify Electrum-4.5.5.tar.gz.asc
   ```
5. Install with pip
   ```bash copy
   sudo apt-get install python3-setuptools python3-pip && python3 -m pip install --user Electrum-4.5.5.tar.gz
   ```
6. Clean up Downloads folder, remove downloaded files
   ```bash copy
   cd ~/Downloads && rm Electrum-4.5.5.tar.gz Electrum-4.5.5.tar.gz.asc ThomasV.asc
   ```
7. Do a system Restart
   ```bash copy
   sudo shutdown -r now
   ```   
### Create a Test wallet
We will set up a test (hot) wallet in Electrum so we can make sure all of our config settings are correct before actually loading up one of our own wallets that contains funds.
1. Run electrum. Run:
   ```bash copy
   electrum
   ```
2. At the welcome screen. Click ```Next```.
3. Rename ```default_wallet``` to ```test_hot_wallet``` (I think this name is less confusing). Click ```Next```.                                                                        
4. Select ```I already have a seed```. Click ```Next```.
5. Head over to [CoinPlate](https://getcoinplate.com/bip39-seed-phrase-mnemonics-generator-offline-online-tool/?v=2a6039655313 "CoinPlate's Homepage") and generate a Bip39 24 word seed phrase .
   Copy and save the seed phrase to a note on your desktop. Don't worry about security as this is just a simple test wallet for testing configuration settings.
6. Copy and paste the seed phase into the Electrum box that is displayed.
7. Click ```Options```, and select ```Bip39 seed```. Click ```OK```. Ignore the warning and click ```Next```.
8. Keep ```native segwit (p2wpkh)``` selected and click ```Next```.
9. Leave the password fields blank, we wont encrypt the wallet data as this is just a test wallet, click ```Next```.
10. Electrum - enable update check. Click ```No```. We will always verify and update our software ourselves.

### Add some watch-only addresses to check Electrs & Bitcoin Core are connected
1. From the Electrum window press CTRL+N, this will bring up the ```New/Restore``` menu.
2. Choose a name for your wallet, you can just name the wallet something like ```satoshi_address``` or ```mr100```. Click ```Next```.
3. Select ```Import Bitcoin addresses or private keys```, click ```Next```.
4. Add any bitcoin addresses that you would like to watch. A simple Google search can return some interesting famous bitcoin addresses. I added the following three addresses out of curiosty and fun. Just add the actual address (long string of 34 characters). i.e if you want to watch mr100 bitcoin address just add ```1Ay8vMC7R1UbyCCZRVULMV7iQpHSAbguJP``` and click ```Next```.

   - 1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa (satoshi_address, shows first Bitcoin transaction to Hal Finney) 
   - 35hK24tcLEWcgNA4JxpvbkNkoAcDGqQPsP (Huobi_address = Huobi exchange hot wallet)
   - 1Ay8vMC7R1UbyCCZRVULMV7iQpHSAbguJP (mr100_address = Unknown, transacts daily in block of 100 BTC)
   
6. No need to set a wallet password as your just watching someone elses address. Click ```Finish```.

   If it's an old address with lots of transactions it will take a while to sync.

===
## Configure Electrum 
1. Configure Electrum to only connect to your personal Bitcoin node via Electrs:
      Click ```Tools``` tab then ```Network```. Unselect ```Select server automatically```. And edit ```Server``` to read: ```localhost:50001:t```

2. Change Electrum base unit:
      Click ```Tools``` tab then ```Preferences```. Change Base unit from mBTC to BTC, then shut down Electrum.

3. Edit Electrum config file. Navigate to the hidden Electrum directory (this directory is only created once Electrum is run for the first time).
      ```bash copy
      cd ~/.electrum
      ```
4. Open config.ini file
   ```bash copy
   nano config
   ```
6. Replace the contents of the file with the below. save and exit:
   ```bash copy
   {
       "auto_connect": false,
       "check_updates": false,
       "decimal_point": 8,
       "log_to_file": true, 
       "oneserver": true,
       "proxy": "socks4:localhost:9050",
       "proxy_password": "",
       "proxy_user": "",
       "server": "localhost:50001:t"
   }
   ```
   This is what each config setting does:
      - auto_connect: false = Don't auto connect to read headers at startup
      - check_updates: false = Dont check for updates
      - decimal_point: 8 = Use BTC not sats
      - log_to_file: true = Log debugging to logs/ folder in data directory 
      - oneserver: true = Only connect to one server
      - proxy: socks4:localhost:9050 = Use tor proxy
      - proxy_password: "" = No password for tor proxy
      - proxy_user: "" = No user deets for tor proxy
      - server: localhost:50001:t = Connect to electrs without ssl
   
===
# Running Bitcoin-Core, Electra and Electrum.
1. Start Bitcoin-Core. Run:
   ```bash copy
   bitcoind -datadir=/media/rez/T7\ Shield -server -daemon
   ```
2. Check Bitcoin Core has synced
   ```bash copy
   bitcoin-cli -datadir=/media/rez/T7\ Shield getblockchaininfo | head
   ```
4. Start Electra
   ```bash copy
   ./target/release/electrs --log-filters INFO --network bitcoin --db-dir ./db --daemon-dir /media/rez/T7\ Shield
   ```
5. Start Electrum
   ```bash copy
   electrum
   ```
If ```Network``` light in the bottom righthand corner is blue then you are connected to your own node running behind Tor. It is now safe to interact with your real wallets.

---
## Connecting Ledger Live
https://jamesachambers.com/fix-linux-ledger-live-usb-connection/

Follow the link for instructions to allow Ledger Live to access Ledger HWW.

---
## Connecting Hardware Wallets to Electrum
Downgrade pip to be able to install all the dependencies required to add HWW support to Electrum:

python3 -m pip uninstall pip
python3 -m pip install pip==22


### Install the required Electrum dependencies for HWW support:
python3 -m pip install hidapi btchip-python ecdsa ledger-bitcoin
https://electrum.readthedocs.io/en/latest/hardware-linux.html

### Update Linux UDEV
https://github.com/spesmilo/electrum/tree/master/contrib/udev

Go to the above link to collect the correct https address for the HWW rules. make the address read https://raw.<github etc.*.rule.sh

like this for adding Ledger support:
wget -q -O - https://raw.github.com/LedgerHQ/udev-rules/blob/master/20-hw1.rules.sh | sudo bash

