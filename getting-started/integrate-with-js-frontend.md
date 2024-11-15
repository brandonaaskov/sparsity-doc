# Integrate with Js Frontend

This guide helps you understand built gameplay image can be used in the frontend, and how a prove is generated to be ready to submitted on-chain.

## Importing Gameplay Image

{% hint style="success" %}
No need to do anything for this step
{% endhint %}

We have provided a boiler plate code in the scaffold project under `<project-folder>/frontend/src/gameplay/gameplay.ts`

This file will handle the necessary import of gameplay images. Allowing us to run code we have written in Rust from Javascript environment.&#x20;



## Using Spin Games Helper

The package `@zkspin/lib` provides the essential tools for working with the gameplay and generating proofs.

### Prerequisite

```bash
npm install @zkspin/lib
```

### Importing

```bash
import * from "@zkspin/lib";
```

### Interact with the Gameplay

#### Initialize a SpinGame Instance

{% tabs %}
{% tab title="OPZK" %}
```typescript
let spin: SpinGame<SpinOPZKProverInput, SpinOPZKProverOutput> = new SpinGame({
        gameplay: new Gameplay(),
        gameplayProver: new SpinOPZKProver(
            {
                operator_url: OPZK_OPERATOR_URL,
            },
            getPlayerNonce,
            getPlayerSignature
        ),
    });

const getPlayerNonce = async () => {
    const player_address = getAccount(config).address;

    if (!player_address) {
        console.error("player address not found");
        throw new Error("player address not found");
    }

    const player_nonce = await readContract(config, {
        abi: SpinOPZKGameContractABI.abi,
        address: GAME_CONTRACT_ADDRESS,
        functionName: "getSubmissionNonce",
        args: [player_address],
    });

    return player_nonce;
};

const getPlayerSignature = async (submissionHash: string) => {
    // get player address

    const player_address = getAccount(config).address;

    if (!player_address) {
        console.error("player address not found");
        throw new Error("player address not found");
    }

    const player_signature = await signMessage(config, {
        message: {
            raw: ethers.getBytes(submissionHash),
        },
    });

    return { player_address, player_signature };
};

```

For local development, obtain `OPERATOR_URL` by deploy a copy locally [local.md](../opzk-deployment/local.md "mention")
{% endtab %}

{% tab title="ZK Only" %}
```typescript
const zkprover = new ZKProver({
    USER_ADDRESS: ZK_USER_ADDRESS,
    USER_PRIVATE_KEY: ZK_USER_PRIVATE_KEY,
    IMAGE_HASH: ZK_IMAGE_MD5,
    CLOUD_RPC_URL: ZK_CLOUD_RPC_URL,
});

const spinZKProver = new SpinZKProver(zkprover);

let spin: SpinGame<SpinZKProverInput, SpinZKProverSubmissionData> = new SpinGame({
    gameplay: new Gameplay(),
    gameplayProver: spinZKProver,
});
```

Use these credentials for cloud prover on us:

```properties
VITE_ZK_CLOUD_USER_ADDRESS="0xd8f157Cc95Bc40B4F0B58eb48046FebedbF26Bde"
VITE_ZK_CLOUD_USER_PRIVATE_KEY="2763537251e2f27dc6a30179e7bf1747239180f45b92db059456b7da8194995a"
VITE_ZK_CLOUD_IMAGE_MD5="b6fa3aff51d333b79e0991f9c8a624c6"
VITE_ZK_CLOUD_URL="https://rpc.zkwasmhub.com:8090"
```
{% endtab %}
{% endtabs %}

#### Start a New Gameplay

```typescript
await spin.newGame({
    initialStates: [total_steps, current_position],
});
```

#### Get Current Game State

<pre class="language-typescript"><code class="lang-typescript"><strong>const s: BigUint64Array = spin.getCurrentGameState();
</strong><strong>console.log(s)
</strong><strong>// [10, 1]
</strong></code></pre>

#### Generate a Prove for Submission

```typescript
const submissionData = await spin.generateSubmission({
    initialState: [
        10 // total_steps
        1, // current_position
    ],
    playerActions: [0, 1, 1, 1], 
    metaData: {
        game_id: BigInt(123),
    },
});
```

The `submissionData` will be used to interact with the blockchain to prove and save the states.



### Trouble Shooting

#### Handling Changes In Gameplay File Path

Our scaffold project after running `zkspin init ...` provides a working example, but the frontend assumes the position of the `gameplay/export` folder relatively.

For whatever reason, if the path to the `export/` folder changes, you can adjust this in `gameplay.ts` file.

```typescript
// frontend/src/gameplay/gameplay.ts
import { GameplayAbstract } from "@zkspin/lib";

import {
    default as init,
    initialize_game,
    step as _step,
    get_game_state,
} from "../../../gameplay/export/js/esm";
//    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ change this path
```

