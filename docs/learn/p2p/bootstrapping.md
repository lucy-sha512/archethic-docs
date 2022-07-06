---
id: bootstrapping
title: Bootstrapping
---

Arch Ethic Blockchain uses Network Transaction and Supervised Multicast that requires some actions to be performed 
when a node is bootstrapping. These operations will ensure synchronization and P2P awareness.

## Joining

When a node wants to join the network the first time, it  requests a node, from  list of preconfigured nodes called "bootstrapping seeds".
The node selected from bootstrapping seeds is the closest in position to the node requesting to join the network.

It  generates a first node transaction containning the following data: 
* IP, port, protocol
* reward address
* key certificate (to ensure the key is coming from a secure element)
Once the network  attests and verifies its transaction, the node will be able to start a SelfRepair process

## Updates

When nodes rejoin the network after some time, depending on if its previous data expired, it will generate a new transaction with the new information

## Synchronization

Once the transaction is validated, the node initilaizes by requesting the list of nodes.

Then, it starts the [Self-Repair](/learn/p2p/self-repair) sequence to get and synchronize the missing transactions and publish its end of sync to the network.

In this way, the entire will know the existence the readiness of this node.

