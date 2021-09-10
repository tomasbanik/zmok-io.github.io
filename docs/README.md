## ZMOK Documentation
> Unlimited, Fast, Fault Tolerant Ethereum JSON-RPC/WS API

## Getting Started
Getting started with ZMOK (https://zmok.io) takes just a few minutes once you’ve  [connected your wallet](https://zmok.io/wallet).

Seamlessly access Ethereum via the ZMOK load-balanced nodes and smart architecture the same way you would via your own nodes. We have built services and APIs around [JSON RPC](https://github.com/ethereum/wiki/wiki/JSON-RPC) over HTTPS that you can use with your favorite libraries and frameworks, on Ethereum networks - Mainnet, Ropsten, Rinkeby and Mainnet enhanced with Front-running.

## Authenticating using an APP ID
ZMOK's Ethereum APIs require a valid APP ID to be included with your request traffic. This identifier should be appended to the request URL.

```sh
curl https://api.zmok.io/<NETWORK>/<YOUR-APP-ID>
```

## Choose a Network
Use one of these endpoints as your Ethereum client provider.

?> **NOTE: Be sure to replace YOUR-APP-ID with a APP ID from your ZMOK dashboard**


| Network | Endpoint |
| :---        |    :----   |
| Mainnet | [https://api.zmok.io/mainnet/YOUR-APP-ID](https://api.zmok.io/mainnet/YOUR-APP-ID)<br/>[wss://api.zmok.io/mainnet/YOUR-APP-ID](wss://api.zmok.io/mainnet/YOUR-APP-ID) |
| Rinkeby | [https://api.zmok.io/testnet/YOUR-APP-ID](https://api.zmok.io/testnet/YOUR-APP-ID)<br/>[wss://api.zmok.io/testnet/YOUR-APP-ID](wss://api.zmok.io/testnet/YOUR-APP-ID) |
| Ropsten | [https://api.zmok.io/ropsten/YOUR-APP-ID](https://api.zmok.io/ropsten/YOUR-APP-ID)<br/>[wss://api.zmok.io/ropsten/YOUR-APP-ID](wss://api.zmok.io/ropsten/YOUR-APP-ID) |
| Mainnet - Front Running | [https://api.zmok.io/fr/YOUR-APP-ID](https://api.zmok.io/fr/YOUR-APP-ID)<br/>[wss://api.zmok.io/fr/YOUR-APP-ID](wss://api.zmok.io/fr/YOUR-APP-ID)|

## Make Requests
Below is a quick command line example using curl:


?> **NOTE: Be sure to replace YOUR-APP-ID with a APP ID from your ZMOK dashboard**

```sh
$ curl -X POST \
-H "Content-Type: application/json" \
--data '{"jsonrpc": "2.0", "id": 1, "method": "eth_blockNumber", "params": []}' \
"https://api.zmok.io/mainnet/YOUR-APP-ID"
```

The result should look something like this:

```sh
$ {"jsonrpc": "2.0","result": "0x657abc", "id":1}
```

[Read more about JSON-RPC](https://github.com/ethereum/wiki/wiki/JSON-RPC)

Same request could be also made with WebSocket:
```sh
$ wscat -c wss://api.zmok.io/mainnet/YOUR-APP-ID
> {"jsonrpc": "2.0", "id": 1, "method": "eth_blockNumber", "params": []}
< {"jsonrpc":"2.0","id":1,"result":"0x657abc"}
```




### List of all supported methods
| Method |
| ------ |
|eth_accounts|
|eth_blockNumber|
|eth_call|
|eth_chainId|
|eth_estimateGas|
|eth_gasPrice|
|eth_getBalance|
|eth_getBlockByHash|
|eth_getBlockByNumber|
|eth_getBlockTransactionCountByHash|
|eth_getBlockTransactionCountByNumber|
|eth_getCode|
|eth_getLogs|
|eth_getStorageAt|
|eth_getTransactionByBlockHashAndIndex|
|eth_getTransactionByBlockNumberAndIndex|
|eth_getTransactionByHash|
|eth_getTransactionCount|
|eth_getTransactionReceipt|
|eth_getUncleByBlockHashAndIndex|
|eth_getUncleByBlockNumberAndIndex|
|eth_getUncleCountByBlockHash|
|eth_getUncleCountByBlockNumber|
|eth_getWork|
|eth_hashrate|
|eth_mining|
|eth_protocolVersion|
|eth_sendRawTransaction|
|eth_submitWork|
|eth_syncing|
|net_listening|
|net_peerCount|
|net_version|
|web3_clientVersion|
|parity_nextNonce|

### Filter Methods
| Method |
| ------ |
|eth_newFilter|
|eth_newBlockFilter|
|eth_getFilterChanges|
|eth_uninstallFilter|

### Pub/Sub Websocket Methods
?> **NOTE: Ethereum Pub/Sub subscription support is only supported over "stateful" transports such as WebSocket.**

[Read more about RPC PUB SUB](https://github.com/ethereum/go-ethereum/wiki/RPC-PUB-SUB)

| Method |
| ------ |
|eth_subscribe|
|eth_unsubscribe|

## Archive data
Archive nodes are full nodes running with a special option known as "archive mode". Archive nodes have all the historical data of the blockchain since the genesis block. If you have a need for data from blocks before the last 128 blocks, you’ll want to access an archive node. For example, to use calls like eth_getBalance of an ancient address will only be possible with an archive node, to interact with smart contracts deployed much earlier in the blockchain, etc.

?> **INFO: Archive calls are available for all users and packages. Supported networks are: MAINNET, RINKEBY, ROPSTEN. <br/><br/>Mainnet FRONT-RUNNING (FR) endpoints do not support archive calls, because the FR nodes are optimized for performance and contain only the pruned data.**


If you are interested in inspecting historical data (data outside of the most recent 128 blocks) for any of the methods listed below, your request requires access to archive data.

| Method |
| ------ |
|eth_getBalance|
|eth_getCode|
|eth_getTransactionCount|
|eth_getStorageAt|
|eth_call|

[filename](front-running.md ':include')

## Hello World
Ethereum and Web3.js “Hello World”:

[https://github.com/zmok-io/ethbalance](https://github.com/zmok-io/ethbalance)

[filename](performance-tuning.md ':include')

## Support
For support, please email to: [support@zmok.io](mailto:support@zmok.io).
