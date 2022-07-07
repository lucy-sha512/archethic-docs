---
id: js
title: Javascript SDK
---

Arch Ethic Blockchain Official Javascript SDK is available on [Github](https://github.com/archethic-foundation/libjs)

---

This SDK helps to generate transaction on top of Arch Ethic Blockchain.

:::info
The [AEWeb](https://github.com/archethic-foundation/aeweb-cli) product uses this SDK to build transaction to deploy web hosting assets in the Arch Ethic Blockchain. Please have a look
:::

## Installation

Initially we have to install and import the package from the npm registry:
```bash
npm install archethic
````

```js
const archethic = require('archethic')
```

## Transaction building

Then we can create a transaction by using the transaction builder

```js
const archethic = require('archethic')
const tx = archethic.newTransactionBuilder("transfer")
  .addUCOTransfer("0000B094E5C0245EB8DF519749CC49EDB545DEBB129A8BAE34D20D3E6118F64880CB", 10000)
  .build("mysecretseed", 0)
  .originSign(originPrivateKey)
```

:::warning
We need to have funds allocated to the previous address of the transaction as it's using UTXO model.
:::

:::info
  Notice the `originPrivateKey`  required to assert the transaction is built from an authenticated device. (See [Proof Of Work](/learn/arch-consensus/proof-of-work) for more details)
:::

## Transaction sending

Once the transaction built, we can send it to the network by passing a given endpoint (i.e https://testnet.archethic.net)

```js
tx = ....
archethic.sendTransaction(tx, endpoint)
```

This will return  a `Promise`, to assert  success or failure.
