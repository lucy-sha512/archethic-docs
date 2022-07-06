---
id: proof-of-work
title: Proof of Work
---


"Proof of Work" ensures an unpredictable and pseudo-random selection of block validation in networks like Bitcoin (mining). But this method uses a lot of energy, and it can still be used against you by hashRate control.

Based on the reliability of the transaction origination devices, Arch Ethic Blockchain employs a novel type of "Proof of Work." The device from which a transaction originated, known as the "Origin Signature," signs the transaction in the Arch Ethic Blockchain.
This allows for the additional security requirements for transaction validation, such as: 
- preventing any transactions in the event of key theft.
- permit the user to check their balance on any smartphone, but only allow transactions to be generated on vetted devices
- enable the organizers of an election to ensure the biometric identity of a voter.

The figure below describes a typical unvalidated transaction on Arch Ethic Blockchain. 
```
|-----------|------|------|---------------------|--------------------|------------------|
|  Address  | Type | Data | Previous public key | Previous signature | Origin signature |
|-----------|------|------|---------------------|--------------------|------------------|
                                                                            |
                                                                            |
                                                                    signature from the device that orginated the transaction    
```

Finding the right public key from a list of public keys that are known to the network and associated with the transaction's "Origin Signature" constitutes the "Proof Of Work." 

This requires conducting a search on a limited number of public keys to confirm that the device has the necessary permissions to create the transaction. The nodes can simultaneously determine which keys are authorised and are aware of their shared secrets.

This verification is performed during the `Validation Stamp` creation by the `Coordinator Node` and ensures the device is authorized to generate the transaction. Brute forcing public keys is not practical in this situation.


Just like any other actor in the system, devices will have their transaction chain allowing them to update their keys. 


:::info
Each origin device's public keys are grouped by a family which helps nodes to determine which set of keys, must be used to produce the Proof of Work. These families are either software keys, USB keys or Biometric keys.
:::

:::info
Each origin device's public key is encrypted and renewed by the network ensuring confidentiality and authenticity of devices.
:::