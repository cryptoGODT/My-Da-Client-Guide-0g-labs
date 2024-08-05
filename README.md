# My-Da-Client-Guide-0g-labs

# DA Client Setup Guide 🖥️🔧

This document outlines the steps to set up your DA client. Follow the instructions below to get your client up and running. 🚀

## Hardware Requirements ⚙️

### Recommended Specifications
- **RAM**: 8 GB 💾
- **CPU**: 2 cores 🧠
- **Bandwidth**: 100 MBps for Download/Upload 🌐

## Installation 🛠️

### Install Dependencies

#### For Linux 🐧
```bash
sudo apt-get update
sudo apt-get install cmake
```


### Install Go
For Linux 🐧
Download the Go Installer:

```bash
wget https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
```
Extract the Archive:

```bash
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz
```
Add Go to Your PATH:
Add the following line to your ~/.profile:

```bash
export PATH=$PATH:/usr/local/go/bin
```

### Download the Source Code 🌐
```bash
git clone -b v1.0.0-testnet https://github.com/0glabs/0g-da-client.git
```

### Configuration 🛠️🔧
```toml
# Blockchain RPC endpoint
--chain.rpc = "JSON RPC node endpoint for the blockchain network" 🌐

# Signer private key
--chain.private-key = "Hex-encoded signer private key" 🔑

# Transaction receipt settings
--chain.receipt-wait-rounds = "Maximum retries to wait for transaction receipt" 🔄
--chain.receipt-wait-interval = "Interval between retries when waiting for transaction receipt" ⏳

# Transaction gas settings
--chain.gas-limit = "Transaction gas limit" 💵

# Memory DB settings
--combined-server.use-memory-db = "Whether to use mem-db for blob storage" 💾
--combined-server.storage.kv-db-path = "Path for level db" 📂
--combined-server.storage.time-to-expire = "Expiration duration for blobs in level db" ⏳

# Logging settings
--combined-server.log.level-file = "File log level" 📝
--combined-server.log.level-std = "Standard output log level" 📜
--combined-server.log.path = "Log file path" 📁

# Disperser server settings
--disperser-server.grpc-port = "Server listening port" 📍
--disperser-server.retriever-address = "GRPC host for retriever" 🌐

# Batcher settings
--batcher.da-entrance-contract = "Hex-encoded da-entrance contract address" 📜
--batcher.da-signers-contract = "Hex-encoded da-signers contract address" 🖊️
--batcher.finalizer-interval = "Interval for finalizing operations" ⏱️
--batcher.finalized-block-count = "Number of blocks between finalized block and latest block" 📈
--batcher.confirmer-num = "Number of Confirmer threads" 🧵
--batcher.max-num-retries-for-sign = "Number of retries before signing fails" 🔄
--batcher.batch-size-limit = "Maximum batch size in MiB" 📦
--batcher.encoding-request-queue-size = "Size of the encoding request queue" 📋
--batcher.encoding-interval = "Interval between blob encoding requests" ⏲️
--batcher.pull-interval = "Interval for pulling from the encoded queue" ⏳
--batcher.signing-interval = "Interval between slice signing requests" ⌛
--batcher.signed-pull-interval = "Interval for pulling from the signed queue" 🕒

# Encoder settings
--encoder-socket = "GRPC host of the encoder" 🌐
--encoding-timeout = "Total time to wait for a response from encoder" ⏱️
--signing-timeout = "Total time to wait for a response from signer" ⏳

# Chain read/write timeout
--chain-read-timeout = "Read timeout for the chain" ⏲️
--chain-write-timeout = "Write timeout for the chain" ⌛

# Logging levels
--combined-server.log.level-file = "trace" 📝
--combined-server.log.level-std = "trace" 📜
--combined-server.log.path = "./../run/run.log" 📁
```

### Running the Client 🚀
**Build the Combined Server**
```bash
make build
```

**Run**
```bash
./bin/combined \
    --chain.rpc https://rpc-testnet.0g.ai \
    --chain.private-key 0x00 \
    --chain.receipt-wait-rounds 180 \
    --chain.receipt-wait-interval 1s \
    --chain.gas-limit 2000000 \
    --combined-server.use-memory-db \
    --combined-server.storage.kv-db-path ./../run/ \
    --combined-server.storage.time-to-expire 300 \
    --disperser-server.grpc-port 51001 \
    --batcher.da-entrance-contract 0xDFC8B84e3C98e8b550c7FEF00BCB2d8742d80a69 \
    --batcher.da-signers-contract 0x0000000000000000000000000000000000001000 \
    --batcher.finalizer-interval 20s \
    --batcher.confirmer-num 3 \
    --batcher.max-num-retries-for-sign 3 \
    --batcher.finalized-block-count 50 \
    --batcher.batch-size-limit 500 \
    --batcher.encoding-interval 3s \
    --batcher.encoding-request-queue-size 1 \
    --batcher.pull-interval 10s \
    --batcher.signing-interval 3s \
    --batcher.signed-pull-interval 20s \
    --encoder-socket 52.198.175.144:34000 \
    --encoding-timeout 600s \
    --signing-timeout 600s \
    --chain-read-timeout 12s \
    --chain-write-timeout 13s \
    --combined-server.log.level-file trace \
    --combined-server.log.level-std  trace \
    --combined-server.log.path ./../run/run.log
```

