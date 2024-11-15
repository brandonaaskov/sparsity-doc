# Deploy spin-operator

spin-operator consists of an Express.js server that accepts player submission and then relay the submission to the blockchain.

## Fill in the environments

Create a new file `.env` and use [`.env.template`](https://github.com/zkspin/opzk/blob/main/spin-operator/.env.template) as example to fill it in.&#x20;

Reference provides details on filling the environments: [spin-operator-reference.md](../references/spin-operator-reference.md "mention")

## Configure wallets

{% hint style="info" %}
#### A note about transaction nonce on EVM

The operator write to the chain in two most common ways. `settle` a submission and `submit` a submission. In order to handle concurrent requests from many different players, the operator cannot reliably rely on a single wallet to submit all the transactions on-chain. This is due to each on-chain transaction needs to have an unique nonce to prevent replay attacks. Thus, a good practice is to confirm a transaction is mined, then submit the second one.
{% endhint %}

One way to scale the operator's concurrency is to let the operator own multiple wallets.



Create a file name `wallets.json`

`wallets.json` should expect a list of EVM addresses and their corresponding private keys like so:

```json
# wallets.json
[
  {
    "address": "0xd3405C7CdE4FCb6fe0fc40030Bdbaf1e932e16B8",
    "privateKey": "0x179d6eef0cbfabe4cca44ba92d6ade9357c88649699c52168eaa5b3ae4042b97"
  },
  {
    "address": "0x0EfCa6b908F2D5E285Fd02742585b86DEeC2c917",
    "privateKey": "0x30daa0ec767d98bfb388b40acf9ab1fd8728632bf3bb960b218f75d82846d26b"
  },
  {
    "address": "0x5517883fEcFA80a901844B52845bdb151eeD96Ec",
    "privateKey": "0xa15d064fcf5828ffa1a5ddad87254cb142fc2ba0d2b9067274e065e001bdd687"
  }
  ...
]
```



{% hint style="warning" %}
Make sure all of the wallets has enough ETH to pay for gas fee.
{% endhint %}

***

## Run spin-operator

There're few options running a spin-operator. To quickly run an instance, we recommend direly run the image from docker-hub.

***

### Run from source

#### Prerequisite&#x20;

* Node.js >= 20

#### Build from source

1. Clone the OPZK repo.

```
git clone https://github.com/zkspin/opzk.git
```

2. Install the NPM packages

```
cd opzk/spin-operator && npm install
```

3. Fill in `.env` following instruction from above

### Run the server

```
npm run start
```

***

## Run from docker

### Prerequisite&#x20;

* Docker version > 25.0.0

### Build the docker image

{% tabs %}
{% tab title="Pull Image from Dockerhub" %}
We already build a latest image of the spin-operator.

The image is under `docker pull zkspin/spin-operator:latest`
{% endtab %}

{% tab title="Build Image Locally" %}
1. Clone the OPZK repo.

```
git clone https://github.com/zkspin/opzk.git
```

2. Build the docker image

```makefile
cd opzk && docker build -t spin-operator .  
```
{% endtab %}
{% endtabs %}

### Run the server&#x20;

{% hint style="info" %}
Make sure you have `.env` and `wallets.json` ready
{% endhint %}

{% hint style="info" %}
Add the docker run option `--network host` if your chain RPC is on localhost
{% endhint %}

{% tabs %}
{% tab title="Run Image from Dockerhub" %}
{% code fullWidth="true" %}
```sh
docker run --env-file ./.env --mount type=bind,source=./wallets.json,target=/app/wallets.json -t zkspin/spin-operator
```
{% endcode %}
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}



