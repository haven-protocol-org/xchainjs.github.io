---
sort: 2
---

# How to use

## Installation

```
yarn add @xchainjs/xchain-haven
```

Following peer dependencies have to be installed into your project. These are not included in `@xchainjs/xchain-haven`.

```
yarn add @xchainjs/xchain-client @xchainjs/xchain-crypto @xchainjs/xchain-util axios haven-core-js
```

## Hacen Client Testing

```
yarn install
yarn test
```

## Basic Usage Example 

### Connect wallet to new Haven Client and check sync state

Decrypt keystore returns `phrase` .\
Create a new Haven client instance\
Preload SDK via HavenClient\
Use the HavenClient to getAddress()\
Use the HavenClient to getBalance() if client is synced or after sync completion\

```ts
// Imports 
import { Network } from "@xchainjs/xchain-client"
import { Client } from "@xchainjs/xchain-haven"
import { decryptFromKeystore } from "@xchainjs/xchain-crypto"
import { assetToBase, baseToAsset, assetAmount } from "@xchainjs/xchain-util"

// Connect wallet to new Haven client instance & check balances of assets 
const connectWallet = async () => {
    let phrase = await decryptFromKeystore(keystore1, password)
    const havenClient = new Client({network: Network.Mainnet, phrase})
    await havenClient.preloadSDK();
    let address = havenClient.getAddress()
    console.log(address)

    const fetchBalances = async () => {
        try {
            const balances = await havenClient.getBalance(address)
            console.log(`Asset ${balances.forEach(asset => console.log(`${asset.asset.symbol}, ${(baseToAsset(asset.amount).amount())}`))}`)
        } catch (error) {
            console.log("Caught: " + error.message)
        }
    }

    if (await havenClient.isSynced()) {
        fetchBalances();
    }
    else {
        const observer: SyncObserver = {
             // we can fetchBalances while syncing, will return balances according to synced height
            next: syncState => console.log(syncState.syncProgress)
            complete: syncState => fetchBalances();
            error: errMessage => console.log("Caught while syncing: " + errMessage)
        }
        havenClient.subscribeSyncProgress(observer)
    }
}

```

### Transfer XHV using Haven Client

Create new instance of HavenClient\
Set correct amount using `xchain-util` helper functions\
Build new transaction using TxParams and call transfer.

```ts
//Imports
import { AssetXHV } from "@xchainjs/xchain-haven"
// transfer should only be allowed after inital syncing completed
const transferXHV = async () => {
    let recipient = await getRecipientAddress() 
    let amountToTransfer = 1
    let amount = assetToBase(assetAmount(amountToTransfer, 12))
    let phrase = await decryptFromKeystore(keystore1, password)
    const havenClient = new Client({network: Network.Mainnet, phrase})
    console.log("Building Transaction")
    try {
        const txid = await havenClient.transfer({
            // wallet index will be ignored by client
            "walletIndex": 0,
            "asset": AssetXHV,
            "amount": amount,
            "recipient": recipient,
            "memo": ""
        })
        console.log(`Amount: ${amount.amount()} Asset: ${AssetXHV.symbol} TransactionId: ${txid} `)
        
    } catch (error) {
        console.log(`Transfer failed:  ${error}`)
    }
}

```

### Get transaction Data & transaction History 

```ts
// Return trasanction data from a txid/hash
const transactionData = async () => {
    let phrase = await decryptFromKeystore(keystore1, password)
    const havenClient = new Client({network: Network.Mainnet, phrase })
    let hash = "insert hash"
    try {
        const txData = await havenClient.getTransactionData(hash)
        console.log(JSON.stringify(txData))
        console.log(`From ${JSON.stringify(txData)}`)

    } catch (error) {
        console.log(`Caught: ${error}`)
    }
}

// Return transaction history
// txHistoryParams > address, offset?, startTime?, asset?, limit? 
const transactionHistory = async () => {
    let phrase = await decryptFromKeystore(keystore1, password)
    const havenClient = new Client({network: Network.Mainnet, phrase })
    let Address = havenClient.getAddress()
    try {
        const txHistory = await havenClient.getTransactions()
        console.log(`Found ${txHistory.total.toString()}`)
        txHistory.txs.forEach(tx => console.log(JSON.stringify(tx)))
    } catch (error) {
        console.log(`Caught: ${error}`)
    }
}

```

### Get transfer Fees

Returns current fees for transaction options on the network.\
Returns fee in Havens base amount

```ts

const getTransferFees = async () => {
    let phrase = await decryptFromKeystore(keystore1, password)
    const havenClient = new Client({network: Network.Mainnet, phrase })
    try {
        const {fast, fastest, average} = await havenClient.getFees()
        console.log(`Fees Fast: ${baseToAsset(average).amount()}`)
        console.log(`Fees Fast: ${baseToAsset(fast).amount()}`)
        console.log(`Fees Fast: ${baseToAsset(fastest).amount()}`)
    } catch (error) {
        console.log(`caught: ${error}`)
    }
}

```

### Get Network and Explorer Data 

Using in-built functions to return useful links and data

```ts
const networkData = async () => {
    let phrase = await decryptFromKeystore(keystore1, password)
    const havenClient = new Client({network: Network.Mainnet, phrase })
    let txid = "insert hash"
    let address = havenClient.getAddress()
    console.log(address)
    try {
        const getNetwork = havenClient.getNetwork()
        const getExplorerTxUrl = havenClient.getExplorerTxUrl(txid)
        const getExplorerUrl = havenClient.getExplorerUrl()
        console.log(`Network: ${getNetwork}`)
        console.log(`Explorer txid url: ${getExplorerTxUrl}`)
        console.log(`Explorer Url: ${getExplorerUrl}`)
    } catch (error) {
        console.log(`caught: ${error}`)
    }
}

```