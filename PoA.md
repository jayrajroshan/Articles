


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
``` geth version```
   
    
Note: Use the 'geth-alltools' version, since we will be using an additional tool called Puppeth for creating the bootnode.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI0ODQ4MTE3NCwtODgyODUyNzUxLDgyMD
I1NDgxLDE3NzkwODAxMzcsLTU2MTc0MzM1OCwxNzQ2NTg0MzAs
MjU3ODEzMzM5LDU5OTE2NDA1NCwtMTI4NzA2MTk3M119
-->