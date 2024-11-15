---
description: Initialize a game project
---

# Create a game project

Setting up a new project template



{% tabs %}
{% tab title="OPZK Prover" %}
<pre class="language-sh"><code class="lang-sh"><strong>zkspin init --type OPZK --wallet dynamic &#x3C;project-name>
</strong></code></pre>
{% endtab %}

{% tab title="ZK Only Prover" %}
<pre><code><strong>zkspin init --type ZK &#x3C;folder-name>
</strong></code></pre>
{% endtab %}
{% endtabs %}

The above command will create a scaffold for a small demo gameplay with two components and an extremely simple demo game called grid-walk.

<details>

<summary>Grid Walk Demo Game</summary>

This game has two variables, `total_step_count` and `current_position`

A valid move to left or right will increase the step count. While the moves are only valid if the positions are with in 0..10&#x20;

</details>



### Provable Gamplay Scaffold

{% hint style="info" %}
The frontend should not contain not meaningful gameplay logic, they should be written inside the provable gameplay in Rust.
{% endhint %}

A Rust project that contains provable gameplay logic.

<details>

<summary>Quickly test out the gameplay in terminal</summary>

`cd <folder-name>/gameplay && cargo run`\
\
First you'll need to enter the intial states. In this demo-game, it's two u64 numbers, enter them one by one. The first number represent the total step count and second number represent the current position.



Then you'll be prompted to enter the gameplay commands.

Enter 0 for going left, enter 1 for going right.

</details>

### Frontend Scaffold

This files are under

```
<project-name>/frontend
```

A barebone React project served with Vite.js.

<details>

<summary>Quickly test out the frontend</summary>

1. Build the ZK image\
   `zkspin build-image gameplay/provable_game_logic/`
2.  Install frontend dependencies

    `cd frontend && npm install`

<!---->

3.  Run the frontend server

    `npm run dev`

`Note that the everything is local so far. Nothing on-chain yet.`

</details>



