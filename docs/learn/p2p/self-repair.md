---
id: self-repair
title: Self Repair
---

Arch Ethic Blockchain uses a self-repair mechanism to be able to sync/re-sync missing transactions to be able
to restore the state of a node.
It employs a multidimensional sharding. In order to guarantee data availability and security, a node must carry out a self-repair multiple times:

- During node bootstrapping
- The loss of connectivity or the appearance of a new node 
- A change to the heuristic algorithm
## Identification

To determine missing transactions, for each cycle of repair, the date of the last sync is persisted.
Therefore, we can decide from this date, on the list of missing BeaconChain transactions to sync. (Reminder: BeaconChain summaries transactions across the entire network each day)

The Self-Repair will then request BeaconChain storage pools to get the missing transactions from those missing days

## Synchronization

To decide whether they must store this transaction, the nodes perform a "Storage Node Election"
We  obtain the list of current storage nodes from the transaction's address to sync and make a replication request to the closest nodes.

Following completion, a fresh last date of sync is persistent for the next cycle
