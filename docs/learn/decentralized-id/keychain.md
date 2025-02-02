---
id: keychain
title: Keychain
---

Archethic's keychain is a new concept to make wallet accessible, configurable, and interoperable with several service providers, and why not multi chains.

It describes a wallet that is stored encrypted on-chain, as only you and the authorized access or person you allowed, be able to decrypt and build transactions from it.

## Key generation

Technically speaking, this wallet - also referenced as `Decentralized Identity` - is made up of a randomly generated `seed` (root key) from which it's possible to generate all keys according to a path of derivation. 

So for any access to a service or an application, a key will be generated on the fly from the `seed` (root key) and the first public key associated with a service or an application. Thus allowing the creation of an infinite number of identities without even having to store related keys. 

![](/img/keychain-seed-paths.svg)

## End-to-end encryption

While this wallet or keychain is on-chain, it remains secure as no other party (at least non-authorized) can access it with service keys. This is possible as we are using end-to-end encryption and elliptic curve cryptography.

Each keychain/wallet generated is embedded into a transaction encrypted using an AES key itself encrypted with a list of authorized keys or authenticated access (biometric, smartphone, USB, etc.)

Once generated, we also create transactions for the access of this keychain. In other words, each access has its transaction chain, where the keychain's location is encrypted as well.

In the next step, to retrieve or access the keychain, the authentication method should retrieve its transaction chain,  decrypt the keychain's location, download the keychain transaction and finally decrypt the keychain with the right AES key.

This avoids disclosure of critical information and prevents unauthorized access.

![](/img/keychain-access-wallet.svg)

## Standard compliance

Archethic decentralized identity and keychain concepts are also compliant with the industry standard in the field of online and digital identity.

Then once created the keychain is embedded on-chain a representation of a [W3C DID (Decentralized Identifier)](https://www.w3.org/TR/did-core/) document which helps the discovery of your key materials.

It displays a JSON message with the list of the public key you own and you allow other parties to interact with, such as your main Archethic public key or your Amazon public for example.

This coupled with [verifiable credentials](https://www.w3.org/TR/vc-data-model/) and [WebAuthn (Website authentication without password)](https://webauthn.io/) make the complete usage of decentralized identity possible.

## Customization

Because this wallet should be your digital identity security, we can customize the services and the way the keys are generated.

Each service in the keychain is joined with other properties customizable:
- derivation path: this will inform how the key will be generated. For example, the default one is `m/650'/0/0` informing us we are using the Archethic derivation method (`650` instead of the usual BIP44) and then the `0/0` indicates the first account and the first key of the chain.

But nothing prevents adding something like: `m/650'/Amazon/0` or `m/650'/JohnDoeUCO@!/0`

- curve: this indicates during the derivate key which elliptic curve we want to use. (Ed25519, NIST, Bitcoin curve)

- hash algorithm: this is used in the transaction address generation from the key produced, by default it's associated with `sha256` but if you want higher security you could use something like `sha3-512` o `blake2b`
