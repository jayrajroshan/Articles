


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
The final file structure should look like this:
![Directory tree](https://github.com/jayrajroshan/Articles/blob/master/img/1c_dir_tree.png?raw=true)
Let's create the folders, first we'll create the project folder of the name 'blockchain' then go to that folder with the cd command.
    mkdir blockchain
    cd blockchain
Now that we are inside the blockchain folder, we'll create two more folders 'data' where we will keep the data for the blockchain network and the nodes we will create. The other folder called 'sc' will hold all the data related to Truffle and it is where we will keep the smart contract. For now we will only work in the data folder, so after creating the folders, we will go to data.

    mkdir data sc
    cd data
In this example we will first create a genesis block, then a bootnode and lastly two nodes to interact with the blockchain. So we will create 4 folders:

    mkdir 1node 2node gen boot
    
Now we will 

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc1MzA5NTcsMTc5Nzk5Mjg0NSwtMTA2Mz
Q3MjAzMCwtMTcwMTE1ODYxOSwxNTE3MzM1Mzk4LC0zNTQwOTA1
NDIsLTg4Mjg1Mjc1MSw4MjAyNTQ4MSwxNzc5MDgwMTM3LC01Nj
E3NDMzNTgsMTc0NjU4NDMwLDI1NzgxMzMzOSw1OTkxNjQwNTQs
LTEyODcwNjE5NzNdfQ==
-->