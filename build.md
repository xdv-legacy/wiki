# Building Divvyd

Installation of divvyd on Ubuntu 14.04 is relatively straightforward with administrator privileges:

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git autoconf autogen automake build-essential software-properties-common libboost-all-dev libcurl4-openssl-dev libdb-dev libdb++-dev libgmp3-dev libminiupnpc-dev libmpfr-dev libssl-dev libcurl4-openssl-dev libjansson-dev pax-utils
sudo apt-get install git scons ctags pkg-config protobuf-compiler libprotobuf-dev libssl-dev python-software-properties g++ make
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 40 --slave /usr/bin/g++ g++ /usr/bin/g++-4.8
sudo update-alternatives --config gcc
wget https://download.libsodium.org/libsodium/releases/LATEST.tar.gz
tar xzf LATEST.tar.gz
cd libsodium-0.6.1/
./configure
make && make check && sudo make install
cd ..
sudo apt-get purge -f boost*
sudo apt-get install python-software-properties
 echo "deb [arch=amd64] http://mirrors.ripple.com/ubuntu/ trusty stable contrib" | sudo tee /etc/apt/sources.list.d/ripple.list 
 wget -O- -q http://mirrors.ripple.com/mirrors.ripple.com.gpg.key | sudo apt-key add -
 sudo apt-get update
 sudo apt-get install boost-all-dev
git clone https://github.com/xdv/divvyd.git
cd divvyd/
scons
```
