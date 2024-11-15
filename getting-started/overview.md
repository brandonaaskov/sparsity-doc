# Overview

## Introduction

This will be a guide that helps you use Spin to develop trustless provable on-chain games.

The game built using the Spin SDK will include **three components**: the provable gameplay written in Rust, the on-chain contract written in Solidity, and the frontend UI written in Javascript with React.

The Spin SDK will provide toolchains to

* generate boilerplate for starting a new project
* build and publish the provable Rust gameplay
* export gameplay to use in browser environment
* integrate everything in the frontend client

[![High Level Components](https://app.eraser.io/workspace/VhcsNlA4uYelufEWe1gi/preview?elements=eVSpiKOUoeKAZNje0UowrQ\&type=embed)](https://app.eraser.io/workspace/VhcsNlA4uYelufEWe1gi?elements=eVSpiKOUoeKAZNje0UowrQ)

### OPZK vs ZK Only

zkSpin currently allows two types of proving: Optimistically Proving Zero Knowledge (OPZK) vs Zero Knowledge Only(ZK Only). &#x20;

#### ZK only

While players play games in the client. The client can record the initial state of the game and corresponding player actions. These variables can then be ZKP proved so that we can verify the final state the player claims is indeed valid and correct.&#x20;

This architecture makes the client generates the proof and then directly submit transactions on-chain similar to traditional Fully On-Chain Games (FOCGs).

However, ZKP is currently expensive and requires a large machine to generate proves. Even with relying on a cloud of large machines, proves can still take minutes to generate. This is where OPZK comes in

#### OPZK

In OPZK, players no longer need to generate a ZKP. Players instead submits the final state of the game to the operator (a web2 server). While operator helps validate the gameplay and attest it on to the network. When gameplays are visible on-chain, challengers can verify and challenge any invalid gameplays by generating a ZKP.&#x20;

Note, the OPZK support will come online once the OPZK provier is released. We are working on making the prover developer-friendly.

#### The difference

From the gameplay developer's view, these two architectures shared very similar experience. The game contracts share the same interface to the game developers. The provable game logic is written the same way.&#x20;

This means after writing game, it's a single toggle from choosing which proving architecture to use.

From the infrastructure's standpoint, OPZK requires a suite of entities to be deployed on a new chain before things can function.&#x20;

This is means while developing on local testnet for OPZK, you'll want to follow this guide: [local.md](../opzk-deployment/local.md "mention")

For any production OPZK, there needs to be game contracts, operators, challengers, DA solutions deployed on the corresponding chains.&#x20;



**Audience of this Guide**

The audience for this guide is **technical developers** who want to build provable game but don’t want to start from the scratch of writing low-level ZK circuits and figuring out the flow building such an application end-to-end.

**Programming Languages**

There also other language of choice for each component. For example, you could use C to write the provable game logic or any other frontend language. However, we only have support for the language in **Rust** for gameplay, **Javascript** for frontend. More supports are in the roadmap to be added.
