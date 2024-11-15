---
description: Deploy a full local spin-OPZK stack for development.
---

# Deploy locally for dev

In this guide, we'll deploy a full spin-OPZK components for local development. This includes&#x20;

1. hardhat local testnet blockchain
2. deploy OPZK contracts to the testnet
3. spin-operator for handling submissions
4. \[optional] spin-challenger for validating submissions
5. \[optional] spin-da for providing nessacary data cache for validating submission



## hardhat local testnet blockchain

1. Obtain the smart contracts

```bash
git clone https://github.com/zkspin/opzk.git
```

2. Go into `spin-contracts`

```bash
cd opzk/spin-contracts/
```

3. Install the dependencies

```bash
npm install
```

4. Run the local hardhat node, keep this terminal live

```bash
npm run node
```

5. In a separate terminal, deploy the smart contracts

```bash
npm run deploy-local-opzk
```

6. Run OPZK operator

```bash
docker run --env-file ./.env.template -e CHAIN_RPC_URL=http://host.docker.internal:8545 --mount type=bind,source=./wallets.json,target=/app/wallets.json -t zkspin/spin-operator
```

7. \[optional] Run OPZK challenger

TODO

8. \[optional] Run spin-DA

