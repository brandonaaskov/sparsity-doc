# Write Gameplay

### Define Your Game State

Think about the parts of your game state that are permanent and you want to store on the blockchain.

Define those states in terms of `u64` numbers inside `definition.rs`

```rust
# gameplay/provable_game_logic/src/definition.rs
pub const STATE_SIZE: usize = 2;

/**
 * Clone: For deep copy
 * Default: For default initialization
 */
#[derive(Clone, Default)]
pub struct SpinGameStates {
    // define your game state here in order, all data types needs to be u64
    pub total_steps: u64,
    pub current_position: u64,
}

```

For example, in the demo there are 2 states: `total_steps` and `current_position`

{% hint style="success" %}
Remember to update STATE\_SIZE when number of state changes.
{% endhint %}

{% hint style="danger" %}
Make sure to only use SpinGameStates to stored global states.
{% endhint %}

### Define the Gameplay

This basic demo game utilizes just three functions. These functions will interface with the ZK prover and frontend. You can find the functions at:

`gameplay/provable_game_logic/src/gameplay.rs`

#### Initialize the Game

Prepares the game to start. The input args is the `SpinGameStates` you defined in form of a vector of `u64`.

<pre class="language-rust"><code class="lang-rust"><strong>// gameplay/provable_game_logic/src/gameplay.rs
</strong>fn initialize_game(args: Vec&#x3C;u64>) {
    let mut game_state = GAME_STATE.lock().unwrap();
    game_state.total_steps = args[0];
    game_state.current_position = args[1];
}
</code></pre>

#### Process a Step in the Game

When the player performs an action and moves to the next step of the game. This action should update the state.

```rust
// gameplay/provable_game_logic/src/gameplay.rs
fn step(input: u64) {
    let mut game_state = GAME_STATE.lock().unwrap();

    match input {
        0 => {
            if game_state.current_position != 0 {
                game_state.current_position -= 1;

                game_state.total_steps += 1;
            }
        }
        1 => {
            if game_state.current_position != MAX_POSITION {
                game_state.current_position += 1;

                game_state.total_steps += 1;
            }
        }
        _ => {
            panic!("Invalid command");
        }
    }
}
```

#### Read the Game State

A viewable function that gets the current state.

```rust
// gameplay/provable_game_logic/src/gameplay.rs
fn get_game_state() -> Vec<u64> {
    let game = GAME_STATE.lock().unwrap().clone();
    return vec![game.total_steps, game.current_position];
}
```



## Advanced

### Define Game States that Won't Live On-Chain

If you want to define any states that only exist within the game session and won't go on-chain, you can do this by adjusting your `STATE_SIZE` variable and adjusting the `get_game_state` and `initialize_game` accordingly.

The caveat here is the frontend won't have access to these states not returned through `get_game_state` anymore. However, we have plan to add a dedicated `get_onchain_game_state` method in the future to allow that.



