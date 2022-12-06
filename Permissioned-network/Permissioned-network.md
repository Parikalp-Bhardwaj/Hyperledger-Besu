# Create a permissioned network.

#Create folders

Each node requires a data directory for the blockchain data.

Create directories for your permissioned network and each of the three nodes, and a data directory for each node:

```
Permissioned-Network/
├── Node-1
│   ├── data
├── Node-2
│   ├── data
└── Node-3
│   ├── data
└── Node-4
    ├── data

```
# Create the configuration file

Copy the following configuration file definition to a file called `ibftConfigFile.json` and save it in the `Permissioned-Network` directory:

```bash
{
  "genesis": {
    "config": {
      "chainId": 1337,
      "berlinBlock": 0,
      "ibft2": {
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

In the Permissioned-Network directory, generate the node key and genesis file:

```bash
besu operator generate-blockchain-config --config-file=ibftConfigFile.json --to=networkFiles --private-key-file-name=key
```

Besu creates the following in the networkFiles directory:

- `genesis.json` - The genesis file including the extraData property specifying the four nodes are validators

```bash
networkFiles/
├── genesis.json
└── keys
    ├── 0x438821c42b812fecdcea7fe8235806a412712fc0
    │   ├── key
    │   └── key.pub
    ├── 0xca9c2dfa62f4589827c0dd7dcf48259aa29f22f5
    │   ├── key
    │   └── key.pub
    ├── 0xcd5629bd37155608a0c9b28c4fd19310d53b3184
    │   ├── key
    │   └── key.pub
    └── 0xe96825c5ab8d145b9eeca1aba7ea3695e034911a
        ├── key
        └── key.pub
```

# Copy the genesis file to the Permissioned-Network directory

Copy the `genesis.json` file to the `Permisssioned-Network` directory.


# Copy the node private keys to the node directories

For each node, copy the key files to the data directory for that node

```bash
Permissioned-Network/
├── genesis.json
├── Node-1
│   ├── data
│   │    ├── key
│   │    ├── key.pub
├── Node-2
│   ├── data
│   │    ├── key
│   │    ├── key.pub
├── Node-3
│   ├── data
│   │    ├── key
│   │    ├── key.pub
├── Node-4
│   ├── data
│   │    ├── key
│   │    ├── key.pub

```

# Create the permissions configuration file
The permissions configuration file defines the nodes and accounts allowlists.

Copy the following permissions configuration to a file called `permissions_config.toml` and save a copy in the `Node-1/data`, `Node-2/data`, `Node-3/data`, and `Node-4/data` directories:

```bash
accounts-allowlist=["0xfe3b557e8fb62b89f4916b721be55ceb828dbd73", "0x627306090abaB3A6e1400e9345bC60c78a8BEf57"]

nodes-allowlist=[]
```

# Start Node-1

```bash
besu --data-path=data --genesis-file=../genesis.json --permissions-nodes-config-file-enabled --permissions-accounts-config-file-enabled --rpc-http-enabled --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*" --rpc-http-cors-origins="*"
```

When the node starts, the enode URL displays. You need the `enode URL` to specify Node-1 as a peer and update the permissions configuration file in the following steps.

![](https://besu.hyperledger.org/en/stable/assets/images/EnodeStartup.png)


# Start Node-2

Start another terminal, change to the `Permissioned-Network/Node-2` directory, and start Node-2:

```bash
besu --data-path=data --genesis-file=../genesis.json --permissions-nodes-config-file-enabled --permissions-accounts-config-file-enabled --rpc-http-enabled --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*" --rpc-http-cors-origins="*" --p2p-port=30304 --rpc-http-port=8546
```

When the node starts, the `enode URL` displays. You need the enode URL to update the permissions configuration file in the following steps.


# Start Node-3

Start another terminal, change to the `Permissioned-Network/Node-3` directory, and start Node-3:

```bash
besu --data-path=data --genesis-file=../genesis.json --permissions-nodes-config-file-enabled --permissions-accounts-config-file-enabled --rpc-http-enabled --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*" --rpc-http-cors-origins="*" --p2p-port=30305 --rpc-http-port=8547
```

When the node starts, the `enode URL` displays. You need the enode URL to update the permissions configuration file in the following steps

# Start Node-4


Start another terminal, change to the `Permissioned-Network/Node-4` directory, and start Node-4:

```bash
besu --data-path=data --genesis-file=../genesis.json --permissions-nodes-config-file-enabled --permissions-accounts-config-file-enabled --rpc-http-enabled --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*" --rpc-http-cors-origins="*" --p2p-port=30306 --rpc-http-port=8548
```

When the node starts, the `enode URL` displays. You need the enode URL to update the permissions configuration file in the following steps.


# Add enode URLs for nodes to permissions configuration file

Replace `<EnodeNode1>`, `<EnodeNode2>`, `<EnodeNode3>`, and `<EnodeNode4>` with the enode URL displayed when starting each node.

`Node-1`
```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"perm_addNodesToAllowlist","params":[["<EnodeNode1>","<EnodeNode2>","<EnodeNode3>","EnodeNode4"]], "id":1}' http://127.0.0.1:8545
```

`Node-2`
```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"perm_addNodesToAllowlist","params":[["<EnodeNode1>","<EnodeNode2>","<EnodeNode3>","EnodeNode4"]], "id":1}' http://127.0.0.1:8546
```

`Node-3`
```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"perm_addNodesToAllowlist","params":[["<EnodeNode1>","<EnodeNode2>","<EnodeNode3>","EnodeNode4"]], "id":1}' http://127.0.0.1:8547
```

`Node-4`
```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"perm_addNodesToAllowlist","params":[["<EnodeNode1>","<EnodeNode2>","<EnodeNode3>","EnodeNode4"]], "id":1}' http://127.0.0.1:8548
```

# Add nodes as peers

Replace `<EnodeNode1>` with the enode URL displayed when starting Node-1.


`Node-2`
```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"admin_addPeer","params":["<EnodeNode1>"],"id":1}' http://127.0.0.1:8546
```

`Node-3`
```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"admin_addPeer","params":["<EnodeNode1>"],"id":1}' http://127.0.0.1:8547
```

`Node-4`
```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"admin_addPeer","params":["<EnodeNode1>"],"id":1}' http://127.0.0.1:8548
```


Replace `<EnodeNode2>` with the enode URL displayed when starting Node-2.

`Node-3`
```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"admin_addPeer","params":["<EnodeNode2>"],"id":1}' http://127.0.0.1:8547
```

`Node-3`
```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"admin_addPeer","params":["<EnodeNode2>"],"id":1}' http://127.0.0.1:8548
```



# Confirm permissioned network is working

curl -X POST --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":1}' localhost:8545

The result confirms `Node-1` (the node running the JSON-RPC service) has three peers (Node-2, Node-3 and Node-4):

```bash
{
  "jsonrpc" : "2.0",
  "id" : 1,
  "result" : "0x3"
}
```

Import the first account from the genesis file into MetaMask and send transactions, as described in [Quickstart tutorial]:

- Address: 0xfe3b557e8fb62b89f4916b721be55ceb828dbd73
- Private key : 0x8f2a55949038a9610f50fb23b5883af3b4ecb3c3bb792cbcefbd1542c692be63
- Initial balance : 0xad78ebc5ac6200000 (200000000000000000000 in decimal)

# Try sending a transaction from an account not in the accounts allowlist

Import the third account from the genesis file into MetaMask and try to send a transaction, as described in [Quickstart tutorial]:

- Address: 0xf17f52151EbEF6C7334FAD080c5704D77216b732
- Private key: 0xae6ae8e5ccbfb04590405997ee2d52d2b330726137b875053c36d94e974d162f
- Initial balance: 0x90000000000000000000000 (2785365088392105618523029504 in decimal)


# Start a node not on the nodes allowlist

In your `Permissioned-Network` directory, create a `Node-5` directory and `data` directory inside it.

Change to the `Node-5` directory and start Node-5 specifying the Node-1 enode URL as the bootnode:

```bash
besu --data-path=data --bootnodes="<EnodeNode1>" --genesis-file=../genesis.json --rpc-http-enabled --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*" --rpc-http-cors-origins="*" --p2p-port=30307 --rpc-http-port=8549
```

Start another terminal and use curl to call the JSON-RPC API net_peerCount method:

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":1}' localhost:8549

```
The result confirms Node-5 has no peers even though it specifies Node-1 as a bootnode:

```bash
{
  "jsonrpc" : "2.0",
  "id" : 1,
  "result" : "0x0"
}
```


