# Overview

## Introduction

This guide will you help you use the Spin SDK to develop trustless and _provable_ on-chain games.

The game built using the Spin SDK will include **three components**: the provable game state written in Rust, the on-chain contract written in Solidity, and the frontend UI written in Javascript with React.

The Spin SDK provides toolchains to:

* generate boilerplate for starting a new project
* build and publish the provable game state
* export the game state for use in the browser (WASM)
* integrate everything in the frontend client

[![High Level Components](https://app.eraser.io/workspace/VhcsNlA4uYelufEWe1gi/preview?elements=eVSpiKOUoeKAZNje0UowrQ\&type=embed)](https://app.eraser.io/workspace/VhcsNlA4uYelufEWe1gi?elements=eVSpiKOUoeKAZNje0UowrQ)

### OPZK vs ZK Only

zkSpin currently allows two types of proving: Optimistically Proving Zero Knowledge (OPZK) vs Zero Knowledge Only (aka ZKP).

#### ZK Only (aka ZKP)

The client can record the initial state of the game and corresponding player actions. These variables can then be ZK-proven so that we can verify the final state the player claims is indeed valid and correct.

This architecture makes the client generates the proof and then directly submit transactions on-chain similar to traditional Fully On-Chain Games (FOCGs).

However, ZKP is currently expensive and requires a large machine to generate proofs. Even with relying on a cloud of large machines, proofs can still take minutes to generate. This is where OPZK comes in.

#### OPZK

In OPZK, players no longer need to generate a ZKP. Players instead submit the final state of the game to the operator (a web2 server). The operator then helps validate the gameplay and attest to it on to the network. When game states are visible on-chain, challengers can verify and challenge any invalid game states by generating a ZKP.

_Note: OPZK support will come online once the OPZK prover is released. We are working on making the prover developer-friendly._

#### The Differences

The game development experience is very similar regardless of which approach you take. The game contracts share the same interface, and the SDK was structured to normalize the interface for interacting with either architecture: after writing game, it's a single toggle to choose which proving architecture to use.

From the infrastructure standpoint, OPZK requires a suite of entities to be deployed on a new chain before things can function.

This means while developing on local testnet for OPZK, you'll want to follow this guide: [local.md](../opzk-deployment/local.md "mention")

For any production OPZK, there needs to be game contracts, operators, challengers, and DA solutions deployed on the corresponding chains.



**Audience of this Guide**

The audience for this guide is **technical developers** who want to build provable games but don’t want to start from scratch writing low-level ZK circuits and figuring out the flow to build such an application end-to-end.

**Programming Languages**

There also other languages of choice for each component. For example, you could use C to write the provable game logic or any other frontend language. However, currently we only support **Rust** for provable game state, and **Javascript** for frontend. Adding support for more languages is always on the roadmap, but prioritized by market demands.
