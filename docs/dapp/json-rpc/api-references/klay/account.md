## klay_accountCreated <a id="klay_accountcreated"></a>

입력된 주소의 계정이 존재하는 경우 `true`를 반환합니다. 해당 주소의 계정이 존재하지 않으면 `false`를 반환합니다.

**Parameters**

| Name                 | Type                            | Description                                                                                                                                                                            |
| -------------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account              | 20-byte DATA                    | Address                                                                                                                                                                                |
| block number or hash | QUANTITY &#124; TAG &#124; HASH | Integer or hexadecimal block number, or the string `"earliest"`, `"latest"` or `"pending"` as in the [default block parameter](./block.md#the-default-block-parameter), or block hash. |

{% hint style="success" %}
NOTE: In versions earlier than Klaytn v1.7.0, only integer block number, the string `"earliest"` and `"latest"` are available.
{% endhint %}

**Return Value**

| Type    | Description                  |
| ------- | ---------------------------- |
| Boolean | 입력으로 받은 주소가 존재하는지 여부를 반환합니다. |

**Example**

```shell
// Request
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_accountCreated","params":["0xa4f42d4d2a3a13874406435500950c9bf2d783db","latest"],"id":1}' https://public-en-baobab.klaytn.net

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":true
}
```


## klay_accounts <a id="klay_accounts"></a>

Returns a list of addresses owned by client.

**Parameters**

None

**Return Value**

| Type                  | Description                    |
| --------------------- | ------------------------------ |
| Array of 20-byte DATA | Addresses owned by the client. |

**Example**

```shell
// Request
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_accounts","params":[],"id":1}' https://public-en-baobab.klaytn.net

// Result
{
  "jsonrpc": "2.0",
  "id":1,
  "result": ["0xc94770007dda54cF92009BFF0dE90c06F603a09f"]
}
```


## klay_encodeAccountKey <a id="klay_encodeaccountkey"></a>

RLP(Recursive Length Prefix)를 사용해 계정 키를 인코딩합니다.

**Parameters**

| Name    | Type      | Description                                                                                                            |
| ------- | --------- | ---------------------------------------------------------------------------------------------------------------------- |
| keytype | QUANTITY  | 계정 키 타입을 의미하는 정수값입니다. 각 계정 키 타입에 관한 자세한 정보는  [Account Key](../../../../klaytn/design/accounts.md#account-key)에서 확인하세요. |
| key     | JSON DATA | 계정 키 객체                                                                                                                |

**Return Value**

| Type | Description    |
| ---- | -------------- |
| DATA | RLP로 인코딩된 계정 키 |


**Example**

```shell
// Request to encode AccountKeyNil
curl -H "Content-Type: application/json" --data '{"jsonrpc": "2.0", "method": "klay_encodeAccountKey", "params": [{"keyType": 0, "key": {}}], "id": 66}' http://127.0.0.1:8551

// Result
{
    "id": 66,
    "jsonrpc": "2.0",
    "result": "0x80"
}
```
```shell
// Request to encode AccountKeyLegacy
curl -H "Content-Type: application/json" --data '{"jsonrpc": "2.0", "method": "klay_encodeAccountKey", "params": [{"keyType": 1, "key": {}}], "id": 30}' http://127.0.0.1:8551

// Result
{
    "id": 30,
    "jsonrpc": "2.0",
    "result": "0x01c0"
}
```
```shell
// Request to encode AccountKeyPublic
curl -H "Content-Type: application/json" --data '{"jsonrpc": "2.0", "method": "klay_encodeAccountKey", "params": [{"keyType": 2, "key": {"x": "0xdbac81e8486d68eac4e6ef9db617f7fbd79a04a3b323c982a09cdfc61f0ae0e8", "y": "0x906d7170ba349c86879fb8006134cbf57bda9db9214a90b607b6b4ab57fc026e"}}], "id": 59}' http://127.0.0.1:8551

// Result
{
    "id": 59,
    "jsonrpc": "2.0",
    "result": "0x02a102dbac81e8486d68eac4e6ef9db617f7fbd79a04a3b323c982a09cdfc61f0ae0e8"
}
```
```shell
// Request to encode AccountKeyFail
curl -H "Content-Type: application/json" --data '{"jsonrpc": "2.0", "method": "klay_encodeAccountKey", "params": [{"keyType": 3, "key": {}}], "id": 79}' http://127.0.0.1:8551

// Result
{
    "id": 79,
    "jsonrpc": "2.0",
    "result": "0x03c0"
}
```
```shell
// Request to encode AccountKeyWeightedMultiSig
curl -H "Content-Type: application/json" --data '{"jsonrpc": "2.0", "method": "klay_encodeAccountKey", "params": [{"keyType": 4, "key": {"threshold": 3, "keys": [{"weight": 1, "key": {"x": "0xc734b50ddb229be5e929fc4aa8080ae8240a802d23d3290e5e6156ce029b110e", "y": "0x61a443ac3ffff164d1fb3617875f07641014cf17af6b7dc38e429fe838763712"}}, {"weight": 1, "key": {"x": "0x12d45f1cc56fbd6cd8fc877ab63b5092ac77db907a8a42c41dad3e98d7c64dfb", "y": "0x8ef355a8d524eb444eba507f236309ce08370debaa136cb91b2f445774bff842"}}, {"weight": 1, "key": {"x": "0xea9a9f85065a00d7b9ffd3a8532a574035984587fd08107d8f4cbad6b786b0cd", "y": "0xb95ebb02d9397b4a8faceb58d485d612f0379a923ec0ddcf083378460a56acca"}}, {"weight": 1, "key": {"x": "0x8551bc489d62fa2e6f767ba87fe93a62b679fca8ff3114eb5805e6487b51e8f6", "y": "0x4206aa84bc8955fcbfcc396854228aa63ebacd81b7311a31ab9d71d90b7ec3d7"}}]}}], "id": 18}' http://127.0.0.1:8551

// Result
{
    "id": 18,
    "jsonrpc": "2.0",
    "result": "0x04f89303f890e301a102c734b50ddb229be5e929fc4aa8080ae8240a802d23d3290e5e6156ce029b110ee301a10212d45f1cc56fbd6cd8fc877ab63b5092ac77db907a8a42c41dad3e98d7c64dfbe301a102ea9a9f85065a00d7b9ffd3a8532a574035984587fd08107d8f4cbad6b786b0cde301a1038551bc489d62fa2e6f767ba87fe93a62b679fca8ff3114eb5805e6487b51e8f6"
}
```
```shell
// Request to encode AccountKeyRoleBased
curl -H "Content-Type: application/json" --data '{"jsonrpc": "2.0", "method": "klay_encodeAccountKey", "params": [{"keyType": 5, "key": [{"keyType": 2, "key": {"x": "0xe4a01407460c1c03ac0c82fd84f303a699b210c0b054f4aff72ff7dcdf01512d", "y": "0xa5735a23ce1654b14680054a993441eae7c261983a56f8e0da61280758b5919"}}, {"keyType": 4, "key": {"threshold": 2, "keys": [{"weight": 1, "key": {"x": "0xe4a01407460c1c03ac0c82fd84f303a699b210c0b054f4aff72ff7dcdf01512d", "y": "0xa5735a23ce1654b14680054a993441eae7c261983a56f8e0da61280758b5919"}}, {"weight": 1, "key": {"x": "0x36f6355f5b532c3c1606f18fa2be7a16ae200c5159c8031dd25bfa389a4c9c06", "y": "0x6fdf9fc87a16ac359e66d9761445d5ccbb417fb7757a3f5209d713824596a50d"}}]}}, {"keyType": 2, "key": {"x": "0xc8785266510368d9372badd4c7f4a94b692e82ba74e0b5e26b34558b0f081447", "y": "0x94c27901465af0a703859ab47f8ae17e54aaba453b7cde5a6a9e4a32d45d72b2"}}]}], "id": 49}' http://127.0.0.1:8551

// Result
{
    "id": 49,
    "jsonrpc": "2.0",
    "result": "0x05f898a302a103e4a01407460c1c03ac0c82fd84f303a699b210c0b054f4aff72ff7dcdf01512db84e04f84b02f848e301a103e4a01407460c1c03ac0c82fd84f303a699b210c0b054f4aff72ff7dcdf01512de301a10336f6355f5b532c3c1606f18fa2be7a16ae200c5159c8031dd25bfa389a4c9c06a302a102c8785266510368d9372badd4c7f4a94b692e82ba74e0b5e26b34558b0f081447"
}
```


## klay_decodeAccountKey <a id="klay_decodeaccountkey"></a>

RLP 인코딩된 계정 키를 디코딩합니다.

**Parameters**

| Type | Description             |
| ---- | ----------------------- |
| DATA | RLP encoded account key |

**Return Value**

| Name    | Type      | Description                                |
| ------- | --------- | ------------------------------------------ |
| keytype | QUANTITY  | Integer value indicating account key type. |
| key     | JSON DATA | Account key object                         |

**Example**

```shell
// Request
curl -H "Content-Type: application/json" --data '{"jsonrpc": "2.0", "method": "klay_decodeAccountKey", "params": ["0x05f898a302a103e4a01407460c1c03ac0c82fd84f303a699b210c0b054f4aff72ff7dcdf01512db84e04f84b02f848e301a103e4a01407460c1c03ac0c82fd84f303a699b210c0b054f4aff72ff7dcdf01512de301a10336f6355f5b532c3c1606f18fa2be7a16ae200c5159c8031dd25bfa389a4c9c06a302a102c8785266510368d9372badd4c7f4a94b692e82ba74e0b5e26b34558b0f081447"], "id": 47}' http://127.0.0.1:8551

// Result
{
    "id": 47,
    "jsonrpc": "2.0",
    "result": {
        "key": [
            {
                "key": {
                    "x": "0xe4a01407460c1c03ac0c82fd84f303a699b210c0b054f4aff72ff7dcdf01512d",
                    "y": "0xa5735a23ce1654b14680054a993441eae7c261983a56f8e0da61280758b5919"
                },
                "keyType": 2
            },
            {
                "key": {
                    "keys": [
                        {
                            "key": {
                                "x": "0xe4a01407460c1c03ac0c82fd84f303a699b210c0b054f4aff72ff7dcdf01512d",
                                "y": "0xa5735a23ce1654b14680054a993441eae7c261983a56f8e0da61280758b5919"
                            },
                            "weight": 1
                        },
                        {
                            "key": {
                                "x": "0x36f6355f5b532c3c1606f18fa2be7a16ae200c5159c8031dd25bfa389a4c9c06",
                                "y": "0x6fdf9fc87a16ac359e66d9761445d5ccbb417fb7757a3f5209d713824596a50d"
                            },
                            "weight": 1
                        }
                    ],
                    "threshold": 2
                },
                "keyType": 4
            },
            {
                "key": {
                    "x": "0xc8785266510368d9372badd4c7f4a94b692e82ba74e0b5e26b34558b0f081447",
                    "y": "0x94c27901465af0a703859ab47f8ae17e54aaba453b7cde5a6a9e4a32d45d72b2"
                },
                "keyType": 2
            }
        ],
        "keyType": 5
    }
}
```

## klay_getAccount <a id="klay_getaccount"></a>

입력으로 받은 주소의 계정 정보를 반환합니다. Klaytn에는 스마트 컨트랙트 계정과 외부 소유 계정(EOA)이 있습니다. 자세한 내용은 [Klaytn 계정](../../../../klaytn/design/accounts.md#klaytn-accounts)을 참고해주세요.

**Parameters**

| Name                 | Type                            | Description                                                                                                                                                                            |
| -------------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| address              | 20-byte DATA                    | Address                                                                                                                                                                                |
| block number or hash | QUANTITY &#124; TAG &#124; HASH | Integer or hexadecimal block number, or the string `"earliest"`, `"latest"` or `"pending"` as in the [default block parameter](./block.md#the-default-block-parameter), or block hash. |

{% hint style="success" %}
NOTE: In versions earlier than Klaytn v1.7.0, only integer block number, the string `"earliest"` and `"latest"` are available.
{% endhint %}

**Return Value**

| Type    | Description             |
| ------- | ----------------------- |
| Account | 계정 유형에 따라 다른 속성을 반환합니다. |

**Example**

```shell
// Request (Account type: Externally Owned Account)
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_getAccount","params":["0x3111a0577f322e8fb54f78d9982a26ae7ca0f722", "latest"],"id":1}' https://public-en-baobab.klaytn.net

// Result
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    accType: 1,
    account: {
      balance: 4985316100000000000,
      humanReadable: false,
      key: {
        key: {
          x: "0x230037a99462acd829f317d0ce5c8e2321ac2951de1c1b1a18f9af5cff66f0d7",
          y: "0x18a7fb1b9012d2ac87bc291cbf1b3b2339356f1ce7669ae68405389be7f8b3b6"
        },
        keyType: 2
      },
      nonce: 11
    }
  }
}
```
```shell
// Request (Account type: Smart Contract Account)
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_getAccount","params":["0x3111a0577f322e8fb54f78d9982a26ae7ca0f722", "latest"],"id":1}' https://public-en-baobab.klaytn.net

// Result
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "accType": 2,
    "account": {
      "balance": "0x0",
      "codeFormat": 0,
      "codeHash": "80NXvdOay02rYC/JgQ7RfF7yoxY1N7W8P7BiPvkIeF8=",
      "humanReadable": false,
      "key": {
        "key": {},
        "keyType": 3
      },
      "nonce": 1,
      "storageRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"
    }
  }
}
```


## klay_getAccountKey <a id="klay_getaccountkey"></a>

입력으로 받은 주소의 외부 소유 계정(EOA)에 해당하는 계정의 키를 반환합니다. 해당 계정이 AccountKeyLegacy이거나 입력으로 받은 주소의 계정이 스마트 컨트랙트 계정이면 빈 값을 반환합니다. 자세한 내용은 [계정 키](../../../../klaytn/design/accounts.md#account-key)를 참고해주세요.

**Parameters**

| Type                            | Description                                                                                                                                                                            |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 20-byte DATA                    | Address                                                                                                                                                                                |
| QUANTITY &#124; TAG &#124; HASH | Integer or hexadecimal block number, or the string `"earliest"`, `"latest"` or `"pending"` as in the [default block parameter](./block.md#the-default-block-parameter), or block hash. |

{% hint style="success" %}
NOTE: In versions earlier than Klaytn v1.7.0, only integer block number, the string `"earliest"` and `"latest"` are available.
{% endhint %}

**Return Value**

| Type       | Description                          |
| ---------- | ------------------------------------ |
| AccountKey | 계정 키는 1개 이상의 공개 키와 1가지 키 타입으로 구성됩니다. |

**Example**

```shell
// Request (AccountKey type: AccountKeyPublic)
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_getAccountKey","params":["0x3111a0577f322e8fb54f78d9982a26ae7ca0f722", "latest"],"id":1}' https://public-en-baobab.klaytn.net

// Result
{
  "jsonrpc": "2.0",
  "id":1,
  "result": {
    key: {
      x: "0x230037a99462acd829f317d0ce5c8e2321ac2951de1c1b1a18f9af5cff66f0d7",
      y: "0x18a7fb1b9012d2ac87bc291cbf1b3b2339356f1ce7669ae68405389be7f8b3b6"
    },
    keyType: 2
  }
}
```
```shell
// Request (AccountKey type: AccountKeyRoleBased)
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_getAccountKey","params":["0x68756d616e616161000000000000000000000000", "latest"],"id":1}' https://public-en-baobab.klaytn.net

// Result (AccountKeyRoleBased has nested key structure)
{
  "jsonrpc": "2.0",
  "id":1,
  "result": {
    key: [{
      key: {
        x: "0x819659d4f08e08d4bd97c6ce5ed2c2eb914201a5b3731eb9d208128df24b97dd",
        y: "0x1824267ab9e55f5a3fb1030f0299fa73fc0037305d5b1d90100e2131af41c010"
      },
      keyType: 2
    }, {
      key: {
        x: "0x73363604ca8776a2883b02046361b7eb6bd11f4fc10700ee51c525bcded134c1",
        y: "0xfc3e3cb3f4f5b709df5a2075107bc73c8618440c08456bafc44ee6f27f9e6326"
      },
      keyType: 2
    }, {
      key: {
        x: "0x95c920eb2571dff37baecdbbee32897e6e448c6725c5ab73569cc6f659684307",
        y: "0xef7839023c48acf710ad322356c12b7c5b7f475515ba7d5834f41a993f42b8f9"
      },
      keyType: 2
    }],
    keyType: 5
  }
}
```
```shell
// Request (AccountKey type: AccountKeyLegacy)
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_getAccountKey","params":["0x44711E89b0c23845b5B2ed9D3716BA42b8a3e075", "latest"],"id":1}' https://public-en-baobab.klaytn.net

// Result
{
  "jsonrpc": "2.0",
  "id":1,
  "result": {
    key: {},
    keyType: 1
  }
}
```


## klay_getBalance <a id="klay_getbalance"></a>

Returns the balance of the account of given address.

**Parameters**

| Name                 | Type                            | Description                                                                                                                                                                            |
| -------------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| address              | 20-byte DATA                    | Address to check for balance.                                                                                                                                                          |
| block number or hash | QUANTITY &#124; TAG &#124; HASH | Integer or hexadecimal block number, or the string `"earliest"`, `"latest"` or `"pending"` as in the [default block parameter](./block.md#the-default-block-parameter), or block hash. |

{% hint style="success" %}
NOTE: In versions earlier than Klaytn v1.7.0, only integer block number, the string `"earliest"` and `"latest"` are available.
{% endhint %}

**Return Value**

| Type     | Description                            |
| -------- | -------------------------------------- |
| QUANTITY | Integer of the current balance in peb. |

**Example**

```shell
// Request
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_getBalance","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f", "latest"],"id":1}' https://public-en-baobab.klaytn.net

// Result
{
  "jsonrpc": "2.0","id":1,
  "result": "0x0234c8a3397aab58" // 158972490234375000
}
```


## klay_getCode <a id="klay_getcode"></a>

Returns code at a given address.

**Parameters**

| Type                            | Description                                                                                                                                                                            |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 20-byte DATA                    | Address                                                                                                                                                                                |
| QUANTITY &#124; TAG &#124; HASH | Integer or hexadecimal block number, or the string `"earliest"`, `"latest"` or `"pending"` as in the [default block parameter](./block.md#the-default-block-parameter), or block hash. |

{% hint style="success" %}
NOTE: In versions earlier than Klaytn v1.7.0, only integer block number, the string `"earliest"` and `"latest"` are available.
{% endhint %}

**Return Value**

| Type | Description                      |
| ---- | -------------------------------- |
| DATA | The code from the given address. |

**Example**

```shell
// Request
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_getCode","params":["0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x2"],"id":1}' https://public-en-baobab.klaytn.net

// Result
{
  "jsonrpc": "2.0",
  "id":1,
  "result":   "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"
}
```


## klay_getTransactionCount <a id="klay_gettransactioncount"></a>

Returns the number of transactions *sent* from an address.

**Parameters**

| Type                            | Description                                                                                                                                                                            |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 20-byte DATA                    | Address                                                                                                                                                                                |
| QUANTITY &#124; TAG &#124; HASH | Integer or hexadecimal block number, or the string `"earliest"`, `"latest"` or `"pending"` as in the [default block parameter](./block.md#the-default-block-parameter), or block hash. |

{% hint style="success" %}
NOTE: In versions earlier than Klaytn v1.7.0, only integer block number, the string `"earliest"` and `"latest"` are available.
{% endhint %}

**Return Value**

| Type     | Description                                                   |
| -------- | ------------------------------------------------------------- |
| QUANTITY | Integer of the number of transactions send from this address. |

**Example**

 ```shell
// Request
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_getTransactionCount","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f","latest"],"id":1}' https://public-en-baobab.klaytn.net

// Result
{
  "jsonrpc": "2.0",
  "id":1,
  "result": "0x1" // 1
}
 ```




## klay_isContractAccount <a id="klay_iscontractaccount"></a>

특정 번호의 블록 시간에서 입력으로 받은 계정의 codeHash가 비어 있지 않은 경우 `true`를 반환합니다. 해당 계정이 EOA이거나 codeHash가 비어 있는 스마트 컨트랙트 계정이면 `false`를 반환합니다.

**Parameters**

| Name                 | Type                            | Description                                                                                                                                                                            |
| -------------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account              | 20-byte DATA                    | Address                                                                                                                                                                                |
| block number or hash | QUANTITY &#124; TAG &#124; HASH | Integer or hexadecimal block number, or the string `"earliest"`, `"latest"` or `"pending"` as in the [default block parameter](./block.md#the-default-block-parameter), or block hash. |

{% hint style="success" %}
NOTE: In versions earlier than Klaytn v1.7.0, only integer block number, the string `"earliest"` and `"latest"` are available.
{% endhint %}

**Return Value**

| Type    | Description                                   |
| ------- | --------------------------------------------- |
| Boolean | 리턴값이 `true`이면 매개변수가 기존에 있는 스마트 컨트랙트 계정 주소입니다. |

**Example**

```shell
// Request
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_isContractAccount","params":["0x2f07d5b3fa1051460099dc9ea0c2975b6ea67776", "latest"],"id":1}' https://public-en-baobab.klaytn.net

// Result
{
    "jsonrpc":"2.0",
    "id":1,
    "result":true
}
```

## klay_sign <a id="klay_sign"></a>

The sign method calculates a Klaytn-specific signature with:
```
sign(keccak256("\x19Klaytn Signed Message:\n" + len(message) + message)))
```

Adding a prefix to the message makes the calculated signature recognizable as a Klaytn-specific signature. This prevents misuse where a malicious dApp can sign arbitrary data, *e.g.*, transaction, and use the signature to impersonate the victim.

**NOTE**: The address to sign with must be unlocked.

**Parameters**

| Name    | Type         | Description     |
| ------- | ------------ | --------------- |
| account | 20-byte DATA | Address         |
| message | N-byte DATA  | Message to sign |

**Return Value**

| Type | Description |
| ---- | ----------- |
| DATA | Signature   |

**Example**

```shell
// Request
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_sign","params":["0x9b2055d370f73ec7d8a03e965129118dc8f5bf83", "0xdeadbeaf"],"id":1}' https://public-en-baobab.klaytn.net

// Result
{
  "jsonrpc": "2.0",
  "id":1,
  "result": "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"
}
```
