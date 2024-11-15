# Deploy spin-contracts

{% hint style="warning" %}
In future version, these contracts will only needed to be deployed once per chain.
{% endhint %}

zkSpin provides a series of on-chain smart contracts. They act as a central hub.

### Prerequisite

* Node.js >= 20.0

## Obtain your chain's information

We need two piece of information:

#### 1. RPC node URL

This can be a public node or from node vendor like [Alchemy](https://www.alchemy.com/supernode) or [Infura](https://www.infura.io/). For a local hardhat or ganache node, this could be `http://localhost:8545`

<details>

<summary>Local development</summary>

You can use hardhat to start a testnet locally for development. Follow the steps below.

</details>

#### 2. Private key of an account with enough gas fee to pay for deployment

Example

```typescript
0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
```

## Deploy the smart contracts

{% hint style="info" %}
This guide deploys the contracts via hardhat
{% endhint %}

1. Obtain the smart contracts

```bash
git clone https://github.com/zkspin/opzk.git
```

2. Go into `spin-contracts`

```
cd opzk/spin-contracts/
```

<details>

<summary>Local development</summary>

Open a new terminal and run `npm run node` to start a local hardhat network.  Leave the `.env` with the same values as in `.env.template`

</details>

3. Fill in `.env` with your account private key and node RPC following `.env.template`&#x20;
4. Install hardhat and dependencies

```bash
npm install
```

5. Deploy the contracts

```bash
npm run deploy-contracts
```

The above command will generate outputs of the contract address deployed like so:

{% hint style="danger" %}
Save the deployed contract addresses somewhere permanente
{% endhint %}

```
Deployed Addresses

OPZKGameModule#MultiSender - 0x5FbDB2315678afecb367f032d93F642f64180aa3
ZkwasmVerifier#AggregatorVerifierCoreStep1 - 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512
ZkwasmVerifier#AggregatorVerifierCoreStep2 - 0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0
ZkwasmVerifier#AggregatorVerifierCoreStep3 - 0xCf7Ed3AccA5a467e9e704C703E8D87F634fB0Fc9
ZkwasmVerifier#AggregatorVerifier - 0xDc64a140Aa3E981100a9becA4E685f962f0cF6C9
OPZKGameModule#SpinOPZKGameContract - 0x5FC8d32690cc91D4c39d9d3abcBD16989F875707
OPZKGameModule#SpinGameRegistryContract - 0x61c36a8d610163660E21a8b7359e1Cac0C9133e1
OPZKGameModule#StakingContract - 0x23dB4a08f2272df049a4932a4Cc3A6Dc1002B33E
```

SpinOPZKGameContract is the contract we'll interact with the most.



