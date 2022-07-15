---
id: beacon-chain
title: Beacon Chain
---

## Concepts

`Beacon Chain` is used as a tracer/marker of a global state but faces some scalability issues.
 Archethic Blockchain uses `Sharding` for the `BeaconChain` as well. Eventually, `Beacon Chain` is sharded and divided into a `subset` defined by the transaction's address and a given date.

For example, a transaction's address starting by `0F` for a given day, will not be stored on the same subset as a transaction's address starting by `9F` for the same day.


## Transaction tracking and timestamping

Each time a transaction is validated, the validation nodes will send the transaction to the right Beacon Chain storage nodes, to transmit the address of the transaction and its timestamp.

For each Beacon Chain interval, a new slot is generated referencing all the transactions during this interval.

At the end of the day, a transaction chain is formed, and the last transaction is computed to generate a summary of the current day for a given subset.

Since each transaction has its storage nodes, `Beacon Chains` are balanced between the storage nodes to ensure better scalability and distribution.

## Status and Network Coordinates of nodes

Beacon Chains also contain the network status of the nodes where the public key starts by the Beacon Chain subset.

The storage nodes in each subset are in charge of:
- Check the node availabilities
- Gather networking information such as latency, bandwidth

At the end of the day, a transaction is formed as well and the last transaction is computed to generate a summary of node availability and network coordinates

## Slot

Each `Beacon Chain` is divided during the day into multiple slots, defined by interval (for instance every 10 min).

That slot contains the following information:
- Transaction summaries: timestamping of the validated transactions
  - Address: Transaction's address
  - Timestamp: Transaction validation time
  - Movements addresses: List of outputs addresses of the transaction
- End of node synchronization: timestamping when a node finished its synchronization
  - Node public key: Node's first public key
  - Timestamp: Time when the node synchronization ended
- P2P view:
  - Availabilities: the binary form of the availability of the sampled nodes for the given subset
  - Network statistics: latency and bandwidth of the sampled nodes for the given subset

