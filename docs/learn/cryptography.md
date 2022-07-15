---
id: cryptography
title: Cryptography
---

Archethic network through the usage of TransactionChains  leverages an adaptative and quantum-safe cryptography.

## Quantum-Safe

The non-disclosure characteristic of public keys combined with the necessity to simultaneously possess many temporary private keys in order to generate a transaction makes it far more difficult for a quantum computer to "break" private keys.

### Non-Discolure of public keys

Archethic [TransactionChains](/learn/transaction-chain) uses a non-disclosure mechanism of public keys using chains of cryptography. 
Each transaction contains an address, which is a hash of the next public key and the previous public key.
Therefore, we don't have way to know which public key is used for a given transaction until a next one is coming.

### Origin Devices

In order to build a transaction, we need to known multiple temporary private keys a key to provide a signature based on the previous private key and a signature based on an origin device private key.
Origin device can be categorized in several families: software, hardware, biometrics. (See [Proof of Work](/learn/arch-consensus/proof-of-work))

Therefore, in order to generate a transaction by breaking private keys by a Quantum computer would be more complex

## Adaptative

To support backward compatibility and to evolve the network as the cryptographic research progesses , the choice cryptographic algorithms is given to people, organizations or countries making Archethic a versioned cryptography or metadata cryptography.


### Public keys

Each public key is prepended by some additional bytes to inform some metadata or algorithm versioning.
This includes:
- a byte to indicate the elliptic curve used (i.e Ed25519, NIST, secp256k1)
- a byte to indicate the origin of the generation (i.e software, hardware, ...)


### Hashes

Similar to the public keys, cryptographic hashes are also versioned with a byte of to identify which algorithms is used (i.e SHA-256).
This information helps to determine the length of a hash and to perform some checks for the validition and for the encoding/decoding of the data.

### Transaction addresses

While transaction addresses are often represented as hashes, Archethic provide a new level of information inside the transaction's address. A byte is prepending the hash with an information regarding the elliptic curve used to generate the public key related.

This is done, in order to be really adaptative. To support the non-disclosure mechanism offered by the transaction chain, we need to know which elliptic was used for a previous transaction to be able to reproduce the previous public key.

For example, imagine we have a transaction address encoded in that way, using a `secp256k1` elliptic

| Hash algorithm | Digest |
|-|-|
| 01 | 01309F1C765967AC7 | 

:::info
Reminder: a transaction address is the hash of the next public key
:::

For a new transaction, if now, we want to use the `ed25519` elliptic curve, we need to know which was used before. For this reason, we have two possibility:
- keep an history of the previous transaction and curves (*not really pratical and not scalable*)
- add a byte in front of each transaction's address to the curve used

So with the new model:

| Curve type | Hash algorithm | Digest | 
|-|-|-|
| 01 | 01 | 01309F1C765967AC7 |

Now we are able to compute the previous public key, with the curve `secp256k1` and continues with new elliptic curve along the way.

This will be even more pratical with On-Chain Decentralized Wallet (`Keychain`) to support multiple derived keys and custom algorithms.

## Default algorithms

Except for hardware compatability issues (HSM, etc..), EdDSA signatures, Curve25519 and AES256 will be used by default on the network.
