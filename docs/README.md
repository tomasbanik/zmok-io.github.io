## ZMOK Documentation
> Unlimited, Fast, Fault Tolerant Ethereum JSON-RPC/WS API

## Getting Started
Getting started with ZMOK (https://zmok.io) takes just a few minutes once you’ve  [connect your wallet](https://zmok.io/wallet).

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

?> **INFO: Archive calls are available for all users/packages without any limits.**


If you are interested in inspecting historical data (data outside of the most recent 128 blocks) for any of the methods listed below, your request requires access to archive data.

| Method |
| ------ |
|eth_getBalance|
|eth_getCode|
|eth_getTransactionCount|
|eth_getStorageAt|
|eth_call|

## Front-Running

With ZMOK your transaction may be front-run against the other transactions in the mempool. The front-running is described as the “act of getting a transaction first in line in the execution queue, right before a known future transaction occurs.”

Methods enhanced with Front-Running:

| Method |
| ------ |
|eth_sendRawTransaction|

!> **INFO: Method eth_sendRawTransaction enhanced with Front-Running is available only for packages with enabled Front-Running.**

Sample usage:

```js
// sendRawTransaction.js

const Web3 = require('web3')
const Tx = require('ethereumjs-tx').Transaction

// connect to ZMOK endpoint enhanced with Front-running
const web3 = new Web3(new Web3.providers.HttpProvider('https://api.zmok.io/fr/YOUR-APP_ID'))

// the address that will send the test transaction
const addressFrom = 'FROM-ADDRESS'
const privateKey = Buffer.from('YOUR-PRIVATE-KEY', 'hex')

// the destination address
const addressTo = 'TO-ADDRESS'

// construct the transaction data
// NOTE: property 'nonce' must be merged in from web3.eth.getTransactionCount
// before the transaction data is passed to new Tx(); see sendRawTransaction below.
const txData = {
  gasLimit: web3.utils.toHex(25000),
  gasPrice: web3.utils.toHex(10e9), // 10 Gwei
  to: addressTo,
  from: addressFrom,
  value: web3.utils.toHex(web3.utils.toWei('1', 'ether')) // thanks @abel30567
  // if you want to send raw data (e.g. contract execution) rather than sending tokens,
  // use 'data' instead of 'value'
  // e.g. myContract.methods.myMethod(123).encodeABI()
}

/** Signs the given transaction data and sends it. Abstracts some of the details of
  * buffering and serializing the transaction for web3.
  * @returns A promise of an object that emits events: transactionHash, receipt, confirmaton, error
*/
const sendRawTransaction = txData =>
  // get the number of transactions sent so far so we can create a fresh nonce
  web3.eth.getTransactionCount(addressFrom).then(txCount => {
    const newNonce = web3.utils.toHex(txCount)
    const transaction = new Tx({ ...txData, nonce: newNonce }, { chain: 'mainnet' }) // or 'rinkeby'
    transaction.sign(privateKey)
    const serializedTx = transaction.serialize().toString('hex')
    return web3.eth.sendSignedTransaction('0x' + serializedTx)
  })


// fire away!
sendRawTransaction(txData).then(result => {
  console.log(result)
  }
)
```

## Hello World
Ethereum and Web3.js “Hello World”:

[https://github.com/zmok-io/ethbalance](https://github.com/zmok-io/ethbalance)

## Support
For support, please email to: [support@zmok.io](mailto:support@zmok.io).
