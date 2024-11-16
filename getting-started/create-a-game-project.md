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

The above command will create two folders for an extremely simple game called grid-walk: a React `frontend` folder, and a provable `gameplay` folder written in Rust.

<details>

<summary>Grid Walk Demo Game</summary>

This game has two variables, `total_step_count` and `current_position`

A valid move to left or right will increase the step count, while the moves are only valid if the positions are between 0 and 10.

</details>



### The Gameplay Folder

{% hint style="info" %}
The frontend should not contain not meaningful gameplay logic. That should be written inside the provable gameplay in Rust.
{% endhint %}


`cd <folder-name>/gameplay && cargo run`\
\
First you'll need to enter the initial states. In this demo game, it's two `u64` numbers: enter them one by one. The first number represents the total step count and second number represents the current position.

You'll then be prompted to enter the gameplay commands.

Enter 0 for going left, enter 1 for going right.


### The Frontend Folder

1. Build the ZK image\
   `zkspin build-image gameplay/provable_game_logic/`
2.  Install frontend dependencies

    `cd frontend && npm install`

<!---->

3.  Run the frontend server

    `npm run dev`

`Note that the everything is local so far. Nothing on-chain yet.`



