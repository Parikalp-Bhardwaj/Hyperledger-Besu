# Create a privacy-enabled network

Configuring a network that supports private transactions requires starting a [Tessera](https://docs.tessera.consensys.net/en/stable/)
node for each Hyperledger Besu node. Besu command line options associate the Besu node with the Tessera node.

# What is Tessera

Tessera is a transaction manager in the Quorum blockchain network, which allows `private transactions` in Quorum. Tessera transaction managers secure communication between peers through a two-way SSL configuration. 

Quorum Tessera (Transaction Manager): The transaction manager performs the following operations:

- Peer-to-peer network creation with other transaction managers.
- Delegation of key management and payload encryption/decryption to the enclave.
- Data handling by using external databases for transaction handling.
- Managing private transaction payload and ensuring their delivery to other    privacy-enabled Quorum nodes that are part of the transaction.

Quorum Private Transaction Flow: Quorum leverages Tessera to enable networks to transact with some or all parties confidentially. Tessera is a stateless JAVA system used to enable `encryption`, `decryption`, and communication of `private transactions` in Quorum.

# Quorum Private Transaction Life Cycle

![Quorum Private Transaction Life Cycle](https://miro.medium.com/max/1400/1*tmQqZw8aMZBVGeHwgekCNA.png)


# Prerequisites
- [Insatll Tessera](https://docs.tessera.consensys.net/en/stable/HowTo/Get-started/Distribution/)


# Create Tessera directories.
Inside each Node-* directory, create a `Tessera` directory:


```bash
IBFT-Network/
├── Node-1
│   ├── data
│   ├── Tessera
├── Node-2
│   ├── data
│   ├── Tessera
├── Node-3
│   ├── data
│   ├── Tessera
└── Node-4
    ├── data
    ├── Tessera
```

# Generate Tessera keys

This example creates an unlocked private key, meaning you do not need a password to decrypt the private key file.

In each `Tessera directory`, generate a public/private key pair for the Tessera node:

Inside the `Node directory` you have a `Tessera directory`
Run this command inside four `Node-1/Tessera` `Node-2/Tessera` `Node-3/Tessera` `Node-4/Tessera` 

```bash 
tessera -keygen -filename nodeKey
```

At the prompt, press Enter to create an unlocked key.

Tessera generates the `public/private` key pair and saves the keys in the nodeKey.pub and nodeKey.key files.

# Create Tessera configuration files.

In the `Tessera directory` for each node, create a file called `tessera.conf`, with the following configuration:


Copy this command and paste it inside `Node-1/Tessera/tessera.conf`

```bash
{
  "mode": "orion",
  "useWhiteList": false,
  "jdbc": {
    "username": "sa",
    "password": "",
    "url": "jdbc:h2:./target/h2/tessera1",
    "autoCreateTables": true
  },
  "serverConfigs":[
    {
      "app":"ThirdParty",
      "serverAddress": "http://localhost:9101",
      "communicationType" : "REST"
    },
    {
      "app":"Q2T",
      "serverAddress": "http://localhost:9102",
      "communicationType" : "REST"
    },
    {
      "app":"P2P",
      "serverAddress":"http://localhost:9103",
      "sslConfig": {
        "tls": "OFF"
      },
      "communicationType" : "REST"
    }
  ],
  "peer": [
    {
      "url": "http://localhost:9203"
    },
    {
      "url": "http://localhost:9303"
    },
    {
      "url": "http://localhost:9403"
    }
  ],
  "keys": {
    "passwords": [],
    "keyData": [
      {
        "privateKeyPath": "nodeKey.key",
        "publicKeyPath": "nodeKey.pub"
      }
    ]
  },
  "alwaysSendTo": []
}
```

Copy this command and paste it inside `Node-2/Tessera/tessera.conf`
```bash
{
  "mode": "orion",
  "useWhiteList": false,
  "jdbc": {
    "username": "sa",
    "password": "",
    "url": "jdbc:h2:./target/h2/tessera1",
    "autoCreateTables": true
  },
  "serverConfigs":[
    {
      "app":"ThirdParty",
      "serverAddress": "http://localhost:9201",
      "communicationType" : "REST"
    },
    {
      "app":"Q2T",
      "serverAddress": "http://localhost:9202",
      "communicationType" : "REST"
    },
    {
      "app":"P2P",
      "serverAddress":"http://localhost:9203",
      "sslConfig": {
        "tls": "OFF"
      },
      "communicationType" : "REST"
    }
  ],
  "peer": [
    {
      "url": "http://localhost:9103"
    },
    {
      "url": "http://localhost:9303"
    },
    {
      "url": "http://localhost:9403"
    }
  ],
  "keys": {
    "passwords": [],
    "keyData": [
      {
        "privateKeyPath": "nodeKey.key",
        "publicKeyPath": "nodeKey.pub"
      }
    ]
  },
  "alwaysSendTo": []
}
```


Copy this command and paste it inside `Node-3/Tessera/tessera.conf`

```bash
{
  "mode": "orion",
  "useWhiteList": false,
  "jdbc": {
    "username": "sa",
    "password": "",
    "url": "jdbc:h2:./target/h2/tessera1",
    "autoCreateTables": true
  },
  "serverConfigs":[
    {
      "app":"ThirdParty",
      "serverAddress": "http://localhost:9301",
      "communicationType" : "REST"
    },
    {
      "app":"Q2T",
      "serverAddress": "http://localhost:9302",
      "communicationType" : "REST"
    },
    {
      "app":"P2P",
      "serverAddress":"http://localhost:9303",
      "sslConfig": {
        "tls": "OFF"
      },
      "communicationType" : "REST"
    }
  ],
  "peer": [
    {
      "url": "http://localhost:9103"
    },
    {
      "url": "http://localhost:9203"
    },
    {
      "url": "http://localhost:9403"
    }
  ],
  "keys": {
    "passwords": [],
    "keyData": [
      {
        "privateKeyPath": "nodeKey.key",
        "publicKeyPath": "nodeKey.pub"
      }
    ]
  },
  "alwaysSendTo": []
}
```


Copy this command and paste it inside `Node-4/Tessera/tessera.conf`
```bash
{
  "mode": "orion",
  "useWhiteList": false,
  "jdbc": {
    "username": "sa",
    "password": "",
    "url": "jdbc:h2:./target/h2/tessera1",
    "autoCreateTables": true
  },
  "serverConfigs":[
    {
      "app":"ThirdParty",
      "serverAddress": "http://localhost:9401",
      "communicationType" : "REST"
    },
    {
      "app":"Q2T",
      "serverAddress": "http://localhost:9402",
      "communicationType" : "REST"
    },
    {
      "app":"P2P",
      "serverAddress":"http://localhost:9403",
      "sslConfig": {
        "tls": "OFF"
      },
      "communicationType" : "REST"
    }
  ],
  "peer": [
    {
      "url": "http://localhost:9103"
    },
    {
      "url": "http://localhost:9203"
    },
    {
      "url": "http://localhost:9303"
    }
  ],
  "keys": {
    "passwords": [],
      "keyData": [
        {
          "privateKeyPath": "nodeKey.key",
          "publicKeyPath": "nodeKey.pub"
        }
      ]
  },
  "alwaysSendTo": []
}
```

# Start the Tessera nodes

In each `Tessera directory`, start Tessera specifying the `configuration file` created in the previous step:

Open 4 terminal

# Terminal 1

Inside `IBFT-Network/Node-1/Tessera` 

```bash
tessera -configfile tessera.conf
```

# Terminal 2

Inside `IBFT-Network/Node-2/Tessera` 

```bash
tessera -configfile tessera.conf
```

# Terminal 3

Inside `IBFT-Network/Node-3/Tessera` 

```bash
tessera -configfile tessera.conf
```

# Terminal 4

Inside `IBFT-Network/Node-4/Tessera` 

```bash
tessera -configfile tessera.conf
```

# Start Besu Node-1

Open another terminal
In the `IBFT-Network/Node-1` directory, start Besu `IBFT-Network/Node-1`:


```bash
besu --data-path=data --genesis-file=../genesis.json --rpc-http-enabled --rpc-http-api=ETH,NET,IBFT,EEA,PRIV --host-allowlist="*" --rpc-http-cors-origins="all" --privacy-enabled --privacy-url=http://127.0.0.1:9102 --privacy-public-key-file=Tessera/nodeKey.pub --min-gas-price=0
```

When the node starts, the `enode URL` displays. Copy the enode URL to specify Node-1 as the bootnode in the following steps.

![](https://besu.hyperledger.org/en/stable/assets/images/EnodeStartup.png)


# Start Besu Node-2

Open another terminal

In the `IBFT-Network/Node-2` directory, start Besu Node-2 specifying the Node-1 enode URL copied when starting Node-1 as the bootnode:



```bash
besu --data-path=data --genesis-file=../genesis.json --bootnodes=<Node-1 Enode URL> --p2p-port=30304 --rpc-http-enabled --rpc-http-api=ETH,NET,IBFT,EEA,PRIV --host-allowlist="*" --rpc-http-cors-origins="all" --rpc-http-port=8546 --privacy-enabled --privacy-url=http://127.0.0.1:9202 --privacy-public-key-file=Tessera/nodeKey.pub --min-gas-price=0

```

The command line specifies the same options as for Node-1 with different ports and Tessera node URL. The `--bootnodes` option specifies the `enode URL` of `Node-1`.


# Start Besu Node-3

Open another terminal

In the `IBFT-Network/Node-3` directory, start Besu Node-3 specifying the `Node-1` `enode URL` copied when starting Node-1 as the bootnode:

```bash
besu --data-path=data --genesis-file=../genesis.json --bootnodes=<Node-1 Enode URL> --p2p-port=30305 --rpc-http-enabled --rpc-http-api=ETH,NET,IBFT,EEA,PRIV --host-allowlist="*" --rpc-http-cors-origins="all" --rpc-http-port=8547 --privacy-enabled --privacy-url=http://127.0.0.1:9302 --privacy-public-key-file=Tessera/nodeKey.pub --min-gas-price=0
```


The command line specifies the same options as for Node-1 with different ports and Tessera node URL. The `--bootnodes` option specifies the `enode URL` of `Node-1`.

# Start Besu Node-4

Open another terminal

In the `IBFT-Network/Node-4` directory, start Besu Node-4 specifying the Node-1 enode URL copied when starting Node-1 as the bootnode:



```bash
besu --data-path=data --genesis-file=../genesis.json --bootnodes=<Node-1 Enode URL> --p2p-port=30306 --rpc-http-enabled --rpc-http-api=ETH,NET,IBFT,EEA,PRIV --host-allowlist="*" --rpc-http-cors-origins="all" --rpc-http-port=8548 --privacy-enabled --privacy-url=http://127.0.0.1:9402 --privacy-public-key-file=Tessera/nodeKey.pub --min-gas-price=0
```

The command line specifies the same options as for Node-1 with different ports and Tessera node URL. The `--bootnodes` option specifies the `enode URL` of `Node-1`.


# Create a privacy-enabled network using the Quorum Developer Quickstart


You can create a `privacy-enabled` network using the `Quorum Developer Quickstart`. It runs a private Hyperledger Besu network that uses `Tessera` as its `private transaction manager`.


We are taking this `network` directory from here `quorum-dev-quickstart`

```bash
npx quorum-dev-quickstart
```

Navigate to the `network/smart_contracts` directory and deploy the private transaction:


```bash
cd network/smart_contracts
npm install
node scripts/private_tx.js
```




- This `network` directory has [private_tx.js](https://gitlab.com/-/ide/project/ethereum-team/blockchian-labs/experiments/besu/tree/main/-/network/private_tx.js/)
 directory it deploy the private transaction:


For the private transaction we are using `web3js-quorum` JavaScript library

`Web3js-Quorum` is an Ethereum JavaScript library extending web3.js that adds supports for GoQuorum and Hyperledger Besu specific JSON-RPC APIs and features. In particular it enables to use web3.js with `private transactions`.



The script deploys the contract and sends an arbitrary value (47) from `Member1` to `Member3`. Once done, it queries all three members (Tessera) to check the value at an address. Only `Member1` & `Member3` has this information as they were involved in the transaction, `Member2` responds with a 0x to indicate it is unaware of the transaction.


```bash
node scripts/private_tx.js
Creating contract...
Getting contractAddress from txHash:  0xc1b57f6a7773fe887afb141a09a573d19cb0fdbb15e0f2b9ed0dfead6f5b5dbf
Waiting for transaction to be mined ...
Address of transaction: 0x8220ca987f7bb7f99815d0ef64e1d8a072a2c167
Use the smart contracts 'get' function to read the contract's constructor initialized value ..
Waiting for transaction to be mined ...
Member1 value from deployed contract is: 0x000000000000000000000000000000000000000000000000000000000000002f
Use the smart contracts 'set' function to update that value to 123 .. - from member1 to member3
Transaction hash: 0x387c6627fe87e235b0f2bbbe1b2003a11b54afc737dca8da4990d3de3197ac5f
Waiting for transaction to be mined ...
Verify the private transaction is private by reading the value from all three members ..
Waiting for transaction to be mined ...
Member1 value from deployed contract is: 0x000000000000000000000000000000000000000000000000000000000000007b
Waiting for transaction to be mined ...
Member2 value from deployed contract is: 0x
Waiting for transaction to be mined ...
Member3 value from deployed contract is: 0x000000000000000000000000000000000000000000000000000000000000007b


```










