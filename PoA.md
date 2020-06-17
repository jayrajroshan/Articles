


# Proof of Authority - Private Networks

 1. [Overview](#overview)
 2. [Prerequisites](#prerequisites)
 3. [Preparing the environment](#preparing-the-environment)

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
Note: Use the 'geth-alltools' version, since we will be using an additional tool called Puppeth for creating the bootnode.
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
    
First we want to create some accounts which we will use to interact with the network. Since we are using PoA, we'll need some accounts which are allowed to sign initially. We will create an account in Node 1 and store all the data for that account in folder 1node and another in Node 2 and the store the data in 2node. We will use geth to make the account. In the terminal, enter:

    geth --datadir /your-directory/blockchain/data/1node account new

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

Now we can choose 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk0Nzc0OTE3OCwxNzk3OTkyODQ1LC0xMD
YzNDcyMDMwLC0xNzAxMTU4NjE5LDE1MTczMzUzOTgsLTM1NDA5
MDU0MiwtODgyODUyNzUxLDgyMDI1NDgxLDE3NzkwODAxMzcsLT
U2MTc0MzM1OCwxNzQ2NTg0MzAsMjU3ODEzMzM5LDU5OTE2NDA1
NCwtMTI4NzA2MTk3M119
-->