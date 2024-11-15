# spin-operator reference

## Environment Variables

### Data Availability

We currently use AWS S3 as a DA placeholder solution. There's roadmap in the future to setup interface for DA, so that DA will be separate from operator-tasks. &#x20;

Fill in the AWS credential with permission to write to S3 buckets.

<table><thead><tr><th width="465">Name</th><th>Description</th></tr></thead><tbody><tr><td>AWS_ACCESS_KEY_ID</td><td>with S3 bucket write permission</td></tr><tr><td>AWS_SECRET_ACCESS_KEY</td><td>with S3 bucket write permission</td></tr><tr><td>AWS_REGION</td><td>with S3 bucket write permission</td></tr></tbody></table>



### Blockchain Environments

Specify the blockchain node RPC URL and contract information.&#x20;

Game contract address can be obtained when deploying the contracts [spin-contracts.md](../opzk-deployment/spin-contracts.md "mention")

Generate an wallet and fill in `OPERATOR_ADDRESS` and `OPERATOR_PRIVATE_KEY` this will be your operator's wallets credentials, it will be mainly responsible for **settling** submissions.

{% hint style="info" %}
The values in `.env.template` are working for locally deployed network through spin-contracts.&#x20;
{% endhint %}

| Name                             | Description                                 |
| -------------------------------- | ------------------------------------------- |
| CHAIN\_RPC\_URL                  | The URL of the node RPC                     |
| OPERATOR\_ADDRESS                | The wallet address of the operator          |
| OPERATOR\_PRIVATE\_KEY           | The private key of the operator             |
| GAME\_CONTRACT\_ADDRESS          | The `SpinOPZKGameContract` contract address |
| GAME\_STAKING\_CONTRACT\_ADDRESS | The `StakingContract` contract address      |

### Operator Config Environments

| Name                             | Description                                         | Default Value |
| -------------------------------- | --------------------------------------------------- | ------------- |
| ALLOW\_INVALID\_SUBMISSION       | Checking player submission, 0 means do not allow    | 0             |
| AUTO\_STAKING\_SUBMISSION\_COUNT | Number of submissions to stakes at once when needed | 100           |
| PORT                             | The port of the express server                      | 8001          |
| HOST                             | The host of the express server                      | 0.0.0.0       |



