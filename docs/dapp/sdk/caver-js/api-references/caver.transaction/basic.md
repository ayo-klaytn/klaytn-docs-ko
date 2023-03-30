# Basic type transaction class <a id="basic-type-transaction-class"></a>

## 레거시 트랜잭션<a id="legacytransaction"></a>

```javascript
caver.transaction.legacyTransaction.create(transactionObject)
```

`LegacyTransaction`은 [레거시 트랜잭션](../../../../../klaytn/design/transactions/basic.md#txtypelegacytransaction)을 의미합니다. [Klaytn 계정](../../../../../klaytn/design/accounts.md#klaytn-accounts)은 [AccountKeyLegacy][]을 사용해야만 `LegacyTransaction`을 실행할 수 있습니다. `transactionObject`는 `LegacyTransaction`을 생성하기 위해 아래와 같은 속성들을 가질 수 있습니다.

`LegacyTransaction`는 멤버변수로서 아래와 같은 속성들을 가지고 있습니다. `선택 사항`인 속성들은 사용자가 `LegacyTransaction`를 생성할 때 `transactionObject`에 선택적으로 주어질 수 있는 속성들을 뜻합니다.

{% hint style="success" %}
NOTE: You can create an instance of `LegacyTransaction` from RLP-encoded strings. Please refer to the below example. NOTE: `caver.transaction.legacyTransaction.create` is supported since caver-js [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

NOTE: As of caver-js [v1.8.1-rc.4](https://www.npmjs.com/package/caver-js/v/1.8.1-rc.4), creating transactions is only supported using the `create` function. If you've been creating transactions using a constructor like `new caver.transaction.legacyTransaction({...})`, please change it to `caver.transaction.legacyTransaction.create({...})`.
{% endhint %}

**properties**

| Name       | Type   | Description                                                                                                                       |
| ---------- | ------ | --------------------------------------------------------------------------------------------------------------------------------- |
| gas        | string | 트랜잭션에서 사용하도록 허락된 최대 트랜잭션 수수료입니다.                                                                                                  |
| value      | string | (선택 사항, default: `'0x0'`) peb로 나타낸 전송될 KLAY 양입니다. `caver.utils.toPeb`를 사용할 수 있습니다.                                                |
| from       | string | (선택 사항) 발신자의 주소입니다. 미입력시 서명에 사용되는 키링 주소로 설정됩니다.                                                                                   |
| to         | string | (선택 사항, default: `'0x'`) 전송받는 계정 주소, 또는 기본 트랜잭션이 스마트 컨트랙트를 실행하는 경우 스마트 컨트랙트 주소입니다. 기본 트랜잭션이 스마트 컨트랙트를 배포하는 경우 `to`는 정의될 필요가 없습니다. |
| input      | string | (선택 사항) 스마트 컨트랙트의 배포와 실행에 사용되는 트랜잭션에 첨부된 데이터입니다.                                                                                  |
| signatures | Array  | (선택 사항) 서명의 배열입니다. 기본 트랜잭션은 하나의 서명만 받을 수 있습니다.                                                                                    |
| nonce      | string | (선택 사항) 발신자의 트랜잭션을 고유하게 식별하기 위해 사용되는 값입니다. 미입력시 논스 설정을 위해 `caver.rpc.klay.getTransactionCount(address, 'pending')`가 사용됩니다.        |
| gasPrice   | string | (선택 사항) 발신자가 지불할 토큰양을 구하기 위한 승수입니다. 미입력시 gasPrice 설정을 위해 `caver.rpc.klay.getGasPrice`가 사용됩니다.                                     |
| chainId    | string | (선택 사항) Klaytn 네트워크의 체인 ID입니다. 미입력시 chainId 설정을 위해 `caver.rpc.klay.getChainId`가 사용됩니다.                                            |

**Example**

```javascript
// KLAY 전송을 위한 legacyTransaction 생성
> caver.transaction.legacyTransaction.create({
    to: '0x9957dfd92e4b70f91131c573293343bc5f21f215',
    value: caver.utils.toPeb(1, 'KLAY'),
    gas: 25000,
})

// 스마트 컨트랙트 배포를 위한 legacyTransaction 생성
> caver.transaction.legacyTransaction.create({
    input: '0x60806...',
    gas: 200000,
})

// 스마트 컨트랙트 실행을 위한 legacyTransaction 생성
> caver.transaction.legacyTransaction.create({
    to: '0xfe6c9118e56a42cbc77aa3b7ee586455e3dc5b6d', // Smart contact address
    input: '0xa9059...',
    gas: 200000,
})

// RLP-인코딩 문자열에서 부터 legacyTransaction 생성
> caver.transaction.legacyTransaction.create('0xf8668204d219830f4240947b65b75d204abed71587c9e519a89277766ee1d00a843132333425a0b2a5a15550ec298dc7dddde3774429ed75f864c82caeb5ee24399649ad731be9a029da1014d16f2011b3307f7bbe1035b6e699a4204fc416c763def6cefd976567')
LegacyTransaction {
    _type: 'TxTypeLegacyTransaction',
    _from: '0x',
    _gas: '0xf4240',
    _nonce: '0x4d2',
    _gasPrice: '0x19',
    _signatures: SignatureData { _v: '0x25', _r: '0xb2a5a...', _s:  '0x29da1...' },
    _to: '0x7b65b75d204abed71587c9e519a89277766ee1d0',
    _input: '0x31323334',
    _value: '0xa'
}
```

## ValueTransfer <a id="valuetransfer"></a>

```javascript
caver.transaction.valueTransfer.create(transactionObject)
```

`ValueTransfer`는 [KLAY 전송 트랜잭션](../../../../../klaytn/design/transactions/basic.md#txtypevaluetransfer)을 나타냅니다. `transactionObject`는 `ValueTransfer`를 생성하기 위해 아래와 같은 속성들을 가질 수 있습니다.

`ValueTransfer`는 멤버변수로서 아래와 같은 속성들을 가지고 있습니다. `선택 사항`인 속성들은 사용자가 ` ValueTransfer` 트랜잭션을 생성할 때 `transactionObject`에 선택적으로 주어질 수 있는 속성들을 뜻합니다.

{% hint style="success" %}
NOTE: You can create an instance of `ValueTransfer` from RLP-encoded strings. Please refer to the below example. NOTE: `caver.transaction.valueTransfer.create` is supported since caver-js [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

NOTE: As of caver-js [v1.8.1-rc.4](https://www.npmjs.com/package/caver-js/v/1.8.1-rc.4), creating transactions is only supported using the `create` function. If you've been creating transactions using a constructor like `new caver.transaction.valueTransfer({...})`, please change it to `caver.transaction.valueTransfer.create({...})`.
{% endhint %}

**properties**

| Name       | Type   | Description                                                                                                                                                          |
| ---------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| value      | string | peb로 나타낸 전송될 KLAY 양입니다. You can use `caver.utils.toPeb`.                                                                                                             |
| from       | string | 발신자의 주소입니다.                                                                                                                                                          |
| to         | string | 전송되는 KLAY를 받을 계정 주소입니다.                                                                                                                                              |
| gas        | string | The maximum amount of transaction fee the transaction is allowed to use.                                                                                             |
| signatures | Array  | (optional) An array of signatures.                                                                                                                                   |
| nonce      | string | (optional) A value used to uniquely identify a sender’s transaction. If omitted, `caver.rpc.klay.getTransactionCount(address, 'pending')` will be used to set nonce. |
| gasPrice   | string | (optional) A multiplier to get how much the sender will pay in tokens. If omitted, `caver.rpc.klay.getGasPrice` will be used to set gasPrice.                        |
| chainId    | string | (optional) The chain id of the Klaytn network. If omitted, `caver.rpc.klay.getChainId` will be used to set chainId.                                                  |

**Example**

```javascript
// valueTransfer 생성
> caver.transaction.valueTransfer.create({
    from: '0x{address in hex}',
    to: '0x9957dfd92e4b70f91131c573293343bc5f21f215',
    value: caver.utils.toPeb(1, 'KLAY'),
    gas: 25000,
})

// RLP-인코딩 문자열에서 valueTransfer 생성
> caver.transaction.valueTransfer.create('0x08f87f3a8505d21dba0083015f90948723590d5d60e35f7ce0db5c09d3938b26ff80ae01947d0104ac150f749d36bb34999bcade9f2c0bd2e6f847f845820feaa03d820b27d0997baf16f98df01c7b2b2e9734ad05b2228c4d403c2facff8397f3a01f4a44eeb8b7f0b0019162d1d6b90c401078e56fcd7495e74f7cfcd37e25f017')
ValueTransfer {
    _type: 'TxTypeValueTransfer',
    _from: '0x7d0104ac150f749d36bb34999bcade9f2c0bd2e6',
    _gas: '0x15f90',
    _nonce: '0x3a',
    _gasPrice: '0x5d21dba00',
    _signatures: [ SignatureData { _v: '0x0fea', _r: '0x3d820...', _s: '0x1f4a4...' } ],
    _to: '0x8723590d5d60e35f7ce0db5c09d3938b26ff80ae',
    _value: '0x1'
}
```

## ValueTransferMemo <a id="valuetransfermemo"></a>

```javascript
caver.transaction.valueTransferMemo.create(transactionObject)
```

`ValueTransferMemo`는 [KLAY 전송 메모 트랜잭션](../../../../../klaytn/design/transactions/basic.md#txtypevaluetransfermemo)을 나타냅니다. `transactionObject`는 ` ValueTransferMemo`를 생성하기 위해 아래와 같은 속성들을 가질 수 있습니다.

` ValueTransferMemo`는 멤버변수로서 아래와 같은 속성들을 가지고 있습니다. `선택 사항`인 속성들은 사용자가 ` ValueTransferMemo` 트랜잭션을 생성할 때 `transactionObject`에 선택적으로 제공될 수 있는 속성들을 뜻합니다.

{% hint style="success" %}
NOTE: You can create an instance of `ValueTransferMemo` from RLP-encoded strings. 아래 예시를 참고해주세요. NOTE: `caver.transaction.valueTransferMemo.create` is supported since caver-js [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

NOTE: As of caver-js [v1.8.1-rc.4](https://www.npmjs.com/package/caver-js/v/1.8.1-rc.4), creating transactions is only supported using the `create` function. If you've been creating transactions using a constructor like `new caver.transaction.valueTransferMemo({...})`, please change it to `caver.transaction.valueTransferMemo.create({...})`.
{% endhint %}

**properties**

| Name       | Type   | Description                                                                                                                                                          |
| ---------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| value      | string | The amount of KLAY in peb to be transferred. You can use `caver.utils.toPeb`.                                                                                        |
| from       | string | The address of the sender.                                                                                                                                           |
| to         | string | The account address that will receive the transferred value.                                                                                                         |
| input      | string | 트랜잭션에 첨부되는 데이터입니다. 메시지는 이 속성으로 전달되어야 합니다.                                                                                                                            |
| gas        | string | The maximum amount of transaction fee the transaction is allowed to use.                                                                                             |
| signatures | Array  | (optional) An array of signatures.                                                                                                                                   |
| nonce      | string | (optional) A value used to uniquely identify a sender’s transaction. If omitted, `caver.rpc.klay.getTransactionCount(address, 'pending')` will be used to set nonce. |
| gasPrice   | string | (optional) A multiplier to get how much the sender will pay in tokens. If omitted, `caver.rpc.klay.getGasPrice` will be used to set gasPrice.                        |
| chainId    | string | (optional) The chain id of the Klaytn network. If omitted, `caver.rpc.klay.getChainId` will be used to set chainId.                                                  |

**Example**

```javascript
// valueTransferMemo 생성
> caver.transaction.valueTransferMemo.create({
    from: '0x{address in hex}',
    to: '0x9957dfd92e4b70f91131c573293343bc5f21f215',
    value: caver.utils.toPeb(1, 'KLAY'),
    gas: 25000,
    input: '0x68656c6c6f',
})

// RLP-인코딩 문자열에서 valueTransferMemo 생성
> caver.transaction.valueTransferMemo.create('0x10f8808204d219830f4240947b65b75d204abed71587c9e519a89277766ee1d00a94a94f5374fce5edbc8e2a8697c15331677e6ebf0b8568656c6c6ff845f84325a07d2b0c89ee8afa502b3186413983bfe9a31c5776f4f820210cffe44a7d568d1ca02b1cbd587c73b0f54969f6b76ef2fd95cea0c1bb79256a75df9da696278509f3')
ValueTransferMemo {
    _type: 'TxTypeValueTransferMemo',
    _from: '0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b',
    _gas: '0xf4240',
    _nonce: '0x4d2',
    _gasPrice: '0x19',
    _signatures: [ SignatureData { _v: '0x25', _r: '0x7d2b0...', _s: '0x2b1cb...' } ],
    _to: '0x7b65b75d204abed71587c9e519a89277766ee1d0',
    _value: '0xa',
    _input: '0x68656c6c6f'
}
```

## AccountUpdate <a id="accountupdate"></a>

```javascript
caver.transaction.accountUpdate.create(transactionObject)
```

`AccountUpdate`는 [계정 업데이트 트랜잭션](../../../../../klaytn/design/transactions/basic.md#txtypeaccountupdate)을 나타냅니다. `transactionObject`는 `AccountUpdate`를 생성하기 위해 아래와 같은 속성들을 가질 수 있습니다.

`AccountUpdate`는 멤버변수로서 아래와 같은 속성들을 가지고 있습니다. `선택 사항`인 속성들은 사용자가 `AccountUpdate` 트랜잭션을 생성할 때 `transactionObject`에 선택적으로 주어질 수 있는 속성들을 뜻합니다.


{% hint style="success" %}
NOTE: You can create an instance of `AccountUpdate` from RLP-encoded strings. Please refer to the below example. NOTE: `caver.transaction.accountUpdate.create` is supported since caver-js [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

NOTE: As of caver-js [v1.8.1-rc.4](https://www.npmjs.com/package/caver-js/v/1.8.1-rc.4), creating transactions is only supported using the `create` function. If you've been creating transactions using a constructor like `new caver.transaction.accountUpdate({...})`, please change it to `caver.transaction.accountUpdate.create({...})`.
{% endhint %}

**properties**

| Name       | Type        | Description                                                                                                                                                          |
| ---------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| from       | string      | The address of the sender.                                                                                                                                           |
| account    | [Account][] | 계정 업데이트를 위해 필요한 정보를 포함하고 있는 [계정][] 인스턴스입니다.                                                                                                                          |
| gas        | string      | The maximum amount of transaction fee the transaction is allowed to use.                                                                                             |
| signatures | Array       | (optional) An array of signatures.                                                                                                                                   |
| nonce      | string      | (optional) A value used to uniquely identify a sender’s transaction. If omitted, `caver.rpc.klay.getTransactionCount(address, 'pending')` will be used to set nonce. |
| gasPrice   | string      | (optional) A multiplier to get how much the sender will pay in tokens. If omitted, `caver.rpc.klay.getGasPrice` will be used to set gasPrice.                        |
| chainId    | string      | (optional) The chain id of the Klaytn network. If omitted, `caver.rpc.klay.getChainId` will be used to set chainId.                                                  |

각 `AccountKey`에 대해 [계정][] 인스턴스를 생성하는 방법은 [Getting Started - Account Update](../../getting-started.md#account-update) 또는 [caver.account.create](../caver.account.md#caver-account-create)를 참고하세요.

**Example**

```javascript
// accountUpdate 생성
> caver.transaction.accountUpdate.create({
    from: '0x{address in hex}',
    gas: 50000,
    account: caver.account.createWithAccountKeyLegacy('0x{address in hex}'),
})

// RLP-인코딩 문자열에서 accountUpdate 생성
> caver.transaction.accountUpdate.create('0x20f88d808505d21dba0083030d4094ffb52bc54635f840013e142ebe7c06c9c91c1625a302a102c93fcbdb2b9dbef8ee5c4748ffdce11f1f5b06d7ba71cc2b7699e38be7698d1ef847f845820fe9a09c2ca281e94567846acbeef724b1a7a5f882d581aff9984755abd92272592b8ea0344fd23d7774ae9c227809bb579387dfcd69e74ae2fe3a788617f54a4001e5ab')
AccountUpdate {
    _type: 'TxTypeAccountUpdate',
    _from: '0xffb52bc54635f840013e142ebe7c06c9c91c1625',
    _gas: '0x30d40',
    _nonce: '0x0',
    _gasPrice: '0x5d21dba00',
    _signatures: [ SignatureData { _v: '0x0fe9', _r: '0x9c2ca...', _s: '0x344fd...' } ],
    _account: Account {
        _address: '0xffb52bc54635f840013e142ebe7c06c9c91c1625',
        _accountKey: AccountKeyPublic { _publicKey: '0x02c93...' }
    }
}
```

## SmartContractDeploy <a id="smartcontractdeploy"></a>

```javascript
caver.transaction.smartContractDeploy.create(transactionObject)
```

`SmartContractDeploy`는 [스마트 컨트랙트 배포 트랜잭션](../../../../../klaytn/design/transactions/basic.md#txtypesmartcontractdeploy)을 나타냅니다. `transactionObject`는 `SmartContractDeploy`를 생성하기 위해 아래와 같은 속성들을 가질 수 있습니다.

`SmartContractDeploy`는 멤버변수로서 아래와 같은 속성들을 가지고 있습니다. `선택 사항`인 속성들은 사용자가 `SmartContractDeploy` 트랜잭션을 생성할 때 `transactionObject`에 선택적으로 주어질 수 있는 속성들을 뜻합니다.

{% hint style="success" %}
NOTE: You can create an instance of `SmartContractDeploy` from RLP-encoded strings. Please refer to the below example. NOTE: `caver.transaction.smartContractDeploy.create` is supported since caver-js [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

NOTE: As of caver-js [v1.8.1-rc.4](https://www.npmjs.com/package/caver-js/v/1.8.1-rc.4), creating transactions is only supported using the `create` function. If you've been creating transactions using a constructor like `new caver.transaction.smartContractDeploy({...})`, please change it to `caver.transaction.smartContractDeploy.create({...})`.
{% endhint %}

**properties**

| Name          | Type    | Description                                                                                                                                                          |
| ------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| from          | string  | The address of the sender.                                                                                                                                           |
| input         | string  | Data attached to the transaction. 배포될 스마트 컨트랙트의 바이트 코드와 그 아규먼트들입니다. [caver.abi.encodeContractDeploy](../caver.abi.md#encodecontractdeploy)를 통해 얻을 수 있습니다.            |
| gas           | string  | The maximum amount of transaction fee the transaction is allowed to use.                                                                                             |
| value         | string  | (선택 사항, default: `'0x0'`) 컨트랙트가 초기화되었을 때 스마트 컨트랙트 주소에 전송 및 저장될 peb으로 나타낸 KLAY 양입니다. You can use `caver.utils.toPeb`.                                                 |
| to            | string  | (선택 사항, default: `'0x'`) 스마트 컨트랙트가 배포되는 주소입니다. 현재는 이 값이 정의되지 않습니다. 특정 주소를 지정하는 기능은 향후 지원될 예정입니다.                                                                     |
| humanReadable | boolean | (선택 사항, default: `false`) 인간이 읽을 수 있는 형식의 주소는 아직 지원되지 않기 때문에 이 값은 false입니다.                                                                                          |
| codeFormat    | string  | (선택 사항, default: `"EVM"`) 스마트 컨트랙트의 코드 포맷입니다. 현재는 오직 EVM만 지원됩니다. 이 값은 할당 후 내부적으로 16진수로 변환됩니다(e.g. `EVM`이 converted to `0x0`).                                        |
| signatures    | Array   | (optional) An array of signatures.                                                                                                                                   |
| nonce         | string  | (optional) A value used to uniquely identify a sender’s transaction. If omitted, `caver.rpc.klay.getTransactionCount(address, 'pending')` will be used to set nonce. |
| gasPrice      | string  | (optional) A multiplier to get how much the sender will pay in tokens. If omitted, `caver.rpc.klay.getGasPrice` will be used to set gasPrice.                        |
| chainId       | string  | (optional) The chain id of the Klaytn network. If omitted, `caver.rpc.klay.getChainId` will be used to set chainId.                                                  |

**Example**

```javascript
// smartContractDeploy 생성
> caver.transaction.smartContractDeploy.create({
    from: '0x{address in hex}',
    input: '0x60806...',
    gas: 100000,
})

// RLP-인코딩 문자열에서 smartContractDeploy 생성
> caver.transaction.smartContractDeploy.create('0x28f9027e1f8505d21dba00830dbba0808094d91aec35bea25d379e49cfe2dff5f5775cdac1a3b9020e60806040526000805534801561001457600080fd5b506101ea806100246000396000f30060806040526004361061006d576000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806306661abd1461007257806342cbb15c1461009d578063767800de146100c8578063b22636271461011f578063d14e62b814610150575b600080fd5b34801561007e57600080fd5b5061008761017d565b6040518082815260200191505060405180910390f35b3480156100a957600080fd5b506100b2610183565b6040518082815260200191505060405180910390f35b3480156100d457600080fd5b506100dd61018b565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34801561012b57600080fd5b5061014e60048036038101908080356000191690602001909291905050506101b1565b005b34801561015c57600080fd5b5061017b600480360381019080803590602001909291905050506101b4565b005b60005481565b600043905090565b600160009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1681565b50565b80600081905550505600a165627a7a7230582053c65686a3571c517e2cf4f741d842e5ee6aa665c96ce70f46f9a594794f11eb00298080f847f845820fe9a0018a9f680a74e275f1f83a5c2c45e1313c52432df4595e944240b1511a4f4ba7a02d762c3417f91b81db4907db832cb28cc64df7dca3ea9be64899ab3f4812f016')
SmartContractDeploy {
    _type: 'TxTypeSmartContractDeploy',
    _from: '0xd91aec35bea25d379e49cfe2dff5f5775cdac1a3',
    _gas: '0xdbba0',
    _nonce: '0x1f',
    _gasPrice: '0x5d21dba00',
    _signatures: [ SignatureData { _v: '0x0fe9', _r: '0x018a9...', _s: '0x2d762...' } ],
    _to: '0x',
    _value: '0x0',
    _input: '0x60806...',
    _humanReadable: false,
    _codeFormat: '0x0'
}
```

## SmartContractExecution <a id="smartcontractexecution"></a>

```javascript
caver.transaction.smartContractExecution.create(transactionObject)
```

`SmartContractExecution`는 [스마트 컨트랙트 실행 트랜잭션](../../../../../klaytn/design/transactions/basic.md#txtypesmartcontractexecution)을 나타냅니다. `transactionObject`는 `SmartContractExecution`를 생성하기 위해 아래와 같은 속성들을 가질 수 있습니다.

`SmartContractExecution`은 멤버변수로서 아래와 같은 속성들을 가지고 있습니다. `선택 사항`인 속성들은 사용자가 `SmartContractExecution` 트랜잭션을 생성할 때 `transactionObject`에 선택적으로 주어질 수 있는 속성들을 뜻합니다.

{% hint style="success" %}
NOTE: You can create an instance of `SmartContractExecution` from RLP-encoded strings. Please refer to the below example. NOTE: `caver.transaction.smartContractExecution.create` is supported since caver-js [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

NOTE: As of caver-js [v1.8.1-rc.4](https://www.npmjs.com/package/caver-js/v/1.8.1-rc.4), creating transactions is only supported using the `create` function. If you've been creating transactions using a constructor like `new caver.transaction.smartContractExecution({...})`, please change it to `caver.transaction.smartContractExecution.create({...})`.
{% endhint %}

**properties**

| Name       | Type   | Description                                                                                                                                                          |
| ---------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| from       | string | The address of the sender.                                                                                                                                           |
| to         | string | 실행할 스마트 컨트랙트 계정의 주소입니다.                                                                                                                                              |
| input      | string | 트랜잭션 실행에 이용되며 트랜잭션에 첨부되는 데이터입니다. 호출할 함수와 전달될 파라미터들을 나타내는 엔코딩된 문자열입니다. [caver.abi.encodeFunctionCall](../caver.abi.md#encodefunctioncall)를 통해 얻을 수 있습니다.              |
| gas        | string | The maximum amount of transaction fee the transaction is allowed to use.                                                                                             |
| value      | string | (optional, default: `'0x0'`) The amount of KLAY in peb to be transferred. You can use `caver.utils.toPeb`.                                                           |
| signatures | Array  | (optional) An array of signatures.                                                                                                                                   |
| nonce      | string | (optional) A value used to uniquely identify a sender’s transaction. If omitted, `caver.rpc.klay.getTransactionCount(address, 'pending')` will be used to set nonce. |
| gasPrice   | string | (optional) A multiplier to get how much the sender will pay in tokens. If omitted, `caver.rpc.klay.getGasPrice` will be used to set gasPrice.                        |
| chainId    | string | (optional) The chain id of the Klaytn network. If omitted, `caver.rpc.klay.getChainId` will be used to set chainId.                                                  |

**Example**

```javascript
// smartContractExecution 생성
> caver.transaction.smartContractExecution.create({
    from: '0x{address in hex}',
    to: '0x{address in hex}',
    input: '0xa9059...',
    gas: 90000,
})

// RLP-인코딩 문자열에서 smartContractExecution 생성
> caver.transaction.smartContractExecution.create('0x30f8c5038505d21dba00830dbba094e3cd4e1cd287235cc0ea48c9fd02978533f5ec2b80946b604e77c0fbebb5b2941bcde3ab5eb09d99ad24b844a9059cbb0000000000000000000000008a4c9c443bb0645df646a2d5bb55def0ed1e885a0000000000000000000000000000000000000000000000000000000000003039f847f845820feaa066e1650b5779f152489633f343581c07938f8b2fc92c919d4dd7c7295d0beacea067b0b79383dbcd42a3aa8ebb1aa4bcb1fc0623ef9e97bc1e9b82d96fe37b5881')
SmartContractExecution {
    _type: 'TxTypeSmartContractExecution',
    _from: '0x6b604e77c0fbebb5b2941bcde3ab5eb09d99ad24',
    _gas: '0xdbba0',
    _nonce: '0x3',
    _gasPrice: '0x5d21dba00',
    _signatures: [ SignatureData { _v: '0x0fea', _r: '0x66e16...', _s: '0x67b0b...' } ],
    _to: '0xe3cd4e1cd287235cc0ea48c9fd02978533f5ec2b',
    _value: '0x0',
    _input: '0xa9059...'
}
```

## Cancel <a id="cancel"></a>

```javascript
caver.transaction.cancel.create(transactionObject)
```

`LegacyTransaction`은 [취소 트랜잭션](../../../../../klaytn/design/transactions/basic.md#txtypecancel)을 의미합니다. `transactionObject`는 `Cancel` 트랜잭션을 생성하기 위해 아래와 같은 속성들을 가질 수 있습니다.

`Cancel`은 트랜잭션 풀에서 같은 논스를 가진 트랜잭션 실행을 취소합니다.

`Cancel`는 멤버변수로서 아래와 같은 속성들을 가지고 있습니다. `선택 사항`인 속성들은 사용자가 `Cancel` 트랜잭션을 생성할 때 `transactionObject`에 선택적으로 주어질 수 있는 속성들을 뜻합니다.

{% hint style="success" %}
NOTE: You can create an instance of `Cancel` from RLP-encoded strings. Please refer to the below example. NOTE: `caver.transaction.cancel.create` is supported since caver-js [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

NOTE: As of caver-js [v1.8.1-rc.4](https://www.npmjs.com/package/caver-js/v/1.8.1-rc.4), creating transactions is only supported using the `create` function. If you've been creating transactions using a constructor like `new caver.transaction.xcancelxx({...})`, please change it to `caver.transaction.cancel.create({...})`.
{% endhint %}

**properties**

| Name       | Type   | Description                                                                                                                                                          |
| ---------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| from       | string | The address of the sender.                                                                                                                                           |
| gas        | string | The maximum amount of transaction fee the transaction is allowed to use.                                                                                             |
| nonce      | string | (optional) A value used to uniquely identify a sender’s transaction. If omitted, `caver.rpc.klay.getTransactionCount(address, 'pending')` will be used to set nonce. |
| signatures | Array  | (optional) An array of signatures.                                                                                                                                   |
| gasPrice   | string | (optional) A multiplier to get how much the sender will pay in tokens. If omitted, `caver.rpc.klay.getGasPrice` will be used to set gasPrice.                        |
| chainId    | string | (optional) The chain id of the Klaytn network. If omitted, `caver.rpc.klay.getChainId` will be used to set chainId.                                                  |

**Example**

```javascript
// cancel 생성
> caver.transaction.cancel.create({
    from: '0x{address in hex}',
    nonce: 1,
    gas: 25000,
})

// RLP-인코딩 문자열에서 cancel 생성
> caver.transaction.cancel.create('0x38f869068505d21dba00830dbba0946b604e77c0fbebb5b2941bcde3ab5eb09d99ad24f847f845820feaa0d9994ef507951a59380309f656ee8ed685becdc89b1d1a0eb1d2f72683ae14d3a07ad5d37a89781f294fab72b254ea9266e4d039ae163db4a4c4752f1fabff023b')
Cancel {
    _type: 'TxTypeCancel',
    _from: '0x6b604e77c0fbebb5b2941bcde3ab5eb09d99ad24',
    _gas: '0xdbba0',
    _nonce: '0x6',
    _gasPrice: '0x5d21dba00',
    _signatures: [ SignatureData { _v: '0x0fea', _r: '0xd9994...', _s: '0x7ad5d...' } ]
}
```

## ChainDataAnchoring<a id="chaindataanchoring"></a>

```javascript
caver.transaction.chainDataAnchoring.create(transactionObject)
```

`ChainDataAnchoring`은 [체인 데이터 앵커링 트랜잭션](../../../../../klaytn/design/transactions/basic.md#txtypechaindataanchoring)을 나타냅니다. `transactionObject`는 `ChainDataAnchoring` 트랜잭션을 생성하기 위해 아래와 같은 속성들을 가질 수 있습니다.

`ChainDataAnchoring`은 멤버변수로서 아래와 같은 속성들을 가지고 있습니다. `선택 사항`인 속성들은 사용자가 `ChainDataAnchoring` 트랜잭션을 생성할 때 `transactionObject`에 선택적으로 주어질 수 있는 속성들을 뜻합니다.

{% hint style="success" %}
NOTE: You can create an instance of `ChainDataAnchoring` from RLP-encoded strings. Please refer to the below example. NOTE: `caver.transaction.chainDataAnchoring.create` is supported since caver-js [v1.6.1](https://www.npmjs.com/package/caver-js/v/1.6.1).

NOTE: As of caver-js [v1.8.1-rc.4](https://www.npmjs.com/package/caver-js/v/1.8.1-rc.4), creating transactions is only supported using the `create` function. If you've been creating transactions using a constructor like `new caver.transaction.chainDataAnchoring({...})`, please change it to `caver.transaction.chainDataAnchoring.create({...})`.
{% endhint %}

**properties**

| Name       | Type   | Description                                                                                                                                                          |
| ---------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| from       | string | The address of the sender.                                                                                                                                           |
| input      | string | 서비스체인의 데이터입니다.                                                                                                                                                       |
| gas        | string | The maximum amount of transaction fee the transaction is allowed to use.                                                                                             |
| nonce      | string | (optional) A value used to uniquely identify a sender’s transaction. If omitted, `caver.rpc.klay.getTransactionCount(address, 'pending')` will be used to set nonce. |
| signatures | Array  | (optional) An array of signatures.                                                                                                                                   |
| gasPrice   | string | (optional) A multiplier to get how much the sender will pay in tokens. If omitted, `caver.rpc.klay.getGasPrice` will be used to set gasPrice.                        |
| chainId    | string | (optional) The chain id of the Klaytn network. If omitted, `caver.rpc.klay.getChainId` will be used to set chainId.                                                  |

**Example**

```javascript
// chainDataAnchoring 생성
> caver.transaction.chainDataAnchoring.create({
    from: '0x{address in hex}',
    gas: 50000,
    input: '0xf8a6a...',
})

// RLP-인코딩 문자열에서 chainDataAnchoring 생성
> caver.transaction.chainDataAnchoring.create('0x48f9010e8204d219830f424094a94f5374fce5edbc8e2a8697c15331677e6ebf0bb8a8f8a6a00000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000001a00000000000000000000000000000000000000000000000000000000000000002a00000000000000000000000000000000000000000000000000000000000000003a0000000000000000000000000000000000000000000000000000000000000000405f845f84325a0e58b9abf9f33a066b998fccaca711553fb4df425c9234bbb3577f9d9775bb124a02c409a6c5d92277c0a812dd0cc553d7fe1d652a807274c3786df3292cd473e09')
ChainDataAnchoring {
    _type: 'TxTypeChainDataAnchoring',
    _from: '0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b',
    _gas: '0xf4240',
    _nonce: '0x4d2',
    _gasPrice: '0x19',
    _signatures: [ SignatureData { _v: '0x25', _r: '0xe58b9...', _s: '0x2c409...' } ],
    _input: '0xf8a6a...'
}
```
## EthereumAccessList <a id="ethereumaccesslist"></a>

```javascript
caver.transaction.ethereumAccessList.create(transactionObject)
```

`EthereumAccessList` represents an [Ethereum access list transaction](../../../../../klaytn/design/transactions/basic.md#txtypeethereumaccesslist). A [Klaytn account](../../../../../klaytn/design/accounts.md#klaytn-accounts) can execute a `EthereumAccessList` only with [AccountKeyLegacy][]. The `transactionObject` can have properties below to create a `EthereumAccessList`.

`EthereumAccessList` has the properties below as its member variables. Properties marked as `optional` refer to properties that can be optionally given in `transactionObject` when the user creates `EthereumAccessList`.

{% hint style="success" %}
NOTE: You can create an instance of `EthereumAccessList` from RLP-encoded strings. Please refer to the below example. NOTE: `caver.transaction.ethereumAccessList` is supported since caver-js [v1.8.0](https://www.npmjs.com/package/caver-js/v/1.8.0).

NOTE: As of caver-js [v1.8.1-rc.4](https://www.npmjs.com/package/caver-js/v/1.8.1-rc.4), creating transactions is only supported using the `create` function. If you've been creating transactions using a constructor like `new caver.transaction.ethereumAccessList({...})`, please change it to `caver.transaction.ethereumAccessList.create({...})`.
{% endhint %}

**properties**

| Name       | Type   | Description                                                                                                                                                                                                                                                                        |
| ---------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| gas        | string | The maximum amount of transaction fee the transaction is allowed to use.                                                                                                                                                                                                           |
| value      | string | (optional, default: `'0x0'`) The amount of KLAY in peb to be transferred. You can use `caver.utils.toPeb`.                                                                                                                                                                         |
| from       | string | (optional) The address of the sender. If omitted, the keyring address used for signing will be set.                                                                                                                                                                                |
| to         | string | (optional, default: `'0x'`) The account address that will receive the transferred value or smart contact address if an ethereum access list transaction execute smart contract. If an ethereum access list transaction deploys a smart contract, `to` does not need to be defined. |
| input      | string | (optional) Data attached to the transaction, used for smart contract deployment/execution.                                                                                                                                                                                         |
| signatures | Array  | (optional) An array of signatures. An ethereum access list transaction can have only one signature.                                                                                                                                                                                |
| nonce      | string | (optional) A value used to uniquely identify a sender’s transaction. If omitted, `caver.rpc.klay.getTransactionCount(address, 'pending')` will be used to set nonce.                                                                                                               |
| gasPrice   | string | (optional) A multiplier to get how much the sender will pay in tokens. If omitted, `caver.rpc.klay.getGasPrice` will be used to set gasPrice.                                                                                                                                      |
| chainId    | string | (optional) The chain id of the Klaytn network. If omitted, `caver.rpc.klay.getChainId` will be used to set chainId.                                                                                                                                                                |
| accessList | Array  | (optional) As an EIP-2930 access list that contains all storage slots and addresses read and written by the transaction.                                                                                                                                                           |

**Example**

```javascript
> caver.transaction.ethereumAccessList.create({
    to: '0x9957dfd92e4b70f91131c573293343bc5f21f215',
    value: caver.utils.toPeb(1, 'KLAY'),
    gas: 40000,
    accessList: [
        {
            address: '0x5430192ae264b3feff967fc08982b9c6f5694023',
            storageKeys: [
                '0x0000000000000000000000000000000000000000000000000000000000000003',
                '0x0000000000000000000000000000000000000000000000000000000000000007',
            ],
        },
    ]
})

> caver.transaction.ethereumAccessList.create('0x7801f90109822710238505d21dba00829c4094c5fb1386b60160614a8151dcd4b0ae41325d1cb801b844a9059cbb0000000000000000000000008a4c9c443bb0645df646a2d5bb55def0ed1e885a0000000000000000000000000000000000000000000000000000000000003039f85bf859945430192ae264b3feff967fc08982b9c6f5694023f842a00000000000000000000000000000000000000000000000000000000000000003a0000000000000000000000000000000000000000000000000000000000000000701a05ac25e47591243af2d6b8e7f54d608e9e0e0aeb5194d34c17852bd7e376f4857a0095a40394f33e95cce9695d5badf4270f4cc8aff0b5395cefc3a0fe213be1f30')
EthereumAccessList {
  _type: 'TxTypeEthereumAccessList',
  _from: '0x0000000000000000000000000000000000000000',
  _gas: '0x9c40',
  _nonce: '0x23',
  _chainId: '0x2710',
  _signatures: SignatureData {
    _v: '0x01',
    _r: '0x5ac25e47591243af2d6b8e7f54d608e9e0e0aeb5194d34c17852bd7e376f4857',
    _s: '0x095a40394f33e95cce9695d5badf4270f4cc8aff0b5395cefc3a0fe213be1f30'
  },
  _to: '0xc5fb1386b60160614a8151dcd4b0ae41325d1cb8',
  _input: '0xa9059cbb0000000000000000000000008a4c9c443bb0645df646a2d5bb55def0ed1e885a0000000000000000000000000000000000000000000000000000000000003039',
  _value: '0x1',
  _accessList: AccessList(0) [],
  _gasPrice: '0x5d21dba00'
}
```

## EthereumDynamicFee <a id="ethereumdynamicfee"></a>

```javascript
caver.transaction.ethereumDynamicFee.create(transactionObject)
```

`EthereumDynamicFee` represents an [Ethereum dynamic fee transaction](../../../../../klaytn/design/transactions/basic.md#txtypeethereumdynamicfee). A [Klaytn account](../../../../../klaytn/design/accounts.md#klaytn-accounts) can execute a `EthereumDynamicFee` only with [AccountKeyLegacy][]. The `transactionObject` can have properties below to create a `EthereumDynamicFee`.

`EthereumDynamicFee` has the properties below as its member variables. Properties marked as `optional` refer to properties that can be optionally given in `transactionObject` when the user creates `EthereumDynamicFee`. And note that `EthereumDynamicFee` does not use `gasPrice`, it uses `maxPriorityFeePerGas` and `maxFeePerGas`.

{% hint style="success" %}
NOTE: You can create an instance of `EthereumDynamicFee` from RLP-encoded strings. Please refer to the below example. NOTE: `caver.transaction.ethereumDynamicFee` is supported since caver-js [v1.8.0](https://www.npmjs.com/package/caver-js/v/1.8.0).

NOTE: As of caver-js [v1.8.1-rc.4](https://www.npmjs.com/package/caver-js/v/1.8.1-rc.4), creating transactions is only supported using the `create` function. If you've been creating transactions using a constructor like `new caver.transaction.ethereumDynamicFee({...})`, please change it to `caver.transaction.ethereumDynamicFee.create({...})`.
{% endhint %}

**properties**

| Name                 | Type   | Description                                                                                                                                                                                                                                                                               |
| -------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| gas                  | string | The maximum amount of transaction fee the transaction is allowed to use.                                                                                                                                                                                                                  |
| value                | string | (optional, default: `'0x0'`) The amount of KLAY in peb to be transferred. You can use `caver.utils.toPeb`.                                                                                                                                                                                |
| from                 | string | (optional) The address of the sender. If omitted, it will be set to the keyring address used for signing.                                                                                                                                                                                 |
| to                   | string | (optional, default: `'0x'`) The account address that will receive the transferred value or smart contact address when an ethereum dynamic fee transaction executes a smart contract. When an ethereum dynamic fee transaction deploys a smart contract, `to` does not need to be defined. |
| input                | string | (optional) Data attached to the transaction, used for smart contract deployment/execution.                                                                                                                                                                                                |
| signatures           | Array  | (optional) An array of signatures. An ethereum dynamic fee transaction can have only one signature.                                                                                                                                                                                       |
| nonce                | string | (optional) A value used to uniquely identify a sender’s transaction. If omitted, it will be set to `caver.rpc.klay.getTransactionCount(address, 'pending')`.                                                                                                                              |
| maxPriorityFeePerGas | string | (optional) Gas tip cap for the transaction in peb. Since Klaytn has a fixed gas price, it should be set to the same value as `caver.rpc.klay.getGasPrice`. If omitted, it will be set to `caver.rpc.klay.getMaxPriorityFeePerGas()`.                                                      |
| maxFeePerGas         | string | (optional) A maximum amount to pay for the transaction to execute. Since Klaytn has a fixed gas price, it should be set to the same value as `caver.rpc.klay.getGasPrice`. If omitted, the value of `baseFeePerGas * 2 + maxPriorityFeePerGas` is set to `maxFeePerGas`.                  |
| chainId              | string | (optional) The chain id of the Klaytn network. If omitted, it will be set to `caver.rpc.klay.getChainId`.                                                                                                                                                                                 |
| accessList           | Array  | (optional) As an EIP-2930 access list that contains all storage slots and addresses read and written by the transaction.                                                                                                                                                                  |

**Example**

```javascript
> caver.transaction.ethereumDynamicFee.create({
    to: '0x9957dfd92e4b70f91131c573293343bc5f21f215',
    value: caver.utils.toPeb(1, 'KLAY'),
    gas: 50000,
    accessList: [
        {
            address: '0x5430192ae264b3feff967fc08982b9c6f5694023',
            storageKeys: [
                '0x0000000000000000000000000000000000000000000000000000000000000003',
                '0x0000000000000000000000000000000000000000000000000000000000000007',
            ],
        },
    ]
})

> caver.transaction.ethereumDynamicFee.create('0x7802f9010f822710258505d21dba008505d21dba00829c40941fc92c23f71a7de4cdb4394a37fc636986a0f48401b844a9059cbb0000000000000000000000008a4c9c443bb0645df646a2d5bb55def0ed1e885a0000000000000000000000000000000000000000000000000000000000003039f85bf8599467116062f1626f7b3019631f03d301b8f701f709f842a00000000000000000000000000000000000000000000000000000000000000003a0000000000000000000000000000000000000000000000000000000000000000780a04fc52da183020a27dc4b684a45404445630e946b0c1a37edeb538d4bdae63040a07d56dbcc61f42ffcbced105f838d20b8fe71e85a4d0344c7f60815fddfeae4cc')
EthereumDynamicFee {
  _type: 'TxTypeEthereumDynamicFee',
  _from: '0x0000000000000000000000000000000000000000',
  _gas: '0x9c40',
  _nonce: '0x25',
  _chainId: '0x2710',
  _signatures: SignatureData {
    _v: '0x',
    _r: '0x4fc52da183020a27dc4b684a45404445630e946b0c1a37edeb538d4bdae63040',
    _s: '0x7d56dbcc61f42ffcbced105f838d20b8fe71e85a4d0344c7f60815fddfeae4cc'
  },
  _to: '0x1fc92c23f71a7de4cdb4394a37fc636986a0f484',
  _input: '0xa9059cbb0000000000000000000000008a4c9c443bb0645df646a2d5bb55def0ed1e885a0000000000000000000000000000000000000000000000000000000000003039',
  _value: '0x1',
  _accessList: AccessList(0) [],
  _maxPriorityFeePerGas: '0x5d21dba00',
  _maxFeePerGas: '0x5d21dba00'
}
```

[AccountKeyLegacy]: ../../../../../klaytn/design/accounts.md#accountkeylegacy
[Account]: ../caver.account.md#account
[계정]: ../caver.account.md#account
