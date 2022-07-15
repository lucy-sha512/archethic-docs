---
id: arch-consensus
title: ARCH consensus
---
Atomic Rotating Commitment Heuristic, `ARCH` is the mechanism behind which Archethic Blockchain achieves Consensus while validating a transaction.
*  Security: Each transaction is automatically validated, meaning that during validation, there is either acceptance or refusal.
*  Data Consistency: Heuristic algorithms store transactions in a distributed manner while guaranteeing access to the most recent write and maximum availability.
* Fault Tolerance: Node election for storage and validation is constantly changing, eliminating the possibility of network dominance by one or more parties.

Arch Consensus is conceptualized around Atomic Commitment, Heuristic algorithm, and Rotating Election.

## Atomic Commitment

The Archethic Blockchain is built on `Hypergeometric distribution` principles, which, in the presence of an unpredictable election and formal consensus, enable one to achieve, with a 99.99999999 percent of certainty, the same result from 197 nodes as one would from 100,000 nodes.

Since it can withstand attacks from 90% of malicious nodes, it enables consensus establishment with a small number of nodes.

By strictly controlling disruptive nodes, which are expelled after an assessment of the source of the conflict, the danger of linked availability is ensured.



## Heuristic Algorithm
Heuristic Algorithms are made up of softwares (interpreters, libraries, etc.) and configuration files.
These are stored in a decentralized manner otherwise known as smart contract chains to eliminate the problem of software integrity running on different nodes and achieve holistic decentralized network.

The Heuristic algorithm is the core behind  unpredictable yet reproducible election called Rotating election.


## Rotating Election
Each rotating election is unpredictable, but still verifiable and reproducible.
The rotating algorithm sort a list of nodes by computing the `Rotating Keys`. It contains an element of unpredicbility as well an element that is shared among all the nodes for reproducing the keys.
The following contents are used for computing Rotating keys.
- `Hash of transaction`: Unpredictable until the transaction arrives.
- `Daily nonce`: Secret shared between the authorized nodes and renewed daily.
- `Node public key`: Last node public key 

Rotating Keys = Hash(Node public key, Daily Nonce, Hash(Transaction))

The purpose of the rotating keys calculation is to provide an unpredictable and reproducible ordered list of the allowed nodes. The scheduling thus obtained allows each of the nodes to find autonomously and share the list of nodes that will be in charge of the validation of the transaction.
They produces a proof, named: `Proof of Election` which can be verified by any other nodes to ensure the right election of nodes.

From this algorithm, we get a list of nodes which can be filtered according to the constraints of the validation of the transaction.
- P2P availability
- Geographical distribution


## Workflow

When a transaction is willing to be validated, its follows the given workflow:

1. The transaction is received by any node (aka `Welcome node`)
2. The `Welcome Node` determines the validation nodes from the `Rotating Election` algorithm and forward the transaction
3. The validation nodes after receiving the transaction start some preliminary job to gather the context of the transaction:
   - Previous transaction
   - List of unspent outputs
4. After the context building, the `Cross Validation Nodes` communicate to the `Coordinator Node` the list of storage nodes involved to gather this information.
5. The `Coordinator Node` can build the `Validation Stamp` and compute the replication tree. Then it transmits them to the `Cross Validation Nodes`.
6. The `Cross Validation Nodes` verify the content of the `Validation Stamp`, sign with or without inconsistencies, and send the `Cross Validation Stamp` to all the validation nodes involved.
7. Once all the `Cross Validation Stamps` are received and if the `Atomic Commitment` is reached, the replication phase starts.
8. Validation nodes send the transaction to the respective storage nodes:
- Storage nodes responsible for the new transaction chain
- Storage nodes responsible for the outputs of the transactions (transaction's movements addresses, recipients)
- Storage nodes responsible for the [Beacon Chain](/learn/sharding/beacon-chain)
9. The storage for the new transaction chain will notify the validation nodes and the `Welcome Node` about the replication, and the `Welcome Node` will notify the client about it.

 

