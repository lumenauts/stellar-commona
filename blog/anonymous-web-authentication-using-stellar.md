---
title: Anonymous web authentication using Stellar
description: Learn how to implement anonymous blockchain-based web authentication with Stellar, JavaScript and Node.js.
image: [Anonymous web authentication](../assets/anonymous-web-authentication.png)
author: Viktor Sokolov
date: 2019.02.29
canonical: https://evilmartians.com/chronicles/anonymous-web-authentication-with-stellar-blockchain
---

_This post was[ originally published](https://evilmartians.com/chronicles/anonymous-web-authentication-with-stellar-blockchain) on the Evil Martians[ blog](https://evilmartians.com/chronicles) by[ Viktor Sokolov](https://github.com/gzigzigzeo) and[ Sergey Nebolsin](https://github.com/nebolsin) - Lead Developers at[ Evil Martians](https://evilmartians.com/)._

**Learn how to use the combination of signed transactions and JWT tokens to authenticate users from a pair of cryptographic keys that identify the blockchain user. This article is based on[ Stellar Web Authentication](https://github.com/stellar/stellar-protocol/blob/master/ecosystem/sep-0010.md) proposal by Sergey Nebolsin and Tom Quisel that now becomes a part of[ Stellar](https://www.stellar.org/) ecosystem. The same approach might be used in theory with other blockchain solutions.**

Strictly speaking, web-based authentication and blockchain do not even have to appear in the same sentence, as technologically these concepts are not more related than apples and oranges. In reality, though, it is the web that is the medium where customers discover blockchain solutions and use the ecosystem of blockchain-based services: from wallets and stocks exchanges to shady ICO websites and lotteries.

Every application that involves blockchain transactions and is facing the web needs to authenticate users somehow and, surprisingly so, the majority of these offerings still rely on traditional ways of HTTP-based authentication: email-password pair or a social media login through[ OAuth](https://en.wikipedia.org/wiki/OAuth) or similar access delegation standards. As this ties the user to a real-world email address or a social account, modern web leaves no place for anonymity. And as the majority of web services store user-related information in their own databases, breaching one centralized data storage can result in an unprecedented[ personal data leak](https://www.nytimes.com/2018/09/28/technology/facebook-hack-data-breach.html).

> Blockchain technology, on the other hand, is based on two pillars: _privacy_ and _decentralization_.

There is no way of associating a real-world person with a specific blockchain account unless they disclose this link deliberately. There is no central data storage anywhere. All the information about users and their actions is distributed among the vast number of nodes of the specific blockchain. It is nearly impossible to fake a node or take the whole network down.

Even while modern authentication solutions for the Web are quite robust, they contradict the whole philosophy of blockchain: disclosure of _any_ personal information, be it an email, a social account, or a plain nickname violates the privacy principle. The promise of decentralization is also broken.

While working on[ Stellar-based](https://www.stellar.org/) project for one of our clients, we faced the need to implement an authentication solution and have studied the blockchain ecosystem for an approach that satisfies both privacy and decentralization requirements and is simple and straightforward to implement. In the end, we chose to build an authentication flow from scratch—on top of a Stellar protocol that our client was already actively using. We took the whole journey from proposal to implementation, and now you are free to use it in your own projects.

[Stellar Web Authentication](https://github.com/stellar/stellar-protocol/blob/master/ecosystem/sep-0010.md) is a secure method of creating the session between two parties—application and user—that _does not_ require a user to provide anything except his blockchain-related data and does not involve any third-party services on the application’s side.

Before we dive into details of implementation, stay with us for a quick recap of some important concepts. Otherwise, jump[ directly](https://evilmartians.com/chronicles/anonymous-web-authentication-with-stellar-blockchain#authenticating-with-blockchain) to code examples.

## What is a blockchain?

If you strip down all the buzz, the idea behind the blockchain is simple: it is just a[ linked list](https://en.wikipedia.org/wiki/Linked_list) of elements that contain information. These elements are called “blocks”, and as they are _chained_ together by a certain data structure, we call the whole thing “blockchain”.

This list is distributed among the nodes on the network in a way that every node possesses a current version of the list (also called a _ledger_) and can validate it. Most importantly, the ledger is cryptographically protected to make any major modification enormously difficult and virtually impossible.

## What is Stellar?

[Stellar network](https://www.stellar.org/) is a blockchain-based payment infrastructure run by a nonprofit foundation. It uses a[ consensus-based protocol](https://www.stellar.org/blog/stellar-consensus-protocol-proof-code/)<span style="text-decoration:underline;"> </span>instead of mining to verify transactions and ensure the integrity of the system, thus solving the transaction speed problem that haunts other blockchain offerings, including Bitcoin. Stellar protocol can be used as a foundation for any “coin” in blockchain sense of the word, it does not have to be a currency per se. Stellar’s own coin is called[ lumen](https://www.stellar.org/lumens/). Here is what we like about Stellar:
- It is _fast_.[ Bitcoin](https://bitcoin.org/en/) or[ Ethereum](https://www.ethereum.org/) payments could take hours or even days to come through. Stellar payments generally take 2 to 5 seconds: since the network does not rely on resource-heavy mining. Stellar Consensus Protocol has also recently become an[ IETF draft proposal](https://tools.ietf.org/html/draft-mazieres-dinrg-scp-05) and is promoted as a way to ensure trust in any distributed storage solutions, not necessarily blockchain.
- It allows issuing[ custom tokens](https://stellarterm.com/#markets) (called[ “assets”](https://www.stellar.org/developers/guides/concepts/assets.html)). Such tokens might be used to represent any third-party entity or asset (like stock valuables) making it possible to implement real-economy accounting on the blockchain.
- It has integrated distributed exchange (called[ SDX](https://www.stellar.org/developers/guides/concepts/exchange.html)). SDX eliminates the requirement to use any third-party centralized exchanges for assets and[ facilitates conversion between third-party currencies.](https://apay.io/in)
-[SDKs](https://github.com/stellar/) implement a lot of features and are easy to use.
- Stellar is based on[ ED25519 elliptical curve](https://ed25519.cr.yp.to/) cryptography.

Like any other blockchain, it has two base networks: test and public. A public network holds actual money, a test one is used for development.

We will use Stellar in our authentication examples, but keep in mind that the same principle might be implemented for many other blockchains.

## What is a “blockchain user”?

The key concept of every blockchain is a _key pair_.

Whatever algorithm is used, key pair consist of two parts: public and private. Think of a public key as of a bank account number: you can safely show it to anyone and expect to be paid. Private (or secret) key, however, should be securely stored on the account owner’s side so it can be used by the owner to perform operations on an account.

> Basically, in blockchain world, _user_ = _key pair_.

Surely, a single person may own dozens or hundreds of different accounts, but there is no way to link them between each other without knowing any personal details: in other words, without disclosing user’s privacy.

Here is what a Stellar key pair looks like:

```
# Public key (starts with "G")
GC5X3FML4S25HDAMJYZJYAC3CKLDWV2Z6YPV3IZXOSHSQKNSKUNQFXQN

# Private key (starts with "S")
SBX7SARQOFS6IM2HS2N5TVK54AEF55E3FHOXBTWA6IPEEJJ4W5WJWE6W
```

A Stellar key pair (representing an account) can be generated in[ Stellar Laboratory](https://www.stellar.org/laboratory/#account-creator?network=public). After creation, you can fund your account with “test” lumens that give you full API access to Stellar network in[ test mode](https://www.stellar.org/laboratory/#?network=test). Note, however, that the account will not physically appear on the _real_ network unless it is funded with real money. For the purposes of pure authentication, however, whether account exists or not is not important: any valid key pair is enough, as you will see later.

## Authenticating with blockchain

First of all, we have to accept the fact that both our user and our application are identified through their respective key pairs _only_. Both parties can safely know each other’s _public_ keys—and nothing else.

An application needs to ensure that a user who tries to get in is the real owner of an account. A user needs to ensure the same thing about an application. If both parties can verify each other’s identity, they can establish a session.

The very important fact about the key pair is that owners can sign and encrypt messages using their private keys. This signing usually happens under two scenarios.

- **Cryptographic signature:** A party that knows the public key can ensure that the message was sent by the real owner of the corresponding private key.
- **End-to-end encryption**: Message can be encrypted in the way that only the party holding a specified public key _and_ knowing encryptor’s public key can decrypt it.

The second scenario is not relevant to our specific use-case, but if you want to see the implementation of end-to-end encryption in JavaScript, or play with the algorithm[ interactively](https://tweetnacl.js.org/#/box)—we recommend taking a look at the[ TweetNaCl.js](https://tweetnacl.js.org/) library (pronounced “tweet-salt”, see the chemistry pun?) that has succinct implementation (some are so tiny they fit in a tweet, hence the name) of most important cryptographic primitives.

In other words, to perform a successful authentication, we need to verify two simple facts:

- An application with a known public key owns its secret key.
- A user with a known public key owns their secret key.

[SEP-0010](https://github.com/stellar/stellar-protocol/blob/master/ecosystem/sep-0010.md) (Stellar Ecosystem Proposal), co-authored by Sergey Nebolsin, describes the flow in detail, but here is the summary:

1. A user opens an application and requests a _challenge_.
2. The server generates a challenge: a regular signed Stellar transaction containing the client’s public key and some additional data to validate later.
3. A user validates a challenge transaction signature and, if it is valid, signs it and sends it back to the application.
4. An application validates an original challenge transaction and user’s signature, and issues a JWT access token.

Let’s take a look at the implementation of each step. Step 1 is a simple HTTP request to a certain endpoint which is just an implementation detail, so let’s skip it and proceed directly to Step 2.

## Generating the challenge server-side

Our challenge transaction performs a single[ Manage Data](https://www.stellar.org/developers/guides/concepts/list-of-operations.html#manage-data) operation and uses an invalid sequence number to prevent it from being executed on a ledger: as we only leverage certain features of Stellar Protocol to perform one-off authentication, we don’t want to mutate the whole blockchain. Stellar uses a binary format known as[ XDR](https://www.stellar.org/developers/guides/concepts/xdr.html) to encode ledger and transaction data. You can build that representation either manually in Stellar’s[ Transaction Builder](https://www.stellar.org/laboratory/#txbuilder?network=test), or programmatically through various[ SDKs](https://www.stellar.org/developers/reference/). When converted to the human-readable format, our transaction will look something like this:

```js
{
  // Server public key
  "sourceAccount": "GCWHSK5KNLKB77NAEA3CKDKLY5GTMNKHXQRPUF6STZOD3J6VYVVZNBRV",
  // Invalid sequence number: prevents transaction from real execution
  "seqNum": 0,
  // Challenge is valid within this time limits to prevent replay attacks,
  // say, time of request + 5 minutes
  "timeBounds": {
    "minTime": 1547346533168,
    "maxTime": 1547346533468
  },
  "operations": [
    {
      "type": "manageData",
      // Client public key
      "sourceAccount": "GAZQFKRTG2LBDG5ARWL4MZ2QLNVZYQHRR7K7RPH2BQAPTSF7MXOMVWXO",
      // Service name
      "name": "Sample auth",
      // Random sequence to make challenges requested simultaneously
      // have different XDR's
      "value": "75d1e919612acdcc161fbe675eead56f995a9c12b58d591104cb272ae0b7ec75"
    }
  ],
  "signatures": [
    // [0] is signed with server private key
  ]
}
```

[Here’s how it looks like](https://www.stellar.org/laboratory/#xdr-viewer?input=AAAAAKx5K6pq1B%2F9oCA2JQ1Lx002NUe8IvoX0p5cPafVxWuWAAAAZAAAAAAAAAAAAAAAAQAAAWhFClswAAABaEUKXFwAAAAAAAAAAQAAAAEAAAAAMwKqMzaWEZugjZfGZ1Bba5xA8Y%2FV%2BLz6DAD5yL9l3MoAAAAKAAAAC1NhbXBsZSBhdXRoAAAAAAEAAABANzVkMWU5MTk2MTJhY2RjYzE2MWZiZTY3NWVlYWQ1NmY5OTVhOWMxMmI1OGQ1OTExMDRjYjI3MmFlMGI3ZWM3NQAAAAAAAAAB1cVrlgAAAEBDbAEEr%2F6oQSEEvXxdLaESc5EglXbm1Wsr7twUL3qIyUaHHe7cJtsH3pGd2T%2BtpNeVXEwicttEAgKYBLz3QsEF&type=TransactionEnvelope&network=test) in Stellar laboratory’s XDR Explorer. Error checking signatures... is expected: accounts used for the sake of example are not funded and do not exist in a public network.

Here is how we can build it on our Node.js server with the help of Stellar[ JavaScript library](https://github.com/stellar/js-stellar-sdk) upon receiving a client’s public key in an HTTP request. These are just the important bits from the[ full implementation](https://github.com/gzigzigzeo/stellar-sep-0010-implementation/blob/master/handlers/challenge.js) available on GitHub.

```js
// ...
// Set server public key and an invalid sequence number.
// Per specification, the number should be 0.
const account = new stellar.Account(
  SERVER_KEY_PAIR.publicKey(),
  INVALID_SEQUENCE
);

// Set time bounds
const minTime = Date.now();
const maxTime = minTime + 300; // + 5 minutes
const timebounds = {
  minTime: minTime.toString(),
  maxTime: maxTime.toString()
};

// Create a Manage Data operation
const op = stellar.Operation.manageData({
  source: req.query.public_key, // ?public_key=...,
  name: "Sample auth",
  value: crypto.randomBytes(32).toString("hex")
});

// Finally, build a transaction
const tx = new stellar.TransactionBuilder(account, { timebounds })
  .addOperation(op)
  .build();
tx.sign(SERVER_KEY_PAIR); // Server key pair with private key, defined somewhere in secrets
const xdr = tx.toEnvelope().toXDR("base64");

// Send HTTP response
res.json({ transaction: xdr });
```

## Validating and signing client-side

After our client had received a server-generated challenge transaction in an HTTP response, it must validate the server’s signature, add their own signature and send it back to exchange a doubly-signed transaction for an access token.

```js
// ...
// get the transaction XDR out of request body
const tx = new stellar.Transaction(body.transaction);
const { signatures } = tx;
const hash = tx.hash();

// SERVER_KEY_PAIR here represents server public key. This key pair can be
// used to check signatures only.
const valid = signatures.some(signature =>
  SERVER_KEY_PAIR.verify(hash, signature.signature())
);

if (!valid) {
  throw "Server signature invalid";
}

// Sign and encode back to XDR
tx.sign(CLIENT_KEY_PAIR); // Client private key, defined in config
const xdr = tx.toEnvelope().toXDR("base64");
```

Now checked and signed challenge transaction should be sent back to the server to obtain an access token. Take a look at the[ full code example](https://github.com/gzigzigzeo/stellar-sep-0010-implementation/blob/master/client.js) in our SEP-0010 implementation repository.

## Validating server-side

We are almost done with our authentication, but it is the time to perform final checks to ensure that the challenge is still valid (at least it did not expire), and the client signature is valid as well. A valid challenge must:
- be signed by a client who has initially been requesting challenge;
- be signed by a server having a valid signature;
- contain a source account equal to the server’s public key;
- contain a single[ ManageData](https://stellar.github.io/js-stellar-sdk/Operation.html#.manageData) operation with source account equal to the client’s public key; and
- not be expired (current time is still within transaction time bounds).

```js
// ...
const tx = new Transaction(req.body.transaction);
const op = tx.operations[0];
const { signatures } = tx;
const hash = tx.hash();

// Source account is ours
if (tx.source != SERVER_KEY_PAIR.publicKey()) {
  return res.json({ error: "Invalid source account." });
}

// Challenge transaction was generated by us
if (
  !signatures.some(signature =>
    SERVER_KEY_PAIR.verify(hash, signature.signature())
  )
) {
  return res.json({ error: "Server signature is missing or invalid." });
}

// ...
```

See the[ full example](https://github.com/gzigzigzeo/stellar-sep-0010-implementation/blob/master/handlers/token.js) for the complete implementation of all these checks.

Now, if everything is OK, server issues a JWT access token that can be used by our client to access the service.

```js
// ...
const token = jwt.sign(
  {
    iss: ENDPOINT, // Issuer URL
    sub: op.source, // Client public key
    iat: Math.floor(Date.now() / 1000), // Issued now
    exp: Math.floor(Date.now() / 1000) + 3600, // Expires in one hour
    jwtid: tx.hash().toString("hex") // Challenge transaction hash as ID
  },
  JWT_SECRET
);

// ...

res.json({ token: token });
```

Finally, take a look at our[ complete example](https://github.com/gzigzigzeo/stellar-sep-0010-implementation) to see the whole authentication service in action.

## How can it be used?

One possible scenario: we need to provide a monthly subscription to a web service paid by cryptocurrency, Lumen or another Stellar-based coin. Here, the first payment and the subsequent authentication can be combined in one UX step: a convenience unimaginable in a world of conventional logins and traditional payment systems.

Here is the example flow:
1. A user sends the first payment to the service owner’s account (server public key).
2. The server acknowledges payment and adds payment source address to the[ the white list of clients](https://github.com/gzigzigzeo/stellar-sep-0010-implementation/blob/master/handlers/token.js#L52) who are allowed to use the service. This could be done using[ Stellar Bridge](https://github.com/stellar/bridge-server), and white list might be a simple plaintext file. Otherwise, the server might periodically monitor incoming payments via a[ Stellar Horizon](https://www.stellar.org/laboratory/#explorer?resource=accounts&endpoint=single&network=public) endpoint.
3. A user authenticates using an API endpoint through a technique described above.
4. The server checks the account against a white list and grants access.

There is only one step missing in our example. SEP-0010 requires certain[ stellar.toml](https://www.stellar.org/developers/guides/concepts/stellar-toml.html) entries to be added for automatic service discovery. It might be a good idea to[ generate](https://github.com/gzigzigzeo/stellar-sep-0010-implementation/blob/master/handlers/toml.js) this file in your real-world application dynamically.

If you are interested in blockchain solutions and ever felt a need to authenticate your customers anonymously and securely, please feel free to study the[ Stellar Web Authentication proposal](https://github.com/stellar/stellar-protocol/blob/master/ecosystem/sep-0010.md) (note it is pretty much a living specification at the moment, but the general approach is not subject to change) and borrow from our[ demo implementation](https://github.com/gzigzigzeo/stellar-sep-0010-implementation) in JavaScript and Node.js.

The idea to use blockchain key pairs for authentication has definitely[ been](https://github.com/bitid/bitid) tried[ before](https://github.com/satoshilabs/slips/blob/master/slip-0013.md).[ Trezor](https://trezor.io/)’s solution is especially[ worth noting](https://wiki.trezor.io/Developers_guide:Trezor_Connect_API_-_Login_with_Trezor) as it makes authentication with a crypto-currency hardware wallet as easy as pressing “Log in with Google” or “Log in with Facebook” buttons. However, the speed of Stellar network and the general accessibility of the surrounding ecosystem makes it a perfect candidate for the passwordless flow. And the[ SEP-0010](https://github.com/stellar/stellar-protocol/blob/master/ecosystem/sep-0010.md) itself was designed with security and the _ease of implementation_in mind, leveraging only the features already built in into the protocol: we will be happy to see similar specifications appear for other blockchains.

At Evil Martians, we have been working on projects involving Stellar, and blockchain in general, for a while, and we will be happy to take on something new in a crypto area.[ Drop us a line](https://evilmartians.com/#talk-to-us) if you have something in mind!
