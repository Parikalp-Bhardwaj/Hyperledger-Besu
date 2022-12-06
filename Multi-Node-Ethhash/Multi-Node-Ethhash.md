# Besu Multi-Node Ethhash

# Download prerequisite
The only prerequisite you need for this article is Java if you don’t have it run this command

`sudo apt-get update &&` 
`sudo apt-get install openjdk-11-jdk`

# Download Binary File And Extract It
Now go to this URL and download the binary file.
- https://hyperledger.jfrog.io/hyperledger/besu-binaries/besu/22.7.6/besu-22.7.6.zip

Now extract it and provide the environment variable

# Create directories
Create directories for your private network, each of the three nodes, and a data directory for each node:


```bash
Private-Network/
    ├── Node-1
    │   ├── data
    ├── Node-2
    │   ├── data
    └── Node-3
        ├── data

```

# Create a genesis file
Copy the following genesis definition to a file called `privateNetworkGenesis.json` and save it in the `Private-Network` directory:


```
{
  "config": {
    "berlinBlock": 0,
    "ethash": {
      "fixeddifficulty": 1000
    },
      "chainID": 1337
  },
  "nonce": "0x42",
  "gasLimit": "0x1000000",
  "difficulty": "0x10000",
  "alloc": {
    "fe3b557e8fb62b89f4916b721be55ceb828dbd73": {
      "privateKey": "8f2a55949038a9610f50fb23b5883af3b4ecb3c3bb792cbcefbd1542c692be63",
      "comment": "private key and this comment are ignored.  In a real chain, the private key should NOT be stored",
      "balance": "0xad78ebc5ac6200000"
    },
    "f17f52151EbEF6C7334FAD080c5704D77216b732": {
      "privateKey": "ae6ae8e5ccbfb04590405997ee2d52d2b330726137b875053c36d94e974d162f",
      "comment": "private key and this comment are ignored.  In a real chain, the private key should NOT be stored",
      "balance": "90000000000000000000000"
    }
  }
}
```

# Start the first node as a bootnode

Now open your 1st terminal
Inside the `Private-Network/Node-1` directory.

Run This 

```bash
besu --data-path=data --genesis-file=../privateNetworkGenesis.json --miner-coinbase fe3b557e8fb62b89f4916b721be55ceb828dbd73 --rpc-http-enabled --host-allowlist="*" --rpc-http-cors-origins="all"
```


# Start Node-2
Now open another terminal
Inside the `Private-Network/Node-2` directory.

In 1st terminal you will get `Enode URL` 
`Copy this URL`

```bash 
besu --data-path=data --genesis-file=../privateNetworkGenesis.json --bootnodes= "PAST HERE" --p2p-port=30304
```

# Start Node-3
Now open another terminal
Inside the `Private-Network/Node-3` directory.


In 1st terminal you will get `Enode URL` 
`Copy this URL`


```bash
besu --data-path=data --genesis-file=../privateNetworkGenesis.json --bootnodes="PAST HERE" --p2p-port=30305
```


# Start another terminal, use curl to call the

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":1}' localhost:8545
```

The result confirms

```
{
  "jsonrpc" : "2.0",
  "id" : 1,
  "result" : "0x2"
}
```







