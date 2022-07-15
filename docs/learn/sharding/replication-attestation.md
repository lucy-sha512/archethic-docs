---
id: replication-attestation
title: Replication attestation
---

To ensure that a particular transaction has at least one confirmation from the storage nodes to guarantee data availability Archethic uses replication attestations.
Without this kind of approach, there is a risk creating a network split or mismatch where beacons and shards aren't connected to one another.
Additionally, Beacon chains will be overloaded with hundereds of messages for a single trnsaction summary.

## Heuristic solution

Each validation node  notifies the replications nodes in charge, and waits for their confirmations to attest to the validity  and the availability of the transaction in the shard before notifying the beacon chain and the welcome node.

For the first time, a single notification from the validation nodes is sufficient to ensure the availability. This attestation will contain a list of signatures from the replication nodes to status about their commitment to store the transaction. However, further attestations and confirmations can be checked later during the time, to provide more security.

For example, a beacon chain receiving 1 attestation from a validation node including X storage confirmations 
will be valid as 3 attestations with their respective confirmations.

### Nested replications

While dealing with transfers for recipients or smart contract call we want to support notifications for recipient's shards only when primary transaction is validated and stored.
The replication can be split by levels: [ Main Chain ] -> [ Recipient Chains]

Then the welcome node will be notified by a validation node with a given number of replica confirmations.
```
          V1
        /  |  \
Chain: S1  S2  S3
       /     |   \
      Recipient Shards
```

### Client notifications

Once a transaction is submitted, the welcome node passes it along to the validation nodes and returns a pending status to the client.
The client would need to sign up for notifications when the transaction was finished. As soon as the transaction has been validated, the validation nodes will asynchronously tell the clients by alerting the welcome.

## Further improvements

To reduce the overall size of the attestations and confirmations we can support signature aggregation or cosigning to embed a single transaction with a bitfield to indicate which nodes signed the transaction replication, which will be signed over by a validation node.


