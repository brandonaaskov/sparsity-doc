# Save & Load State on Blockchain

For most blockchain gaming usages, we can think of the blockchain as a secure and permanent database.&#x20;

This database is expensive to write, but relatively cheap to read.



We have seen we can generate proofs, whether in OPZK or ZK only implementation of zkSpin. When writing the states to the blockchain, we'll need to provide those proves.&#x20;



### Reading Game State from the Chain

We are using [WalletConnect's Web3Modal](https://docs.walletconnect.com/appkit/react/core/installation). The frontend wallet library does not matter. The example here is just trying to show what contracts and functions of the smart contracts are called.&#x20;



{% tabs %}
{% tab title="OPZK" %}
```typescript
import {
    SpinOPZKGameContractABI,
    getOnchainStatesWagmi
} from "@zkspin/lib";
import { http, createConfig } from 'wagmi'
import { mainnet, sepolia } from 'wagmi/chains'
import { getAccount, readContract } from "wagmi/actions";

export const config = createConfig({
  chains: [mainnet, sepolia],
  transports: {
    [mainnet.id]: http(),
    [sepolia.id]: http(),
  },
})

const GAME_CONTRACT_ADDRESS = <>;
const OPZK_GAME_ID = <>;
const STORAGE_STATE_SIZE = 2; // number of u64 in your GameState

export const getOnchainGameState = async () => {
    return getOnchainStatesWagmi(
        getAccount(config).address!,
        GAME_CONTRACT_ADDRESS,
        OPZK_GAME_ID,
        STORAGE_STATE_SIZE,
        readContract,
        config
    );
};
```
{% endtab %}

{% tab title="ZK only" %}
```typescript
import {
    decodeBytesToU64Array,
    GameStateStorageABI,
    SpinZKGameContractABI,
} from "@zkspin/lib";

async function getOnchainGameStates(): Promise<bigint[]> {
    const storageContractAddress = (await readContract(config, {
        abi: SpinZKGameContractABI.abi,
        address: GAME_CONTRACT_ADDRESS,
        functionName: "getStorageContract",
        args: [],
    })) as `0x${string}`;

    const userAccount = getAccount(config);

    const result = (await readContract(config, {
        abi: GameStateStorageABI.abi,
        address: storageContractAddress,
        functionName: "getStates",
        args: [userAccount.address],
    })) as string;

    // result is in bytes, abi decode it, state is 2 u64 bigint
    // this is the same number as STATE_SIZE in definition.rs
    const decoded = decodeBytesToU64Array(result, 2);

    return decoded;
}
```


{% endtab %}
{% endtabs %}



### Writing Game State to the Chain

{% tabs %}
{% tab title="OPZK" %}
```typescript
import {
    SpinOPZKProverOutput,
} from "@zkspin/lib";

async function submit_to_operator(
    submissionData: SpinOPZKProverOutput
): Promise<any> {

    const response = await fetch(`${OPZK_OPERATOR_URL}/submitTransaction`, {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
        },
        body: JSON.stringify(submission.data, (_, v) =>
            typeof v === "bigint" ? v.toString() : v
        ),
    })

    const data = await response.json();
    return data;
}
```

Gameplays are submitted to operators, operators will handle on-chain interactions.
{% endtab %}

{% tab title="ZK only" %}
```typescript
import {
    SpinZKProverSubmissionData,
    decodeBytesToU64Array,
    SpinZKGameContractABI,
} from "@zkspin/lib";

async function verify_onchain(submission: SpinZKProverSubmissionData) {
    const result = await writeContract(config, {
        abi: SpinZKGameContractABI.abi,
        address: GAME_CONTRACT_ADDRESS,
        functionName: "submitGame",
        args: [
            submission.game_id,
            submission.finalState,
            submission.playerInputsHash,
            submission.proof,
            submission.verify_instance,
            submission.aux,
            [submission.instances],
        ],
    });
    const transactionReceipt = waitForTransactionReceipt(config, {
        hash: result,
    });
    return transactionReceipt;
}
```

For zk-only, the prove is directly submitted on-chain.
{% endtab %}
{% endtabs %}

