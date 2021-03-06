# Notice

At this stage, this API is in alpha and subject to change. However, we will always do our best to maintain backwards-compatibility.

Note that for technical reasons, any function that claims to return a Uint8Array will instead return a base64 string that can be decoded to the Uint8Array.

# API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### Table of Contents

*   [createHmac](#createhmac)
*   [createSignature](#createsignature)
*   [getPrimarySigningPub](#getprimarysigningpub)
*   [getPrivilegedSigningPub](#getprivilegedsigningpub)
*   [decrypt](#decrypt)
*   [isAuthenticated](#isauthenticated)
*   [isInitialized](#isinitialized)
*   [createAction](#createaction)
*   [encrypt](#encrypt)
*   [sendDataTransaction](#senddatatransaction)

## createHmac

Creates an HMAC on data with a key belonging to the user. Also allows an HMAC to be created with the asymmetric keys between two users with an ECDH shared secret.

### Parameters

*   `args` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** All parameters are passed in an object

    *   `args.data` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) | [Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array))** The data to HMAC. If given as a string, must be base64
    *   `args.key` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Specify the key tree to use. Available key trees are `primaryKey`, `privilegedKey`, `primarySigning`, and `privilegedSigning`
    *   `args.path` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Specify the derivation path to the key to use
    *   `args.pub` **([Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array) | [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))?** If using `primarySigning` or `privilegedSigning`, specify the foreign public key to use when deriving the ECDH shared secret. The HMAC will be created with the shared secret key. Must be a 65-byte decompressed value, if given as a string it must be encoded as base64
    *   `args.reason` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** If using `privilegedKey` or `privilegedSigning`, specify the reason for performing this operation
    *   `args.sequence` **([Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array) | [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))** The 32-byte sequence value for use when deriving ECDH shared secrets. Useful when deriving more than one shared secret between the same two public keys. If given as a string, must be base64 (optional, default `0`)
    *   `args.originator`  

<!---->

*   Throws **ValidationError** Invalid key, the key must be `primaryKey`, `privilegedKey`, `primarySigning`, or `privilegedSigning`
*   Throws **ValidationError** No public key provided when using ECDH with `primarySigning` or `privilegedSigning`

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)<[Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array)>** The HMAC value

## createSignature

Creates a cryptographic signature with a key belonging to the user. The SHA-256 hash is used with ECDSA.

### Parameters

*   `args` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** All parameters are passed in an object

    *   `args.data` **([Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array) | [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))** The data to sign. If string must be base64
    *   `args.key` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Specify the key tree to use. Available key trees are `primarySigning` and `privilegedSigning`
    *   `args.path` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Specify the derivation path to the key to use
    *   `args.reason` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** If using `privilegedSigning`, specify the reason for performing this operation
    *   `args.originator`  

<!---->

*   Throws **ValidationError** Invalid key, the key must be `primarySigning` or `privilegedSigning`

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)<[Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array)>** The ECDSA message signature

## getPrimarySigningPub

Returns one of the keys from the primary signing public key tree

### Parameters

*   `obj` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Parameters to this function are given in an object (optional, default `{}`)

    *   `obj.path` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The derivation path of the key to return
    *   `obj.format` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The desired [KeeTree Format](https://github.com/p2ppsr/keetree) of the returned key (optional, default `'string'`)
    *   `obj.formatOptions` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** The options for the specified [KeeTree Format](https://github.com/p2ppsr/keetree) (optional, default `{}`)
    *   `obj.originator`  

<!---->

*   Throws **InvalidStateError** Library not initialized or no user is currently authenticated

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)<([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) | CryptoKey | [Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array))>** The specified key in the specified format

## getPrivilegedSigningPub

Returns one of the keys from the privileged signing public key tree

### Parameters

*   `obj` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Parameters to this function are given in an object (optional, default `{}`)

    *   `obj.path` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The derivation path of the key to return
    *   `obj.reason` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Your reason for accessing the privileged signing public key
    *   `obj.format` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The desired [KeeTree Format](https://github.com/p2ppsr/keetree) of the returned key (optional, default `'string'`)
    *   `obj.formatOptions` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** The options for the specified [KeeTree Format](https://github.com/p2ppsr/keetree) (optional, default `{}`)
    *   `obj.originator`  

<!---->

*   Throws **InvalidStateError** Library not initialized or no user is currently authenticated

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)<([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) | CryptoKey | [Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array))>** The specified key in the specified format

## decrypt

Decrypts data with a key belonging to the user. Also allows data to be decrypted with the asymmetric keys between two users with an ECDH shared secret.

### Parameters

*   `args` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** All parameters are passed in an object (optional, default `{}`)

    *   `args.ciphertext` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) | [Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array))** The data to decrypt. If given as a string must be base64
    *   `args.key` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Specify the key tree to use. Available key trees are `primaryKey`, `privilegedKey`, `primarySigning`, and `privilegedSigning`
    *   `args.path` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Specify the path on the given key tree to use.
    *   `args.pub` **([Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array) | [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))?** If using `primarySigning` or `privilegedSigning`, specify the foreign public key to use when deriving the ECDH shared secret. The data will be decrypted with the ECDH shared secret key between the user's private key and the given public key. Must be 65-byte decompressed value, if string must be base64
    *   `args.returnType` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Specify the data type for the returned plaintext. Available types are `string` and `Uint8Array` (optional, default `string`)
    *   `args.reason` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** If using `privilegedKey` or `privilegedSigning`, specify the reason for performing this operation
    *   `args.sequence` **([Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array) | [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))** The 32-byte sequence value for use when deriving ECDH shared secrets. Useful when deriving more than one shared secret between the same two public keys. If given as a string, must be base64 (optional, default `0`)
    *   `args.originator`  

<!---->

*   Throws **ValidationError** Invalid key, the key must be `primaryKey`, `privilegedKey`, `primarySigning`, or `privilegedSigning`
*   Throws **ValidationError** No foreign public key provided when using ECDH with `primarySigning` or `privilegedSigning`

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)<([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) | [Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array))>** The decrypted plaintext

## isAuthenticated

Checks if a user is currently authenticated.

Returns **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Indicating whether a user is currently authenticated.

## isInitialized

Checks if the library is currently initialized.

Returns **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Indicating whether the library is currently initialized.

## createAction

This function allows you to broadcast transactions to the BSV blockchain. It allows inputs to be created that are signed with the keys from the signing trees, or any other custom WIF key you may provide. R-puzzle is used to achieve valid input signatures.

### Parameters

*   `obj` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** All parameters for this function are provided in an object

    *   `obj.outputs` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)<[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)>** The array of transaction outputs (amounts and scripts) that you want in the transaction. Each object contains "satoshis" and "script". (optional, default `[]`)
    *   `obj.description` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The description of the Action being taken by this transaction.
    *   `obj.senderWIF` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** The WIF string of a custom key to use for input signing. This will be used in place of keyName and keyPath if it is provided.
    *   `obj.keyName` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The name of one of the signing key trees to use for input signing (valid options are "primarySigning" and "privilegedSigning"). (optional, default `primarySigning`)
    *   `obj.keyPath` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The derivation path on the key tree for the key to use to sign inputs. (optional, default `m/1033/1`)
    *   `obj.privilegedReason` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** The message to show when asking the user to authorize the use of their privileged key.
    *   `obj.data` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)<([String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) | [Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array))>** Optional. PUSHDATA fields to include in a first zero-value OP_RETURN output. If strings are provided they must be encoded in base64. Generally you should provide a Uint8Array such as with `new TextEncoder().encode('hello')` (optional, default `[]`)
    *   `obj.labels` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)<[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>** Labels to apply to this Action. (optional, default `[]`)
    *   `obj.originator`  
    *   `obj._recursionCounter`   (optional, default `0`)
    *   `obj._lastRecursionError`  

<!---->

*   Throws **InvalidStateError** Library not initialized or no user is currently authenticated.

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)<[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)>** An object containing "txid", "rawTransaction" and "inputProofs".

## encrypt

Encrypts data with a key belonging to the user. Also allows data to be encrypted with the asymmetric keys between two users with an ECDH shared secret.

### Parameters

*   `args` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** All parameters are passed in an object (optional, default `{}`)

    *   `args.data` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) | [Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array))** The data to encrypt. If given as a string, must be base64
    *   `args.key` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Specify the key tree to use. Available key trees are `primaryKey`, `privilegedKey`, `primarySigning`, and `privilegedSigning`
    *   `args.path` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Specify the path on the given key tree to use.
    *   `args.pub` **([Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array) | [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))?** If using `primarySigning` or `privilegedSigning`, specify the foreign public key to use when deriving the ECDH shared secret. The data will be encrypted with the ECDH shared secret key between the user's private key and the given public key. Must be a 65-byte decompressed value, if given as a string then it must be base64
    *   `args.returnType` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Specify the data type for the returned ciphertext. Available types are `string` and `Uint8Array` (optional, default `string`)
    *   `args.reason` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** If using `privilegedKey` or `privilegedSigning`, specify the reason for performing this operation
    *   `args.sequence` **([Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array) | [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))** The 32-byte sequence value for use when deriving ECDH shared secrets. Useful when deriving more than one shared secret between the same two public keys. If given as a string, must be base64 (optional, default `0`)
    *   `args.originator`  

<!---->

*   Throws **ValidationError** Invalid key, the key must be `primaryKey`, `privilegedKey`, `primarySigning`, or `privilegedSigning`
*   Throws **ValidationError** No foreign public key provided when using ECDH with `primarySigning` or `privilegedSigning`

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)<([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) | [Uint8Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array))>** The encrypted ciphertext

## sendDataTransaction

\[EPRECATED] sendDataTransaction helper. Allows creating OP_RETURN Actions. You should use createAction instead.

### Parameters

*   `obj` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** All parameters for this function are provided in an object

    *   `obj.data` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)<[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>** The array of PUSHDATA fields to include in the OP_RETURN output. If the string is base64 it will be efficiently encoded to save space.
    *   `obj.reason` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Your reason for sending a transaction on behalf of this CWI user
    *   `obj.senderWIF` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** The WIF string of a custom key to use for input signing. This will be used in place of keyName and keyPath if it is provided.
    *   `obj.keyName` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The name of one of the signing key trees to use for input signing (valid options are "primarySigning" and "privilegedSigning"). (optional, default `primarySigning`)
    *   `obj.keyPath` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The derivation path on the key tree for the key to use to sign inputs (optional, default `m/1033/1`)
    *   `obj.originator`  

<!---->

*   Throws **InvalidStateError** Library not initialized or no user is currently authenticated

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)<[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>** The TXID string of the broadcasted transaction.
