


# Proof of Authority - Private Networks

 1. [Overview](#Overview)

## Overview
One of the most important part of any blockchain network is the consensus protocol that is being used on the network. The most widely used consensus protocol is 'Proof of Work' or 'PoW' which is used in a lot of major chains like Bitcoin and Ethereum main network (as of June 2020). This protocol requires the nodes to compete against each other by trying to find an answer to a puzzle and whoever reaches the answer first, gets a reward. This protocol is very resource intensive and inefficient. 
PoW is primarily used in public networks where anyone can join the network and we can't verify the authenticity of a node.

Proof of Authority is an alternate consensus mechanism for blockchain networks. It is primarily used in permissioned private blockchain network. Essentially, only the nodes which have been verified to be authentic are allowed to join the network. This means that the nodes have an implicit trust among each other and they do not need to verify their authenticity on the network. Therefore the nodes don't have to go through the resource intensive puzzles to create a new block.

In this tutorial we will be using a program called Geth, which is a full Ethereum node made using Go. As Geth is a full Ethereum node, we can keep a full copy of the chain. We can connect to the main ethereum chain and sync with that, or we can create our own private network. For private networks, Geth supports both PoW and PoA consensus protocols. The PoA implementation of Geth is called 'Clique'

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg3ODA1MjI5NiwtNTYxNzQzMzU4LDE3ND
Y1ODQzMCwyNTc4MTMzMzksNTk5MTY0MDU0LC0xMjg3MDYxOTcz
XX0=
-->