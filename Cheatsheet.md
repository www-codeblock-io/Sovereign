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



Adding Bash Script to Desktop
To add a bash script to the desktop with a launchable icon on Ubuntu-based systems, follow these steps:

Create the script: Write your bash script in a file named bash.sh (or any other name you prefer, but make sure it has a .sh extension). Save it in a location you prefer, such as ~/Desktop.
Make the script executable: Open a terminal and navigate to the directory where your script is located. Run the command chmod +x bash.sh to make the script executable.
Create a .desktop file: Create a new file named bash.desktop in the same directory as your script. This file will contain the desktop entry information.
Here’s an example bash.desktop file:

[Desktop Entry]
Version=1.0
Type=Application
Terminal=true
Icon=/path/to/icon.png
Name=Bash Script
Exec=/path/to/bash.sh
Comment=Run Bash Script

Replace /path/to/icon.png with the actual path to the icon you want to use, and /path/to/bash.sh with the actual path to your script.

Configure the icon: You can specify the icon file in the Icon field. If you don’t have an icon file, you can create one or use a default icon. For example, you can use the gnome-panel-launcher icon:
Icon=gnome-panel-launcher

Save and validate the .desktop file: Save the bash.desktop file and validate it using the desktop-file-validate command:
desktop-file-validate bash.desktop

If there are no errors, the file is valid.

Move the .desktop file to a suitable location: Move the bash.desktop file to a location where GNOME (or your desktop environment) can find it. Common locations include:
~/Desktop (your desktop directory)
~/.local/share/applications (your user’s application directory)
/usr/share/applications (system-wide application directory)
Log out and log back in: After moving the .desktop file, log out and log back in to your system to ensure the icon is updated.
Verify the icon: Right-click on the desktop, select “Create Launcher” (or similar), and browse to the location of your bash.desktop file. The icon should now be visible and launchable.
Remember to replace the placeholder values (e.g., /path/to/icon.png, /path/to/bash.sh) with the actual paths and file names for your script and icon.
