### Install NVM

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```
```
nvm install v16.10.0

```
### Create folder

Create new folder on your server or Local Computer

```bash
mkdir lesson_1
```
Than go to this folder
```bash
cd lesson_1
```

### Install

```bash
npm init -y
npm install webpack webpack-cli --save-dev
npm install copy-webpack-plugin --save-dev
npm install ton-client-web-js
npm install light-server

```
### Create

```bash
mkdir src
mkdir dist
```

```bash
|- /src
  |- index.js
|- /dist
  |- index.html
```

### Create Webpack Configuration file
```
webpack.config.js

```
Paste this in the file

```
const CopyPlugin = require('copy-webpack-plugin');

module.exports = {
  mode: 'development',
  plugins: [
    new CopyPlugin({
      patterns: [
        { from: './node_modules/ton-client-web-js/tonclient.wasm' }
      ],
    }),
  ],
};
```
### Than go to your index.js file
```
 index.js
```

Paste now this and than I will explain.

```
import { Account } from '@tonclient/appkit';
import {
  signerKeys,
  TonClient,
  MessageBodyType,
  signerNone,
  //abiContract,
} from "@tonclient/core";
import { libWeb } from '@tonclient/lib-web';
import contractPackageSM from './contracts/SafeMultisig.js';
import transfer_abi from './contracts/Transfer.js';



TonClient.useBinaryLibrary(libWeb);

/* Multisig contract from js with ABI and TVC*/

const SafeMultisig = {
  abi: contractPackageSM.abi,
  tvc:contractPackageSM.tvc,
}


/* Temporary function for testing */

function addHTML(message) {
  document.body.insertAdjacentHTML("beforeend", `<p>${message}</p>`);
}

/* Temporary functions for signing via TONSDK */

async function create_wallet_with_mnemonic(){
  const client = new TonClient({
    network: {
      //if you wanna use dev net of Everscale use this
      endpoints: ['net.ton.dev']
      //if you wanna use dev net of Everscale use this
      //  endpoints: ['main.ton.dev']
    }
  });
  const SEED_PHRASE_WORD_COUNT = 12;
  const SEED_PHRASE_DICTIONARY_ENGLISH = 1;
  const HD_PATH = "m/44'/396'/0'/0/0";


  const seed = (await client.crypto.mnemonic_from_random({
    dictionary: SEED_PHRASE_DICTIONARY_ENGLISH,
    word_count: SEED_PHRASE_WORD_COUNT,
    path: HD_PATH,
  }).catch(e => console.log("ERROR:", e)));
  addHTML("New Wallet");
  addHTML("Seed phrase of new user: "+ seed['phrase']);

  const keysgen = (await client.crypto.mnemonic_derive_sign_keys({
    dictionary: SEED_PHRASE_DICTIONARY_ENGLISH,
    word_count: SEED_PHRASE_WORD_COUNT,
    phrase: seed.phrase,
    path: HD_PATH,
  }).catch(e => console.log("ERROR:", e)));

  addHTML("Public key "+keysgen['public']);
  addHTML("Private key "+keysgen['secret']);


  const new_wallet = new Account(
    SafeMultisig,
    {
      signer: signerKeys(keysgen),
      client
    }
  );

  addHTML(`Address for deposit ${await new_wallet.getAddress()}.`);

};

window.create_wallet_with_mnemonic = create_wallet_with_mnemonic;

/* Function for Withdraw */

async function transfer_ever(){

/*
  keyPair - Signer keys from user wallet
  recepient - Address of receiver wallet
*/

  const multisig = new Account(SafeMultisig, {
    signer: signerKeys(keyPair),
    client,
  });
  console.log("Call `submitTransaction` function");

  const body = (await client.abi.encode_message_body({
    abi: abiContract(transfer_abi.abi),
    call_set: {
      function_name: "transfer",
      input: {
        comment: Buffer.from("My comment").toString("hex"),
      },
    },
    is_internal: true,
    signer: signerNone(),
  })).body;

  const transactionInfo = (await multisig.run("submitTransaction", {
    dest: recipient,
    value: 100_000_000,
    bounce: false,
    allBalance: false,
    payload: body,
  }));
  console.log(transactionInfo);

```
