# Self-custody quick start

Within the Internet Computer ecosystem, Internet Computer Protocol tokens (ICP tokens) are a native utility token. ICP tokens play a key role in both the governance and economics of the Internet Computer.

This *Self-Custody Quick Start* scenario assumes:

-   You are a new ICP token holder.

-   You want to understand what you can do with your ICP tokens.

-   You want to know how to convert, transfer, or lock your ICP tokens using the SDK command-line interface `dfx`.

If you aren’t yet a token holder, you’ll need to purchase ICP tokens from an exchange or receive a token grant before you can take custody. For an overview of how to get ICP tokens and custody options, see [How you can get ICP tokens](/concepts/tokens-cycles.md#get-cycles) and [Choosing self-custody for digital assets](custody-options-intro).

If you are using another application—such as the [Network Nervous System (NNS) application](https://nns.ic0.app) or the user interface provided by a hardware wallet—to interact with your ICP tokens, you should refer to the documentation for that application.

This *Self-Custody Quick Start* focuses solely on interacting with ICP tokens using the SDK command-line interface `dfx`.

## How you can use ICP tokens

The following diagram provides a simplified overview of the three most common ways you can use tokens.

![developers guide:icp tokens how to use](../_attachments/icp-tokens-how-to-use.svg)

As this diagram suggests, how you use ICP tokens depends primarily on your goals in acquiring them.

If you are a **developer** or **entrepreneur**, ICP tokens can be converted to **cycles**. Cycles can then be used to build and deploy applications to deliver products and services to the market.

If you are a member of the community interested in **participating in governance** and influencing the direction of the Internet Computer, you can lock up ICP tokens in a stake—called a neuron—so that you can submit and vote on proposals.

## Before you begin

To get started, verify the following:

-   You have an internet connection and access to a shell terminal on your local **Intel-based macOS** or **Linux** computer.

-   You know how to open a new terminal shell on your local computer and how to run basic command-line programs in the terminal.

-   You hold ICP tokens in a self-custody wallet.

-   You have downloaded and installed the SDK by running the following command in a terminal on your local computer:

    ``` bash
    sh -ci "$(curl -fsSL https://internetcomputer.org/install.sh)"
    ```

-   You have created a backup copy of the public/private key for the identity you are using for self-custody.

    For example, if you are using the default developer identity created using the SDK `dfx` command-line interface, you should have a backup of the `~/.config/dfx/identity/default/identity.pem` file stored in a secure location.

-   You have a secure environment in which to perform operations involving ICP tokens.

    As a security best practice, any operations that involve the transfer of ICP tokens would require both an air-gapped computer with minimal hardware and software and a computer connected to the network. In practice, this requires moving files between two computers and taking other precautions to minimize risks.

    For simplicity, this page describes how to complete key tasks using a single computer connected to the network.

## Connect to the ledger and get your account identifier

All ICP token transactions are recorded in a ledger canister running on the Internet Computer. These instructions assume that you are using the `default` developer identity that `dfx` has created for you.

This identity is represented by a **principal** data type and a textual representation of the principal often referred to as your **principal identifier**. This representation of your identity is similar to a Bitcoin or Ethereum address.

However, the principal associated with your developer identity is typically not the same as your **account identifier** in the ledger. The principal identifier and the account identifier are related—both provide a textual representation of your identity—but they use different formats.

To connect to the ledger and get account information:

1.  Create an empty `dfx.json` file in your current directory by running the following command:

    ``` bash
    echo '{}' > dfx.json
    ```

2.  Check the current status of the Internet Computer network and your ability to connect to it by running the following command

    ``` bash
    dfx ping ic
    ```

    You should see output similar to the following:

        {
          "ic_api_version": "0.18.0"  "impl_hash": "d639545e0f38e075ad240fd4ec45d4eeeb11e1f67a52cdd449cd664d825e7fec"  "impl_version": "8dc1a28b4fb9605558c03121811c9af9701a6142"  "replica_health_status": "healthy"  "root_key": [48, 129, 130, 48, 29, 6, 13, 43, 6, 1, 4, 1, 130, 220, 124, 5, 3, 1, 2, 1, 6, 12, 43, 6, 1, 4, 1, 130, 220, 124, 5, 3, 2, 1, 3, 97, 0, 129, 76, 14, 110, 199, 31, 171, 88, 59, 8, 189, 129, 55, 60, 37, 92, 60, 55, 27, 46, 132, 134, 60, 152, 164, 241, 224, 139, 116, 35, 93, 20, 251, 93, 156, 12, 213, 70, 217, 104, 95, 145, 58, 12, 11, 44, 197, 52, 21, 131, 191, 75, 67, 146, 228, 103, 219, 150, 214, 91, 155, 180, 203, 113, 113, 18, 248, 71, 46, 13, 90, 77, 20, 80, 95, 253, 116, 132, 176, 18, 145, 9, 28, 95, 135, 185, 136, 131, 70, 63, 152, 9, 26, 11, 170, 174]
        }

3.  (Optional) Confirm the developer identity you are currently using by running the following command:

    ``` bash
    dfx identity whoami
    ```

    In most cases, you should see that you are currently using your `default` developer identity. For example:

        default

4.  (Optional) View the textual representation of the principal for your current identity by running the following command:

    ``` bash
    dfx identity get-principal
    ```

    This command displays output similar to the following:

        tsqwz-udeik-5migd-ehrev-pvoqv-szx2g-akh5s-fkyqc-zy6q7-snav6-uqe

5.  Get the account identifier for your developer identity by running the following command:

    ``` bash
    dfx ledger account-id
    ```

    This command displays the ledger account identifier associated with your developer identity. For example, you should see output similar to the following:

        03e3d86f29a069c6f2c5c48e01bc084e4ea18ad02b0eec8fccadf4487183c223

6.  Check your account balance by running the following command:

    ``` bash
    dfx ledger --network ic balance
    ```

    This command displays the ICP token balance from the ledger account. For example, you should see output similar to the following:

        10.00000000 ICP

## Convert ICP tokens to cycles

If you want to use your ICP tokens in your ledger account to power application development, you first must convert them to cycles and transfer the cycles to a canister that will be your cycles wallet.

To convert ICP tokens to cycles:

1.  Create a new canister with cycles by transferring ICP tokens from your ledger account by running a command similar to the following:

    ``` bash
    dfx ledger --network ic create-canister <controller-principal-identifier> --amount <icp-tokens>
    ```

    This command converts the number of ICP tokens you specify for the `--amount` argument into cycles, and associates the cycles with a new canister identifier controlled by the principal you specify.

    For example, the following command converts 1.25 ICP tokens into cycles and specifies the principal identifier for the default identity as the controller of the new canister:

        dfx ledger --network ic create-canister tsqwz-udeik-5migd-ehrev-pvoqv-szx2g-akh5s-fkyqc-zy6q7-snav6-uqe --amount 1.25

    If the transaction is successful, the ledger records the event and you should see output similar to the following:

        Transfer sent at BlockHeight: 20
        Canister created with id: "53zcu-tiaaa-aaaaa-qaaba-cai"

2.  Install the cycles wallet code in the newly-created canister placeholder by running a command similar to the following:

    ``` bash
    dfx identity --network ic deploy-wallet <canister-identifer>
    ```

    For example:

        dfx identity --network ic deploy-wallet 53zcu-tiaaa-aaaaa-qaaba-cai

    This command displays output similar to the following:

        Creating a wallet canister on the IC network.
        The wallet canister on the "ic" network for user "default" is "53zcu-tiaaa-aaaaa-qaaba-cai"

## Transfer ICP tokens to another account

If you want to transfer ICP tokens to another account in the ledger, you need to know the account identifier for the destination account.

To transfer ICP tokens to another account:

1.  Check that you are using an identity with control over the ledger account by running the following command:

    ``` bash
    dfx identity whoami
    ```

2.  Check the current balance in the ledger account associated with your identity by running the following command:

    ``` bash
    dfx ledger --network ic balance
    ```

3.  Transfer ICP tokens to another account by running a command similar to the following:

    ``` bash
    dfx ledger --network ic transfer <destination-ledger-account-id> --icp <ICP-amount> --memo <numeric-memo>
    ```

    For example:

        dfx ledger --network ic transfer ae6e1a76da5725bbbf0c5c035aaf0525b791e0f0f7cce27d8e27826389871406 --icp 5 --memo 12345

    This example illustrates how to transfer ICP tokens to the specified account using a whole number with the `--icp` command-line option.

    -   You can also specify fractional units of ICP tokens—called **e8s**—using the `--e8s` option, either on its own or in conjunction with the `--icp` option.

    -   Alternatively, you can use the `--amount` to specify the number of ICP tokens to transfer with fractional units up to 8 decimal places, for example, as `5.00000025`.

    The destination address can be an address in the ledger canister running on the Internet Computer network, an account you have added using the [Network Nervous System application](https://nns.ic0.app), or the address for a wallet you have on an exchange.

    If you transfer the ICP tokens to an account in the [Network Nervous System application](https://nns.ic0.app), you might need to refresh the browser to see the transaction reflected.

    For more information about using the `dfx ledger` command-line options, see [dfx ledger](/references/cli-reference/dfx-ledger.md).

## Lock ICP tokens by staking them in a neuron

If you want to lock up ICP tokens to participate in governance and earn rewards, you must use the [Network Nervous System (NNS) application](https://nns.ic0.app) or `dfx canister call` commands.

Because locking up ICP tokens to create staked neurons is a more complex process when using the SDK command-line interface than it is when using the [Network Nervous System (NNS) application](https://nns.ic0.app), the steps aren’t included in this guide.

For information about the Network Nervous System, see [Understanding the Internet Computer’s Network Nervous System, Neurons, and ICP Utility Tokens](https://medium.com/dfinity/understanding-the-internet-computers-network-nervous-system-neurons-and-icp-utility-tokens-730dab65cae8).

For additional details about setting the locked period and dissolve delay for a neuron, see [Getting Started \| The Internet Computer Network Nervous System Application & Wallet](https://medium.com/dfinity/getting-started-on-the-internet-computers-network-nervous-system-app-wallet-61ecf111ea11)
