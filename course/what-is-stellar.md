---
title: What is Stellar?
description: Stellar makes sending money as easy as email. It provides a way for people and financial institutions like banks, remittance companies, and mobile payment apps to send payments between each other in any asset quickly and at almost no cost.
order: 0
---

[![What is Stellar?](http://img.youtube.com/vi/ixerXWJrDr0/0.jpg)](http://www.youtube.com/watch?v=ixerXWJrDr0)

## Lesson Description
Stellar makes sending money as easy as email. It provides a way for people and financial institutions like banks, remittance companies, and mobile payment apps to send payments between each other in any asset quickly and at almost no cost.

## Lesson Transcript
Stellar makes sending money as easy as email.

To understand what Stellar is and what problems it solves, it helps to start with how payments work today.

If you and I are in the same place, it is easy for you to send me money. We can just exchange cash. It’s instant and it’s free.

Even if we are at the same financial institution, it’s still simple. The internal ledger of accounts is updated, subtracting from my account and adding to your account.

Things become more complex when a transaction involves multiple financial institutions. My bank doesn't have a formal relationship with your bank so that means the money has to be routed through middlemen, adding fees and delays.  

And things get convoluted when I am holding one currency and you prefer another, likely in another country. There has to be a conversion in the middle, exposing to you third parties and currency volatility.

This friction is most felt by the poor, who are transferring smaller amounts. The average remittance fee is around 8-9%.

This friction causes money to move today as if the Internet doesn’t exist. And this is the problem that Stellar sets out to solve.

A good way to understand the current state of payments is to explore how email has evolved from complexity to simplicity by implementing an open internet protocol.

In the early days of electronic messages, if we were on the same computer, it was simple to send a message. I would just leave the message on the computer and you’d see it the message the next time you got on.

When we were in the same local area network, it was also fairly simple. I would just send a message over our local area network and you would get the message.

Things got more complex when we had different networks that were using different protocols. You’d have to route messages through the networks, and, if there was ever a problem, you wouldn’t know if your message was delivered or not.

This complexity was eliminated when the simple mail transfer protocol, or SMTP, was introduced in the 80s. SMTP provided this open standard for sending messages around the internet. Now anyone can send a message to anyone seamlessly.

As great as SMTP is, sending money is more complex than sending email.

First, how do you digitally represent all currencies and assets? One new token to rule them all won’t work. Just like humans naturally are resistant to everyone using one language, the future of money will likely be similar - people want to use dollars, or Euros, or shekels - many different currencies according to local preference and culture.

Second, how do you make all these different currencies interchangeable? It shouldn’t matter if you are using one and I am using the other. We need to be able to transact across currencies.

And third, unlike email where I can send the same message to multiple people, I should not be able to send $20 to you then turn around and send the same $20 to somebody else. This is what is known as the double spend problem. How do you solve this double spend problem, and do it in a scalable way?

To solve these three primary problems, Stellar provides a three-layered protocol.

To solve first the problem of representing any asset, Stellar uses anchors.

Any entity can issue a token on the Stellar ledger, and we call these issuers anchors.

Just as you give a bank cash and see it show up in your bank account in digital IOU form, you give Stellar “anchors” assets and they tokenize those assets on the Stellar network.

For example remittance companies issue fiat and exchanges issue crypto on the network.

You can simply send one of these tokens from one account to another, but Stellar goes beyond anchoring assets and powering payments for those assets.

To solve the second problem of interchangeability, Stellar provides a built-in decentralized exchange. Offers to trade one token for another at a specific price are represented as an order book built into the Stellar ledger.

The transactions through the DEX can have up to 6 hops, so if the best payment path goes from Nigerian Naira to bitcoin to lumens to Chinese Yuan, the network will automatically perform that asset transfer in one atomic transaction, so no one will end up holding an asset that they don’t want.

And layer three solves the third problem - preventing double spending at scale using the Stellar Consensus Protocol, or SCP - Stellar’s consensus mechanism alternative to Bitcoin’s proof of work.

Because SCP is just message passing, it requires significantly less energy and money and allows for thousands of transactions per second with very low fees.

Unlike other message passing consensus protocols, there is no centralized entity creating a list of who can participate in consensus.

This means Stellar is an open membership network where you pick the validators you trust, and everybody in the network doesn’t have to agree on the same validator list.

Together, Stellar’s anchored assets, decentralized exchange, and scalable consensus model can make sending money as easy as email, operating behind the scenes like email’s SMTP and creating financial inclusion for the unbanked.
