# Building Divvyd

Installation of divvyd on Ubuntu 14.04 is relatively straightforward with administrator privileges:

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git autoconf autogen automake build-essential software-properties-common libboost-all-dev libcurl4-openssl-dev libdb-dev libdb++-dev libgmp3-dev libminiupnpc-dev libmpfr-dev libssl-dev libcurl4-openssl-dev libjansson-dev pax-utils
sudo apt-get install scons ctags pkg-config protobuf-compiler libprotobuf-dev libssl-dev python-software-properties g++ make
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 40 --slave /usr/bin/g++ g++ /usr/bin/g++-4.8
sudo update-alternatives --config gcc
wget https://download.libsodium.org/libsodium/releases/LATEST.tar.gz
tar xzf LATEST.tar.gz
cd libsodium*
./configure
make && make check && sudo make install
cd ..
sudo apt-get purge -f boost*
echo "deb [arch=amd64] http://mirrors.ripple.com/ubuntu/ trusty stable contrib" | sudo tee /etc/apt/sources.list.d/ripple.list 
wget -O- -q http://mirrors.ripple.com/mirrors.ripple.com.gpg.key | sudo apt-key add -
sudo apt-get update
sudo apt-get install boost-all-dev
cd ..
git clone https://github.com/xdv/divvyd.git
cd divvyd/
scons
```

After compilation, divvyd will be available within the 'build' directory.  divvyd.cfg should be placed in the ~/.config/divvy directory.  Create the following directories:

```
mkdir ~/.config
mkdir ~/.config/divvy
```

The published documentation will be updated to reflect the initial validating node as referenced in the following example divvy.cfg:

```
# Allow other peers to connect to this server.
#
[server]
port_rpc
port_peer
port_wss_admin
#port_ws_public
#ssl_key = /etc/ssl/private/server.key
#ssl_cert = /etc/ssl/certs/server.crt

[port_rpc]
port = 5055
ip = 0.0.0.0
admin = 127.0.0.1
protocol = http

[port_peer]
port = 51255
ip = 0.0.0.0
protocol = peer

[port_wss_admin]
port = 6056
ip = 0.0.0.0
admin = 127.0.0.1
protocol = wss

[node_size]
medium

# This is primary persistent datastore for divvyd.  This includes transaction
# metadata, account states, and ledger headers.
[node_db]
type=RocksDB
path=<userpath>/.config/divvy/db/rocksdb
open_files=2000
filter_bits=12
cache_mb=256
file_size_mb=8
file_size_mult=2

[database_path]
<userpath>/.config/divvy/db

# This needs to be an absolute directory reference, not a relative one.
# Modify this value as required.
[debug_logfile]
<userpath>/.config/divvy/debug.log

[sntp_servers]
time.windows.com
time.apple.com
time.nist.gov
pool.ntp.org

# Where to find some other servers speaking the Divvy protocol.
#
[ips]
vld0.xdv.co

[validators]
n9KUE5b9PpkkE1rNBgnsgFm1PmdzNzxdrziU1txAcvuFemxs6THW VLD0

# Ditto.
[validation_quorum]
1

[consensus_threshold]
-1

[ledger_history]
full

# Turn down default logging to save disk space in the long run.
# Valid values here are trace, debug, info, warning, error, and fatal
[rpc_startup]
{ "command": "log_level", "severity": "warning" }
```

Change <userpath> to the correct location.  When install is complete start Divvy by executing divvyd

In the instance a connection to the network does not happen automatically, execute:

```
./divvyd connect vld0.xdv.co 51255
```
