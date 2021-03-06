---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the qiibee API!

We have language bindings in Python (more languages coming soon) and additional examples using the cURL command (Shell tab). You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Environments

> To choose your environment, use this code:

```python
import qbsdk

# for going live
api = qbsdk.Api('my_very_secret_api_key', qbsdk.Mode.live)

# for sandbox
api = qbsdk.Api('my_very_secret_api_key', qbsdk.Mode.sandbox)
```

```shell
# query live
curl https://api.qiibee.com/tokens

# query sandbox
curl https://api-sandbox.qiibee.com/tokens
```

Once you install your SDK, you are able to use 2 types of environments:

* sandbox - Base url: `https://api-sandbox.qiibee.com`
* live - Base url: `https://api.qiibee.com`


The `sandbox` environment is to be used for testing your code under conditions that replicate the `live` environment but using test tokens to detect software bugs early.

The `live` environment is to be used to run your production application. 

The 2 environments are completely separate and use separate API keys, available on request. Loyalty tokens and transactions on the `sandbox` cannot be migrated automatically to the `live` environment.

To switch to the `live` environment, you would have to make sure a loyalty token has been created for production use, and that you provide the correct mode (`live` or `sandbox`) as a parameter to the SDK or switch the base API url as shown above, if you are using the API directly.

# Authentication

> To authorize, use this code:

```python
import qbsdk

api = qbsdk.Api('my_very_secret_api_key')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Bearer my_very_secret_api_key"
```

> Make sure to replace `my_very_secret_api_key` with your API key.

qiibee uses API keys to allow access particular endpoints that are only accessible to brands. 

For those particular endpoints it is expected that the request contains the following header:

`Authorization: Bearer my_very_secret_api_key `

<aside class="notice">
You must replace <code>my_very_secret_api_key</code> with your personal API key.
</aside>

# Transactions

## Get all transactions

```python
import qbsdk

api = qbsdk.Api('my_very_secret_api_key')
api.get_transactions()
```

```shell
curl "https://api.qiibee.com/transactions"
```

> The above command returns JSON structured like this:

```json
[
  {
    "to": "0x36003f0f6bde4cf2ddeba253f00d0a71b4ee8de6",
    "from": "0x853fc3ab19b0fd862f6a1c75055feb0f0e9922ec",
    "blockHash": "0x0607b2a500db0e90d27f8f44424118218fd10b942a8afa0e46e209f93679eb62",
    "blockNumber": 869569,
    "hash": "0x1da1258d04762fba4a566a2a622b58242bbbf014ab4c238003d997bb4ef5e07a",
    "input": "0xa9059cbb00000000000000000000000036003f0f6bde4cf2ddeba253f00d0a71b4ee8de6000000000000000000000000000000000000000000000000000000000000000a",
    "nonce": 128039,
    "state": "processed",
    "status": true,
    "timestamp": 1578981311,
    "transactionIndex": 0,
    "value": "10",
    "txType": "redeem",
    "contractFunction": "transfer",
    "chainId": 17224,
    "token": {
      "contractAddress": "0xf2e71f41e670c2823684ac3dbdf48166084e5af3",
      "decimals": 18,
      "description": "This is a test coin",
      "name": "Test Coin",
      "rate": 10,
      "symbol": "TEST",
      "totalSupply": "37500000000000000000000000",
      "website": "qiibee.com"
    },
    "contractAddress": "0xF2e71f41E670C2823684AC3DBDF48166084e5Af3"
  }
]  
```

This endpoint retrieves all transactions ordered descendingly by their timestamp. It allows paging and filters using the query parameters described below.


<aside class="warning"> This endpoint is not yet supported for no-wallet loyalty contracts. </aside>

### HTTP Request

`GET https://api.qiibee.com/transactions`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
symbol | None | Token symbol to filter by. (Use either the symbol or the contractAddress, not both)
contractAddress | None | Token contract address to filter by. (Use either the symbol or the contractAddress, not both)
limit | 100 | Define what is the maximum number of transactions the response can contain (maximum is 100).
wallet | None | Wallet address to filter by (wallet == to or wallet == from)


## Get a specific transaction

```python
import qbsdk

api = qbsdk.Api('my_secret_api')
api.get_transaction('0x1da1258d04762fba4a566a2a622b58242bbbf014ab4c238003d997bb4ef5e07a')
```

```shell
curl "https://api.qiibee.com/transactions/0x1da1258d04762fba4a566a2a622b58242bbbf014ab4c238003d997bb4ef5e07a"
```	

> The above command returns JSON structured like this:

```json
{
  "blockHash": "0x0607b2a500db0e90d27f8f44424118218fd10b942a8afa0e46e209f93679eb62",
  "blockNumber": 869569,
  "chainId": 17224,
  "condition": null,
  "creates": null,
  "from": "0x853FC3aB19b0FD862f6a1c75055FeB0f0e9922ec",
  "hash": "0x1da1258d04762fba4a566a2a622b58242bbbf014ab4c238003d997bb4ef5e07a",
  "input": "0xa9059cbb00000000000000000000000036003f0f6bde4cf2ddeba253f00d0a71b4ee8de6000000000000000000000000000000000000000000000000000000000000000a",
  "nonce": 128039,
  "raw": "0xf8aa8301f42780830f424094f2e71f41e670c2823684ac3dbdf48166084e5af380b844a9059cbb00000000000000000000000036003f0f6bde4cf2ddeba253f00d0a71b4ee8de6000000000000000000000000000000000000000000000000000000000000000a8286b4a02e0797295ec111f7de457baa460a14293fbf61d7e4c97b7060619078c0f62456a00b92a93d99265c17ec41a8d43acc20d3ce17efbc652926bafda24d63ec03d977",
  "to": "0x36003f0f6bde4cf2ddeba253f00d0a71b4ee8de6",
  "transactionIndex": 0,
  "value": "10",
  "status": true,
  "contract": "0xF2e71f41E670C2823684AC3DBDF48166084e5Af3",
  "timestamp": 1578981311,
  "confirms": 7,
  "token": {
    "contractAddress": "0xf2e71f41e670c2823684ac3dbdf48166084e5af3",
    "decimals": 18,
    "description": "This is a test coin",
    "name": "Test Coin",
    "rate": 10,
    "symbol": "TEST",
    "totalSupply": "37500000000000000000000000",
    "website": "qiibee.com"
  },
  "state": "processed"
}
```

This endpoint retrieves a specific transaction by its hash.

<aside class="warning"> Even though a transaction has been submitted to the chain, until it is accepted as part of the block (processed) it will appear as missing if queried by its hash and the endpoint will return a 404 code. </aside>

### HTTP Request

`GET https://api.qiibee.com/transactions/<HASH>`

### URL Parameters

Parameter | Description
--------- | -----------
HASH | Blockchain hash (identifier) of the transaction.


## Construct and send signed brand transaction

```python
import qbsdk

api = qbsdk.Api('my_very_secret_api_key')

brand_address_private_key = 'my_very_secret_brand_private_key'
token_symbol = 'SVT'

wallet = qbsdk.Wallet(brand_address_private_key, token_symbol, api, qbsdk.TransferStrategy.brand)
wallet.setup()
transfer_receiver = 'address_of_receiver'

send_transaction(transfer_receiver, i)
```

```shell
# Constructing and signing transactions in shell is not supported.
# Some implementation of the web3 library or alternative is required to achieve that
```

> The above command returns a SDK-specific data type with the following relevant fields:

```json
{
	"hash": "0x99d3a9b58720e4d6e6024799719853ceca2d69947af67b675321a736d8803e79",
	"state": "pending" 
}
```

This operation allows you to construct a transaction with the correct parameters, sign it with your brand private key and send it. Thus it is a multi-step operation that is fully supported only at the level of the SDK.

To achieve this using the raw endpoints you would need to make use of a library equivalent to [`web3`](https://github.com/ethereum/web3.js/) to construct the transaction and sign it.

The returned value containts the transaction `hash` that allows you to track transaction completion.

## Get a raw unsigned transaction

```python
import qbsdk

api = qbsdk.Api('my_secret_api')

from_address = '0x87265a62c60247f862b9149423061b36b460f4bb'
to_address = '0xb99c958777f024bc4ce992b2a0efb2f1f50a4dcf'
value = 1000000000000000000
contract_address = '0x883ed48b3210082cf82fb92ce81f0b17bdec4f81'
api.get_raw_transaction(from, to, value, contract_address)
```

```shell
curl "https://api.qiibee.com/transactions/raw?from=0x87265a62c60247f862b9149423061b36b460f4bb&to=0xb99c958777f024bc4ce992b2a0efb2f1f50a4dcf&symbol=SVT&transferAmount=10"
```	

> The above command returns JSON structured like this:

```json
{
  "from": "0x87265a62c60247f862b9149423061b36b460f4bb",
  "nonce": "0x14",
  "gasPrice": "0x0",
  "gasLimit": "0xf4240",
  "to": "0x883ed48b3210082cf82fb92ce81f0b17bdec4f81",
  "value": "0x0",
  "data": "0xa9059cbb000000000000000000000000b99c958777f024bc4ce992b2a0efb2f1f50a4dcf000000000000000000000000000000000000000000000000000000000000000a",
  "chainId": 17224
}
```

This endpoint returns a raw transaction (unsigned) which uses the nonce based on the blockchain `transactionCount` for that address.


<aside class="warning"> If an SDK is available for your programming language, we highly recommend you to use the `Wallet` class to construct valid transactions and send them instead of this function, since it takes care of transaction construction and signing for you. </aside>


### HTTP Request

`GET https://api.qiibee.com/transactions/raw`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
from | None | Address of the sender
to | None | Receiver address of the loyalty tokens
contractAddress | None | Loyalty Token contract address. (Use either the symbol or the contractAddress, not both)
symbol | None | Loyalty Token symbol. (Use either the symbol or the contractAddress, not both)
transferAmount | None | Amount of loyalty tokens being sent (in wei)
txType | transfer | Can be either `transfer`, `earn`, `redeem`, `debit`. The latter 3 create a transaction for the no-wallet contract, and `transfer` creates a transaction for the wallet contract.

## Post signed transaction


```python
import qbsdk

api = qbsdk.Api('my_secret_api')

signed_tx_string = '0xf8aa8305ee6e80830f424094f80c1e40d50f383330a886f03f147f49342ad7c780b844a9059cbb00000000000000000000000087265a62c60247f862b9149423061b36b460f4bb000000000000000000000000000000000000000000000000000000000000000a8286b5a0e8a236e4b57e79ee0eec88d8bc7a7ef1c44a1d181bad366621e72c341490e550a0683ce6443acc4384ea2b18fcaba416b8f7379963b6a0c596cea423cade502eb3'

api.post_transaction(signed_tx_string)
```

```shell
curl -d '{"data": "0xf8aa8305ee6e80830f424094f80c1e40d50f383330a886f03f147f49342ad7c780b844a9059cbb00000000000000000000000087265a62c60247f862b9149423061b36b460f4bb000000000000000000000000000000000000000000000000000000000000000a8286b5a0e8a236e4b57e79ee0eec88d8bc7a7ef1c44a1d181bad366621e72c341490e550a0683ce6443acc4384ea2b18fcaba416b8f7379963b6a0c596cea423cade502eb3" }' -H "Content-Type: application/json" -X POST  https://api.qiibee.com/transactions
```
> The above command returns JSON structured like this:

```json
{ 
	"to":"0x87265a62c60247f862b9149423061b36b460f4bb",
	"from":"0x36003f0f6bde4cf2ddeba253f00d0a71b4ee8de6",
	"contract":"0xf80c1e40d50f383330a886f03f147f49342ad7c7",
	"hash":"0x1a5eefc67c0e98709bd0954827fc68312f5596a7e9c3140dfe94f8c53925769b",
	"nonce":388718,
	"state":"pending",
	"status":"pending",
	"value":"10",
	"chainId":17225
}
```

Sends a signed transaction to the qiibee chain. 

<aside class="warning"> If an SDK is available for your programming language, we highly recommend you use the `Wallet` class to send transactions, since it takes care of transaction construction and signing for you. </aside>

### HTTP Request

`POST https://api.qiibee.com/transactions`

### JSON body Parameters

Parameter | Default | Description
--------- | ------- | -----------
data | None | Signed transaction data in HEX format.

# Tokens

## Get all tokens

```python
import qbsdk

api = qbsdk.Api('my_very_secret_api_key')
api.get_tokens()
```

```shell
curl "https://api.qiibee.com/transactions"
```

> The above command returns JSON structured like this:


```json

{
  "private": [
      {
      "contractAddress": "0x883ed48b3210082cf82fb92ce81f0b17bdec4f81",
      "decimals": 18,
      "description": "Participate in surveys and get rewarded with Survey Token which you can exchange for QBX. Click on \"Learn more\" to participate.",
      "name": "Survey Token",
      "rate": 1,
      "symbol": "SVT",
      "totalSupply": "1000000000000000000000000",
      "website": "https://qiibee.com",
      "logoUrl": "https://tokens.qiibee.com/svt/logo.png"
    }
  ],
  "public": [
      {
      "contractAddress": "0x2467aa6b5a2351416fd4c3def8462d841feeecec",
      "symbol": "QBX",
      "name": "qiibeeToken",
      "totalSupply": "1242352941000000000000000000",
      "decimals": 18,
      "balance": "0",
      "description": "With qiibee, businesses around the world can run their loyalty programs on the blockchain.Exchange loyalty tokens to QBX and enter the crypto world",
      "website": "https://www.qiibee.com",
      "logoUrl": "https://s3.eu-central-1.amazonaws.com/tokens.qiibee/qbx/logo.png"
    }
  ]
}
```

This endpoint retrieves the tokens relevant to the qiibee platform. The `private` field contains a list of tokens deployed on the private blockchain.
	
The `public` field contains a list of relevant tokens on the Ethereum main-net. It can be accessed by passing in `public = true` as a parameter.

### HTTP Request

`GET https://api.qiibee.com/tokens`

## Get specific token


```shell
curl "https://api.qiibee.com/tokens/0x883ed48b3210082cf82fb92ce81f0b17bdec4f8"
```

```python
import qbsdk

api = qbsdk.Api('my_very_secret_api_key')
api.get_token('0x883ed48b3210082cf82fb92ce81f0b17bdec4f8')
```

> The above command returns JSON structured like this:

```json
{
  "private": {
    "contractAddress": "0x883ed48b3210082cf82fb92ce81f0b17bdec4f81",
    "decimals": 18,
    "description": "Participate in surveys and get rewarded with Survey Token which you can exchange for QBX. Click on \"Learn more\" to participate.",
    "name": "Survey Token",
    "rate": 1,
    "symbol": "SVT",
    "totalSupply": "1000000000000000000000000",
    "website": "https://qiibee.com",
    "balance": "0",
    "logoUrl": "https://tokens.qiibee.com/svt/logo.png",
    "type": "wallet"
  }
}
```

Returns a specific Loyalty Token on the qiibee chain.



### HTTP Request

`GET https://api.qiibee.com/tokens/<CONTRACT_ADDRESS>`

### URL Parameters

Parameter | Description
--------- | -----------
CONTRACT_ADDRESS | The CONTRACT_ADDRESS of the token to fetch.

# Addresses


## Get a specific address

```shell
curl "https://api.qiibee.com/addresses/0x2A381d5701255B20656e5A2332F8D9038CfF5Dd2"
```

```python
import qbsdk

api = qbsdk.Api('my_very_secret_api_key')
api.get_address('0x2A381d5701255B20656e5A2332F8D9038CfF5Dd2')
```

> The above command returns JSON structured like this:

```json
{
  "transactionCount": 5,
  "balances": {
    "private": {
      "SVT": {
        "balance": "892300000000000000000000",
        "contractAddress": "0x883ed48b3210082cf82fb92ce81f0b17bdec4f81"
      },
      "TEST": {
        "balance": "890000000000000000000000",
        "contractAddress": "0xf2e71f41e670c2823684ac3dbdf48166084e5af3"
      },
    }
  }
}
```

Returns address information along with balances for each Loyalty Token and optionally each relevant main-net token.



### HTTP Request

`GET https://api.qiibee.com/address/<ADDRESS>`

### URL Parameters

Parameter | Description
--------- | -----------
ADDRESS | The address to query for.


# Network

## Get network data (latest block)

```shell
curl "https://api.qiibee.com/net"
```

```python
import qbsdk

api = qbsdk.Api('my_very_secret_api_key')
api.get_latest_block()
```

> The above command returns JSON structured like this:

```json
{
  "author": "0xdb99f8da29143c1650ba2dcc7c1dbfebfdceefa6",
  "extraData": "0xde830205078f5061726974792d457468657265756d86312e33362e30826c69",
  "hash": "0xb84102cb9ff68ff2e25a62790ad29f99bec63f8c1e0abbeb133c8afe88d1bf87",
  "miner": "0xdb99f8Da29143c1650ba2DCc7c1DBfebFDcEeFA6",
  "number": 872807,
  "parentHash": "0xf9b0128c3922bd165e69941aea5cb32a2a8de20d87971b1a473d01d34812f930",
  "receiptsRoot": "0xf2bebb57f9cb892f1227d1f92d9143c479e7fc86d8b38ab8fb072cf68e58f572",
  "signature": "b80a1a36d4792806befeb86da19a82bbec5a288878f2e387a588d9fd59a9c02d540b680111ef20daa6b11a3146def94eb221705c9cd7cd98b3625fd7aa5e3d2801",
  "size": 764,
  "stateRoot": "0x9f56fe23520d14e0c44aae7f2151f2abd391a34e3631f1c6ba2bc14ce3d1fe6b",
  "step": "1579086350",
  "timestamp": 1579086350,
  "transactions": [
    "0xf0d660dd0da64f4ae491b2cb22ee17b9dadec0f920fa1e25b702e4306eb3c191"
  ],
  "transactionsRoot": "0x7e6b49fd29b5871cd13228ac76604db37f5d007ffbca963417be0dac2936ee85",
  "chainId": 17224
}	
```

Returns data fields of the latest block in the chain alongside the chain id.
	
### HTTP Request

`GET https://api.qiibee.com/net`

# Prices

## Get current prices

```shell
curl "https://api.qiibee.com/prices/?from=0x883ed48b3210082cf82fb92ce81f0b17bdec4f81&to=CHF,EUR"
```

```python
import qbsdk

api = qbsdk.Api('my_very_secret_api_key')
api.get_prices('0x883ed48b3210082cf82fb92ce81f0b17bdec4f81', ['USD', 'CHF'])
```

> The above command returns JSON structured like this:

```json
{
  "CHF": "0.0210492399",
  "EUR": "0.0209613126"
}
```

In case the loyalty program decided to peg the loyalty token to QBX, this returns the FIAT price of one unit of a given Loyalty Token. This endpoint uses a third-party provider to get the ETH echange rate. The QBX/ETH exchange rate is fetched from the Coinsuper exchange.

This can be used in case the loyalty program decided to peg the loyalty token to QBX.

### HTTP Request

`GET https://api.qiibee.com/prices`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
from | None | Loyalty Token contract address.
to | USD | Comma separated currency symbols list to convert into. For example: USD,EUR,CHF. 


## Get price history

```shell
curl "https://api.qiibee.com/prices/history?from=0x883ed48b3210082cf82fb92ce81f0b17bdec4f81&to=AUD&limit=3"
```

```python
import qbsdk

api = qbsdk.Api('my_very_secret_api_key')
api.get_prices_history('0x883ed48b3210082cf82fb92ce81f0b17bdec4f81', 'AUD', 3)
```

> The above command returns JSON structured like this:

```json
[
  {
    "time": 1578873600,
    "price": 0.012641263091352615
  },
  {
    "time": 1578960000,
    "price": 0.011690807295838489
  },
  {
    "time": 1579046400,
    "price": 0.011875396707207064
  }
]
```

In case the loyalty program decided to peg the loyalty token to QBX, this returns the historical FIAT price values of one unit of a given Loyalty Token for a desired currency. This endpoint uses a third-party provider to get the ETH exchange rate. The QBX/ETH exchange rate is fetched from the Coinsuper exchange.

This can be used in case the loyalty program decided to peg the loyalty token to QBX.

### HTTP Request

`GET https://api.qiibee.com/prices/history`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
from | None | Loyalty Token contract address.
to | USD | Comma separated currency symbols list to convert into. For example: USD,EUR,CHF. 
limit | 30 | The number of data points to return.
