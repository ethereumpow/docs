# Prepare a full node for ETHW



## 1. Setup

### 1.1 Recommended Hardware Requirements

- A fast CPU with 4+ cores
- 16GB+ of RAM
- At least 2 TB space of fast SSD drive
- 25 MBit/s bandwith 

### 1.2 ETHW source code

The golang implementation of the ETHW protocol is at GitHub `https://github.com/ethereumpow/go-ethereum`

Download the code and build it

```shell
git clone https://github.com/ethereumpow/go-ethereum.git
cd go-ethereum
make
```



## 2. Prepare 

The ethw fork need to prepare the data before Ethereum total terminal difficulty(TTD) was reached. There are several ways to prepare chinadata: 

### 2.1 Download the prepared data 

The following commands were run at  `ubuntu 22.04 LTS` platform. The downloaded file is about`1.1T`.  You can download the file `chaindata_15509444.tar.lz4` via BT network or HTTP. 

 BT is recommeded download method since the p2p network provides better downlaod speed.



#### 2.1.1 Download data file

Install download tool packages

```shell
sudo apt install transmission-cli -y
sudo apt install lz4 -y
sudo apt install aria2 -y
```



The file can be download by BT transmission:

```shell
cd YOUR_DOWNLOAD_DIR

wget http://snapshot.ethwscan.com/15509444.torrent
# check your download
openssl sha256 15509444.torrent 
SHA256(15509444.torrent)= ecf242df51d286906c2ef3a06bdba5bed608881a61f49a76a990d1ba57bd8ff2
transmission-cli -w .  15509444.torrent

```



or  download from magnet:

```shell
magnet:?xt=urn:btih:76612d6d83fa1a2b2dc4fa9f9059b4e8409d1328&dn=chaindata%5F15509444.tar.lz4&tr=udp%3A%2F%2Ftracker.ethereumpow.org%3A6969%2Fannounce&tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337%2Fannounce&tr=udp%3A%2F%2F9.rarbg.com%3A2810%2Fannounce&tr=udp%3A%2F%2Ftracker.openbittorrent.com%3A6969%2Fannounce&tr=http%3A%2F%2Ftracker.openbittorrent.com%3A80%2Fannounce&tr=udp%3A%2F%2Ftracker.torrent.eu.org%3A451%2Fannounce&tr=udp%3A%2F%2Fopen.stealth.si%3A80%2Fannounce&tr=https%3A%2F%2Fopentracker.i2p.rocks%3A443%2Fannounce&tr=udp%3A%2F%2Fexodus.desync.com%3A6969%2Fannounce&tr=udp%3A%2F%2Ftracker1.bt.moack.co.kr%3A80%2Fannounce&tr=udp%3A%2F%2Ftracker.tiny-vps.com%3A6969%2Fannounce&tr=udp%3A%2F%2Ftracker.moeking.me%3A6969%2Fannounce&tr=udp%3A%2F%2Ftracker.dler.org%3A6969%2Fannounce&tr=udp%3A%2F%2Fopen.demonii.com%3A1337%2Fannounce&tr=udp%3A%2F%2Fexplodie.org%3A6969%2Fannounce&tr=udp%3A%2F%2Fchouchou.top%3A8080%2Fannounce&tr=udp%3A%2F%2Fbt.oiyo.tk%3A6969%2Fannounce&tr=https%3A%2F%2Ftracker.nanoha.org%3A443%2Fannounce&tr=https%3A%2F%2Ftracker.lilithraws.org%3A443%2Fannounce&tr=http%3A%2F%2Ftracker3.ctix.cn%3A8080%2Fannounce&tr=http%3A%2F%2Ftracker.nucozer-tracker.ml%3A2710%2Fannounce&ws=http%3A%2F%2Fsnapshot.ethwscan.com%2Fchaindata%5F15509444.tar.lz4&ws=http%3A%2F%2Fsnapshot-us1.ethwscan.com%2Fchaindata%5F15509444.tar.lz4&ws=http%3A%2F%2Fsnapshot-eu1.ethwscan.com%2Fchaindata%5F15509444.tar.lz4

```



or download from http

```shell
aria2c -s16 -x16 -k100M http://snapshot.ethwscan.com/chaindata_15509444.tar.lz4
```



After download is finished, verify download file

```shell
openssl sha256  chaindata_15509444.tar.lz4 
SHA256(chaindata_15509444.tar.lz4)= c69fb808346f2efdf6b080cfa034b3dcbdce981068bc42ac55db31a2c3d2eea0

```



#### 2.1.2 Decompress and unpack the files

```
lz4 -cd chaindata_15509444.tar.lz4 | tar xf -
```



#### 2.1.3 Verify chinadata in a stand-alone network(optional)

```shell
# create a new dataspace
geth --datadir YOUR_DATA_SPACE
# Ctrl+C to quit 
rm -fr YOUR_DATA_SPACE/geth/chaindata
mv YOUR_DOWNLOAD_DIR/chaindata YOUR_DATA_SPACE/geth/

geth --datadir YOUR_DATA_SPACE --syncmode full --nodiscover console

# Check log is clean
# check block number 
> eth.blockNumber
15509444

```



### 2.2 sync the chaindata from Ethereum network

Also, you can do a full sync from ethereum network to get the chinadata before ETHW fork(It could take one week or two weeks).

 Stop the sync before total terminal difficulty(TTD) 58,750,000,000,000,000,000,000 is reached.



## 3. Run 

To run a fullnode,  execute this command after ETHW fork

```
geth --datadir YOUR_DATA_SPACE --syncmode full
```



