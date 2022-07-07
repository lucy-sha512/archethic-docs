---
id: mining
title: Mining
---

Transaction validation (aka Mining) defines processes and validations algorithms.

Once a transaction is under validation a given process is dedicated to this transaction.
Depending of the number of validation nodes several validation implementation are defined.

Along with validation workflow and processes, this context defines high levels functions to get the validation nodes and to assert their election.

## Standalone validation workflow

When there not a single validation nodes (during the network initialization), a process is spawn to manage the transaction validation as a Task to run it (fire-off)

```mermaid
stateDiagram-v2
   direction LR
    [*] --> Idle
    Idle --> ValidateTransaction
    ValidateTransaction: Validate transaction
    ValidateTransaction --> Replicate
    Replicate --> Replicate: Wait confirmations
    Replicate --> NotifyAttestation
    NotifyAttestation: Notify transaction attestation
    NotifyAttestation --> [*]

```

## Distributed validation

### Worflow

```mermaid
sequenceDiagram
    Client->>+WelcomeNode: Submit new transaction
   par send transaction
       WelcomeNode->>+Coordinator: 
   and 
      WelcomeNode->>+CrossValidationNode: 
   end
   WelcomeNode ->>-Client: Transaction submited

   par build context 
      Coordinator ->>+PreviousStorageNode: Fetch previous transaction
      Coordinator ->>+PreviousStorageNode: Fetch unspent outputs
   and
      CrossValidationNode ->>+PreviousStorageNode: Fetch previous transaction
      CrossValidationNode ->>+PreviousStorageNode: Fetch unspent outputs
      CrossValidationNode ->>+Coordinator: Notify context and availability
   end
   
   
   Coordinator ->>+Coordinator: Build validation stamp
   Coordinator ->>+CrossValidationNode: Send validation stamp

   par wait validation stamp
     CrossValidationNode ->>+CrossValidationNode: Cross validate the stamp
     CrossValidationNode ->>+CrossValidationNode: Notify cross validation stamp
     CrossValidationNode ->>+Coordinator: Notify cross validation stamp
   end

   par chain replication 
      Coordinator ->>+ChainStorageNode: Replicate transaction
      CrossValidationNode ->>+ChainStorageNode: Replicate transaction
   end

   ChainStorageNode ->>+ChainStorageNode: Validate transaction and store transaction
   alt transaction valid 
      ChainStorageNode ->>+ CrossValidationNode: Replication confirmation
      ChainStorageNode ->>+ Coordinator: Replication confirmation
   end

   par notify replication
      CrossValidationNode-->+WelcomeNode: Confirm replication
      Coordinator-->+WelcomeNode: Confirm replication
      CrossValidationNode-->+PreviousStorageNode: Confirm replication
      Coordinator-->+PreviousStorageNode: Confirm replication
      CrossValidationNode-->+BeaconChain: Confirm replication
      Coordinator-->+BeaconChain: Confirm replication
   end

   WelcomeNode-->Client: Notify replication confirmations
```

### FSM

When there are multiple validation nodes, a distributed workflow process is spawn as FSM to define the states and evolution of the ARCH consensus algorithm.

This FSM process is ran by all the validation nodes.

Therefore each validation maintains a `Registry` of all the pending transaction validation processes, to be able to redirect P2P messages to the right process.

```mermaid
stateDiagram-v2
    state role_state <<fork>>
    state join_state <<join>>

    [*] --> Idle

    Idle --> Idle: Prevalidate transaction & Build context
    Idle --> role_state

    role_state --> Coordinator: first of elected validation nodes
    role_state --> CrossValidationNode: other node

    state Coordinator {
        state if_state_enough_context <<choice>>
        [*] --> WaitContext

        WaitContext: Waiting context and confirmations
        WaitContext --> WaitContext: Add context and node confirmation

        EnoughContextAndConfirmations: Enough context and confirmations ?

        WaitContext --> EnoughContextAndConfirmations
        EnoughContextAndConfirmations --> if_state_enough_context

        if_state_enough_context --> CreateValidationStamp: yes
        if_state_enough_context --> WaitContext: no

        CreateValidationStamp: Create validation stamp
        CreateValidationStamp --> NotifyValidationStamp
        NotifyValidationStamp: Notify validation stamp 
        NotifyValidationStamp --> [*]
    }

    CrossValidationNode: Cross Validation Node
    state CrossValidationNode {
        [*] --> NotifyContext
        NotifyContext: Notify transaction context

        NotifyContext --> WaitValidationStamp
        WaitValidationStamp: Wait validation stamp to validate

        WaitValidationStamp --> ValidateValidationStamp
        ValidateValidationStamp: Verify validations tamp

        ValidateValidationStamp --> ValidateValidationStamp: create cross validation stamp
        ValidateValidationStamp --> SendCrossValidationStamp
        SendCrossValidationStamp: Send the cross validation stamp to all
        SendCrossValidationStamp --> [*]
    }

    Coordinator --> join_state
    CrossValidationNode --> join_state


    join_state --> WaitCrossValidationStamps

    WaitCrossValidationStamps: Wait cross validation stamps
    state WaitCrossValidationStamps {
        state if_state_enough <<choice>>
        state if_state_atomic_commitment <<choice>>
        
        [*] --> WaitingStamps
        WaitingStamps: Wait

        WaitingStamps --> WaitingStamps: Add cross validation stamp
        WaitingStamps --> EnoughStamps

        EnoughStamps: Enough cross validation stamps ?

        EnoughStamps --> if_state_enough

        if_state_enough --> AtomicCommitmentReached: yes                                                                          
        if_state_enough --> WaitingStamps: no                                                                                     
                                                                                                                                    
        AtomicCommitmentReached: Atomic commitment reached ?                                                           
                                                                                                                                    
        AtomicCommitmentReached --> if_state_atomic_commitment                                                                    
                                                                                                                                    
        if_state_atomic_commitment --> [*]: yes                                                    
        if_state_atomic_commitment --> [*]: no
   }

    WaitCrossValidationStamps --> Replication

    state Replication {
        [*] --> NotifyTransaction
        NotifyTransaction: Notify transaction
        WaitAck: Waiting replicas confirmations
        WaitAck --> WaitAck: Add ack

        NotifyTransaction --> WaitAck

        state if_state_enough_replicas <<choice>>
        EnoughConfirmations: Enough confirmations ?
        WaitAck --> EnoughConfirmations

        EnoughConfirmations --> if_state_enough_replicas
        NotifyAttestation: Notify replication attestation
        if_state_enough_replicas --> NotifyAttestation: yes
        if_state_enough_replicas --> WaitAck: no
        NotifyAttestation --> [*]
    }

```
