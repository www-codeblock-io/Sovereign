# Sovereign - Bitcoin Personal Node

# Software
- Ubuntu 22.04 LTS
- Bitcoin-Core
- Tor
- ProtonVPN
- EPS (Electrum Personal Server)
- Electrum wallet
- Specter wallet
- Sparrow wallet

(optional)
- Brave Browser
- Mullvad VPN
- Telegram

---
## HARDWARE

Bitcoin Core minimum system requirements:
- Ram: 1GB
- CPU: Some ARM chipsets >1.GHz
- HDD: 650GB (however large the Bitcoin blockchain currently is.)
- Download: 500 MB/day (15 GB/month)
- Upload: 5 GB/day (150 GB/month)

Any old laptop you have laying around should meet the above specifications. Just add an external 2T SSD drive with an unlimited broadband connection.

I purchased a second hand entry level [HP portable laptop](https://www.amazon.com/gp/aw/d/B0C9N3H8F2/ref=ox_sc_act_image_1?smid=AYY6S2IYYQJGA "Amazon HP Laptop") from Amazon.com for $140.

- Ram: 16GB
- CPU: Intel celeron 1.1 GHz
- HDD: 64GB

I then added this external 2T SSD drive for $140.
[Link](https://a.co/d/itXEh1A "Amazon.com")

___
## VERIFY & INSTALL SOFTWARE

For all Bitcoin software, it’s a particularly important security step to verify the release. This is done to ensure the installation file you download has not been compromised. Follow download instructions and use gpg in the terminal as instructed.

___
## Install Linux Ubuntu 22.04 LTS
We're installing Ubuntu 22.04 LTS because at the time of writing the Tor project have not released a package for Ubuntu 24.04.4 LTS.

### Deactivate Secure-Boot
Secure boot signing authorities have made mistakes in the past ([section 3, page 2 under 'Disadvantages'](https://www.nsa.gov/portals/75/documents/what-we-do/cybersecurity/professional-resources/BootSecurityModesAndRec_20190522.pdf)). We will verify all the software ourselves and not trust a third-party to do this for us.

Enter into the laptops Bios menu:

Press the laptops power-on button then immediatly press the Function key that enters the Bios menu. For the HP Laptop i'm using it was the F2 key, but it could also be F9, F10, F11 or F12 depending on the laptop brand. A quick google search should provide an accurate answer.

Once the system Bios has loaded, proceed to turn-off ```Secure Boot``` (usually located under the ```security``` tab), save and exit. Then turn-off the laptop.

On a seperate PC head over to [Ubuntu Releases](https://releases.ubuntu.com/22.04/ "Ubuntu releases") and download the Ubuntu ISO image ```Ubuntu-22.04.4-desktop-amd64.iso```, save the ISO file to your Downloads folder:


Also save the ```SHA256SUMS``` and ```SHA256SUMS.gpg``` files to the same folder.


### Verify download signitures
Note, the slightly different commands, depending if you are on Windows or Linux.
[Reference Link](https://ubuntu.com/tutorials/how-to-verify-ubuntu#2-necessary-software "Ubuntu tutorial")

#### Windows10
Verify signitures. Open a Terminal window (CTRL+ALT+T). Run:
```bash copy
gpg --keyid-format long --verify SHA256SUMS.gpg SHA256SUMS.txt
```
Verify ISO file, Run:
```bash copy
certutil -hashFile <ISO_file_path> SHA256
```
Now open the SHA256SUMS.txt file in your Downloads folder and compare the SHA256 checksum inside the file with the checksum that was printed to the Terminal. It will be a long hash, verify both hashes/numbers match, if so, your ISO download is authentic.

#### Linux/MacOS
Verify signitures. Open a Terminal window (CTRL+ALT+T). Run:
```bash copy
gpg --keyid-format long --verify SHA256SUMS.gpg SHA256SUMS.txt
```
Verify ISO file, Run:
```bash copy
sha256sum -c SHA256SUMS 2>&1 | grep OK
```
The output you want will look similar to the following:

```ubuntu-22.04.4-desktop-amd64.iso: OK```


### Install OS:
Read all of the below instructions (including links) before proceeding.

General installation instructions:
[Link](https://ubuntu.com/tutorials/install-ubuntu-desktop#3-create-a-bootable-usb-stick "https://ubuntu.com/tutorials/install-ubuntu-desktop#3-create-a-bootable-usb-stick")

Create correct partition table (not always required):
[Link](https://ubuntuhandbook.org/index.php/2022/04/install-ubuntu-2204-step-by-step/amp/ "Ubuntuhandbook.org")

Flashing a USB drive with an ISO image will place a write protection on the USB drive, this is to prevent accidental deletion of your bootable USB drive data. If you make a mistake and need to reformat the USB drive then use something like [Aomei Partition Assistant](https://www.aomeitech.com/download.html#pa "Aomitech.com") to remove the write protection so you can reformat the drive and start again (or to reclaim the USB drive to be used for something else).


### Once Installation has finished
Update package manager. Run:
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
Navigate to systemd directory. Run:
```bash copy
cd /etc/systemd
```
Open logind configuration file. Run: 
```bash copy
sudo nano logind.conf
```
Uncomment HandleLidSwitch and make it equal to inore, save and exit:

```HandleLidSwitch=ignore```

Restart the systemd daemon (be aware that this will log you out of Ubuntu) with this command. Run:
```bash copy
sudo systemctl restart systemd-logind
```

___
## Install Proton VPN
Download repo. Run:
```bash copy
wget https://repo.protonvpn.com/debian/dists/stable/main/binary-all/protonvpn-stable-release_1.0.4_all.deb
```

Install repo. Run:
```bash copy
sudo dpkg -i ./protonvpn-stable-release_1.0.4_all.deb && sudo apt update
```

Check the repo package integrity by checking its checksum. Run:
```bash copy
echo "62a9d849835de8a5664cf95329458bf1966780b15cec420bf707b5f7278b9027  protonvpn-stable-release_1.0.4_all.deb" | sha256sum --check -
```

Install ProtonVPN desktop app. Run:
```bash copy
sudo apt install proton-vpn-gnome-desktop
```

Check for updates to ensure you’re running the latest version:
```bash copy
sudo apt update && sudo apt upgrade
```

Linux system tray icon (optional).
   
   By default, GNOME doesn’t support tray icons. Enable this functionality. Run:
```bash copy
sudo apt install libayatana-appindicator3-1 gir1.2-ayatanaappindicator3-0.1 gnome-shell-extension-appindicator
```
Headover to [ProtonVPN](https://account.protonvpn.com/signup "ProtonVPN.com") and Sign-up to a free account.


**_Connect to ProtonVPN whenever accessing a web browser on your node._**

---
## Install Mullvad VPN (optional) 
[Mullvad](https://mullvad.net/en/download/vpn/linux "Mullvad.net") recommended by the official Tor website. It's a paid service but has no email requirement and can use BTC to pay for the service. The internet speed should also be faster than a free ProtonVPN account.

___
## Install Brave Browser (optional)
Enter the following commands one at a time. Run:
```bash copy
sudo apt install curl
```
```bash copy
sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg
```
```bash copy
echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg] https://brave-browser-apt-release.s3.brave.com/ stable main"|sudo tee /etc/apt/sources.list.d/brave-browser-release.list
```
```bash copy
sudo apt update && sudo apt install brave-browser
```

___
## Install Telegram Desktop (optional)
A quick solution for encrypted files/messages.
```bash copy
sudo apt install telegram-desktop
```
___
## Install Bitcoin Core

Run the below commands to install from Source code. [Resource link](https://github.com/bitcoin/bitcoin/blob/master/doc/build-unix.md "Github.com/bitcoin").

Additional video tutorial, for further reference.
[Canadian Bitcoiners](https://youtu.be/C8i0uJHmUoQ?si=gjiJaak2_b6ZT_CN)

Enter the below commands, one at a time. Run:
```bash copy
sudo apt-get install build-essential libtool autotools-dev automake pkg-config bsdmainutils python3
```
```bash copy
sudo apt-get install libevent-dev libboost-dev
```
```bash copy
sudo apt install libsqlite3-dev
```
```bash copy
sudo apt-get install libzmq3-dev
```
```bash copy
sudo apt install systemtap-sdt-dev
```
```bash copy
sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools
```
```bash copy
sudo apt install qtwayland5
```
```bash copy
sudo apt-get install libqrencode-dev
```
```bash copy
sudo apt-get install git
```
```bash copy
mkdir -p src && cd src
```
```bash copy
git clone https://github.com/bitcoin/bitcoin.git
```
```bash copy
cd bitcoin
```
Check out the most recent release (or whatever version you want to use). You can check what the latest release is by heading over to [Github/bitcoin](https://github.com/bitcoin/bitcoin) scroll down the page and on the right, under the 'Releases' heading, you will see the latest release number stated.
```bash copy
git checkout v26.2   
```
```bash copy
./autogen.sh
```
```bash copy
./configure
```

This next command will take a very long time, maybe around 1 hour as all of the code gets compiled. Be patient and don't run anything else on the machine while this job is running.
```bash copy
make
```

This also takes some time, it will run some tests to check everything has been made correctly, be patient... come back in 15-30 minutes.
```bash copy
Make check
```

This will be quick.
```bash copy
sudo make install
```

### If the above fails
The other option you have is to install using a binary package. Follow the links below (make sure to verify your download before installing).

Download Bitcoin Core
[Link](https://bitcoincore.org/en/download/ "Bitcoincore.org")

Install instructions
[Link](https://bitcoin.org/en/full-node#linux-instructions "Bitcoin.org")

---
## Run Bitcoin Core (start initial blockchain download)
Plug in your external 2T SSD drive.

Then start Bitcoin Core. Run:
```bash copy
bitcoin-qt
``` 
You will be greated with the Bitcoin Core Welcome screen, it is here that we will let Bitcoin Core know where our external SSD drive is located.

1. Select 'Use a custom data directory'.
2. Click the three dots to the right and navigate to your external SSD drive location. Mine was located at ```/media/rez/T7 Shield```.
4. Uncheck ```Limit block chain storage```. We want to download the complete Bitcoin blockchain from the genisis block.
5. Click ```OK```.

Bitcoin Core will now create the main 'data-directory' inside your external SSD drive. It is here that all Bitcoin Core configuaration files will be stored along with the complete blockchain data. Get familiar with this 'data-directory' (SSD Drive) location as we will be accessing it later.

If you have trouble locating your external SSD Drive, try the following, open a new Terminal. Run:
```bash copy
cd /media && dir
```
You should see your user directory listed, ```cd``` into this dir which should contain your external drive.

Leave the laptop power plugged in and wait for the Bitcoin blockchain to download, Bitcoin Core will show a % complete and a rough estimate of how much time remains until completion. Once the download has completed you will be greated with a Welcome screen and a prompt to create a new wallet.


### Allow incoming connections
Configure your WIFI router to allow Bitcoin core incoming connections on port:8332. If you do not do this then EPS and electrum will not be able to connect to your node.
[Instructions](https://bitcoin.org/en/full-node#network-configuration)

### Edit Bitcoin core config file. Run:
Once Bitcoin core is fully synced you will be greeted with a welcome screen asking if you would like to ```Create a new wallet```.  We won't actually use the bitcoin-core wallet so we will ignore this prompt and proceed to edit the bitcoin.config file.

From the Bitcoin Core GUI click on ```Settings```, ```Options```, then ```Open Configuration File```.

Enter the below text, save and exit:
```bash copy
# server=1 (RBS) needs to be on for EPS to share data/commnuicate with Bitcoin Core. txindex=1 allows any transaction to be looked up by Bitcoin Core, not just your own wallet.
server=1
txindex=1
listen=1

proxy=127.0.0.1:9050
bind=127.0.0.1

# only connect to Tor hidden services, not even IPv4/IPv6 nodes
onlynet=onion

# we won't actually use the bitcoin-core wallet
# However, if you use JoinMarket, you do need it
disablewallet=1

# If running tor, walletbroadcast=0 prevents the node from rebroadcasting transactions without tor.
walletbroadcast=0
```

You can now close Bitcoin Core by either clickig on the ```X``` or by closing the terminal window where you run the original ```bitcoin-qt``` commmand.

___
## Install Tor

[Official website](https://support.torproject.org/apt/tor-deb-repo/)

Verify your CPU architecture. Run:
```bash copy
dpkg --print-architecture
```

Install apt-transport-https. Run:
```bash copy
sudo apt install apt-transport-https
```

Check what Linux Distribution you have installed. Run:
```bash copy
lsb_release -c
```

Create a new file in /etc/apt/sources.list.d/ named tor.list. Run:
```bash copy
cd /etc/apt/sources.list.d/
```

Open the file. Run:
```bash copy
sudo nano tor.list
```

Add the following entries inside the file, save and close:
```bash copy
   deb     [arch=amd64 signed-by=/usr/share/keyrings/deb.torproject.org-keyring.gpg] https://deb.torproject.org/torproject.org jammy main
   deb-src [arch=<amd64 signed-by=/usr/share/keyrings/deb.torproject.org-keyring.gpg] https://deb.torproject.org/torproject.org jammy main
```

Update apt. Run:
```bash copy
sudo apt update
```

Install Tor. Run:
```bash copy
sudo apt install tor deb.torproject.org-keyring
```

Check Tor is installed. Run:
```bash copy
tor --version
```
### Configure Tor to start at system Boot (optional but recommended)
Enable Tor to start automatically on boot, this is your sovereign bitcoin node, don't use it to surf the web aimlessly. Run:
```bash copy
sudo systemctl enable tor
```

### Controlling Tor
Manage Tor using systemd:
```bash copy
sudo systemctl restart tor
```
To check the status:
```bash copy
sudo systemctl status tor
```

---
## Set up Tor hidden service
Configure Bitcoin Core to only connect using Tor. [Reference link](
https://en.bitcoin.it/wiki/Setting_up_a_Tor_hidden_service)

Additional video tutorial, for further reference: [Canadian Bitcoiners](
https://www.youtube.com/watch?v=goTkOt8Rr1Q&list=PLZqQw4mLW-r3UEyL12FG4koZq6lOLsURF&index=3)

Make changes to Tor configuration file. Run:
```bash copy
cd /etc/tor
```
```bash copy
sudo nano torrc
```

Enter the below text at the bottom of the file, save and exit:
```bash copy
ControlPort 9051
CookieAuthentication 1
CookieAuthFileGroupReadable 1
```

Check what your Bitcoin Core user name is. Run:
```bash copy
ps -eo user,group,comm |egrep 'bitcoind|bitcoin-qt' |awk '{print "Bitcoin user: " $1}'
```

Now add your Bitcoin Core user name to your Tor group. For example my command would be ```sudo usermod -a -G debian-tor rez``` as my Bitcoin user name is rez. Run:
```bash copy
sudo usermod -a -G debian-tor <user name found from above command>
```
If the above command did not work, double check your Tor user group name, run this command for a list of user groups, look for a user group with Tor in it's name. Run:
```bash copy
getent group
```

The above configuration sets up an automatic hidden service that is initiated by Bitcoin Core. On the next startup of bitcoin, Bitcoin Core will generate a file called ```onion_private_key``` in the data directory. KEEP THIS SAFE. If someone copies this file they can run a server with your .onion address. While a malicious party cannot necessarily associate the server with you as a person, as long as your server has the same xxxx.onion address they will know it is run by the same person.

If you delete this file, the next time bitcoind loads it will generate a new key file and xxxxxxxx.onion address.  For absolute security delete your ```onion_private_key``` file at each reboot or some frequent interval.

Close the Bitcon Core GUI by clicking on the ```X``` in the top right hand corner.

### Verify Bitcoin Core is running behind Tor, option: 1
1. Start Bitcoin Core GUI.
2. Once loaded enter the ```Network``` screen by pressing CTL+N.
3. Click on the ```Peers``` tab.
You should now see a list of all the Bitcoin peers that your node is connected too. Under the ```Network``` column all the row values should say ```Onion```, meaning you are only connected to peers over the onion ntwork.  


### Verify Bitcoin Core is running behind Tor, option: 2
Check Bitcoin Core is now running behind Tor, from the Bitcoin Core GUI click on 'Window', 'Consol', then type the below command into the consol shell after the > and press enter. Run:
```bash copy
getnetworkinfo
```

Scroll up until you see the networks section. Your node is running behind Tor if the 'ipv4' and 'onion' networks have the below settings (there will also be other settings in each network group but they can be ignored):
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
For the extra security concious, you can also check the debug.log file in the Bitcoin data-directory.

At some point during startup in the bitcoin/debug.log file you will see:
```tor: Got service ID XXXXXXXXXXX, advertising service XXXXXXXXXXX.onion:8333```

### Verify Bitcoin Core is running behind Tor, option: 4
You will learn a lot if you ```cd``` into your bitcoin data-directory delete the 'debug.log' file (don't worry bitcoin core just creates a new one at startup), restart Tor, and then Bitcoin Core. Then shut down Bitcoin core again once the application has completly loaded. Bitcoin Core will have created a new debug.log file and printed some debug statements during shut down.

Take a look at the new debug.log file. Run:
```bash copy
nano debug.log
```
You will see a lot of the actions listed that the bitcoin core software completed during start-up and shut-down, including starting a ```Tor control thread```, opening a ```Tor connection``` and finally connecting to the Tor netork using your Tor encryption key.

---
## Install EPS (Electrum Private Server)
https://armantheparman.com/bitcoin-core-and-electrum-personal-server-electrum-wallet-on-a-raspberry-pi/

https://armantheparman.com/electrum-server/

### Download EPS source code and signiture
Head over to [electrum-personal-server](https://github.com/chris-belcher/electrum-personal-server/releases) and make a note of the latest release version number. At time of writing it is ```0.2.4```, update the below command to the latest version number if this is now out of date. Run:
```bash copy
cd ~/Downloads && wget https://github.com/chris-belcher/electrum-personal-server/archive/refs/tags/eps-v0.2.4.tar.gz
```

Download the file signiture. Run:
```bash copy
wget https://github.com/chris-belcher/electrum-personal-server/releases/download/eps-v0.2.4/eps-v0.2.4.tar.gz.asc
```

Download Chris Belchers public key, there are multiple links on this [page](https://github.com/chris-belcher/electrum-personal-server/releases) for you to manually verify Chris Belchers public key. Run:
```bash copy
wget https://github.com/chris-belcher/electrum-personal-server/raw/master/docs/pubkeys/belcher.asc
```

Import the key to your system keychain. Run:
```bash copy
gpg --import belcher.asc
```
Verify the source file was signed by Chris Belcher. Run:
```bash copy
gpg --verify eps-v0.2.4.tar.gz.asc
```
In the Terminal output, you want to see ```Good Signiture from Chris Belcher```, ignore the warning.

### Prepare for the build
Extract the source file, rename and save onto your Desktop, change ```rez``` in the command to your system user name. Run:
```bash copy
tar -xvf eps-v0.2.4.tar.gz -C /home/rez/Desktop
```
Navigate to Desktop and rename the extracted file to ```eps```. Remember to change the file version number if you downloaded a newer version of EPS. Run:
```bash copy
cd ~/Desktop && mv electrum-personal-server-eps-v0.2.4 eps
```
Update the package list. Run:
```bash copy
sudo apt update
```
Install pip3. Run:
```bash copy
sudo apt install python3-pip
```
Check pip3 was installed.
```bash copy
pip3 --version
```
Upgrade pip3 to the latest version. Run:
```bash copy
sudo pip3 install --upgrade pip
```
### Complete EPS installation
Navigate into the EPS directory. Run:
```bash copy
cd ~/Desktop/eps
```

Complete installation by running the below command. Note: do not use ```sudo``` and make sure to leave in the final ```.```  
```bash copy
pip3 install --user . 
```

Ignore the Terminal warning about PATH, a PATH veriable will be added the next time you log out and back into the laptop again.

Clean up Downloads folder, remove downloaded files. Run:
```bash copy
cd ~/Downloads && rm belcher.asc eps-v0.2.4.tar.gz eps-v0.2.4.tar.gz.asc
```

## Don’t run EPS yet!
## First complete these two tasks:
- Wait for Bitcoin Core to fully synchronise.
- Install Electrum and create a test wallet.

---
## Install Electrum
Download ThomasV public key and import into keychain. Run:
```bash copy
cd ~/Downloads && wget https://raw.githubusercontent.com/spesmilo/electrum/master/pubkeys/ThomasV.asc && gpg --import ThomasV.asc
```

[Resource link](https://electrum.org/#download)

### Installation from Python sources
Install dependencies. Run:
```bash copy
sudo apt-get install python3-pyqt5 libsecp256k1-dev python3-cryptography
```
Download package. Run:
```bash copy
wget https://download.electrum.org/4.5.5/Electrum-4.5.5.tar.gz
```
Verify signitures. Run:
```bash copy
wget https://download.electrum.org/4.5.5/Electrum-4.5.5.tar.gz.asc
gpg --verify Electrum-4.5.5.tar.gz.asc
```
Install with PIP. Run:
```bash copy
sudo apt-get install python3-setuptools python3-pip
python3 -m pip install --user Electrum-4.5.5.tar.gz
```
Clean up Downloads folder, remove downloaded files. Run:
```bash copy
cd ~/Downloads && rm Electrum-4.5.5.tar.gz Electrum-4.5.5.tar.gz.asc ThomasV.asc
```
### Create a Test wallet
We will set up a test (hot) wallet in Electrum so we can make sure all of our config settings are correct before actually loading up one of our wallets that contains funds.
Run electrum. Run:
```bash copy
electrum
```
1. At the welcome screen. Click ```Next```.
2. Rename ```default_wallet``` to ```test_wallet``` (I think this name is less confusing). Click ```Next```.                                                                        
3. Select ```I already have a seed```. Click ```Next```.
4. Head over to [CoinPlate](https://getcoinplate.com/bip39-seed-phrase-mnemonics-generator-offline-online-tool/?v=2a6039655313 "CoinPlate's Homepage") and generate a Bip39 24 word seed phrase .

Copy and save the seed phrase to a note on your desktop. Don't worry about security as this is just a simple test wallet for testing configuration settings.

6. Copy and paste the seed phase into the Electrum box that is displayed.
7. Click ```Options```, and select ```Bip39 seed```. Click ```OK```. Ignore the warning and click ```Next```.
8. Keep ```native segwit (p2wpkh)``` selected and click ```Next```.
9. Leave the password fields blank, we wont encrypt the wallet data as this is just a test wallet, click ```Next```.
10. Electrum - enable update check. Click ```No```. We will always verify and update our software ourselves.

### Export your ```test_wallet``` public key
In Electrum, click on the ```Wallet``` tab, then ```Information```. Copy and save the long zpub key displayed. We will need to enter this public key into the EPS config.ini file later.

---
## Configure Electrum 
1. Configure Electrum to only connect to your personal Bitcoin node:
   
      Click ```Tools``` tab then ```Network```. Unselect ```Select server automatically```. And edit ```Server``` to read: ```127.0.0.1:50002```

2. Change Electrum base unit:

      Click ```Tools``` tab then ```Preferences```. Change Base unit from mBTC to BTC, then shut down Electrum.

3. Edit Electrum config file:

   Navigate to the hidden Electrum directory (this directory is only created once Electrum is run for the first time). Run:
   ```bash copy
   cd ~/.electrum
   ```
   Open config.ini file. Run:
   ```bash copy
   nano config.ini
   ```
   Edit four fields in Electrums config.ini file, to match the below (leave the other fields untouched). 
   - auto_connect: false
   - check_updates: false
   - oneserver: true
   - server: 127.0.0.1:50002:s


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

1. Under ```[master-public-keys]``` add any wallet public key (xpub, ypub, zpub) that you would like to have access too. EPS is Electrums link to your Bitcoin node, If you do not add the wallet public key (xpub, ypub, zpub) to this EPS config.ini file then Electrum will not use your node to pull transaction information from your node. Any wallet that you want to use with Electrum needs the public key added to this EPS config.ini file.

2. Under the ```[bitcoin-rpc]``` heading, add the path to your external SSD drive in the ```datadir``` section :

   ```datadir = /media/rez/T7 Shield```

The above path is to my SSD drive, you will need to change the user name ```rez``` to whatever your user name is and also the name of your SSD drive.

3. Under the ```[watch-only-addresses]``` heading, add any bitcoin addresses that you would like to watch. A simple Google search can return some interesting famous bitcoin addresses. I added the following three addresses out of curiosty and fun. 

   ```satoshi_address = 1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa```

   ```huobi_address = 35hK24tcLEWcgNA4JxpvbkNkoAcDGqQPsP```

   ```mr100_address = 1Ay8vMC7R1UbyCCZRVULMV7iQpHSAbguJP```

4. Save and exit out of the config.ini file.
5. Logout, and log in again.
6. Navigate to the EPS directory. Run:
   ```bash copy
   cd ~/Desktop/eps
   ```
7. Run the EPS server. The first time the server is run it will import all configured addresses as watch-only into the Bitcoin node, and then exit. Run:
   ```bash copy
   electrum-personal-server config.ini
   ```
8. If the wallets contain historical transactions you need to run a rescan to pull all of the transaction data. Run:
   ```bash copy
   electrum-personal-server --rescan config.ini
   ```
   It will ask for the earliest wallet creation date. Enter 1 to start from block height 1, and y to confirm. The first          initial scan can take a while, but a pop up box will be displayed with a % completion displayed.

   Once the server has finished scanning it will gracefully shut down on it's own, so you can leave the server to do it's job    and come back in an hour or so.
   
---
# Running Bitcoin-Core, EPS and Electrum.
When using Electrum connected to your own personal Bitcoin node via EPS (electrum personal server), you can only use one electrum wallet at a time. In our case Electrum Desktop Wallet. Here are the steps you need to follow:

1. Add the Wallet public key to EPS config file (as described above).
2. Start Bitcoin-Core, wait for it to fully sync. Run:
   ```bash copy
   bitcoin-qt
   ```
3. Start EPS. Run:
   ```bash copy
   cd ~/Desktop/eps && electrum-personal-server config.ini
   ```
4. Start Electrum. Run:
   ```bash copy
   electrum
   ```
Now interact with your wallet as usual but with the peace of mind that you are connected to your own node rnning behind Tor.
