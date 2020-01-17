# Unified ID

**Summary:** Unified ID is a system in which a combination of [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) mnemonics and [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) is used for multi-purpose blockchain keys and identity. The mnemonic acts as an "account" which can be used between different platforms of which different platforms can build reputation for.

# Keys


| Name  | Derivation Path   | Purpose   |
|---|---|---|
| Identity Key   | m/80'/0'/0'/0   | General purpose key, safe to hold private key in memory. Reputation is built against this key on various platforms.   |
| Wallet Key   | m/80'/0'/0'/1  | Wallet key, NOT safe to hold private key in memory. Should only be derived as required. Used to access user funds.   |

# Wallet

| Symbol  | Format   | Additional Information   |
|---|---|---|
| ATMOS   | publickey   | EOS Token, **Contract**: `nsuidcntract`, **Chain**: `0`   |
| EOS   | publickey   | **Contract**: `nsuidcntract`, **Chain**: `1`   |


### Notes for EOS and EOS Tokens
EOS based tokens make use of Novusphere's modified "pay2key" contract. The correct contract account is detailed in the "Additional Information" section. This account accepts deposits via a transfer where the memo is the public key. Withdrawals to an EOS account are done by calling `transfer` with a special `to` address of `EOS1111111111111111111111111111111114T1Anm` and the memo being `eosaccount:usermemo`, for example, `nsfoundation:hello world`. Every token in the "pay2key" contract has a chain (id) associated with it as shown in the table above.

# Novusphere pay2key Signature

### Version 3

| Index   | Type   | Description   |
|---|---|---|
| 0  | int8   | version (3)   |
| 1  | int8   | length   |
| 2  | int64   | chain   |
| 10  | bytes(33)   | from public key   |
| 43  | bytes(33)   | to public key   |
| 76  | int64   | amount   |
| 84  | int64   | fee   |
| 92  | int64   | nonce   |
| 100 | bytes(var) | memo |

Memo can be a variable length with a maximum length of 150 characters (bytes). 

After constructing the transaction buffer (`tx`), sign the sha256 hash:
```js
    const hashed = ecc.sha256(tx, 'hex');
    const sig = ecc.signHash(hashed, fromPrivateKey);
```

Nonce must increment for every transaction, a simple solution is to use the current time:
```js
const nonce = (new Date()).getTime();
```

EOS asset values can be converted into their numerical representation by multiplying by `Math.pow(10, precision)` for example, since EOS has a precision of 4 `1.1234 EOS` would be `11234`.

