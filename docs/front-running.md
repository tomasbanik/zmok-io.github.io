# Front-Running

With ZMOK your transaction may be front-run against the other transactions in the mempool. The front-running is described as the “act of getting a transaction first in line in the execution queue, right before a known future transaction occurs”.

With ZMOK FRONT-RUNNING a new submited transaction is uniquely distributed accross the very large cluster of distributed Ethereum nodes. This ensures that this transaction will be part of the next mined block as fast as possible.

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
  * @returns A promise of an object that emits events: transactionHash, receipt, confirmation, error
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

# ProviderError: exceeds the configured cap (1.00 ether).
Users with enabled Front-Running, have also access to endpoints/Geth nodes with preconfigured param --rpc.txfeecap=0, which disable the default max transaction fee. More info: https://geth.ethereum.org/docs/interface/command-line-options.

Sample usage:

```sh
$ curl -X POST \
-H "Content-Type: application/json" \
--data '{"jsonrpc": "2.0", "id": 1, "method": "eth_sendRawTransaction", "params": ["0xf867808082520894036d0e47e9844e6d0fb5bd104043599f889fc215880de0b6b3a7640000801ca08095e722b96d9f13f3bf5ac997d7cedbd2c0f82295d47b770faad526b4e7e519a05262eef932e223130d1f72664162b897c6129f0164e0806ed959b9abd6f23a39"]}' \
"https://api.zmok.io/notxfeecap/YOUR-APP-ID"
```
