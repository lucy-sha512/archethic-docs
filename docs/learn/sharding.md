---
id: sharding
title: Sharding
---
By distributing processing and storage power, sharding breaks up data into manageable chunks across the entire decentralised network to support scalable systems.
Within distributed networks, sharding poses a number of challenges, such as: 
* Finding specific data without searching the entire network 
* Sharded data consistency across the network 
* Data loss due to corruption 
* Attacks on the data nodes 
* Unlimited data network without overloading the global network.

Archethic blockchain employs Sharding with redesigned self discovery and data synchronization to ensure avalaibility irrespective of a disaster.
Prioritizing data freshness is ensured by Heuristics to allow recovery from the best network path.




#### Building Blocks of Sharding in Archethic Blockchain
 - ` Transaction Chains `
 With Transaction Chain paradigm, transactions can be divided into chain, to ensure a concurrent processing as the opposite of traditional blockchains.
- `Avalability`
The storage layers ensures avalability even in case of disaster through `Replication`.
-`Unlimited Network Storage`
Archethic blockchain controls overloading of the global network by definning rule for nymber of replicas per chain. 
This replication rule changes as new nodes are added to the network.
The replication rule is refined as the number of transaction in the chain increases.
* The number of replicas for a transaction chain initially will be the first instantiation of the rule.
* After the above, the replicas are changed based on hypergeometric distribution.
* When the transaction is validated only the cryptographic evidence is kept on the chain for older transactions.
- `Election of Storage Nodes`
Each time a transaction is performed the storage node election associated with the transaction chain is performed.

-`Complete Sharding`
Other new blockchain networks start to use `Sharding` but sometimes not in a complete form: 
- either storage
- either validation

Archethic Blockchain supports a complete sharding scheme for validation and for storage.


## Validation

Each transaction is validated by a new set of rotating nodes.

This ensures the distribution of validation and the processing, to achieve a linear scalability and a high TPS.

Because transaction are using the UTXO model, 
there is no reality out of the transaction, so the network is not subject to issue like:

- cross shards synchronization
- state channels communication

To get the state of a transaction, only the transaction and the transaction inputs will be taken into consideration.

## Storage

After the validation of the transaction, validation nodes will be in charge to send the transaction to several pools of nodes:
- Transaction Chain Storage Pool: All the transaction associated with the same chain must be replicated on the storage nodes associated with the new transaction's address.
- I/O Storage Pool: Each validated transaction is replicated on the storage nodes associated with the addresses of the transaction input/outputs:
  - Transaction movements addresses storage pools
  - Node movements public key storage pools
  - Recipients addresses storage pools
- Beacon Storage Pool: Each transaction address must be replicated on the storage nodes of the associated address subset [See BeaconChain](/learn/sharding/beacon-chain)

> For each transaction, the Transaction Chain Storage Pool will change, assuring a completed distribution of nodes and the data replication. Nevertheless, nothing prevents the storage nodes to overlap within the chain.

## Rotating Election

Like the validation nodes election, the storage nodes election is subject to a rotating election.
In other terms, each transaction will have its own shard and storage nodes.

The storage node election is based on:
- the address of the transaction
- the storage nonce: a stable secret known by the network
- the list of nodes

This permits any node to perform this computation autonomously to reproduce this list and to request a transaction from the closest node.

To ensure the best availability of the data, this list is refined by some criteria, such as:
- P2P availability
- Geographical distribution

