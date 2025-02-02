---
id: transaction-chain
title: TransactionChains
---

Due to resource limitations, it is not possible to chain together billions of transactions into a single branch of chained blocks.
By using transaction chains rather than blocks of transactions,  Archethic network overcomes the above constraint.
The transactions in  Archethic Blockchain are atomic meaning there is only one transaction in each block, and each transaction has its own validation evidence and chain.

## Transaction In Archethic Blockchain

- ### Pending transaction

A pending transaction is a transaction that does not have validation.
The structure of a pending transaction is described below:
```
|-----------|------|------|---------------------|--------------------|------------------|
|  Address  | Type | Data | Previous public key | Previous signature | Origin signature |
|-----------|------|------|---------------------|--------------------|------------------|
                      |      
                      |
      |---------|------|--------|------------|------------|
      | Content | Code | Ledger | Ownerships | Recipients |
      |---------|------|--------|------------|------------|
                            |      |
                            |      |
                  |-----|-------|  |-----------------|--------|  
                  | UCO | Token |  | Authorized keys | Secret |
                  |-----|-------|  |-----------------|--------|                     
  

```


- Address: Corresponds to the hash of the public key of the transaction
- Type: Defines the functional role of the transaction
- Data: Contains all the operations to be performed (transfers, smart contracts, key authorizations, etc.)
   - Content: Can contain any kind of data. It can be used to host some data (HTML page, text, image, code, etc.) 
   - Code: Defines the smart contract code to be interpreted by the node. More details on [Smart-Contracts](/build/smart-contracts) section.
   - Ledger: Defines several types of ledger operations
      - UCO: for the cryptocurrency transfers
      - Token: for non-financial transactions (intended for P2P uses - as tokens, loyalties, etc.)
   - Ownerships: Define some cryptographic authorizations and delegations
      - Authorized keys: list of authorized keys to be able to decrypt secrets
      - Secrets: Encrypted contents which can be decrypted by the authorized keys
   - Recipients: Additional recipients to target smart contracts
- Previous public key: Corresponds to the public key associated to the previous transaction
- Previous signature: Corresponds to the signature of the private key associated with the mentioned previous public key
- Origin signature: Corresponds to the signature of the device or software that generated the transaction. This is used on the [Proof Of Work](/learn/arch-consensus/proof-of-work) mechanism and is a necessary condition of its validation.

- ### Validated transaction

A pending trnsaction with validation proofs is known as `validated transaction`.
The figure below describes the structure of a validated transaction

```
|------------------|-------------------------|
| Validation Stamp | Cross Validation Stamps |
|------------------|-------------------------|
         |                      |
         |             |-----------------|-----------|
         |             | Node public key | Signature |     
         |             |-----------------|-----------|
         |
|-----------|---------------|--------------------|-------------------|-------------------|------------|--------|-----------|
| Timestamp | Proof of Work | Proof of Integrity | Proof of Election | Ledger Operations | Recipients | Errors | Signature |
|-----------|---------------|--------------------|-------------------|-------------------|------------|--------|-----------|
                                                                           |
                                 |-----|-----------------------|-----------------|
                                 | Fee | Transaction movements | Unspent outputs |
                                 |-----|-----------------------|-----------------|

```

- Validation Stamp: Stamp generated by the coordinator node
  - Proof of work: Corresponds to the public key matching the origin signature (More details on the [Proof of Work](/learn/arch-consensus/proof-of-work) section).
  - Proof of integrity: Proves the linkage of the previous transactions
  - Proof of election: Proves the validation node's rotating election and permit to reproduce it later (See [Rotating Election](/learn/arch-consensus#rotating-election))
  - Ledger operations: Contains all the ledger operations that will be taken into account by the network
    - fee: Transaction's fee
    - transaction movements: Issuer and resolved transaction movements
    - Unspent outputs: List of the remaining unspent outputs of the transaction chain after validation
  - Recipients: List of resolved addresses of the recipients
  - Errors: Any errors found in the validation (i.e. pending transaction error)
  - Signature: Cryptographic signature of the entire stamp by the coordinator's key
- Cross validation stamps: To be considered as validated, the `Validation Stamp` must be joined as many `Cross Validation Stamp` as required by the Heuristic Algorithms. 
  They are signatures of the given validation stamp.
  - Node public key: Correspond to the node's public key which generate this `Cross Validation Stamp`'s signature
  - Signature: Correspond to the signature of the `Cross Validation Stamp` for the mentioned public key
  - Inconsistencies: In case of inconsistencies or disagreement, it will contain a list of inconsistencies noted


## Transaction Chains
The validated transactions are stored as a chain that can only be updated from last validated transaction in the chain.
* The address of any transaction in the same chain could be used as a destination address, it is not necessary to specify the last transaction in the chain.
* Once the public key is disclosed, it is considered expired.



## Principles

:::note Replication
* All transactions associated with the same chain must be replicated on the storage nodes associated with the address of the last validated transaction in the chain.
* Transactions chains associated with network operation must be fully replicated on all nodes .
* Each validated transaction must be replicated on the storage nodes associated with the addresses of the associated transactions.
* Each transaction address must be replicated on the Beacon chains
:::


:::note Liveness
Each validated transaction is stored as a chain than can only be updated from the last validation transaction in the chain. The last transaction on a chain becomes the *authoritative* transaction. 
:::

:::note Quantum resistant
For security reason, once the public key is disclosed, it is considered as expired, only the hash of the public key of the next transaction(aka `address`) is announced.
This allows the next public key to be kept until the next transaction on the chain.
:::

:::note Malicious node management
If a node is responsible for validation of a transaction is determined as invalid caused by an anamoly, it will be acertained as `malicious`.
Malicious nodes are eventually banned from the network.

:::


Addressing solution
Any transaction chain address may be used as the destination address.
The chain's final transaction address need not be specified.
:::

:::note Stateless transactions
A transaction cannot change state since it uses the *UTXO* (Unspent Transaction Output) model.
Nothing exists outside of the verified transactions
:::

:::note UTXO mining
List of unspent outputs does not need to be specified by the sender of the transaction
All unspent outputs will be reintegrated directly into the last transaction.
:::

