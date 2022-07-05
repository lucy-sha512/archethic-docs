---
id: beacon-chain
title: Beacon Chain
---

The "Sharding" mechanism is being used by Archethic Blockchain to provide scalability in terms of storage and validation.

However, a "Beacon Chain" is employed to maintain a global synchronization and reference because it is impossible to know all the transactions in a well-distributed network with well-sharded nodes.

## Concepts

`Beacon Chain` is used as tracer/marker of a global state but to face some scalability issues, Archethic Blockchain is using `Sharding` also for the `BeaconChain`.

This means that the `Beacon Chain` is sharded and divided into a `subset` defined by the transaction's address and a given date.

For example, a transaction's address starting by `0F` for a given day, will not be stored on the same subset as a transaction's address starting by `9F` for the same day.


## Transaction tracking and timestamping

Each time a transaction is validated, the validation nodes will send the transaction to the right Beacon Chain storage nodes, to transmit the address of the transaction and its timestamp.

For each Beacon Chain interval, a new slot is generated referencing all the transactions during this interval.

At the end of the day, a transaction chain is formed, and the last transaction is computed to generate a summary of the current day for a given subset.

Because each transaction has its storage nodes, `Beacon Chains` are balanced between the storage nodes to ensure better scalability and distribution.

## Status and Network Coordinates of nodes

Beacon Chains also contain the network status of the nodes where the public key starts by the Beacon Chain subset.

The storage nodes in each subset are in charge of:
- check the node availabilities
- gather networking information such as latency, bandwidth

At the end of the day, a transaction is formed as well and the last transaction is computed to generate a summary of node availability and network coordinates

## Slot

Each `Beacon Chain` is divided during the day into multiple slots, defined by interval (for instance every 10 min).

That slot contains the following information:
- Transaction summaries: timestamping of the validated transactions
  - address: Transaction's address
  - timestamp: Transaction validation time
  - movements addresses: List of outputs addresses of the transaction
- End of node synchronization: timestamping when a node finished its synchronization
  - node public key: Node's first public key
  - timestamp: Time when the node synchronization ended
- P2P view:
     - availabilities: the binary form of the availability of the sampled nodes for the given subset
     - network statistics: latency and bandwidth of the sampled nodes for the given subset

