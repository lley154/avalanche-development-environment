# Avalanche Development Environment

## Setup Ubuntu Linux OS
Download Avalanche starterkit
```
$ git clone https://github.com/lley154/avalanche-starter-kit.git
$ cd avalanche-starter-kit
```

Start Docker desktop if not already running

Build and run the dev container image
```
$ docker build -f .devcontainer/Dockerfile -t mydevcontainer .
$ docker run -it --rm \
  -v "$(pwd):/workspace" \
  -w /workspace \
  mydevcontainer
```

## Start the local network
```
vscode ➜ /workspace (main) $ avalanche network start
```

If successful, you will see the following
```
...
Network ready to use.

+--------------------------------------------------------------------------+
|                               PRIMARY NODES                              |
+-------+------------------------------------------+-----------------------+
| NAME  | NODE ID                                  | LOCALHOST ENDPOINT    |
+-------+------------------------------------------+-----------------------+
| node1 | NodeID-7Xhw2mDxuDS44j42TCB6U5579esbSt3Lg | http://127.0.0.1:9650 |
+-------+------------------------------------------+-----------------------+
| node2 | NodeID-MFrZFVCXPv5iCn6M9K6XduxGTYp891xXZ | http://127.0.0.1:9652 |
+-------+------------------------------------------+-----------------------+
```

List the accounts created on the local networks
```
vscode ➜ /workspace (main) $ avalanche key list
```

The default values for the local network are as follows:
```
Address: 0x8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC
Private Key: 56289e99c94b6912bfc12adc093c9b51124f0dc54ac7a766b2bc5ccf558d8027
Network Name: Avalanche Local
RPC URL: http://localhost:9650/ext/bc/C/rpc
Chain ID: 43112
Currency Symbol: AVAX
```

Now create the subnet Layer 1 network
```
vscode ➜ /workspace (main) $ avalanche blockchain create myblockchain
✔ Subnet-EVM
✔ Proof Of Authority
✔ Get address from an existing stored key (created from avalanche key create or avalanche key import)
✔ ewoq
✓ Validator Manager Contract owner address 0x8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC
✔ I want to use defaults for a test environment
Chain ID: 43112
Token Symbol: AVAX
prefunding address 0x8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC with balance 1000000000000000000000000
Installing subnet-evm-v0.7.0...
subnet-evm-v0.7.0 installation successful
File /home/vscode/.avalanche-cli/subnets/myblockchain/chain.json successfully written
✓ Successfully created blockchain configuration
Run 'avalanche blockchain describe' to view all created addresses and what their roles are
```

Show information about the new blockchain
```
vscode ➜ /workspace (main) $ avalanche blockchain describe myblockchain
```

List the local blockchains
```
vscode ➜ /workspace (main) $ avalanche blockchain list
+--------------+--------------+---------+---------------------------------------------------+------------+------------+-----------+
|    SUBNET    |    CHAIN     | CHAINID |                       VMID                        |    TYPE    | VM VERSION | FROM REPO |
+--------------+--------------+---------+---------------------------------------------------+------------+------------+-----------+
| myblockchain | myblockchain |   43112 | qDNV9vtxZYYNqm7TN1mYBuaaknLdefDbFK8bFmMLTJQJKaWjV | Subnet-EVM | v0.7.0     | false     |
+--------------+--------------+---------+---------------------------------------------------+------------+------------+-----------+
```

Now deploy the local blockchain
```
vscode ➜ /workspace (main) $ avalanche blockchain deploy myblockchain
✔ Local Network
Deploying [myblockchain] to Local Network
...
✓ Proof of Authority Validator Manager contract successfully
...
✓ L1 is successfully deployed on Local Network
...
✓ ICM is successfully deployed
...
✓ Relayer is successfully deployed

+---------------------------------------------------------------------------------------------------------------------------------+
|                                                           MYBLOCKCHAIN                                                          |
+---------------+-----------------------------------------------------------------------------------------------------------------+
| Name          | myblockchain                                                                                                    |
+---------------+-----------------------------------------------------------------------------------------------------------------+
| VM ID         | qDNV9vtxZYYNqm7TN1mYBuaaknLdefDbFK8bFmMLTJQJKaWjV                                                               |
+---------------+-----------------------------------------------------------------------------------------------------------------+
| VM Version    | v0.7.0                                                                                                          |
+---------------+-----------------------------------------------------------------------------------------------------------------+
| Validation    | Proof Of Authority                                                                                              |
+---------------+--------------------------+--------------------------------------------------------------------------------------+
| Local Network | ChainID                  | 43112                                                                                |
|               +--------------------------+--------------------------------------------------------------------------------------+
|               | SubnetID                 | GEieSy2doZ96bpMo5CuHPaX1LvaxpKZ9C72L22j94t6YyUb6X                                    |
|               +--------------------------+--------------------------------------------------------------------------------------+
|               | BlockchainID (CB58)      | 2dwmSSx23remrKQaamfK5kvJN4sHdc3ZunsArPeF4MQgQ96UMi                                   |
|               +--------------------------+--------------------------------------------------------------------------------------+
|               | BlockchainID (HEX)       | 0xd795202b7ff797f32ece8cc56aea2ad6a5c2bc596f41a921922749734c014d6d                   |
|               +--------------------------+--------------------------------------------------------------------------------------+
|               | RPC Endpoint             | http://127.0.0.1:43333/ext/bc/2dwmSSx23remrKQaamfK5kvJN4sHdc3ZunsArPeF4MQgQ96UMi/rpc |
+---------------+--------------------------+--------------------------------------------------------------------------------------+
...

```

Check network status
```
vscode ➜ /workspace (main) $ avalanche network status
Network is Up:
  Number of Nodes: 2
  Number of Custom VMs: 0
  Network Healthy: true
  Custom VMs Healthy: true

+--------------------------------------------------------------------------+
|                               PRIMARY NODES                              |
+-------+------------------------------------------+-----------------------+
| NAME  | NODE ID                                  | LOCALHOST ENDPOINT    |
+-------+------------------------------------------+-----------------------+
| node1 | NodeID-7Xhw2mDxuDS44j42TCB6U5579esbSt3Lg | http://127.0.0.1:9650 |
+-------+------------------------------------------+-----------------------+
| node2 | NodeID-MFrZFVCXPv5iCn6M9K6XduxGTYp891xXZ | http://127.0.0.1:9652 |
+-------+------------------------------------------+-----------------------+

+---------------------------------------------------------------------------+
|                                  L1 NODES                                 |
+-------+------------------------------------------+------------------------+
| NAME  | NODE ID                                  | LOCALHOST ENDPOINT     |
+-------+------------------------------------------+------------------------+
| node1 | NodeID-BDLkjGDnmunkRFaoLuTFTxhzRnXJMAGyH | http://127.0.0.1:43333 |
+-------+------------------------------------------+------------------------+
```

## Deploy Smart Contract
```
vscode ➜ /workspace (main) $ mkdir -p src/my-contracts
```
Save the following solidity contact to src/my-contracts/NumberStorage.sol
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
 
contract NumberStorage {
 
    uint256 num;
  
    function getNum() public view returns(uint) {
        return num; 
    }
  
    function setNum(uint _num) public {
        num = _num;
    }
  
}
```

Using Foundry, compile & deploy the NumberStorage.sol smart contract
```
vscode ➜ /workspace (main) $ forge create --rpc-url local-c --private-key $PK src/my-contracts/NumberStorage.sol:NumberStorage --broadcast
[⠊] Compiling...
No files changed, compilation skipped
Deployer: 0x8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC
Deployed to: 0x5DB9A7629912EBF95876228C24A848de0bfB43A9
Transaction hash: 0x80e564ae0539343d3b41bc6ebe2dde8d73cd76e5b0cc5e6e508cc02c3bebd9cf

```

Submit a transaction to the smart contract and set the number 42 into the state variable.  Use the smart contract address displayed above.
```
vscode ➜ /workspace (main) $ cast send --rpc-url local-c --private-key $PK 0x5DB9A7629912EBF95876228C24A848de0bfB43A9 "setNum(uint)" 42
```

Now, read the value of the state variable.
```
vscode ➜ /workspace (main) $ cast call --rpc-url local-c 0x5DB9A7629912EBF95876228C24A848de0bfB43A9 "getNum()(uint)")"
```

Further readings
https://academy.avax.network/course/interchain-messaging





