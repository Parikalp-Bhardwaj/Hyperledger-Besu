# QBFT 

# The Different Between `QBFT` and `IBFT` is

`QBFT` recommeded enterprise-grade consensus protocol for private network.

`IBFT` is supported for existing private network.


# 1. Create directories

Each node requires a data directory for the blockchain data.

Create directories for your private network, each of the four nodes, and a data directory for each node.

```bash
    QBFT-Network/
    ├── Node-1
    │   ├── data
    ├── Node-2
    │   ├── data
    ├── Node-3
    │   ├── data
    └── Node-4
        ├── data

```
# Create a configuration file

The configuration file defines the IBFT 2.0 genesis file and the number of node key pairs to generate.

Copy the following configuration file definition to a file called `qbftConfigFile.json` and save it in the `QBFT-Network` directory:


```
{
  "genesis": {
    "config": {
      "chainId": 1337,
      "berlinBlock": 0,
      "qbft": {
        "blockperiodseconds": 2,
        "epochlength": 30000,
        "requesttimeoutseconds": 4
      }
    },
    "nonce": "0x0",
    "timestamp": "0x58ee40ba",
    "gasLimit": "0x47b760",
    "difficulty": "0x1",
    "mixHash": "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
    "coinbase": "0x0000000000000000000000000000000000000000",
    "alloc": {
      "fe3b557e8fb62b89f4916b721be55ceb828dbd73": {
        "privateKey": "8f2a55949038a9610f50fb23b5883af3b4ecb3c3bb792cbcefbd1542c692be63",
        "comment": "private key and this comment are ignored.  In a real chain, the private key should NOT be stored",
        "balance": "0xad78ebc5ac6200000"
      },
      "627306090abaB3A6e1400e9345bC60c78a8BEf57": {
        "privateKey": "c87509a1c067bbde78beb793e6fa76530b6382a4c0241e5e4a9ec0a0f44dc0d3",
        "comment": "private key and this comment are ignored.  In a real chain, the private key should NOT be stored",
        "balance": "90000000000000000000000"
      },
      "f17f52151EbEF6C7334FAD080c5704D77216b732": {
        "privateKey": "ae6ae8e5ccbfb04590405997ee2d52d2b330726137b875053c36d94e974d162f",
        "comment": "private key and this comment are ignored.  In a real chain, the private key should NOT be stored",
        "balance": "90000000000000000000000"
      }
    }
  },
  "blockchain": {
    "nodes": {
      "generate": true,
        "count": 4
    }
  }
}
```

# Generate node keys and a genesis file

In the `QBFT-Network` directory, generate the node key and genesis file:

Now Open Your terminal and run this


```bash
besu operator generate-blockchain-config --config-file=qbftConfigFile.json --to=networkFiles --private-key-file-name=key
```

Besu creates the following in the networkFiles directory:

```bash
networkFiles/
├── genesis.json
└── keys
    ├── 0x438821c42b812fecdcea7fe8235806a412712fc0
    │   ├── key
    │   └── key.pub
    ├── 0xca9c2dfa62f4589827c0dd7dcf48259aa29f22f5
    │   ├── key
    │   └── key.pub
    ├── 0xcd5629bd37155608a0c9b28c4fd19310d53b3184
    │   ├── key
    │   └── key.pub
    └── 0xe96825c5ab8d145b9eeca1aba7ea3695e034911a
        ├── key
        └── key.pub
```

# Copy the genesis file to the IBFT-Network directory

Copy the `genesis.json` file to the `QBFT-Network` directory.

# Copy the node private keys to the node directories

For each node, copy the `key` files to the `data` directory for that node


```bash
QBFT-Network/
├── genesis.json
├── Node-1
│   ├── data
│   │    ├── key
│   │    ├── key.pub
├── Node-2
│   ├── data
│   │    ├── key
│   │    ├── key.pub
├── Node-3
│   ├── data
│   │    ├── key
│   │    ├── key.pub
├── Node-4
│   ├── data
│   │    ├── key
│   │    ├── key.pub

```

# Start the first node as the bootnode

open the terminal 

In the `QBFT-Network/Node-1` directory:

Run This

```bash
besu --data-path=data --genesis-file=../genesis.json --rpc-http-enabled --rpc-http-api=ETH,NET,QBFT --host-allowlist="*" --rpc-http-cors-origins="all"
```

When the node starts, the `enode URL` displays. Copy the enode URL to specify Node-1 as the bootnode in the following steps.


![URL](https://besu.hyperledger.org/en/stable/assets/images/EnodeStartup.png)


# Start Node-2

Start another terminal, change to the `QBFT-Network/Node-2` directory and start Node-2 specifying the Node-1 `enode URL` copied when starting Node-1 as the bootnode:

In 1st terminal you will get `Enode URL` 
`Copy this URL`

![URL](https://besu.hyperledger.org/en/stable/assets/images/EnodeStartup.png)



```bash
besu --data-path=data --genesis-file=../genesis.json --bootnodes=<Node-1 Enode URL> --p2p-port=30304 --rpc-http-enabled --rpc-http-api=ETH,NET,QBFT --host-allowlist="*" --rpc-http-cors-origins="all" --rpc-http-port=8546
```

# Start Node-3

Start another terminal, change to the `QBFT-Network/Node-3` directory and start Node-3 specifying the Node-1 `enode URL` copied when starting Node-1 as the bootnode:

In 1st terminal you will get `Enode URL` 
`Copy this URL`


![URL](https://besu.hyperledger.org/en/stable/assets/images/EnodeStartup.png)

```bash
besu --data-path=data --genesis-file=../genesis.json --bootnodes=<Node-1 Enode URL> --p2p-port=30305 --rpc-http-enabled --rpc-http-api=ETH,NET,QBFT --host-allowlist="*" --rpc-http-cors-origins="all" --rpc-http-port=8547
```


# Start Node-4
Start another terminal, change to the `QBFT-Network/Node-4` directory and start Node-4 specifying the Node-1 `enode URL` copied when starting Node-1 as the bootnode:


In 1st terminal you will get `Enode URL` 
`Copy this URL`


![URL](https://besu.hyperledger.org/en/stable/assets/images/EnodeStartup.png)


```bash
besu --data-path=data --genesis-file=../genesis.json --bootnodes=<Node-1 Enode URL> --p2p-port=30306 --rpc-http-enabled --rpc-http-api=ETH,NET,QBFT --host-allowlist="*" --rpc-http-cors-origins="all" --rpc-http-port=8548
```


# Confirm the private network is working

Start another terminal, use curl to call the JSON-RPC API qbft_getvalidatorsbyblocknumber method and confirm the network has four validators:

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"qbft_getValidatorsByBlockNumber","params":["latest"], "id":1}' localhost:8545
```

# The result displays the four validators:

```bash
{
  "jsonrpc" : "2.0",
  "id" : 1,
  "result" : [ "0x73ced0bd3def2e2d9859e3bd0882683a2e6835fb", "0x7a175f3542ceb60bf80fb536b3f42e7a30c0a6d7", "0x7f6efa6e34f8c9b591a9ad4763e21b3fca31bcd6", "0xc64140f1c9d5bb82e54976e568ad39958c3e94be" ]
}
```
