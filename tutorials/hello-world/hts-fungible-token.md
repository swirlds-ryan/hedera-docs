---
description: >-
  Hello World sequence: Create a new fungible token using Hedera Token Service
  (HTS).
---

# HTS: Fungible Token

## What you will accomplish

* [ ] Create and mint a new fungible token on HTS
* [ ] Query the token balance

The repo, [`github.com/hedera-dev/hello-future-world`](https://github.com/hedera-dev/hello-future-world/), is intended to be used alongside this tutorial.

***

## Prerequisites

Before you begin, you should have completed the "Create and Fund Account" sequence:&#x20;

{% content-ref url="create-fund-account.md" %}
[create-fund-account.md](create-fund-account.md)
{% endcontent-ref %}

***

## Get started

### Set up project

To follow along, start with the `main` branch, which is the _default branch_ of the repo. This gives you the initial state from which you can follow along with the steps as described in the tutorial.

{% hint style="warning" %}
You should already have this from the "Create and Fund Account" sequence. If you have not completed this, you are strongly encouraged to do so.

Alternatively, you may wish to create a `.env` file and populate it as required.
{% endhint %}

In the terminal, from the `hello-future-world` directory,
enter the subdirectory for this sequence.

```shell
cd 04-hts-ft-sdk/
```

Reuse the `.env` file by copying the one that you have previously created into the directory for this sequence.

```shell
cp ../00-create-fund-account/.env ./
```

Next, install the dependencies using `npm`.

```shell
npm install
```

Then open the `script-hts-ft.js` file in a code editor, such as VS Code.

***

### Write the script

An almost-complete script has already been prepared for you, and you will only need to make a few modifications (outlined below) for it to run successfully.

#### Step 1: Configure HTS token to be created

To create a new HTS token, we will use `TokenCreateTransaction`. This transaction requires many properties to be set on it.

* For fungible tokens (which are analogous to ERC20 tokens), set the token type to `TokenType.FungibleCommon`.
* Set the token name and token symbol based on your name (or nickname).
* Set the decimal property to `2`.
* Set the initial supply to 1 million.

```js
    let tokenCreateTx = await new TokenCreateTransaction()
        .setTokenType(TokenType.FungibleCommon)
        .setTokenName("bguiz coin")
        .setTokenSymbol("BGZ")
        .setDecimals(2)
        .setInitialSupply(1_000_000)
        .setTreasuryAccountId(accountId)
        .setAdminKey(accountKey)
        .setFreezeDefault(false)
        .freezeWith(client);
```
<details>

<summary>HTS token create details</summary>

* Token Type: Fungible tokens, declared using `TokenType.FungibleCommon`, may be thought of as analogous to *ERC20* tokens. Note that HTS also supports another token type, `TokenType.NonFungibleUnique`, which may be thought of as analogous to *ERC721* tokens.
* Token Name: This is the full name of the token. For example, "Singapore Dollar".
* Token Symbol: This is the abbreviation of the token's name. For example, "SGD".
* Decimals: This is the number of decimal places the currency uses. For example, `2` mimics "cents", where the smallest unit of the token is 0.01 (1/100) of a single token.
* Initial Supply: This is the number units of the token to "mint" when first creating the token. Note that this is specified in the smallest units, so `1_000_000` initial supply when decimals is 2, results in `10_000` full units of the token being minted. It might be easier to think about it as "one million cents equals ten thousand dollars".
* Treasury Account ID: This is the account that the initial supply is credited to. For example, using `accountId` would mean that your own account receives all the tokens when they are minted.
* Admin Key: This is the account that is authorised to administrate this token. For example, using `accountKey` would mean that your own account would get to perform actions such as minting additional supply.

</details>

#### Step 2: Mirror Node API to query specified token balance

Now query the token balance of our account. Since the _treasury account_ was configured as being your own account, it will have the entire initial supply of the token.

You will want to use the Mirror Node API with the path `/api/v1/accounts/{idOrAliasOrEvmAddress}/tokens` for this task.

* Specify `accountId` within the URL path
* Specify `tokenId` as the `token.id` query parameter
* Specify `1` as the `limit` query parameter (you are only interested in one token)

The string, including substitution, should look like this:

```js
    const accountBalanceFetchApiUrl =
        `https://testnet.mirrornode.hedera.com/api/v1/accounts/${accountId}/tokens?token.id=${tokenId}&limit=1&order=desc`;
```

<details>

<summary>Learn more about Mirror Node APIs</summary>

You can explore the Mirror Node APIs interactively via its Swagger page:
[Hedera Testnet Mirror Node REST API](https://testnet.mirrornode.hedera.com/api/v1/docs/#/).

You can perform the same Mirror Node API query as `accountBalanceFetchApiUrl` above.
This is what the relevant part of the Swagger page would look like when doing so:

![](../../.gitbook/assets/hello-world--hts--mirror-node-swagger.drawing.svg "Mirror Node API Swagger for account tokens, with annotations.")

You can learn more about the Mirror Nodes via its documentation:
[REST API](https://docs.hedera.com/hedera/sdks-and-apis/rest-api).

</details>

***

### Run the script

Run the script using the following command:

```shell
node script-hts-ft.js
```

You should see output similar to the following:

```
accountId: 0.0.1201
tokenId: 0.0.5878530
tokenExplorerUrl: https://hashscan.io/testnet/token/0.0.5878530
accountTokenBalance: 1000000
accountBalanceFetchApiUrl: https://testnet.mirrornode.hedera.com/api/v1/accounts/0.0.1201/tokens?token.id=0.0.5878530&limit=1&order=desc
```

Open `tokenExplorerUrl` in your browser and check that:

* (1) The token should exist, and its "token ID" should match `tokenId`.
* (2) The "name" and "symbol" should be shown as the same values derived from your name (or nickname) that you chose earlier.
* (3) The "treasury account" should match `accountId`.
* (4) Both the "total supply" and "initial supply" should be `10,000`.

<img src="../../.gitbook/assets/hello-world--hts--token.drawing.svg" alt="HTS transaction in Hashscan, with annotated items to check." class="gitbook-drawing">

{% hint style="info" %}
Note that "total supply" and "initial supply" are not displayed as `1,000,000` because of the 2 decimal places configured. Instead these are displayed as `10,000.00`.
{% endhint %}

***

## Complete

Congratulations, you have completed the **Hedera Token Service** Hello World sequence! 🎉🎉🎉

You have learnt how to:

* [x] Create and mint a new fungible token on HTS
* [x] Query the token balance

***

### Next Steps

Now that you have completed this Hello World sequence, you have interacted with Hedera Token Service (HTS). There are [other Hello World sequences](../) for Hedera Smart Contract Service (HSCS), and Hedera File Service (HFS), which you may wish to check out next.

***

## Cheat sheet

<details>

<summary>Skip to final state</summary>

To skip ahead to the final state, use the `completed` branch. This gives you the final state with which you can compare your implementation to the completed steps of the tutorial.

```shell
git fetch origin completed:completed
git checkout completed
```

To see the full set of differences between the initial and final states of the repo, you can use `diff`.

```shell
cd 04-hts-ft-sdkdir/
git diff main..completed -- ./
```

Alternatively, you may view the `diff` rendered on Github:
[`hedera-dev/hello-future-world/compare/main..completed`](https://github.com/hedera-dev/hello-future-world/compare/main..completed) (This will show the `diff` for *all* sequences.)

Note that the branch names are delimited by `..`, and not by `...`, as the latter finds the `diff` with the latest common ancestor commit, which _is not_ what we want in this case.

</details>

***

<table data-card-size="large" data-view="cards"><thead><tr><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody>
<tr><td align="center"><p>Writer: Brendan, DevRel Engineer</p><p><a href="https://github.com/bguiz">GitHub</a> | <a href="https://blog.bguiz.com">Blog</a></p></td><td><a href="https://blog.bguiz.com">https://blog.bguiz.com</a></td></tr>
<tr><td align="center"><p>Editor: Abi Castro, DevRel Engineer</p><p><a href="https://github.com/a-ridley">GitHub</a> | <a href="https://twitter.com/ridley___">Twitter</a></p></td><td><a href="https://twitter.com/ridley___">https://twitter.com/ridley___</a></td></tr>
<tr><td align="center"><p>Editor: Michiel, Developer Advocate</p><p><a href="https://github.com/michielmulders">GitHub</a> | <a href="https://www.linkedin.com/in/michielmulders/">LinkedIn</a></p></td><td><a href="https://www.linkedin.com/in/michielmulders/">https://www.linkedin.com/in/michielmulders/</a></td></tr>
  <tr><td align="center"><p>Editor: Ryan Arndt, DevRel Education</p><p><a href="https://github.com/swirlds-ryan">GitHub</a> | <a href="https://www.linkedin.com/in/ryaneh/">LinkedIn</a></p></td><td><a href="https://www.linkedin.com/in/ryaneh/">https://www.linkedin.com/in/ryaneh/</a></td></tr>
</tbody></table>

***
