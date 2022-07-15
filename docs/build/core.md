---
id: core
title: Core development
---

Archethic Node repository can be found [here](https://github.com/Archethic-foundation/Archethic-node)


## Technology Stack

The technology stack for  Blockchain consists of:
- Elixir
- C
- ScyllaDB

### Why Elixir ?


With P2P systems and Blockchain technologies fault tolerance, low latency, and concurrent/parallelism are essential to achieve. Elixir supports all of the above requirements.

We can reach fast information processing and high TPS for better scalability since each Elixir code executes inside a lightweight, segregated threads of execution (referred to as processes).

When software is used in a real-world setting, errors inevitably occur, far more when the network, file systems, and other external resources are taken into account. Elixir provides supervisors who describe how to restart parts of your system when things go awry, going back to a known initial state that is guaranteed to work.
This feature ensures in case of failure, the entire system will not be down, and the isolation provided by the processes and their restarting strategy helps us to achieve it.

Functional programming promotes a coding style that helps developers write code that is short, concise, and maintainable. Out of the box, Erlang VM provides the capability to hot reload code, which is the best-case scenario if we want to provide on-chain governance without restarting nodes.

### Why C ?
We interact with hardware components for  projects like Hybrid Root of Trust with Trusted Platform Module(TPM), Decentralized Identity with Security Keys. C seems the best candidate for interfacing such hardware devices with libraries Esys library(TPM ) and libpiv(Security key).

In addition to the above for intensive and complex  computing, tasks we rely on C to perform those processing.

### Why ScyllaDB ?

ScyllaDB is a NoSQL database built from the idea of Cassandra - Wide Column Database - but with more efficiency in terms of memory consumption and CPU processing.
As it's implemented in C++, it's faster and lightweight and takes advantage of low-level Linux primitives.

We are using a Wide Column Database, but we want to be able to fetch only some part of the data, so a column database fits really well for this kind of purpose.
Moreover, we want a database with a high throughput in writing, and ScyllaDB fits really well with its LSM storage engine.

## Structure

Code base is divided into domains (contexts) for better single responsibility principle:

<!-- Source of the SVG on https://markmap.js.org/repl
## Archethic_web 

### Explorer UI

- TransactionChain explorer
- BeaconChain Live
- OracleChain Live
- Node listing
- Metrics/Dashboard
- Governance

### API

#### REST
- Transaction sending
- Transaction fee

#### GraphQL: Transaction queries

## Archethic

### Crypto
- Node keystore
- Shared secrets store

### P2P
- InMemory tables
- Messaging

### TransactionChain
- Transaction data
- Transaction building

### Election
- Hypergeometric distribution
- Validation & Storage nodes
- Heuristic constraints

### Mining
- Distributed/Standalone workflow
- Pending transaction validation
- Proof of work
- Transaction fee

### Account
- UCO & NFT Balances
- UTXO lookup

### Contracts
- Interpreter
- Worker

### OracleChain
- Scheduler
- Services

### BeaconChain
- Subset
- Slot
- Summary
- Scheduler

### SharedSecrets
- Scheduler
- Node renewal
- Origin renewal
- InMemory tables

### Bootstrap
- Network initialization
- Node joining

### SelfRepair
- Scheduler
- Sync

### Replication
- Transaction validation
- Transaction downloading

### Networking
- IP lookup
- NAT traversal
- Port forwarding

### Governance
- Proposal analyzer
- Proposal testing
- Pools

### DB
- Storage layer

### Metrics
- Collector
- Scheduler
- Parser
-->

![core structure](/img/core_structure.png)
