# Read ORBIS data from TON blockchain

1. Use the TON API to get the distribution contract state

  ```curl
  curl -X 'GET' \
  'https://tonapi.io/v2/blockchain/accounts/EQAW7leUiBZs6OoLoWO1YoviLyAOkRQAe1ZGMBjv8QtXZU_3/methods/get_giver_data' \
  -H 'accept: application/json'
  ```

  Response:

  ```json
{
  "success": true,
  "exit_code": 0,
  "stack": [
    {
      "type": "cell",
      "cell": "b5ee9c720101010100240000438013b4c9cf8ed83b279b42f270456758152f8fe8bfa86e8479af0a0f74bc22b58130"
    },
    {
      "type": "cell",
      "cell": "b5ee9c72010101010024000043801d90ad38ae7b0446aded89cf86d0a34aaaeea9c2a3ee55655c736ce3bcffd27fd0"
    },
    {
      "type": "cell",
      "cell": "b5ee9c720101010100240000438007beade88f97ead3bd25588fc805142725c73e03d6d5ceee44605bc62f2c012230"
    },
    {
      "type": "num",
      "num": "0x9184e72a000"
    },
    {
      "type": "num",
      "num": "0x681ec952"
    },
    {
      "type": "num",
      "num": "0x170"
    },
    {
      "type": "cell",
      "cell": "b5ee9c7201010d010044000202c70102020120030302014804050201200404020120060602012006070201200808020148090902012009090201200a0a0201200b0b0201200c0c000b14d8030c1c46"
    },
    {
      "type": "num",
      "num": "0x609bcb7e3417ae"
    },
    {
      "type": "num",
      "num": "0x60451217674e5c"
    }
  ]
}
```

2. In the received JSON object, locate the `stack` field. It is an array of objects.  
You’ll need to extract value at indices **4**, **5**, and **8** - that would be 

- `stack[4]` — the **timestamp of the last ORBCOIN distribution** (in Unix time)
- `stack[5]` — the number of **OM NFT**s in circulation 
- `stack[8]` — the total amount of **locked ORB tokens** (`amount_to_distribute`)

> **Note:** All these values are represented as **HEX strings** and need to be decoded to integers.


## Calculating the amount of ORB in circulation
```
circulating_supply = initial_supply - amount_to_distribute
```
where:
- `initial_supply` = 42'000'000 ORB
- `amount_to_distribute` - the amount of ORB locked for distribution.

## Fetch Balance of the Distribution Contract

```curl
curl -X 'GET' \
  'https://tonapi.io/v2/accounts/EQAW7leUiBZs6OoLoWO1YoviLyAOkRQAe1ZGMBjv8QtXZU_3/jettons/EQByqBGqyDy1MsJi-we7a18xqRFxaXlDWkrW0ukrz-6XKPwd' \
  -H 'accept: application/json'
```

Response:

```json
{
  "balance": "27182896062232494",
  "wallet_address": {
    "address": "0:ec8569c573d822356f6c4e7c36851a5557754e151f72ab2ae39b671de7fe93fe",
    "is_scam": false,
    "is_wallet": false
  },
  "jetton": {
    "address": "0:a3268c7869c9aeed4183d7d45239dca6f1a5f1757c97cca792ab9551ea036a23",
    "name": "ORBcoin",
    "symbol": "ORB",
    "decimals": 9,
    "image": "https://cache.tonapi.io/imgproxy/fNu5BSt97Esh2IBykYqUL8sIBmUJpc7Yrk_i8Df3Rcs/rs:fill:200:200:1/g:no/aHR0cHM6Ly9vcmJpcy5tb25leS9zdG9yYWdlL3N0eWxlL2NvaW5Mb2dvMy5wbmc.webp",
    "verification": "none",
    "score": 0
  }
}
```




