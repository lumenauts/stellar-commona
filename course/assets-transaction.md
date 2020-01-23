---
title: Accounts & Transactions
description: As a user, you interact with the Stellar network through accounts. Accounts are saved in the global ledger. Everything else in the ledger - assets, trades, trustlines, etc. - are owned by a particular account.
order: 30
---

[![Accounts and transactions](http://img.youtube.com/vi/g5FHjOKV1-E/0.jpg)](http://www.youtube.com/watch?v=g5FHjOKV1-E)

## Lesson Description
As a user, you interact with the Stellar network through accounts. Accounts are saved in the global ledger. Everything else in the ledger - assets, trades, trustlines, etc. - are owned by a particular account.

## Lesson Transcript
### Intro
As a user, you interact with the Stellar network through accounts.

Accounts are saved in the global ledger.

Everything else in the ledger - assets, trades, trustlines, etc are owned by a particular account.

### Cryptography
Account access is controlled by public/private key cryptography.

Every Stellar account has a public key, which always starts with a G, and a secret seed, which always starts with an S. We will abbreviate them like this.

The public key is safe to share - other people need it to identify your account and verify that you authorized a transaction.

Your secret seed, however, is private information that proves you own your account. You should be very careful how share your secret seed, which we will talk about later in this video. It’s kind of like the combination to a lock - anyone who knows the combination can open the lock.

You can also set up what is called multi-signature accounts, requiring the signature of multiple secret seeds to authorize transactions.

### Transactions
Transactions are how you apply changes to your account.

For an account to perform a transaction - for example, make a payment - the transaction must be signed by the secret seed associated with that account’s public key.

During every round of consensus, nodes check the signature on each transaction to insure that they were signed with the appropriate secret seed before including the transaction in the ledger.

When submitted, this specific example transaction would deduct 20 lumens from your account and send them to your desired destination.

### Operations
Transactions are made up of individual operations.

Each operation is a command that applies changes to your account on the ledger.

Operations are used to send payments, enter orders into the decentralized exchange, issue trustlines to be able to hold assets, and more.

### Wallets
You can access your account and create and sign transactions using apps called Wallets.

Unlike physical wallets where the asset, or cash, is stored inside the wallet, cryptocurrency wallets don’t store your assets.

Instead, they store your secret seed, ideally in a secure manner, and/or sign transactions with your secret seed.

Your assets are not stored in your wallet, but rather are associated with your account in the global Stellar ledger.

A wallet usually gives you a way to view your account and the assets and data associated with that account.

It often also helps you compose transactions

As well as sign them and submit them to the network.

### Security
Since wallets have access to your secret seed, it’s important to employ safe wallet practices.

The basic security tactic is to spread your assets across two or more wallets on a spectrum from secure to accessible:

On one end, you have what is referred to as a cold wallet - a more secure but harder-to-access place to store your secret seed

And on the other, you have a hot wallet - a less secure but more accessible location

Cold wallets are usually paper wallets - just your secret seed written down on a piece of paper - or hardware wallets - devices somewhat similar to USB drives that are not connected to the internet. The most frequently used hardware wallets are the Ledger Nano S and Trezor - physical devices that store your secret seed offline, sign transactions offline, and submit transactions to the network via a separate connected device, but never let your secret seed leave the device itself.

Hot wallets are devices that are connected to the internet - often an app on your phone, an app on your computer, or a password manager on your computer. They allow you to compose transactions or copy and paste your secret seed into a web wallet that will compose transactions. Because hot wallets are exposed to the Internet, someone could steal your secret seed - so you should only keep as much value in a hot wallet as you are willing to lose.

On this wallet spectrum, people keep the majority of their holdings on the secure side, and just enough to use on the accessible side. They top off a hot wallet with assets from their cold wallet when necessary, like banks move assets from the vault to the cash register.

And different wallets fall at different points across this spectrum - from hardware wallets to mobile wallets to web wallets etc.

### Federation
To make sending and receiving easier, the Stellar federation protocol resolves Stellar addresses into Stellar public keys

Stellar addresses are email-like, human-readable addresses that are much easier to type out and share than a public key.

Stellar addresses have two parts: a username and a domain. They are split by an asterisk instead of an @ symbol.

An asterisk is used so that usernames can be email addresses.

When someone else sends assets to your account, instead of typing in a long, error-prone string of random characters as a public key, the assets can be sent to your simple Stellar address.
