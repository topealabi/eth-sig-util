# Eth-Sig-Util [![CircleCI](https://circleci.com/gh/MetaMask/eth-sig-util.svg?style=svg)](https://circleci.com/gh/MetaMask/eth-sig-util)

[![Greenkeeper badge](https://badges.greenkeeper.io/MetaMask/eth-sig-util.svg)](https://greenkeeper.io/)

A small collection of ethereum signing functions.

You can find usage examples [here](https://github.com/flyswatter/js-eth-personal-sign-examples) 

[Available on NPM](https://www.npmjs.com/package/eth-sig-util)

## Supported Signing Methods

Currently there is only one supported signing protocol. More will be added as standardized. 

- Personal Sign (`personal_sign`) [geth thread](https://github.com/ethereum/go-ethereum/pull/2940)


## Installation

```
npm install eth-sig-util --save
```

## Methods

### concatSig(v, r, s)

All three arguments should be provided as buffers.

Returns a continuous, hex-prefixed hex value for the signature, suitable for inclusion in a JSON transaction's data field.

### normalize(address)

Takes an address of either upper or lower case, with or without a hex prefix, and returns an all-lowercase, hex-prefixed address, suitable for submitting to an ethereum provider.

### personalSign (privateKeyBuffer, msgParams)

msgParams should have a `data` key that is hex-encoded data to sign.

Returns the prefixed signature expected for calls to `eth.personalSign`.

### recoverPersonalSignature (msgParams)

msgParams should have a `data` key that is hex-encoded data unsigned, and a `sig` key that is hex-encoded and already signed.

Returns a hex-encoded sender address.

### signTypedData (privateKeyBuffer, msgParams)

Signs typed data as per [EIP712](https://github.com/ethereum/EIPs/pull/712).

Data should be under `data` key of `msgParams`. The method returns prefixed signature.

### recoverTypedSignature ({data, sig})

Return address of a signer that did `signTypedData`.

Expects the same data that were used for signing. `sig` is a prefixed signature.

### typedSignatureHash (typedData)

Return hex-encoded hash of typed data params according to [EIP712](https://github.com/ethereum/EIPs/pull/712) schema.

### extractPublicKey (msgParams)

msgParams should have a `data` key that is hex-encoded data unsigned, and a `sig` key that is hex-encoded and already signed.

Returns a hex-encoded public key.


### getEncryptionPublicKey(address)

Takes an address of either upper or lower case, with or without a hex prefix, and returns the address owner’s encryption public key in base64

### encrypt(recieverPublicKey, msgParams, version)

Takes a base64 recieverPublicKey, a json object for msgParams of the form {data: ‘message’}, and a plaintext version parameter which identify the encryption curve and algorithm. The only version currently available is ‘x25519-xsalsa20-poly1305’.

Encrypts the msgParams data attribute using [nacl.box](https://tweetnacl.js.org/#/box)


### decrypt(encryptedData, recieverPrivateKey)
Takes a payload of the form

{ version: 'x25519-xsalsa20-poly1305',
nonce: '1dvWO7uOnBnO7iNDJ9kO9pTasLuKNlej',
ephemPublicKey: 'FBH1/pAEHOOW14Lu3FWkgV3qOEcuL78Zy+qW1RwzMXQ=',
ciphertext: 'f8kBcl/NCyf3sybfbwAKk/np2Bzt9lRVkZejr6uh5FgnNlH/ic62DZzy' };

And returns utf8 data
