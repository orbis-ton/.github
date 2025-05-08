# Read ORBIS data from TON blockchain

1. Use the TON API to get the distribution contract state

  ```curl
  curl -X 'GET' \
  'https://tonapi.io/v2/blockchain/accounts/EQCto1TgtdVY_ehQofMPcpUzo0HREUU_zWGFpxX_RIeQKswH/methods/get_giver_data' \
  -H 'accept: application/json'
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



