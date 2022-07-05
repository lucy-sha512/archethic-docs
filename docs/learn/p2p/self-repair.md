---
id: self-repair
title: Self Repair
---

Archethic Blockchain is using a self-repair mechanism to be able to sync/re-sync missing transactions to be able
to restore the state of a node.

Because Archethic is using a multidimensional sharding, a node needs to execute a self-repair on multiple occasions, to ensure data availability and security:

- When the node bootstrap
- Disconnection or arrival of the new node
- Modification in Heuristic Algorithm

## Identification

To be able to determine which transactions are missing, for each cycle of repair, the date of the last sync is persisted.
Therefore, we can decide from this date, on the list of missing BeaconChain transactions to sync. (Reminder: BeaconChain summaries transactions across the entire network each day)

The Self-Repair will then request BeaconChain storage pools to get the missing transactions from those missing days

## Synchronization

Because we are using rotating election, nodes need to perform the `Storage Node Election` to determine if they need to store this transaction.

In that case, we will get the list of existing storage nodes from the transaction's address to sync and request from the closest nodes for the transaction to be replicated.

Once finalized, a new last date of sync is persisted for the next cycle.

