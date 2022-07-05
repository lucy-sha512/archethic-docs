---
id: replication-attestation
title: Replication attestation
---

How to ensure that a particular transaction has at least one confirmation from the storage nodes to guarantee data availability.
Without this kind of approach, we risk creating a network split or mismatch where beacons and shards aren't connected to one another.
Additionally, we don't want to broadcast hundreds of messages for a single transaction summary, which would overload the beacon chain.

## Heuristic solution

Each validation node will notify the replications nodes in charge, and wait their confirmations to attest the validity 
and the availability of the transaction in the shard before to notify the beacon chain and the welcome node.

In 1st approx, a single notification from the validation nodes is sufficient to ensure the availability. This attestation will contains a list of signatures, 
from the replication nodes, to status about their commitment to store the transaction. 
However, further attestations and confirmations can be checked later during the time, to provide more security.

For example, a beacon chain receiving 1 attestation from a validation node including X storage confirmations 
will be valid as 3 attestations with their respective confirmations.

### Nested replications

 When we are dealing with recipients for transfer or smart contract calls, we want to support the recipients shards will be notified only when the main transaction is validated and stored.
So for example, the replication can be splitted by levels: [ Main Chain ] -> [ Recipient Chains]

Then the welcome node will be notified by a validation node with a given number of replica confirmations.

```
          V1
        /  |  \
Chain: S1  S2  S3
       /     |   \
      Recipient Shards
```

### Client notifications

When a transaction is submitted, the welcome node will transfer this transaction to the validation nodes and return to the client a pending status.
The client would have to subscribe to notifications about the completion of the transaction.
Once the transaction will be validated and the validation nodes by notifing the welcome will notify the clients asynchronously.

## Further improvements

To reduce the overall size of the attestations and confirmations we can support signature aggregation or cosigning to embed a single transaction with a bitfield
to indicate which nodes signed the transaction replication, which will be signed over by a validation node.


