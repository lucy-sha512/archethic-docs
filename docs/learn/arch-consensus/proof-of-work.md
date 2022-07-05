---
id: proof-of-work
title: Proof of Work
---


In networks like Bitcoin `Proof of Work` to ensure an unpredictable and pseudo random election of block validation (mining).But this technique requires a lot of energy consumption and can still be subject to exploit by HashRate control.

Archethic Blockchain is using a new kind of `Proof of Work` based on  the authenticity of the transaction origination devices. The transactions in Archethic Blockchain is signed by the device from which it is originated refered as `Origin Signature`
This allows the additional security requirements on transaction validation like:
- prohibit any transaction even in case of key theft
- allow user to consult their balance on any smartphone, but generate a transaction only on a trusted device
- enable the organizers of an election to ensure biometric identity of a voter

The `Proof Of Work` consists of finding the right public key associated to the `Origin Signature` of the transaction
from a list of public keys known by the network. This involves a search on a
finite set of public keys ensuring that the device is definitely authorized to generate the
transaction and that, at the same time, the nodes able to find that key are authorized
and know the Shared Secrets of the Nodes.
This verification is performed during the `Validation Stamp` creation by the `Coordinator Node` and ensure the device is authorized to generate the transaction
Brute forcing public keys  not viable herE



Just like any other actor into the system, devices will have their own transaction chain allowing them to update their keys. 


:::info
Each origin device public keys are grouped by family which helps nodes to determine which set of keys, must be played to produce the Proof of Work. (i.e: software, usb, biometric).
:::

:::info
Each origin device public key is encrypted and renewed by the network ensuring confidentiality and authenticity of devices.
:::

