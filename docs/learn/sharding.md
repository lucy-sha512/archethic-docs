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

Archethic blockchain employs `Sharding` with redesigned self discovery and data synchronization to ensure avalaibility irrespective of a disaster.
Prioritizing data freshness is ensured by Heuristics to allow recovery from the best network path.

#### Building Blocks of Sharding in Archethic Blockchain
 - ` Transaction Chains `
 With Transaction Chain paradigm, transactions can be divided into chain, to ensure a concurrent processing unlike traditional blockchains.
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
A fresh group of rotating nodes validates every transaction.This ensures the distribution of validation and the processing, to achieve a linear scalability and a high TPS.
Transaction use the UTXO model so there is no reality out of the transaction preventing the issues in the network like:
- cross-shard synchronisation 
- communication via state channels

Only the transaction and the transaction inputs will be taken into account when determining the status of a transaction.

## Storage
The storage nodes connected to the new transaction's address  replicates all transactions belonging to the same chain.
Upon transaction validation, validation nodes are responsible for sending the transaction to several pools of nodes:
- I/O Storage Pool: Every valid transaction is duplicated on the storage nodes connected to the input/output addresses of the transaction:
- Storage pools are addressed by transaction movements.
- Public key storage pools node motions
- Addressing recipients' storage pools
Each transaction address for the Beacon Storage Pool is duplicated on the storage nodes of the associated address subset [See BeaconChain].
(/learn/sharding/beacon-chain)

> For each transaction, the Transaction Chain Storage Pool will change, assuring a completed distribution of nodes and the data replication. Nevertheless, nothing prevents the storage nodes to overlap within the chain.

## Rotating Election

The storage nodes election is also subject to a rotating selection, just like the election for validation nodes 
That means, every transaction will have a unique set of storage nodes and shards.

The selection of the storage node is based on the following factors: 
- the transaction's address 
- the network's knowledge of the storage nonce- a stable secret

This enables any node to independently reproduce this list and request a transaction from the closest node by doing this computation.

This list has been narrowed down according to some criteria, like:
- P2P compatibility
- Distribution by region
