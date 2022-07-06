---
id: token
title: Tokens
---

An essential component of Arch Ethic is the `token`.
With new use cases and trends, our world is currently evolving toward digitalization and tokenization (royalties, collection, proof of ownership, etc.)

Users of Arch Ethic have the native ability to create and transact with an unlimited-defined (custom) token.

## Native 

Arch Ethic tokens are considered as native  for developers, there is no need to create a smart contract to mint or transfer tokens.

The transaction's structure support  its design  and extension of the ledger model (more than UCO-only transaction)

To make them effective and performant, all traces of complexity have been eliminated. This makes the transfer of tokens as straightforward as the transfer of UCO (just UTXO), making it quick and affordable.

## Standardized

Arch Ethic's tokens are also unified through a specification to help implementers, developers and users to have a clear understanding of their definition.

For more details please take a look at the [AEIP-2](https://github.com/Arch Ethic-foundation/aeip/blob/main/AEIP-2.md)

## Creation

To create a token, you have to:
1. set the transaction's type to "token"
2. defined in the transaction's data the token definition

### Fungible

Here is an example of a fungible token: 
```json
{
  "supply": NB_OF_TOKEN_TO_CREATE,
  "type": "fungible",
  "symbol": "TOKEN_SYMBOL",
  "name": "TOKEN_NAME",
  "properties": []
}
```

### Non-fungible

Here is another example of a non-fungible token: 
```json
{
  "supply": SIZE_OF_THE COLLECTION,
  "type": "non-fungilbe",
  "name": "COLLETION NAME",
  "symbol": "COLLETION_SYMBOL",
  "properties": [
    [ {"name": "image", "value": "link"} ],
    [ {"name": "image", "value": "link"} ] ,
    [ {"name": "image", "value": "link"} ] 
  ]
}
```

During the transaction validation, the miners will understand how to interpret this transaction and create the relative assets and UTXOs to make transfers possible right away.

## Usability

To make use of those custom tokens, a developer or a user can easily build a new transaction and mentioning in the ledger operations to send this token.

Example of Token ledger operation in the transaction:
```JSON
{
  to: "RECIPIENT_ADDRESS",
  amount: NB_OF_TOKEN_TO_TRANSFER,
  token_address: "ADDRESS_OF_THE_GENERATED_ADDRESS"
} 
```