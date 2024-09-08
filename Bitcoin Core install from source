## Install Bitcoin Core from Source
Run the below commands to install from Source code. [Resource link](https://github.com/bitcoin/bitcoin/blob/master/doc/build-unix.md "Github.com/bitcoin").

Building DBD for legacy wallet use
https://github.com/bitcoin/bitcoin/blob/master/doc/build-unix.md#berkeley-db

Additional video tutorial, for further reference.
[Canadian Bitcoiners](https://youtu.be/C8i0uJHmUoQ?si=gjiJaak2_b6ZT_CN)

1. Enter the below commands, one at a time. Run:
   ```bash copy
   sudo apt-get install build-essential libtool autotools-dev automake pkg-config bsdmainutils python3
   ```
2. Install dependencies
   ```bash copy
   sudo apt-get install libevent-dev libboost-dev
   ```
3. Install dependencies
   ```bash copy
   sudo apt install libsqlite3-dev
   ```
4. Install dependencies
   ```bash copy
   sudo apt-get install libzmq3-dev
   ```
5. Install dependencies
   ```bash copy
   sudo apt install systemtap-sdt-dev
   ```
6. Install dependencies
   ```bash copy
   sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools
   ```
7. Install dependencies
   ```bash copy
   sudo apt install qtwayland5
   ```
8. Install dependencies
   ```bash copy
   sudo apt-get install libqrencode-dev
   ```
9. Intall Git
   ```bash copy
   sudo apt-get install git
   ```
10. Make src directory and move into it
   ```bash copy
   mkdir -p src && cd src
   ```
11. Clone the bitcoin github repo
   ```bash copy
   git clone https://github.com/bitcoin/bitcoin.git
   ```
12. Move into the repo folder
```bash copy
   cd bitcoin
   ```
13. Check out the most recent release (or whatever version you want to use). You can check what the latest release is by heading over to [Github/bitcoin](https://github.com/bitcoin/bitcoin) scroll down the page and on the right, under the 'Releases' heading, you will see the latest release number stated.
   ```bash copy
   git checkout v26.2   
   ```
14. Autogen.sh
   ```bash copy
   ./autogen.sh
   ```
15. Confilgure
   ```bash copy
   ./configure
   ```
16. This next command will take a very long time, maybe around 1 hour as all of the code gets compiled. Be patient and don't run anything else on the machine while this job is running.
   ```bash copy
   make
   ```
17. This also takes some time, it will run some tests to check everything has been made correctly, be patient... come back in 15-30 minutes.
   ```bash copy
   make check
   ```

18. This will be quick.
   ```bash copy
   sudo make install
   ```
### Build Berkeley DB 4.8
EPS requires you to create a legacy Bitcoin Core wallet. This can only be done by downgrading the Berkely DB installation to version 4.8.
https://github.com/bitcoin/bitcoin/blob/master/doc/build-unix.md#berkeley-db
