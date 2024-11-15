---
description: Export and validate the gameplay for client & prover
---

# Build & Test Gameplay

### Test Gameplay in Terminal

{% hint style="info" %}
Make you have Rust installed
{% endhint %}

```
cd gameplay/ && cargo run
```

This will open a terminal prompt for playing the game.&#x20;

All states and inputs are in form of `u64`&#x20;

### Build Gameplay Image

```
zkspin build-image gameplay/provable_game_logic
```

<details>

<summary>What was built?</summary>

This will build 3 WASM packages, one for use inside Node.js, one for use inside Browser, and one for the zk prover.&#x20;

You could check these out in folder `gameplay/export/js` and `gameplay/export/wasm`

</details>



### Dry-run Gameplay Image

{% hint style="success" %}
Make sure you have [zkwasm-cli](https://github.com/DelphinusLab/zkWasm?tab=readme-ov-file#install-instructions) installed & built
{% endhint %}

```
zkspin dry-run gameplay/provable_game_logic <path-cli> \
    --initial <comma separate u64 list> \
    --action <comma separate u64 list>
```

The \<path-cli> is the path to zkwasm-cli. It's located in `zkwasm/target/debug/zkwasm-cli` after building the zkwasm.



Example

```bash
zkspin dry-run gameplay/provable_game_logic ../../zkwasm/target/debug/zkwasm-cli  \
    --initial 10,1 \
    --action 0,1,1,1,1
```

In the example above, we set the initial state to `total_step` to 10 and `current_position` to 1. The order correspond to how the `initialize_game` function is written.



### Publish Gameplay Image

This step will publish the image to a cloud prover. Currently the cloud prover is freely provided by Spin and no credential is needed.

```bash
zkspin publish-image gameplay/provable_game_logic/
```

Image Commitment, Image MD5 Hash will be printed

{% hint style="success" %}
Save the Image Commitments, MD5 hashes, and Game ID.&#x20;
{% endhint %}

<details>

<summary>OPZK only</summary>

The above step only publish the image to the cloud prover. If you want to also publish the game on-chain. \


Game ID will only be printed if on-chain parameters are setup. This game ID is an on-chain ID.

{% code overflow="wrap" %}
```
zkspin publish-image -p <private-key> -u <rpc-url> -r <registry contract address>  -n <game name> -d <game description> -a <author name> gameplay/provable_game_logic/
```
{% endcode %}

</details>



