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
We found a new version of Avalanche-CLI v1.8.10 upstream. You are running v1.8.4
Do you want to update?: 
    Yes
  ▸ No
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
✔ Local Network
+--------+------+---------+-------------------------------------------------+-------+---------------------+---------------+
|  KIND  | NAME | SUBNET  |                     ADDRESS                     | TOKEN |       BALANCE       |    NETWORK    |
+--------+------+---------+-------------------------------------------------+-------+---------------------+---------------+
| stored | ewoq | C-Chain | 0x8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC      | AVAX  |  50000000.000000000 | Local Network |
+        +      +---------+-------------------------------------------------+-------+---------------------+---------------+
|        |      | P-Chain | P-custom18jma8ppw3nhx5r4ap8clazz0dps7rv5u9xde7p | AVAX  |  30000000.000000000 | Local Network |
+        +      +---------+-------------------------------------------------+-------+---------------------+---------------+
|        |      | X-Chain | X-custom18jma8ppw3nhx5r4ap8clazz0dps7rv5u9xde7p | AVAX  | 300000000.000000000 | Local Network |
+--------+------+---------+-------------------------------------------------+-------+---------------------+---------------+
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

Now create the subnet network
```
vscode ➜ /workspace (main) $ avalanche blockchain create myblockchain --sovereign=false
✔ Subnet-EVM
✔ Proof Of Authority
✔ Get address from an existing stored key (created from avalanche key create or avalanche key import)
✔ ewoq
✓ Validator Manager Contract owner address 0x8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC
✔ I want to use defaults for a test environment
✔ Subnet-EVM
✔ I want to use defaults for a test environment
Chain ID: 333
Token Symbol: XYZ
prefunding address 0x8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC with balance 1000000000000000000000000
Installing subnet-evm-v0.7.3...
subnet-evm-v0.7.3 installation successful
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
| myblockchain | myblockchain |     333 | qDNV9vtxZYYNqm7TN1mYBuaaknLdefDbFK8bFmMLTJQJKaWjV | Subnet-EVM | v0.7.3     | false     |
+--------------+--------------+---------+---------------------------------------------------+------------+------------+-----------+
```

Now deploy the local blockchain
```
vscode ➜ /workspace (main) $ avalanche blockchain deploy myblockchain
✔ Local Network
Deploying [myblockchain] to Local Network

Network has already been booted.
Your blockchain control keys: [P-custom18jma8ppw3nhx5r4ap8clazz0dps7rv5u9xde7p]
Your subnet auth keys for chain creation: [P-custom18jma8ppw3nhx5r4ap8clazz0dps7rv5u9xde7p]
CreateSubnetTx fee: 0.000010278 AVAX
Subnet has been created with ID: GEieSy2doZ96bpMo5CuHPaX1LvaxpKZ9C72L22j94t6YyUb6X
Now creating blockchain...
CreateChainTx fee: 0.000074830 AVAX
+-------------------------------------------------------------------+
|                         DEPLOYMENT RESULTS                        |
+---------------+---------------------------------------------------+
| Chain Name    | myblockchain                                      |
+---------------+---------------------------------------------------+
| Subnet ID     | GEieSy2doZ96bpMo5CuHPaX1LvaxpKZ9C72L22j94t6YyUb6X |
+---------------+---------------------------------------------------+
| VM ID         | qDNV9vtxZYYNqm7TN1mYBuaaknLdefDbFK8bFmMLTJQJKaWjV |
+---------------+---------------------------------------------------+
| Blockchain ID | DS9HCA98oANHA4t8kwHUB61MjXJ3DAdyqjZj3R9b9atUnsSs1 |
+---------------+                                                   |
| P-Chain TXID  |                                                   |
+---------------+---------------------------------------------------+

Restarting node node2 to track subnet
Restarting node node1 to track subnet
Waiting for http://127.0.0.1:9652/ext/bc/DS9HCA98oANHA4t8kwHUB61MjXJ3DAdyqjZj3R9b9atUnsSs1/rpc to be available
Waiting for http://127.0.0.1:9650/ext/bc/DS9HCA98oANHA4t8kwHUB61MjXJ3DAdyqjZj3R9b9atUnsSs1/rpc to be available
✓ Local Network successfully tracking myblockchain
✓ Subnet is successfully deployed Local Network

RPC Endpoint: http://127.0.0.1:9652/ext/bc/DS9HCA98oANHA4t8kwHUB61MjXJ3DAdyqjZj3R9b9atUnsSs1/rpc
ICM Messenger has already been deployed to myblockchain
ICM Registry successfully deployed to myblockchain (0xE15e24b9DF067400066ba70b664855Bf1BCc3A86)
ICM Messenger successfully deployed to c-chain (0x253b2784c75e510dD0fF1da844684a1aC0aa5fcf)
ICM Registry successfully deployed to c-chain (0x17aB05351fC94a1a67Bf3f56DdbB941aE6c63E25)
✓ ICM is successfully deployed

Generating relayer config file at /home/vscode/.avalanche-cli/runs/network_20250521_125111/icm-relayer-config.json
Relayer version icm-relayer-v1.6.4
Installing Relayer
Executing Relayer
✓ Relayer is successfully deployed

+-------------------------------------------------------------------------------------------------------------------------------+
|                                                          MYBLOCKCHAIN                                                         |
+---------------+---------------------------------------------------------------------------------------------------------------+
| Name          | myblockchain                                                                                                  |
+---------------+---------------------------------------------------------------------------------------------------------------+
| VM ID         | qDNV9vtxZYYNqm7TN1mYBuaaknLdefDbFK8bFmMLTJQJKaWjV                                                             |
+---------------+---------------------------------------------------------------------------------------------------------------+
| VM Version    | v0.7.3                                                                                                        |
+---------------+---------------------------------------------------------------------------------------------------------------+
| Validation    |                                                                                                               |
+---------------+--------------------------+------------------------------------------------------------------------------------+
| Local Network | ChainID                  | 333                                                                                |
|               +--------------------------+------------------------------------------------------------------------------------+
|               | SubnetID                 | GEieSy2doZ96bpMo5CuHPaX1LvaxpKZ9C72L22j94t6YyUb6X                                  |
|               +--------------------------+------------------------------------------------------------------------------------+
|               | Owners (Threhold=1)      | P-custom18jma8ppw3nhx5r4ap8clazz0dps7rv5u9xde7p                                    |
|               +--------------------------+------------------------------------------------------------------------------------+
|               | BlockchainID (CB58)      | DS9HCA98oANHA4t8kwHUB61MjXJ3DAdyqjZj3R9b9atUnsSs1                                  |
|               +--------------------------+------------------------------------------------------------------------------------+
|               | BlockchainID (HEX)       | 0x1c3b5575e3fd971c4d3cb9af5c8d1e66ba173feab6abb32a548b9953c25a0192                 |
|               +--------------------------+------------------------------------------------------------------------------------+
|               | RPC Endpoint             | http://127.0.0.1:9652/ext/bc/DS9HCA98oANHA4t8kwHUB61MjXJ3DAdyqjZj3R9b9atUnsSs1/rpc |
+---------------+--------------------------+------------------------------------------------------------------------------------+

+------------------------------------------------------------------------------------+
|                                         ICM                                        |
+---------------+-----------------------+--------------------------------------------+
| Local Network | ICM Messenger Address | 0x253b2784c75e510dD0fF1da844684a1aC0aa5fcf |
|               +-----------------------+--------------------------------------------+
|               | ICM Registry Address  | 0xE15e24b9DF067400066ba70b664855Bf1BCc3A86 |
+---------------+-----------------------+--------------------------------------------+

+--------------------------+
|           TOKEN          |
+--------------+-----------+
| Token Name   | XYZ Token |
+--------------+-----------+
| Token Symbol | XYZ       |
+--------------+-----------+

+---------------------------------------------------------------------------------------------------------------------------------------+
|                                                        INITIAL TOKEN ALLOCATION                                                       |
+-------------------------+------------------------------------------------------------------+--------------+---------------------------+
| DESCRIPTION             | ADDRESS AND PRIVATE KEY                                          | AMOUNT (XYZ) | AMOUNT (WEI)              |
+-------------------------+------------------------------------------------------------------+--------------+---------------------------+
| Used by ICM             | 0xB6ff9E7858631CAE66bE09c6548EAabc5ebc947e                       | 600          | 600000000000000000000     |
| cli-teleporter-deployer | 25fa6db0528df2a226de7c3558e8d3cb2532147336920b943d92fc0fea99f6cb |              |                           |
+-------------------------+------------------------------------------------------------------+--------------+---------------------------+
| Main funded account     | 0x8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC                       | 1000000      | 1000000000000000000000000 |
| ewoq                    | 56289e99c94b6912bfc12adc093c9b51124f0dc54ac7a766b2bc5ccf558d8027 |              |                           |
+-------------------------+------------------------------------------------------------------+--------------+---------------------------+

+---------------------------------------------------------------------------------------------------------+
|                                             SMART CONTRACTS                                             |
+---------------+--------------------------------------------+--------------------------------------------+
| DESCRIPTION   | ADDRESS                                    | DEPLOYER                                   |
+---------------+--------------------------------------------+--------------------------------------------+
| ICM Messenger | 0x253b2784c75e510dD0fF1da844684a1aC0aa5fcf | 0x618FEdD9A45a8C456812ecAAE70C671c6249DfaC |
+---------------+--------------------------------------------+--------------------------------------------+

+----------------------------------------------------------------------+
|                      INITIAL PRECOMPILE CONFIGS                      |
+------------+-----------------+-------------------+-------------------+
| PRECOMPILE | ADMIN ADDRESSES | MANAGER ADDRESSES | ENABLED ADDRESSES |
+------------+-----------------+-------------------+-------------------+
| Warp       | n/a             | n/a               | n/a               |
+------------+-----------------+-------------------+-------------------+

+------------------------------------------------------------------------------------------------+
|                                      MYBLOCKCHAIN RPC URLS                                     |
+-----------+------------------------------------------------------------------------------------+
| Localhost | http://127.0.0.1:9652/ext/bc/DS9HCA98oANHA4t8kwHUB61MjXJ3DAdyqjZj3R9b9atUnsSs1/rpc |
+-----------+------------------------------------------------------------------------------------+

+--------------------------------------------------------------------------+
|                               PRIMARY NODES                              |
+-------+------------------------------------------+-----------------------+
| NAME  | NODE ID                                  | LOCALHOST ENDPOINT    |
+-------+------------------------------------------+-----------------------+
| node1 | NodeID-7Xhw2mDxuDS44j42TCB6U5579esbSt3Lg | http://127.0.0.1:9650 |
+-------+------------------------------------------+-----------------------+
| node2 | NodeID-MFrZFVCXPv5iCn6M9K6XduxGTYp891xXZ | http://127.0.0.1:9652 |
+-------+------------------------------------------+-----------------------+

+------------------------------------------------------------------------------------------------------+
|                                           WALLET CONNECTION                                          |
+-----------------+------------------------------------------------------------------------------------+
| Network RPC URL | http://127.0.0.1:9652/ext/bc/DS9HCA98oANHA4t8kwHUB61MjXJ3DAdyqjZj3R9b9atUnsSs1/rpc |
+-----------------+------------------------------------------------------------------------------------+
| Network Name    | myblockchain                                                                       |
+-----------------+------------------------------------------------------------------------------------+
| Chain ID        | 333                                                                                |
+-----------------+------------------------------------------------------------------------------------+
| Token Symbol    | XYZ                                                                                |
+-----------------+------------------------------------------------------------------------------------+
| Token Name      | XYZ Token                                                                          |
+-----------------+------------------------------------------------------------------------------------+
...

```

Check network status
```
vscode ➜ /workspace (main) $ avalanche network status
Network is Up:
  Number of Nodes: 2
  Number of Custom VMs: 1
  Network Healthy: true
  Custom VMs Healthy: true

+------------------------------------------------------------------------------------------------+
|                                      MYBLOCKCHAIN RPC URLS                                     |
+-----------+------------------------------------------------------------------------------------+
| Localhost | http://127.0.0.1:9652/ext/bc/DS9HCA98oANHA4t8kwHUB61MjXJ3DAdyqjZj3R9b9atUnsSs1/rpc |
+-----------+------------------------------------------------------------------------------------+

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
vscode ➜ /workspace (main) $ export PK=56289e99c94b6912bfc12adc093c9b51124f0dc54ac7a766b2bc5ccf558d8027
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
vscode ➜ /workspace (main) $ cast call --rpc-url local-c 0x5DB9A7629912EBF95876228C24A848de0bfB43A9 "getNum()(uint)"
```

Further readings
https://academy.avax.network/course/interchain-messaging





