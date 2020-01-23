---
title: Consensus Protocol
description: In this video, we explore the Stellar Consensus Protocol, or Federated Byzantine Agreement, and compare it to Proof of Work and Byzantine Fault Tolerance.
order: 50
---

[![Consensus protocol](http://img.youtube.com/vi/X3Gj2nQZCNM/0.jpg)](http://www.youtube.com/watch?v=X3Gj2nQZCNM)

## Lesson Description
In this video, we explore the Stellar Consensus Protocol, or Federated Byzantine Agreement, and compare it to Proof of Work and Byzantine Fault Tolerance.

## Lesson Transcript
### Intro
Now that we have covered what the Stellar network is, we will explore - at a superficial level - the features of the Stellar Consensus Protocol, or how consensus is achieved in the Stellar network.

### What is Consensus?
Key to decentralized ledger technology is achieving consensus.

Unlike a bank’s centralized, single ledger of account balances, the validators in a network must all agree on each ledger every several seconds to minutes and update each of their ledgers to reflect the state of the network - which transactions just happened and how they change account balances.

The network must also be able to tolerate validators that lie or send incorrect messages.

This process of coming to agreement is called consensus, and it should solve the classic double spend problem - preventing someone from spending their money twice.

There are several protocols for achieving consensus in a network.

### POW
Bitcoin and Ethereum currently use Proof of Work to achieve consensus, a protocol developed in 2009 where nodes on the network compete with electricity-consuming computing power to solve cryptographic puzzles and reach consensus.

### PBFT
Many other protocols in the distributed ledger space have implemented some form of practical byzantine fault tolerance - or PBFT - a consensus protocol laid out in a whitepaper back in 1999.

In PBFT, validators send messages back and forth and use a simple voting process where a new ledger is confirmed if 66% of the validators agree on that ledger.

Ripple and Hyperledger use this protocol, and it is significantly faster and cheaper than Proof of Work, but sacrifices decentralization to achieve those features.

### FBA
Stellar originally used Ripple’s PBFT protocol.

But in 2015, Professor David Mazieres, head of Stanford’s Secure Computer Systems Group and Chief Scientist at SDF, introduced an alternative to PBFT called the Stellar Consensus Protocol, or Federated Byzantine Agreement, a decentralized alternative to PBFT.

The best way to understand federated byzantine agreement, or FBA, is to compare its features to the features of PoW and PBFT.

### Open Membership
The first important feature is open or closed membership, or who can participate in the consensus process.

#### PoW
In PoW, anyone with a mining rig can participate in consensus, and miners - the equivalent of a Stellar validator - can join and leave the network without impacting the consensus process. That said, to become a validator you must be able to spend thousands of dollars on a mining rig.

#### PBFT
In a PBFT environment, there must be an approved validator list defined by a centralized authority, often the company or developers behind the protocol. Anyone can spin up a validator, but you can only participate in consensus if the controlling authority decides to add you to the validator list. This means you can’t have validators joining and leaving the network without consensus being impacted.

This requirement for an approved validator list means that PBFT is a centralized and closed membership system. Even if you have 1 million nodes on the approved list of validators, and you’d still be centralized because there’s one authority deciding who is on the list. It works if you know and trust every validator on the list, but that’s not feasible in the real world.

#### FBA
In FBA, there is no single, master validator list chosen by a centralized authority.

Rather, each validator decides which other validators they trust, and their list of trusted validators is called their quorum slice.

The quorum slices of each validator overlap to form a quorum, or network-wide consensus on a transaction.

Without the need for one centralized authority to decide on the validator list, you create an open membership network. Anyone can spin up a validator and participate in consensus if any other participating validator adds you to their quorum slice.

Because there is no one master authority deciding which nodes get to participate in consensus, the network exhibits decentralization.

Like Bitcoin, you can have validators joining and leaving the network without much impact on consensus.

It’s important to note that the Stellar network is not yet as decentralized as, say, Bitcoin. But it’s construction inherently allows for growing decentralization (unlike PBFT and Ripple’s construction) as more and more nodes are added to the network and new quorum slices form.

So to summarize this feature, quorum slices allow for open membership and therefore decentralization.

### Safety Preference
The second important feature is safety.

According to what is called the FLP impossibility proof, in distributed environments like Bitcoin and Stellar, a consensus mechanism can have at most two of these three properties:
- fault tolerance
- safety
- liveness
This is a proven result. Any distributed consensus system on the Internet must sacrifice one of these features.

### Fault Tolerance
Fault tolerance means that the system can survive the failure of a validator at any point. Basically all consensus protocols choose this as one of their three properties.

Different consensus protocol types diverge though when it comes to the last two properties - safety vs liveness.

#### Safety
Safety is a guarantee that something bad will never happen. That bad thing is an accidental fork. This means that if the network cannot agree on the ledger, it will not fork and the validators will not create two different ledgers.

Instead, the network will stop making progress. You’ll manually have to go in and figure out what is going on.

Put in the context of a legal system, a legal system prefers “safety” if it errors on the side of making sure no innocent people will be jailed, but, as a result, some guilty people will escape justice.

#### Liveness
Liveness is a guarantee that something good will happen. That good thing is ledgers closing. This means that the network will always close a ledger to be live, responsive, and accepting future transactions.

This means that validators may diverge on different ledgers, causing an accidental fork. Network participants won’t know which ledger the network will ultimately decide to take, exposing the network to double-spend risks.

Another legal system analogy: a legal system that errors on the side of jailing criminals instead of never jailing innocent people prefers “liveness”, meaning that sometimes the system may fail and jail an innocent.

To boil down the difference between safety and liveness: You can’t be sure that a distributed system both won’t accidentally fork (safety) and that it will keep making progress (liveness). So you have to chose which of those options you want to forgo.

To close out the legal system analogy: It’s incredibly difficult for a legal system to guarantee that both no innocents will be jailed (safety) and all criminals will be jailed (liveness). A legal system cannot achieve both, so its construction must favor one or the other.

#### PoW
PoW favors liveness over safety, meaning that if China’s Internet were to get cut off from the rest the world, a fork in the Bitcoin blockchain would occur. Miners in China would keep building on one blockchain, and miners outside of China would fork and create their own ledger, opening up users to double-spends.

To avoid this risk, PoW users wait for several ledgers to get confirmed before feeling certain that a transaction was included and that it is not on some alternative fork of the ledger. This increases transaction times by 6-12x, though, as we will discuss later.

#### PBFT/FBA
Most PBFT implementations favor safety over liveness, as does the SCP implementation of FBA. In the event of an accidental fork, it halts progress of the network until consensus can be reached.

If it means not confirming new transactions, Stellar believes that is a better outcome than confirming transactions that people will later disagree about. This is a crucial feature in the minds of central banks, banks, and other fiat anchors when deciding which DLT to build on.

Ripple claims that its PBFT system doesn’t sacrifice any of the three properties, which is inconsistent with the FLP impossibility proof.

### Low Latency
The third feature is low latency, or more simply, transaction speed.

#### PoW
In Bitcoin, the mining of a block occurs every 10 minutes, and because Bitcoin favors liveness over safety, you must wait for 6 blocks to be confirmed before having certainty that your transaction was included on the primary chain. This causes transaction times in the range of 1 hour.

#### PBFT/FBA
Unlike PoW, there is no mining process with large computational effort in PBFT and FBA - it is just message passing with a voting process. Because it’s just message passing, the transactions confirm much faster - every 3-5 seconds.

And because SCP favors safety over liveness, you don’t have to wait for several ledgers to get confirmed. This preference for safety shortens the transaction time by 6-12x.

### Asymptotic Security
The final feature is asymptotic security. This means no amount of computing power can overtake the network.

#### PoW
PoW does not exhibit asymptotic security, as some irrational attacker or someone with outside incentives could defeat PoW consensus with what is called a 51% attack.

Miner’s computing power performs consensus in PoW, and if you have 51% of the computing power on the network, you dictate consensus.

If the top few miners in the Bitcoin network colluded, or a large government felt threatened by Bitcoin and invested large amounts of resources into mining rigs, the network could be overtaken.

#### PBFT/FBA
But in PBFT and in FBA, a 51% attack using computing power is not possible, because computing power is not a feature of consensus.

In PBFT, 66% of validators would have to collude to sabotage consensus, but this is unlikely, as a centralized authority has control over which validators are allowed on the validator list.

In FBA, there is a complex web of overlapping quorum slices that make it impossible for even a supermajority of nodes to collude to sabotage consensus. An attacker could create 1 million validators, and unless other validators go so far as to add them to quorum slices, they will be unable to affect consensus. In PoW, you shift that trust onto the hope that one group is not able to control all the mining power.

### Summary
So to recap, FBA (and the Stellar implementation of FBA called the Stellar Consensus Protocol):
1. provides an open membership and therefore decentralized alternative to PBFT (decentralized)
2. that closes transactions in 3-5 seconds (low latency)
3. prefers safety over liveness (safety)
4. and is not susceptible to the 51% attacks of PoW (asymptotic security)
