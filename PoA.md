


# Proof of Authority - Private Networks

 1. [Overview](#overview)
 2. [Prerequisites](#prerequisites)
 3. [Preparing the environment](#preparing-the-environment)
 4. [Creating the network](#creating-the-network)
 5. [Deploying Smart Contracts with Truffle](#deploying-smart-contracts-with-truffle)

## Overview
One of the most important part of any blockchain network is the consensus protocol that is being used on the network. The most widely used consensus protocol is 'Proof of Work' or 'PoW' which is used in a lot of major chains like Bitcoin and Ethereum main network (as of June 2020). This protocol requires the nodes to compete against each other by trying to find an answer to a puzzle and whoever reaches the answer first, gets a reward. This protocol is very resource intensive and inefficient. 
PoW is primarily used in public networks where anyone can join the network and we can't verify the authenticity of a node.

Proof of Authority or 'PoA' is an alternate consensus mechanism for blockchain networks. It is primarily used in permissioned private blockchain network. Essentially, only the nodes which have been verified to be authentic are allowed to join the network. This means that the nodes have an implicit trust among each other and they do not need to verify their authenticity on the network. Therefore the nodes don't have to go through the resource intensive puzzles to create a new block.

In this tutorial we will be using a program called Geth, which is a full Ethereum node made using Go. As Geth is a full Ethereum node, we can keep a full copy of the chain. We can connect to the main ethereum chain and sync with that, or we can create our own private network. For private networks, Geth supports both PoW and PoA consensus protocols. The PoA implementation of Geth is called 'Clique'. It was implemented in Ethereum with the [EIP 225](https://eips.ethereum.org/EIPS/eip-225). You can read more about the workings of the protocol at the given link.

## Prerequisites
 

- [Node.Js and npm](https://nodejs.org/en/)
- [Geth](https://geth.ethereum.org/)
- [Truffle](https://www.trufflesuite.com/truffle)

## Preparing the environment
### Node.Js
For Linux, we can use the apt package manager. Open the terminal and input the following commands:

    sudo apt update
    sudo apt install nodejs
    sudo apt install npm
  
We can check if the installation was successful, by checking the version of node.js and npm installed on the system. Use the commands given below.

    node -v
    npm -v
  
If you get an output, like `v12.18.0`, then the installation was successful.

For Windows, we can use the installation package from the [download page](https://nodejs.org/en/download/). 

### Geth
For Linux, we can use the PPA repository for Ethereum.
Open the terminal and input the following commands:

    sudo add-apt-repository -y ppa:ethereum/ethereum

	sudo apt-get update
	sudo apt-get install ethereum

This will install Geth in your system along with additional tools, like Puppeth which we will use.
We can again check for the version to see if the installation was successful. We will use the following code:

    geth version

   
![Geth version](https://github.com/jayrajroshan/Articles/blob/master/img/1_geth_ver.png?raw=true)
    
 For Windows,  we can use the downloader available at the [download page](https://geth.ethereum.org/downloads/).

> Note: Use the 'geth-alltools' version, since we will be using an additional tool called Puppeth for creating the bootnode.

Once installed we can use command prompt to check the version to check if the installation was successful.

    geth version

![Geth version (Windows)](https://github.com/jayrajroshan/Articles/blob/master/img/1b_geth_ver.png?raw=true)

### Truffle
Truffle can be installed using the Node package manager or npm. Put the following code in either the Linux Terminal or Windows command prompt:

    npm install truffle -g
Truffle will be installed on your system.

## Creating the network

First let's make our working directory, and all the folders we need within it. We can do this via the file manager in both Windows and Linux, or using the mkdir command in linux.
The final folder structure should look like this:
![Directory tree](https://github.com/jayrajroshan/Articles/blob/master/img/1c_dir_tree.png?raw=true)
Let's create the folders, first we'll create the project folder of the name 'blockchain' then go to that folder with the cd command.
    mkdir blockchain
    cd blockchain
Now that we are inside the blockchain folder, we'll create two more folders 'data' where we will keep the data for the blockchain network and the nodes we will create. The other folder called 'sc' will hold all the data related to Truffle and it is where we will keep the smart contract. For now we will only work in the data folder, so after creating the folders, we will go to data.

    mkdir data sc
    cd data
    
In this example we will first create a genesis block, then a bootnode and lastly two nodes to interact with the blockchain. So we will create 4 folders:

    mkdir 1node 2node gen boot
    
First we want to create some accounts which we will use to interact with the network. Since we are using PoA, we'll need some accounts which are allowed to sign initially. We will create an account in Node 1 and store all the data for that account in folder 1node and another in Node 2 and the store the data in 2node. We will use geth to create the accounts. Replace 'your-directory' with the directory where you have stored the folder and in the terminal, enter:

    geth --datadir /your-directory/blockchain/data/1node account new

> Note: If you don't know the directory of the folder go to that folder and enter the 'cd' command in Windows or the 'pwd' command in Linux to show you the full working directory which is required for the --datadir. You can also use relative paths.

![New account](https://github.com/jayrajroshan/Articles/blob/master/img/2_node_account.png?raw=true)
    
You will be prompted to enter a password for this account, enter a suitable password and remember it. The account will then be created and we'll get a public address of the key. This address will be used to identify the account on the network. Keep a note of this address, you can copy this and store it in a txt file.  
Now we'll make a key.txt file for storing the password we used, this will be later used to unlock the account so that we can use it for transactions. We'll go to the folder 1node where the data for node 1 is kept and make a file called key.txt, put the password in and save it. We'll use the nano text editor, you can use your favorite text editor. Just don't give yourself a headache by using using Vim.

    cd 1node
    nano key.txt
    
We'll do the same thing to create an account for node 2:

    geth --datadir /your-directory/blockchain/data/2node account new

Again give a simple password and store the address somewhere.
Then go to the folder 2node and create a key.txt file with the password for this account.

    cd ../2node
    nano key.txt

Now we will create the Genesis block. The genesis block is the first block that is created for the blockchain and it stores all the configuration for the network. We will use a tool called Puppeth to create the genesis block. First we will go to the gen folder where we want to store the config file for the genesis block. Then start Puppeth. Open the terminal and enter the following code:

    cd gen
    puppeth
    
You will be prompted to enter a name for the network, enter the name you want, it's always a good idea to not use spaces, rather use '-' or '_' to create longer names like "my_eth_chain".
Now we'll get several options as to what we want Puppeth to do. Since we want to create a new genesis block, we'll select the option '2. Configure new genesis' by entering the number 2.
Then select '1. Create new genesis from scratch' by entering the number 1 since we don't have any existing configurations.
Now it will ask for which consensus engine to use for the network, we have two options:
1. Ethash which is an implementation of the Proof of Work consensus protocol and,
2. Clique Which is an implementation of the Proof of Authority consensus protocol
For this tutorial we will be using Clique, so enter 2 and press Enter.
![Puppeth configuration](https://github.com/jayrajroshan/Articles/blob/master/img/5_pup_gen_clique.png?raw=true)

Now we can choose the time taken to create a new block. The actual time taken for each individual block will vary, but the network will try to keep the average time it takes to create a new block close to the given time.
I have given it as 1 second, but we can give it as we want, depending on the use of the network.
Now it will ask for the addresses of the accounts which will be allowed to seal(or create) a new block at the start of the network. So paste the addresses of the two accounts we created and press enter. Keep a note that 0x is already written in Puppeth, so copy after the 0x in the account address.

> Note: We can add new accounts which are allowed to seal later.

In PoA, there is no mining rewards, but the transactions will need some amount of gas fee, so it's a good idea to pre-fund our signer accounts. Puppeth will now ask you to enter the account addresses which should be pre-funded, enter the two accounts we created.

![Seal accounts](https://github.com/jayrajroshan/Articles/blob/master/img/7_pup_gen_fund.png?raw=true)
> Note: We can later fund accounts using faucets, but it's out of scope for now.

Press Enter to pre-fund the pre-compile addresses
Now we need to specify a network ID with which we will find the network. This network ID will be a number, so enter a simple network ID.
Now we have completed the configuration of the genesis block so Puppeth will come back to the first page. Now we need to export the configuration so we can start using it to deploy our network.
Choose the option '2. Manage existing genesis' by entering the number 2. Then select the option '2. Export genesis configurations' by entering 2.
Puppeth create json file with the configuration and ask where to save it. If you started Puppeth while in the gen folder then just press enter since it takes the current directory by default otherwise you can specify the folder here.
![Puppeth export](https://github.com/jayrajroshan/Articles/blob/master/img/8_pup_gen_export.png?raw=true)

Now we have our Genesis configurations and we can use it to start our nodes. If you want to check the contents of this configuration, you can open the json file in any text editor.
To create the two nodes, we need to initialise both the nodes with the same genesis file. Enter the following command to initialise Node 1 and store all the data for Node 1 in the folder 1node.

    --datadir /your-directory/blockchain/data/1node/ init /your-directory/blockchain/data/gen/rpds.json
    
This will initialise Node 1 with the genesis file we created
![Initialise node](https://github.com/jayrajroshan/Articles/blob/master/img/9_geth_init_account.png?raw=true)
Do the same thing for the second node after changing the data directory.

    --datadir /your-directory/blockchain/data/2node/ init /your-directory/blockchain/data/gen/rpds.json

Now we have created the genesis configuration and intialised the nodes using the genesis file. The nodes we created are two separate nodes which were initialised with the same genesis file. To create a blockchain network, all the blocks need to be able to find and connect with other nodes. There are two ways we can do this.
The first is to manually enter the addresses of each node on our network to other nodes. This process is not feasible as the time required for this will increase to a substantial amount when the number of nodes increase. 
The other way is by using a 'bootnode'. A bootnode is a node which has one job that is network discovery. It allows all the nodes in the network to discover each other and connect with each other so that we don't have to manually connect each node. We can use any node as the bootnode, however, Geth gives us a dedicated light-weight bootnode. To use this bootnode, we will first need to create a key file for the bootnode. Go to the boot folder that we created and use the following command:

    bootnode -genkey boot.key
This will create a file called 'boot.key' which we will use to run the bootnode. You can name the file anything such as 'discover.key'. To run the bootnode we will need to be in the directory with the 'boot.key' file and run the following command:

    bootnode -nodekey boot.key -verbosity 9 -addr :30310

We give two additional options with the command, which are '-verbosity' and '-addr'. Verbosity is used so that we can log events which are happening on the network and the bootnode. The addr parameter is defined to specify an address for the bootnode, here we have just specified the port, it will take the localhost as it's address.
This command will bring the bootnode online and it will display the address we will need to connect with the bootnode, the address will start with "enode" and it will have a string of characters and ending with the address and the port for the bootnode. Make a note of this address, as we will need to specify this for the other nodes, so that they can find and connect to the bootnode.
![Bootnode online](https://github.com/jayrajroshan/Articles/blob/master/img/10_boot.png?raw=true)

Now that our bootnode is online, we will finally bring our other nodes online. Use the following command for bringing Node 1 online (change the bootnode address and account address):

    geth --identity node1 --rpc --rpcaddr 0.0.0.0 --rpcport 8080 --rpccorsdomain "*" --datadir /your-directory/blockchain/data/1node --allow-insecure-unlock --rpcapi "eth,web3" --networkid 1234 --bootnodes enode://34bf3c9de1e7cd3c672a020bc3b24f484df78527d206d4f459ebf63cf4b104028b2778d40b5caa13a65a73ddd18e042f679334c4d3691d275a7ded13aac93111@127.0.0.1:0?discport=30310 --unlock  0x1A4D8871901C26Aecb803222FE3B6762dab53fCA --password "/root/rpds-mern/blockchain/data/1node/key.txt" --mine console

This will bring the Node 1 online. We have passed several parameters in this command. I'll explain them one by one,

 - --identity is used to identify the node on the network.
 - --rpc is used to enable RPC communication for the node. We use RPC to issue commands to the node.
 - --rpcaddr defines the RPC address for this node.
 - --rpcport defines which port is open for RPC communication to this node.
 - --rpccorsdomain defines which remote resources like url addresses can access this node, we have  given a "*" to indicate all resources can access the node.
 - --datadir tells us where the data for the node is stored.
 - --allow-insecure-unlock is used so that we can unlock an account using the Web3 API via HTTP.
 - --rpcapi defines the APIs we can access via RPC.
 - --networkid is used to identify the network.
 - --bootnodes specifies the addresses of the bootnodes that the node will connect to discover other nodes on the network.
 - --unlock specifies the accounts which are associated with this node that we want to unlock so that we can transact using the account.
 - --password specifies a text file which stores the password for the account.
 - --mine in PoA specifies that the sealing nodes can start to seal or create new blocks.
 - console starts a JavaScript console to interact with the node
![Node1 online](https://github.com/jayrajroshan/Articles/blob/master/img/11_a_node1.png?raw=true)
The node after coming online will connect to the bootnode and start looking for other nodes on the network (or peers). The bootnode will send all the information about the other nodes and Node 1 will connect to them.
If we see the bootnode now, we'll see a lot of network activity:
![Bootnode activity](https://github.com/jayrajroshan/Articles/blob/master/img/11_c_boot_final.png?raw=true)

Let's bring node 2 online, use the following code:

 

    geth --identity node2 --port 30304 --rpc --rpcaddr 0.0.0.0 --rpcport 8081 --rpccorsdomain "*" --datadir /your-directory/blockchain/data/2node --allow-insecure-unlock --rpcapi "eth,web3" --networkid 1234 --bootnodes enode://34bf3c9de1e7cd3c672a020bc3b24f484df78527d206d4f459ebf63cf4b104028b2778d40b5caa13a65a73ddd18e042f679334c4d3691d275a7ded13aac93111@127.0.0.1:0?discport=30310 --unlock  0x0E7736F9260c9063DB1bfAebE3B1D2aF20cf605e --password "/your-directory/blockchain/data/2node/key.txt" --mine --ipcdisable console


For node 2, we can see we have some additional parameters and we have to change some of the existing parameters.

 - --port 30304 is used to specify the http port to access the node.
 - We have to specify a different RPC port 
 - We have to specify the account which was specified for node 2

When the node 1 finds node 2 and connects to it, both the nodes will start sealing new blocks. 
![Node1 mining](https://github.com/jayrajroshan/Articles/blob/master/img/11_node1.png?raw=true)

Now our Network is up and running. Now we will use Truffle to connect to our network and deploy our smart contract.

## Deploying Smart Contracts with Truffle
### Setting up Truffle project
First we need to set up the directories and configuration files that Truffle will use. The easiest way to do this is to initialise a blank project. Go to the 'sc' directory where we will set up the Truffle project and enter the following command:

    truffle init

Now truffle will create 3 folders for us: contracts, migrations and test. The folders are self descriptive in that contracts stores all the smart contracts files written in Solidity, migrations stores the configurations for running migration of smart contracts to our network and test houses test scripts. It also creates a file with the name `truffle-config.js`, this file is used to define the networks which Truffle will connect to and define some network variables. 
![Truffle init](https://github.com/jayrajroshan/Articles/blob/master/img/12_truffle_init.png?raw=true)

For this tutorial, I am assuming you have a smart contract ready. So first of all we have to place this smart contract in the contracts folder. The smart contracts written in Solidity have an extension of .sol.  If we go into the contracts folder, we will see that a smart contract called `Migrations.sol` is already present. This smart contract is used to deploy our smart contracts on the blockchain network. We will place our smart contract in the same folder.
![Smart Contracts](https://github.com/jayrajroshan/Articles/blob/master/img/13_contract.png?raw=true)

> Note: You can use [Remix IDE](https://remix.ethereum.org/) to develop and test your smart contracts.

Now that we have our smart contract in the Truffle contracts folder, we will tweak the migration settings for our purposes. If we see inside the migrations folder, we will see a javascript file present with the name `1_initial_migration.js`. This file is used to deploy the Migration.sol smart contract to our network which is used to deploy further smart contracts.
We need to create a new javascript file called `2_deploy_contracts.js` which will be used to tell Truffle which smart contracts we want to deploy. The number the the beginning of the fie name signify the order in which these scripts need to run, as we will need to deploy the `Migrations.sol` contract first, hence we have 1 in `1_initial_migration.js`. Enter the following in the newly created file:

```javascript
var MyContract = artifacts.require("MyContract");

module.exports = function(deployer) {
  deployer.deploy(MyContract);
};
```

> Note: Change 'MyContract' under require to the name of your contract to your contract.

Before we use truffle to interact with our blockchain network, we need to tell Truffle about the network we want to use. For this, we will add our network configuration in `truffle-config.js`.  Open the file with any text editor or IDE and add the following under networks:
````javascript
	network-name: {
				host: "127.0.0.1"
				port: 8080,
				network_id: 1234,
				gas: 8500000
				},
````

![Truffle-config.js](https://github.com/jayrajroshan/Articles/blob/master/img/14_truffle_config.png?raw=true)

> Note: You can also uncomment the development network and change the port and network_id.

Now save the file and exit the text editor.
We are ready to deploy our smart contract on the network, but before that we will need to compile our smart contracts

<!--stackedit_data:
eyJwcm9wZXJ0aWVzIjoidGl0bGU6IFByb29mIG9mIFdvcmtcbm
F1dGhvcjogSmF5cmFqIFJvc2hhblxuIiwiaGlzdG9yeSI6WzU1
MzQzMjU3NCwtOTA2ODEwNiwtMTE2NTg2MDMzOCwzNzY2Nzg5NT
EsLTE2NzE2NDMyNCwtNDkwMTEwMjYyLDY3NzUxOTE2MSwzMDI3
NTkyMTMsOTQyNDAxMjE1LC0xNjY2MDc0MDE1LC0yOTU2OTM5Nz
gsNjE2NDg4NzU2LC0xOTY0OTEyNTQ2LDEwNzg1ODk4MjYsNjM2
NDgwODU0LC04MTYyNDA3NjEsMTc5Nzk5Mjg0NSwtMTA2MzQ3Mj
AzMCwtMTcwMTE1ODYxOSwxNTE3MzM1Mzk4XX0=
-->