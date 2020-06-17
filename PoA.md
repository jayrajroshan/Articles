


# Proof of Authority - Private Networks

 1. [Overview](#overview)
 2. [Prerequisites](#prerequisites)
 3. [Preparing the environment](#preparing-the-environment)
 4. [Creating the network](#creating-the-network)

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
![Intialise nodes](https://github.com/jayrajroshan/Articles/blob/master/img/9_geth_init_account.png?raw=true)
Do the same thing for the second node after changing the data directory.

    --datadir /your-directory/blockchain/data/2node/ init /your-directory/blockchain/data/gen/rpds.json

Now we have created the genesis configuration and intialised the nodes using the genesis file. The nodes we created are two separate nodes which were initialised with the same genesis file. To create a blockchain network, all the blocks need to be able to find and connect with other nodes. There are two ways we can do this.
The first is to manually enter the addresses of each node on our network to other nodes. This process is not feasible as the time required for this will increase to a substantial amount when the number of nodes increase. 
The other way is by using a 'bootnode'. A bootnode is a node which has one job that is network discovery. It allows all the nodes in the network to discover each other and connect with each other so that we don't have to manually connect each node. We can use any node as the bootnode, however, Geth gives us a dedicated light-weight bootnode. To use this bootnode, we will first need to create a key file for the bootnode. Go to the boot folder that we created and use the following command:

    bootnode -genkey boot.key
This will create a file called 'boot.key' which we will use to run the bootnode. You can name the file anything such as 'discover.key'. To run the bootnode we will

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk4NDI3Njk3NSwtMTk2NDkxMjU0NiwxMD
c4NTg5ODI2LDYzNjQ4MDg1NCwtODE2MjQwNzYxLDE3OTc5OTI4
NDUsLTEwNjM0NzIwMzAsLTE3MDExNTg2MTksMTUxNzMzNTM5OC
wtMzU0MDkwNTQyLC04ODI4NTI3NTEsODIwMjU0ODEsMTc3OTA4
MDEzNywtNTYxNzQzMzU4LDE3NDY1ODQzMCwyNTc4MTMzMzksNT
k5MTY0MDU0LC0xMjg3MDYxOTczXX0=
-->