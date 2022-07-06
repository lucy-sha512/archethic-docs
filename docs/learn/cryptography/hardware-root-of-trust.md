---
id: hardware-root-of-trust
title: Harware Root of Trust
---

## What is Hardware root of trust?

A hardware root of trust is the foundation on which all security operations of a computing system depends. It contains the keys used for cryptographic functions and enables a secure boot process. 
It is inherently trusted, and therefore must be secure by design. The most secure implementation of a root of trust is in hardware making it immune from malware attacks. As such, it can be a stand-alone security module or implemented as a security module within a processor or system on chip (SoC) ([source](https://www.rambus.com/blogs/hardware-root-of-trust/#:~:text=for%20my%20application%3F-,What%20is%20hardware%20root%20of%20trust%3F,must%20be%20secure%20by%20design.)).


## Why is hardware the root of trust for Arch Ethic?
The ability to sustain the network even with more than 90% of malicious nodes effectively comes from two very important steps.

- Identifying the malicious nodes effectively and preemptively.
- Banish and effectively ban the malicious nodes.

While the first part involves complex algorithms implemented in the software layer, the second part involves uniquely tying a node's identity to its hardware. This way, the malicious node, once identified and banished, cannot rejoin the network by changing its representative identity.

The latter can be effectively achieved using the concept of a hardware root of trust. With hardware root of trust, a unique keypair is injected into the water at the time of fabrication, thus tying the identity with this key pair. Once, this keypair is certified, the node cannot effectively use another keypair, once banished, thus eliminating identity forgery. This is, in principle, possible due to the fact once a key pair is injected, it cannot be changed for that particular hardware.

Further, with this hardware root of trust, we can also ensure that no two nodes have the same public key tied to their identity. Thus, using hardware root of trust contributes significantly to the increased miner security and makes the Arch Ethic blockchain more resilient against bad actors.

## Implementation of Hardware Root of Trust (HRT)
To implement the hardware root of trust, within the Arch Ethic ecosystem, two technologies have been used.

- Trusted Platform Module (TPM 2.O)
- Yubico Yubikey (PIV)

Both of these technologies are based on Secure Element (SE) which are certified under Common Criteria (CC).

## Placement of HRT in Arch Ethic Blockchain/Ecosystem

Along with the initial software crypto library, the HRT is now the de-facto crypto engine for all the cryptographic operations carried out by Arch Ethic miners. This includes signature (ECC), verification (ECC), encryption (AES), decryption (AES), hashing (SHA2/SHA3), etc.

The Arch Ethic node software delegates all the cryptography operations to the HRT libraries of TPM and Yubikey, which are specially developed for this purpose by Uniris.

Given the latency constraints, a new hybrid mechanism has been developed involving the usage of the software crypto library and the HRT libraries (TPM/Yubikey). In this approach, the root of trust - crypto operation is still HRT based while the stem and branches will be software crypto library based.

The advantages of this mechanism over pure HRT-based mechanism is:
- Possibility of key aggregation
- Freedom of using newer elliptic curves that are not yet supported by TPM/Yubikey.
- Scalability without impacting security and performance.
