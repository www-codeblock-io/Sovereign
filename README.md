# Sovereign - Bitcoin Personal Node
last updated: 8th September 2024.

# Project goal
A secure and sovereign Bitcoin node using software self-verified (non branded/containerised), enabling the use of hardware wallets while preserving the privacy of personal addresses and balances when interacting with the Bitcoin network.

I recommend in the first instance to always consult a software projects official installation instructions. This will have the most queries and bugs already logged by users and answered by the project maintainers. External guides can be out-of-date and not actively maintained. This guide has a subheading stating the last update and a link to each official software project page.

Think of this as a general guide with some trouble shooting tips. I have included all the Terminal commands that I personally ran to achieve the above goal.

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

---
## SOFTWARE
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
- Ledger Live
  
___
## VERIFY & INSTALL SOFTWARE

For all Bitcoin software, it’s a particularly important security step to verify the release. This is done to ensure the installation file you download has not been compromised. Follow download instructions and use gpg in the terminal as instructed.

___
# Install Ubuntu 22.04 LTS
I installed Ubuntu 22.04 LTS because at the time of install, the Tor project had not released support for Ubuntu 24.04 LTS.

### Deactivate Secure-Boot
If repurposing an old Windows laptop, Deactivate Secure-Boot.

Secure boot signing authorities have made mistakes in the past ([section 3, page 2 under 'Disadvantages'](https://www.nsa.gov/portals/75/documents/what-we-do/cybersecurity/professional-resources/BootSecurityModesAndRec_20190522.pdf)). I will verify all the software myself and not trust a third-party to do this for me.

1. Press the laptops power-on button then immediatly press the Function key that enters the Bios menu. For the HP Laptop I'm using it was the F2 key. [Bios key by manufacturer. ](https://gadgetmates.com/bios-key-by-manufacturer)
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


### Complete install
Follow the [official](https://ubuntu.com/tutorials/install-ubuntu-desktop#3-create-a-bootable-usb-stick "https://ubuntu.com/tutorials/install-ubuntu-desktop#3-create-a-bootable-usb-stick") installation instructions to install Ubuntu.

[Create partition table](https://ubuntuhandbook.org/index.php/2022/04/install-ubuntu-2204-step-by-step/amp/ "Ubuntuhandbook.org") (not always required).


### Reusing bootable USB
Flashing a USB drive with an ISO image will place a write protection on the USB drive, to prevent accidental deletion of your bootable USB drive data. If you make a mistake and need to reformat the USB drive then use something like [Aomei](https://www.aomeitech.com/download.html#pa "Aomitech.com") to remove the write protection so you can reformat the drive and start again (or to reclaim the USB drive to be used for something else).

---
## Configure Ubuntu
Once Installation has finished.
1. Update package manager. Run:
   ```bash copy
   sudo apt update && sudo apt upgrade
   ```

### Install Libfuse
[Official website](https://github.com/AppImage/AppImageKit/wiki/FUSE)
Installing libfuse2 for Ubuntu 22.04 LTS will allow us to run downloaded AppImages such as Specter and Ledger Live Linux desktop apps.
1. Install libfuse2
   ```bash copy
   sudo add-apt-repository universe && sudo apt install libfuse2
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
3. Uncomment ```HandleLidSwitch``` and make it equal to ```HandleLidSwitch=ignore```, save and exit.
   

4. Restart the systemd daemon (be aware that this will log you out of Ubuntu) with this command
   ```bash copy
   sudo systemctl restart systemd-logind
   ```

___
# Install Proton VPN
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


**_Connect to ProtonVPN whenever accessing a web browser on your machine._**

---
# Install Mullvad VPN (optional) 
[Mullvad](https://mullvad.net/en/download/vpn/linux "Mullvad.net") recommended by the official Tor website. It's a paid service but has no email requirement and can use BTC to pay for the service. The internet speed should also be faster than a free ProtonVPN account.

___
# Install Brave Browser (optional)
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
# Install Telegram Desktop (optional)
A quick solution for encrypted files/messages.
1. Install Telegram from apt repo. Run:
   ```bash copy
   sudo apt install telegram-desktop
   ```

---
# Install Bitcoin Core
Head over to [Bitcoin Core](https://bitcoincore.org/en/download/ "Bitcoincore.org") and:
- Download ```Linux(tgz) 64 bit``` and save to Downloads folder.
- Scroll down the page, under the heading ```Verify your download```, click on ```Linux verification instructions```, then follow the instructions to verify your download.

### Complete install
[Reference Link](https://bitcoin.org/en/full-node#linux-instructions "Bitcoin.org")

Unzip the bitcoin folder
```bash copy
tar xzf bitcoin-27.1-x86_64-linux-gnu.tar.gz
```
Install bitcoin core
```bash copy
sudo install -m 0755 -o root -g root -t /usr/local/bin bitcoin-27.1/bin/*
```
Clean up
```bash copy
cd ~/Downloads && sudo rm -r bitcoin-27.1-x86_64-linux-gnu.tar.gz guix.sigs SHA256SUMS.asc SHA256SUMS
```

---
## Run Bitcoin Core (start initial blockchain download)
Plug in your external 2T SSD drive.

1. Then start Bitcoin Core. Run:
   ```bash copy
   bitcoin-qt
   ``` 
   You will be greated with the Bitcoin Core Welcome screen, it is here that we will let Bitcoin Core know where our external SSD drive is located.

2. Select ```Use a custom data directory```.
3. Click the three dots to the right and navigate to your external SSD drive location. Mine was located at ```/media/rez/T7 Shield```.
4. Uncheck ```Limit block chain storage```. We want to download the complete Bitcoin blockchain from the genisis block.
5. Click ```OK```.

Bitcoin Core will now create the main ```data-directory``` inside your external SSD drive. It is here that all Bitcoin Core configuaration files will be stored along with the complete blockchain data. Get familiar with this ```data-directory``` (SSD Drive) location as we will be accessing it again later.

If you have trouble locating your external SSD Drive, try the following, open a new Terminal. Run:
   ```bash copy
   cd /media && dir
   ```
   You should see your user directory listed, ```cd``` into this dir which should contain your external drive.

6. Leave the laptop power plugged in and wait for the Bitcoin blockchain to download, Bitcoin Core will show a % complete and a rough estimate of how much time remains until completion.


---
## Configure Bitcoin Core
Once Bitcoin core is fully synced you will be greeted with a welcome screen asking if you would like to ```Create a new wallet```.  

We won't be using Bitcoin Cores hot wallet (will be using Electrum instead) so ignore this prompt and and instead open Bitcoin Core config file.

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
   bitcoin-cli -datadir=/media/<User Name>/<External SSD Name> stop
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

---
# Install Tor
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
12. Configure Tor to start at system Boot (optional but recommended)
   ```bash copy
   sudo systemctl enable tor
   ```

### Tor Systemctl commands
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
1. Start Bitcoin Core GUI 
```bash copy
bitcoin-qt
```
2. Once loaded enter the ```Network``` screen by pressing ```CTL+N```.
3. Click on the ```Peers``` tab.
   You should now see a list of all the Bitcoin peers that your node is connected too. Under the ```Network``` column all the row values should say ```Onion```, meaning you are only connected to    peers over the onion ntwork. 

### Verify Bitcoin Core is running behind Tor, option: 2
1. Check Bitcoin Core ```NetworkInfo```. From the Bitcoin Core GUI click 'Window' tab, 'Consol', then type the below command after the ```>``` and press enter.
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

1. Navigate to you main Bitcoin data-directory. Then activate tail with a grep on Tor.
   ```bash copy
   tail -f debug.log | grep tor
   ```
2. Now in a seperate Terminal start bitcoin-qt.
    ```bash copy
    bitcoin-qt
    ```
3. Once you see the pop up notification that Bitcoin Core is available (loaded). Then stop Bitcoin Core again.
    ```bash copy
    bitcoin-cli -datadir=/media/<User Name>/<External SSD Name> stop
    ```

   You will see a lot of the Tor actions printed to the Terminal that Bitcoin Core completed during start-up and shut-down, including starting a ```Tor control thread```, opening a ```Tor connection``` and finally connecting to the Tor network using your Tor encryption key.


---
# Install Electrs
[Official website](https://github.com/romanz/electrs/tree/master?tab=readme-ov-file)

[Why Electrs?](https://blog.casa.io/electrum-server-performance-report/)

### Build dependencies
1. Install recent Rust
   ```bash copy
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```
2. Install Cargo
   ```bash copy
   apt install cargo
   ```
3. Update package manager
   ```bash copy
   sudo apt update
   ```
4. Install dependencies for Rust-RocksDB
   ```bash copy
   sudo apt install clang cmake build-essential
   ```

I chose to compile electrs by statically linking to librocksdb, which has less dependencies.

5. Install dependencies 
   ```bash copy
   sudo apt install -y libgflags-dev libsnappy-dev zlib1g-dev libbz2-dev liblz4-dev libzstd-dev
   ```
6. Clone the repo
   ```bash copy
   git clone -b v7.8.3 --depth 1 https://github.com/facebook/rocksdb && cd rocksdb
   ```
7. Install using make
   ```bash copy
   make shared_lib -j $(nproc) && sudo make install-shared
   ```
8. Clean up directory
   ```bash copy
   cd .. && rm -r rocksdb
   ```
6. Prepare man page generation (optional)
   ```bash copy
   cargo install cfg_me
   ```
7. Download electrs repo
   ```bash copy
   cd ~/Downloads && git clone https://github.com/romanz/electrs
   ``` 
8. Move into electrs directory
   ```bash copy
   cd electrs
   ```
### Build
Note: you need to have enough free RAM to build electrs. The build will fail otherwise. Close those 100 old tabs in the browser. 

9. First build should take ~20 minutes
   ```bash copy 
   cargo build --locked --release
   ```
   During installation build the below warning was thrown, I installed as the warning suggested:

   ```warning: electrs (lib) generated 1 warning (run cargo fix --lib -p electrs to apply 1 suggestion)
    Finished release [optimized] target(s) in 5m 53s```

   I ran the command suggested above
   ```bash copy
   cargo fix --lib -p electrs
   ```

### Generating man pages
Run ```cfg_me man``` to see man page immediately or run ```cfg_me -o electrs.1 man``` to save it into a file (```electrs.1```).


---
## Configure Electrs
1. First lets move the Electrs folder to /usr/local/bin and keep it next to our Bitcoin Core executables
   ```bash copy
   mv ~/Downloads/electrs /usr/local/bin
   ```
2. Create a config file for Electrs
   ```bash copy
   cd ~ && mkdir .electrs && nano /.electrs/config.toml   
   ```
3. Copy below text into config.toml, save and exit
   ```bash copy
   # DO NOT EDIT THIS FILE DIRECTLY - COPY IT FIRST!
   # If you edit this, you will cry a lot during update and will not want to live anymore!
   
   # This is an EXAMPLE of how configuration file should look like.
   # Do NOT blindly copy this and expect it to work for you!
   # If you don't know what you're doing consider using automated setup or ask an experienced friend.
   
   # This example contains only the most important settings.
   # See docs or electrs man page for advanced settings.
   
   # File where bitcoind stores the cookie, usually file .cookie in its datadir
   cookie_file = "/media/USERNAME/<NAME OF EXTERNAL SSD>/.cookie"
   
   # The listening RPC address of bitcoind, port is usually 8332
   daemon_rpc_addr = "127.0.0.1:8332"
   
   # The listening P2P address of bitcoind, port is usually 8333
   daemon_p2p_addr = "127.0.0.1:8333"
   
   # Directory where the index should be stored. It should have at least 70GB of free space.
   db_dir = "/usr/local/bin/electrs/db"
   
   # bitcoin means mainnet. Don't set to anything else unless you're a developer.
   network = "bitcoin"
   
   # The address on which electrs should listen. Warning: 0.0.0.0 is probably a bad idea!
   # Tunneling is the recommended way to access electrs remotely.
   electrum_rpc_addr = "127.0.0.1:50001"
   
   # How much information about internal workings should electrs print. Increase before reporting a bug.
   log_filters = "INFO"

   ```
## Build Electrs server index
4. Check size of current Bitcoin Core block directory
   ```bash copy
   du -ch /media/<USER>/<NAME_OF_SSD_DRIVE>/blocks/blk*.dat | tail -n1
   ```
   This will print the size of the existing Bitcoin Core block directory. The final Electrs index DB will be about 10-20% the size of the Bitcoin Core block directory.

5. Start Electrs. First sync can take 1-2 days depending on CPU/hardware.
   ```bash copy
   cd /usr/local/bin/electrs && ./target/release/electrs --log-filters INFO --network bitcoin --db-dir ./db --daemon-dir /media/<USERNAME>/<NAME_OF_SSD_DRIVE
   ```
7. Check final size of Electrs server index DB
   ```bash copy
   du /usr/local/bin/electrs/db
   ```
## Monitoring Electrs
1. Install prometheus
   ```bash copy
   sudo apt install prometheus
   ```
3. Edit prometheus.yml file
   ```bash copy
   cd /etc/prometheus/ && sudo nano prometheus.yml
   ```
   Scroll to the bottom of the file and add the below, save and exit.
     ```bash copy
      - job_name: electrs
        static_configs:
          - targets: ['localhost:4224']
      ```
4. Restart prometheus
     ```bash copy
     sudo systemctl restart prometheus
     ```
6. Check collected metrics
     ```bash copy
      brave-browser 'http://localhost:9090/graph?g0.range_input=1h&g0.expr=index_height&g0.tab=0'
      ```
Click on ```Status``` tab then ```Runtime & Build Information``` tab to see if prometheus loaded and is pulling stats for further user querying.  
  
---
# Install Electrum
[Official website](https://electrum.org/#download)
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
6. Clean up
   ```bash copy
   cd ~/Downloads && sudo rm -r Electrum-4.5.5.tar.gz Electrum-4.5.5.tar.gz.asc ThomasV.asc
   ```
7. System Restart
   ```bash copy
   sudo shutdown -r now
   ```   

---
## Configure Electrum
1. The Electrum config file is located in a hidden Electrum directory, however this directory is only created once Electrum is run for the first time. To prevent any privacy leaks we can run Electrum with some flags to make sure it only connects to our Electrs server.
   ```bash copy
   electrum --oneserver --server 127.0.0.1:50001:t --proxy socks5:127.0.0.1:9150
   ```
   Once Electrum loads, immediatly close it down again so we can edit the config file that was just created.
 
2. Open Electrum config file
   ```bash copy
   cd ~/.electrum && nano config
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

The next time we start Electrum we can simply run the command ```electrum``` and let the config file handle the rest.

### Create a Test wallet
We will set up a test (hot) wallet in Electrum so we can make sure all of our config settings are correct before actually loading up one of our own wallets that contains funds.
1. Start electrum. Run:
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

### Add some watch-only addresses
We will use these to confirm Electrs & Bitcoin Core are connected correctly.
1. From the Electrum window press CTRL+N, this will bring up the ```New/Restore``` menu.
2. Choose a name for your wallet, you can just name the wallet something like ```satoshi_address``` or ```mr100```. Click ```Next```.
3. Select ```Import Bitcoin addresses or private keys```, click ```Next```.
4. Add any bitcoin addresses that you would like to watch. A simple Google search can return some interesting famous bitcoin addresses. I find it more intuative to create a new watch wallet for each address. Out of curiosty and fun, I added the following three addresses to three individual wallets named ```satoshi_address, huobi_address and mr100_address```. Just add the actual address (long string of 34 characters). i.e if you want to watch mr100 bitcoin address just add ```1Ay8vMC7R1UbyCCZRVULMV7iQpHSAbguJP``` and click ```Next```.

   - 1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa (satoshi_address, shows first Bitcoin transaction to Hal Finney) 
   - 35hK24tcLEWcgNA4JxpvbkNkoAcDGqQPsP (Huobi_address = Huobi exchange hot wallet)
   - 1Ay8vMC7R1UbyCCZRVULMV7iQpHSAbguJP (mr100_address = Unknown, transacts daily in blocks of 100 BTC)
   
6. No need to set a wallet password as your just watching someone elses Bitcoin address. Click ```Finish```.
 
---
# Running, Bitcoin-Core, Electra & Electrum.
1. Start Bitcoin-Core. Run:
   ```bash copy
   bitcoind -datadir=/media/<USERNAME>\<NAME_OF_SSD_DRIVE> -server -daemon
   ```
2. Start Electra
   ```bash copy
   ./target/release/electrs --log-filters INFO --network bitcoin --db-dir ./db --daemon-dir /media/<USERNAME>\<NAME_OF_SSD_DRIVE>
   ```
5. Start Electrum
   ```bash copy
   electrum
   ```
If ```Network``` light in the bottom righthand corner is blue then you are connected to your own node running behind Tor. It is now safe to interact with your real wallets.

---
# Install Specter wallet
[Official Website](https://specter.solutions/index.html)
1. Create a new directory in Downloads folder called ```Specter``` and move into it.
   ```bash copy
   mkdir ~/Downloads/Specter && cd ~/Downloads/Specter
  ```
3. Download Specter, SHA256SUMS and SHA256SUMS.asc to Downloads folder
   ```bash copy
   wget https://github.com/cryptoadvance/specter-desktop/releases/download/v2.0.5/specter_desktop-v2.0.5-x86_64-linux-gnu.tar.gz https://github.com/cryptoadvance/specter-desktop/releases/download/v2.0.5/SHA256SUMS https://github.com/cryptoadvance/specter-desktop/releases/download/v2.0.5/SHA256SUMS.asc

4. Now head over to [here](https://specter.solutions/downloads/) and click on ```Verify signiture```, below ```For Linux```. Follow the instructions to verfiy both the signiture and the file.

   On step #2, Brave browser blocked the download, I had to manually resume/approve the download by clicking on the small yellow triangle in the top righthand corner of the browser window. I also had to rename the file to ```pgp_keys.asc``` as Brave-browser wanted to name the file by the long RSA number.

5. Extract the download
   ```bash copy
   tar -xvf ~/Downloads/Specter/specter_desktop-v2.0.5-x86_64-linux-gnu.tar.gz
   ```
6. Set AppImage to executable and move to Desktop
   ```bash copy
   chmod +x Specter-2.0.5.AppImage && mv Specter-2.0.5.AppImage ~/Desktop
   ```
7. Start Specter by double clicking the AppImage on the Desktop or Open a new Terminal window (```CTRL+ALT+T```)
   ```bash copy
   cd ~/Desktop && ./Specter-2.0.5.AppImage
   ```

### Set UDEV rules
8. Naigate to Specter directory
   ```bash copy
   cd ~/Downloads/Specter
   ```
9. Apply these rules by copying them to `/etc/udev/rules.d/` and notifying `udevadm`.
Your user will need to be added to the `plugdev` group, which needs to be created if it does not already exist. Run the following commands one at a time:
  ```bash copy
  sudo cp udev/*.rules /etc/udev/rules.d/
  ```
  ```bash copy
  sudo udevadm trigger
  ```
  ```bash copy
  sudo udevadm control --reload-rules
  ```
  ```bash copy
  sudo groupadd plugdev
  ```
  ```bash copy
  sudo usermod -aG plugdev `whoami`
  ```
   
10. Clean up, delete old files/directories
   ```bash copy
   cd ~ && rm -r ~/Downloads/Specter   

---
# Install Sparrow wallet
???

---
# Hardware Wallet support
To allow Electrum to use Hardware Wallets, downgrade pip to allow installation of required dependencies:
```bash copy
python3 -m pip uninstall pip && python3 -m pip install pip==22
```
## Electrum HWW support
[Reference Link](https://electrum.readthedocs.io/en/latest/hardware-linux.html)
1. Install dependencies
   ```bash copy
   python3 -m pip install hidapi btchip-python ecdsa ledger-bitcoin
   ```
   Our Linux system will also require udev rules from each Hardware device manufacturer. These were all included during the installation of Specter (as Specter privided a handy folder containing all the most popular hardware device Udev rules).
   
## Install Ledger Live (optional)
If you are still using Ledger or needing access to to the Ledger Live suite then follow below instructions to set required Udev rules so you can connect your Ledger device.

1. Head over to [Ledger Live](https://download.live.ledger.com/latest/linux) and download the Linux .Appimage file to your Downloads folder.
2. Add UDEV rules for Ledger Hardware Wallet (USB device)
   ```bash copy
   wget -q -O - https://raw.githubusercontent.com/LedgerHQ/udev-rules/master/add_udev_rules.sh | sudo bash
   ```
3. Set Ledger Live AppImage file to executable
   ```bash copy
   chmod +x ledger-live-desktop-2.85.1-linux-x86_64.AppImage
   ```
4. Shorten the name and move to the Desktop
   ```bash copy
   mv ledger-live-desktop-2.85.1-linux-x86_64.AppImage ~/Desktop/LedgerLive-2.85.1.AppImage
   ```
3. Start Ledger Live by double clicking the AppImage file on the Desktop or Open a new Terminal window (```CTRL+ALT+T```)
   ```bash copy
   cd ~/Desktop && ./LedgerLive-2.85.1.AppImage
   ```
