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

We have language bindings in Python (more languages coming soon) and additional examples in Shell. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Environments

> To choose your environment, use this code:

```python
import qbsdk

# for production
api = qbsdk.Api('my_very_secret_api_key', qbsdk.Mode.live)

# for sandbox
api = qbsdk.Api('my_very_secret_api_key', qbsdk.Mode.sandbox)
```


Once you install your SDK, you are able to use 2 types of environments:

* sandbox - Base url: `https://api-sandbox.qiibee.com`
* live - Base url: `https://api.qiibee.com`


The `sandbox` environment is to be used for testing your code under conditions that replicate the `production` environment but using tokens that are not backed by real value, to avoid any risk of loss of funds and to detect software bugs early.

The `live` environment is to be used to run your production application. 

The 2 environments are completely disjoint and use separate API keys, available on request. Tokens and transactions on the `sandbox` cannot be migrated automatically to the `live` environment.

To switch to the `live` environment, you would have to make sure a token has been created for production use, and that you provide the correct mode (`live` or `sandbox`) as a parameter to the SDK or switch the base API url as shown above, if you are using the API directly.

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

For those particular endpoints it is expected that the the request contains the following header:

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
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific transaction by its hash.

<aside class="warning"> Even though a transaction has been submitted to the chain, until it is accepted as part of the block (processed) it will appear as missing if queried by its hash and the endpoint will return a 404 code. </aside>

### HTTP Request

`GET https://api.qiibee.com/transactions/<HASH>`

### URL Parameters

Parameter | Description
--------- | -----------
HASH | The ID of the kitten to retrieve

## Get a raw unsigned transaction

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

