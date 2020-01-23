---
title: Nodes, Network, & Ledger
description: In the Stellar ecosystem, nodes store the global ledger and together make up the Stellar network.
order: 10
---

[![Nodes, network, and ledger](http://img.youtube.com/vi/sTQuGoBeKto/0.jpg)](http://www.youtube.com/watch?v=sTQuGoBeKto)

## Lesson Description
In the Stellar ecosystem, nodes store the global ledger and together make up the Stellar network.

## Lesson Transcript
### Intro
In the Stellar ecosystem, nodes store the global ledger and together make up the Stellar network.

### Nodes
This is a node.

A node is basically a computer. The computer doesn’t have to be special like the complex and expensive mining rigs of Bitcoin. It can be a simple laptop connected to the internet, which opens up access to the network for the unbanked and for developing world businesses.

Each node runs an instance of Stellar Core - open-source software created by the Stellar Development Foundation.

### Node Types
There are two basic kinds of nodes:

Watchers

Watcher nodes monitor the network, submit transactions, and store the current version of the ledger

Like a regular Bitcoin node, they do not participate in consensus.

Validators

If a node is set up to participate in consensus, it is called a validator.

Validators serve a purpose somewhat similar to Bitcoin miners - they agree on sets of transactions every several seconds via consensus and update the global ledger.

The more validator nodes in the network, the better.

As more companies build their businesses on top of Stellar and run their own validators, the network becomes more robust and decentralized.

### Ledger
Each node contains a complete copy of the global ledger.

This ledger records every transaction in the system, contains the list of all the accounts and balances, and holds all the orders in the decentralized exchange exchange.

During each round of consensus, the network of validators agrees on which transactions to apply to the global ledger.

When the new set of transactions is applied, a new “last closed ledger” is created.

Each ledger is cryptographically linked to a unique previous ledger, creating a historical ledger chain that goes back to the genesis ledger.

Watcher and validator nodes also have the option of storing a historical archive of the network and past ledgers, helping other nodes catch up and join the network.

### Network
All the nodes together make up the Stellar network, like the servers that make up the network that is the Internet today.

### Node Incentives
Unlike running a node in Bitcoin, running a Stellar node does not directly generate income.

If your business on top of Stellar - if you are a decentralized exchange interface, an anchor, a remittance company, or some other business - your incentives for running a validator include:
- Participating in consensus, adding more security to the network.
- Submitting transactions to the network without depending on a third party.
- Getting quicker access to ledger data.
- Setting up customizations that will automatically trigger some action based on some event.
- Preserving and directly accessing the history of the network by storing all past ledgers.
- As an anchor, providing your tokenholders with a trusted node to submit transaction to and see the state their own accounts
