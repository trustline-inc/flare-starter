# Flare JSON-RPC Guide

Flare uses [Athereum](https://medium.com/avalancheavax/athereum-ethereum-on-avalanche-consensus-ava-labs-7effcb94b797), a friendly fork of Ethereum that runs the EVM with some additional features provided by the [Avalanche Platform](https://www.avalabs.org/). Since Flare is fully EVM-compatible, you can use Ethereum's JSON-RPC API to interact with the Flare Network. Check out the documentation on Ethereum's [JSON-RPC API](https://eth.wiki/json-rpc/API) to learn more about the JSON-RPC protocol. The JSON-RPC API is a foundational data transfer protocol for interacting with any EVM-based network. JSON-RPC is the primary protocol to interact with an Athereum/Ethereum client. Libraries in other languages, such as [Web3.js](https://web3js.readthedocs.io/en/v1.3.0/), are wrappers around JSON-RPC.

In this guide, we will interact with Flare's Coston Test Network using a commanline terminal. The JSON-RPC endpoint for the testnet is `https://coston.flare.network/ext/bc/C/rpc`.

## Examples

Any of the JSON-RPC methods described in the Ethereum Wiki mentioned above can be used with Flare. Here is an example of calling the JSON-RPC API using `curl`:

```
$ curl -X POST https://coston.flare.network/ext/bc/C/rpc \
  --data '{"jsonrpc": "2.0", "method": "web3_clientVersion", "params":[], "id":67 }' \
  -H "Content-Type: application/json"
{"jsonrpc":"2.0","id":67,"result":"Athereum 1.0"}
```

The testnet response for the `web3_clientVersion` method us shows that the EVM uses the [JSON-RPC 2.0 Specification](https://www.jsonrpc.org/specification). The client, which is a software implementation of Athereum, responds with information about the testnet [node](https://ethereum.org/en/developers/docs/nodes-and-clients/) that we are connecting to through the endpoint.

There are a lot of great API methods so I recommend that you take a look at the full list. Let's take a look at some more examples:

### **`eth_accounts`**

Returns a list of addresses owned by the client.

```
$ curl -X POST https://coston.flare.network/ext/bc/C/rpc \
  --data '{"jsonrpc": "2.0", "method": "eth_accounts", "params":[],"id": 67}' \
  -H "Content-Type: application/json" \
  | python -m json.tool
{
    "id": 67,
    "jsonrpc": "2.0",
    "result": [
        "0xdd5f754dc228450e818a2af8e412426feb954ce4",
        "0x02e1c8693e1ceb256f295515df1fb731edf399a8",
        "0x562b24f340c082771b27f285db979f0e3eb81cee",
        "0xb3bb6397c235c569d5cb405c1a3f81112b04612b",
        "0x543ed104265ae2f19f01c98ed5d9210632091dcc",
        "0xe3760fc9bacf3574da5358486ea2d82d358c04b7",
        "0x519ae06912d23588cf5b71e19064cfd1fa381758",
        "0xcea79be98b0e3e101ef33ba16210f283eecf4988",
        "0x44cfee2a75c57647add67828db92f7864a5b0be6",
        "0x73fc45a2f61fa3f99a65b5e23feba3a243f88b21",
        "0x8fff43c6c83fc9ec844a9781d4f3f1093c9e6a7c"
    ]
}
```

### **`eth_blockNumber`**

Returns the number of the most recent block.

```
$ curl -X POST https://coston.flare.network/ext/bc/C/rpc \
  --data '{"jsonrpc": "2.0", "method": "eth_blockNumber", "params":[],"id": 67}' \
  -H "Content-Type: application/json"
{"jsonrpc":"2.0","id":67,"result":"0x388a4"}
```

Some results are encoded in hex format and prefixed with `0x`. To decode the actual value, there are several options. One option is to use the JavaScript console:

```
> parseInt('0x388a4', 16)
231588
```

It can also be decoded via the commandline with Python:

```
$ python -c "print int('0x388a4', 16)"
231588
```

We can now see that the most recent block is number `231588`.

### **`eth_getBlockByNumber`**

Returns information about the specified block. Note that the parameter expects a hex-encoded number.

```
$ curl -X POST https://coston.flare.network/ext/bc/C/rpc \
  --data '{"jsonrpc": "2.0", "method": "eth_getBlockByNumber", "params": ["0x388a4", true],"id": 67}' \
  -H "Content-Type: application/json"
{
  "id": 67,
  "jsonrpc": "2.0",
  "result": {
    "difficulty": "0x1",
    "extraData": "0xd883010903846765746888676f312e31342e32856c696e7578d6dd3e6e27472e544f805783de8ff48f84b5147ac38cd0191f84ff9b9f161d6a",
    "gasLimit": "0x7a1200",
    "gasUsed": "0x5208",
    "hash": "0x69ad4fbb5cda48271d6daba112d44ddf52b6c4f4a7f1cef1b9bff84a56de4f29",
    "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
    "miner": "0x0100000000000000000000000000000000000000",
    "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "nonce": "0x0000000000000000",
    "number": "0x388a4",
    "parentHash": "0x6284ee941c7b641865bb52271dacbe9770d6f218e3f81a38bba30c055dea4bb0",
    "receiptsRoot": "0x056b23fbba480696b65fe5a59b8f2148a1299103c4f57df839233af2cf4ca2d2",
    "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
    "size": "0x2ad",
    "stateRoot": "0x05ef08b75ef358cbb4001c1dca21717f6a9c4e3c1d3415df85f6ed961e6c72cc",
    "timestamp": "0x602d7621",
    "totalDifficulty": "0x388a4",
    "transactions": [
      {
          "blockHash": "0x69ad4fbb5cda48271d6daba112d44ddf52b6c4f4a7f1cef1b9bff84a56de4f29",
          "blockNumber": "0x388a4",
          "from": "0x1c22a9dc4bfe93c49bba31c36a887c01b5eba265",
          "gas": "0x33450",
          "gasPrice": "0x4a817c800",
          "hash": "0x0c80fb2a0086701992c14a36690fb3fb01fa640410c1e2765a7d9ca388892b66",
          "input": "0x",
          "nonce": "0xbce5",
          "r": "0xb79d3aa808781c4257e598dccbbbb50cddccdf6de2372077cc07862a3f66218e",
          "s": "0x1b2d12d868780633ff793db4b3a225786ce61efb88de08d7cd171bfde43c28c4",
          "to": "0x0a218d00e8a71dbd8240758e062422c7f565ec6a",
          "transactionIndex": "0x0",
          "v": "0x25",
          "value": "0xde0b6b3a7640000"
      }
    ],
    "transactionsRoot": "0x3c413200f2845f9ae816a093c3098378e0b1b90faff36c34afe4a496e99625c0",
    "uncles": []
  }
}
```

### **`eth_getBalance`**

Gets the balance for an account in wei (1 Spark = 10^18 wei).

```
$ curl -X POST https://coston.flare.network/ext/bc/C/rpc \
  --data '{"jsonrpc": "2.0", "method": "eth_getBalance", "params":["0xb7b685dD1b52D2939bD6f5d9515635c5a633fFAA", "latest"],"id": 67}' \
  -H "Content-Type: application/json"
{"jsonrpc":"2.0","id":67,"result":"0x277370eb4dcb872296277e34800"}
$ python -c "print '{:.4f}'.format(int('0x277370eb4dcb872296277e34800', 16) * 10**-18)"
50010010001099.9922
```

The Python command formats the account balance to Spark.
