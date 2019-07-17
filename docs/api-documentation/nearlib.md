# nearlib.js

## KeyPair

Access Key based signer that uses Wallet to authorize app on the account and receive the access key.

### getPublicKey

Get the public key.

### getSecretKey

Get the secret key.

#### Examples

```javascript
// Passing existing key into a function to store in local storage
 async setKey(accountId, key) {
     window.localStorage.setItem(
         BrowserLocalStorageKeystore.storageKeyForPublicKey(accountId), key.getPublicKey());
     window.localStorage.setItem(
         BrowserLocalStorageKeystore.storageKeyForSecretKey(accountId), key.getSecretKey());
 }
```

### fromRandomSeed

Generate a new keypair from a random seed

#### Examples

```javascript
const keyWithRandomSeed = KeyPair.fromRandomSeed();
keyWithRandomSeed.getPublicKey()
// returns [PUBLIC_KEY]

keyWithRandomSeed.getSecretKey()
// returns [SECRET_KEY]
```

### encodeBufferInBs58

Encode a buffer as string using bs58

#### Parameters

-   `buffer` **[Buffer](https://nodejs.org/api/buffer.html)** 

#### Examples

```javascript
KeyPair.encodeBufferInBs58(key.publicKey)
```

## KeyPair

This class provides key pair functionality (generating key pairs, encoding key pairs).

### Parameters

-   `publicKey` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** 
-   `secretKey` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### getPublicKey

Get the public key.

### getSecretKey

Get the secret key.

#### Examples

```javascript
// Passing existing key into a function to store in local storage
 async setKey(accountId, key) {
     window.localStorage.setItem(
         BrowserLocalStorageKeystore.storageKeyForPublicKey(accountId), key.getPublicKey());
     window.localStorage.setItem(
         BrowserLocalStorageKeystore.storageKeyForSecretKey(accountId), key.getSecretKey());
 }
```

### fromRandomSeed

Generate a new keypair from a random seed

#### Examples

```javascript
const keyWithRandomSeed = KeyPair.fromRandomSeed();
keyWithRandomSeed.getPublicKey()
// returns [PUBLIC_KEY]

keyWithRandomSeed.getSecretKey()
// returns [SECRET_KEY]
```

### encodeBufferInBs58

Encode a buffer as string using bs58

#### Parameters

-   `buffer` **[Buffer](https://nodejs.org/api/buffer.html)** 

#### Examples

```javascript
KeyPair.encodeBufferInBs58(key.publicKey)
```

## BrowserLocalStorageKeystore

Wallet based account and signer that uses external wallet through the iframe to sign transactions.

## Account

Near account and account related operations.

### Parameters

-   `nearClient`  

### Examples

```javascript
const account = new Account(nearjs.nearClient);
```

### createAccount

Creates a new account with a given name and key,

#### Parameters

-   `newAccountId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** id of the new account.
-   `publicKey` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** public key to associate with the new account
-   `amount` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** amount of tokens to transfer from originator account id to the new account as part of the creation.
-   `originator`  
-   `originatorAccountId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** existing account on the blockchain to use for transferring tokens into the new account

#### Examples

```javascript
const createAccountResponse = await account.createAccount(
   mainTestAccountName,
   keyWithRandomSeed.getPublicKey(),
   1000,
   aliceAccountName);
```

### addAccessKey

Adds a new access key to the owners account for an some app to use.

#### Parameters

-   `ownersAccountId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** id of the owner's account.
-   `newPublicKey` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** public key for the access key.
-   `contractId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** if the given contractId is not empty, then this access key will only be able to call
         the given contractId.
-   `methodName` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** If the given method name is not empty, then this access key will only be able to call
         the given method name.
-   `fundingOwner` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** account id to own the funding of this access key. If empty then account owner is used by default.
         fundingOwner should be used if this access key would be sponsored by the app. In this case the app would
         prefer to own funding of this access key, to get it back when the key is removed.
-   `fundingAmount` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** amount of funding to withdraw from the owner's account and put to this access key.
         Make sure you that you don't fund the access key when the fundingOwner is different from the account's owner.

#### Examples

```javascript
const addAccessKeyResponse = await account.addAccessKey(
   accountId,
   keyWithRandomSeed.getPublicKey(),
   contractId,
   "",
   "",
   10);
```

### createAccountWithRandomKey

Creates a new account with a new random key pair. Returns the key pair to the caller. It's the caller's responsibility to
manage this key pair.

#### Parameters

-   `newAccountId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** id of the new account
-   `amount` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** amount of tokens to transfer from originator account id to the new account as part of the creation.
-   `originatorAccountId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** existing account on the blockchain to use for transferring tokens into the new account

#### Examples

```javascript
const createAccountResponse = await account.createAccountWithRandomKey(
    newAccountName,
    amount,
    aliceAccountName);
```

### viewAccount

Returns an existing account with a given `accountId`

#### Parameters

-   `accountId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** id of the account to look up

#### Examples

```javascript
const viewAccountResponse = await account.viewAccount(existingAccountId);
```

## Near

Javascript library for interacting with near.

### Parameters

-   `nearClient` **NearClient** 

### callViewFunction

Calls a view function. Returns the same value that the function returns.

#### Parameters

-   `contractAccountId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** account id of the contract
-   `methodName` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** method to call
-   `args` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** arguments to pass to the method

#### Examples

```javascript
const viewFunctionResponse = await near.callViewFunction(
  contractAccountId, 
  methodName, 
  args);
```

### scheduleFunctionCall

Schedules an asynchronous function call. Returns a hash which can be used to
check the status of the transaction later.

#### Parameters

-   `amount` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** amount of tokens to transfer as part of the operation
-   `originator`  
-   `contractId`  
-   `methodName` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** method to call
-   `args` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** arguments to pass to the method
-   `sender` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** account id of the sender
-   `contractAccountId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** account id of the contract

#### Examples

```javascript
const scheduleResult = await near.scheduleFunctionCall(
    0,
    aliceAccountName,
    contractName,
    'setValue', // this is the function defined in a wasm file that we are calling
    setArgs);
```

### deployContract

Deploys a smart contract to the block chain

#### Parameters

-   `contractId`  
-   `wasmByteArray`  
-   `contractAccountId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** account id of the contract
-   `wasmArray` **[Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array)** wasm binary

#### Examples

```javascript
const response =  await nearjs.deployContract(contractName, data);
```

### getTransactionStatus

Get a status of a single transaction identified by the transaction hash.

#### Parameters

-   `transactionHash` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** unique identifier of the transaction

#### Examples

```javascript
// get the result of a transaction status call
const result = await this.getTransactionStatus(transactionHash)
```

### waitForTransactionResult

Wait until transaction is completed or failed.
Automatically sends logs from contract to `console.log`.

[MAX_STATUS_POLL_ATTEMPTS](MAX_STATUS_POLL_ATTEMPTS) defines how many attempts are made.
[STATUS_POLL_PERIOD_MS](STATUS_POLL_PERIOD_MS) defines delay between subsequent [getTransactionStatus](getTransactionStatus) calls.

#### Parameters

-   `transactionResponseOrHash` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object))** hash of transaction or object returned from [submitTransaction](submitTransaction)
-   `options` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** object used to pass named parameters (optional, default `{}`)
    -   `options.contractAccountId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** specifies contract ID for better logs and error messages

#### Examples

```javascript
const result = await this.waitForTransactionResult(transactionHash);
```

### loadContract

Load given contract and expose it's methods.

Every method is taking named arguments as JS object, e.g.:
`{ paramName1: "val1", paramName2: 123 }`

View method returns promise which is resolved to result when it's available.
State change method returns promise which is resolved when state change is succesful and rejected otherwise.

Note that `options` param is only needed temporary while contract introspection capabilities are missing.

#### Parameters

-   `contractAccountId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** contract account name
-   `options` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** object used to pass named parameters
    -   `options.sender` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** account name of user which is sending transactions
    -   `options.viewMethods` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>** list of view methods to load (which don't change state)
    -   `options.changeMethods` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>** list of methods to load that change state

#### Examples

```javascript
// this example would be a counter app with a contract that contains the incrementCounter and decrementCounter methods
window.contract = await near.loadContract(config.contractName, {
  viewMethods: ["getCounter"],
  changeMethods: ["incrementCounter", "decrementCounter"],
  sender: nearlib.dev.myAccountId
});
```

Returns **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** object with methods corresponding to given contract methods.

### createDefaultConfig

Generate a default configuration for nearlib

#### Parameters

-   `nodeUrl` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** url of the near node to connect to (optional, default `'http://localhost:3030'`)

#### Examples

```javascript
Near.createDefaultConfig();
```

## WalletAccessKey

Access Key based signer that uses Wallet to authorize app on the account and receive the access key.

### Parameters

-   `appKeyPrefix`  
-   `walletBaseUrl`   (optional, default `'https://wallet.nearprotocol.com'`)
-   `signer`   (optional, default `null`)

### Examples

```javascript
// if importing WalletAccessKey directly
const walletAccount = new WalletAccessKey(contractName, walletBaseUrl)
// if importing in all of nearLib and calling from variable
const walletAccount = new nearlib.WalletAccessKey(contractName, walletBaseUrl)
// To access this signer globally
window.walletAccount = new nearlib.WalletAccessKey(config.contractName, walletBaseUrl);
// To provide custom signer where the keys would be stored
window.walletAccount = new nearlib.WalletAccessKey(config.contractName, walletBaseUrl, customSigner);
```

### isSignedIn

Returns true, if this WalletAccount is authorized with the wallet.

#### Examples

```javascript
walletAccount.isSignedIn();
```

### getAccountId

Returns authorized Account ID.

#### Examples

```javascript
walletAccount.getAccountId();
```

### requestSignIn

Redirects current page to the wallet authentication page.

#### Parameters

-   `contractId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** contract ID of the application
-   `title` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** name of the application
-   `successUrl` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** url to redirect on success
-   `failureUrl` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** url to redirect on failure

### signOut

Sign out from the current account

#### Examples

```javascript
walletAccount.signOut();
```

### signBuffer

Sign a buffer. If the key for originator is not present,
this operation will fail.

#### Parameters

-   `buffer` **[Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array)** 
-   `originator` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** 

## WalletAccount

Wallet based account and signer that uses external wallet through the iframe to sign transactions.

### Parameters

-   `appKeyPrefix`  
-   `walletBaseUrl`   (optional, default `'https://wallet.nearprotocol.com'`)
-   `keyStore`   (optional, default `new BrowserLocalStorageKeystore()`)

### Examples

```javascript
// if importing WalletAccount directly
const walletAccount = new WalletAccount(contractName, walletBaseUrl)
// if importing in all of nearLib and calling from variable 
const walletAccount = new nearlib.WalletAccount(contractName, walletBaseUrl)
// To access wallet globally use:
window.walletAccount = new nearlib.WalletAccount(config.contractName, walletBaseUrl);
```

### isSignedIn

Returns true, if this WalletAccount is authorized with the wallet.

#### Examples

```javascript
walletAccount.isSignedIn();
```

### getAccountId

Returns authorized Account ID.

#### Examples

```javascript
walletAccount.getAccountId();
```

### requestSignIn

Redirects current page to the wallet authentication page.

#### Parameters

-   `contract_id` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** contract ID of the application
-   `title` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** name of the application
-   `success_url` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** url to redirect on success
-   `failure_url` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** url to redirect on failure

#### Examples

```javascript
walletAccount.requestSignIn(
    myContractId,
    title,
    onSuccessHref,
    onFailureHref);
```

### \_completeSignInWithAccessKey

Complete sign in for a given account id and public key. To be invoked by the app when getting a callback from the wallet.

### signOut

Sign out from the current account

#### Examples

```javascript
walletAccount.signOut();
```

### signBuffer

Sign a buffer. If the key for originator is not present,
this operation will fail.

#### Parameters

-   `buffer` **[Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array)** 
-   `originator` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** 