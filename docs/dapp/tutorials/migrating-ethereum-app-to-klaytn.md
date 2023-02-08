# Migrating Ethereum App to Klaytn

## Table of Contents <a href="#table-of-contents" id="table-of-contents"></a>

* [1. Introduction](migrating-ethereum-app-to-klaytn.md#1-introduction)
* [2. Klaytn의 이더리움과의 호환성](migrating-ethereum-app-to-klaytn.md#2-klaytn-has-ethereum-compatibility)
* [3. 이더리움에서 Klaytn으로 노드 연결 변경](migrating-ethereum-app-to-klaytn.md#3-change-node-connection-from-ethereum-to-klaytn)
* [4. Klaytn 노드와의 상호작용: `BlockNumber` 컴포넌트](migrating-ethereum-app-to-klaytn.md#4-interact-with-klaytn-node-blocknumber-component)
* [5. 컨트랙트와의 상호작용: `Count` 컴포넌트](migrating-ethereum-app-to-klaytn.md#5-interact-with-the-contract-count-component)
  * [5-1. Klaytn에 Count 컨트랙트 배포](migrating-ethereum-app-to-klaytn.md#5-1-deploy-count-contract-on-klaytn)
  * [5-2. 컨트랙트 인스턴스 생성](migrating-ethereum-app-to-klaytn.md#5-2-create-a-contract-instance)
  * [5-3. Interact with contract](migrating-ethereum-app-to-klaytn.md#5-3-interact-with-contract)

## 1. Introduction <a href="#1-introduction" id="1-introduction"></a>

본 튜토리얼은 이더리움 애플리케이션에서 Klaytn으로의 이전에 대한 가이드를 제공합니다. Klaytn 사용 경험은 없어도 괜찮습니다. 간단한 블록체인 애플리케이션을 통해 어떻게 이더리움 애플리케이션에서 Klaytn으로 이전하는지 보여드리도록 하겠습니다.

여기서는 이더리움 애플리케이션에서 Klaytn으로 이전하는 데에 필요한 코드 수정만을 중점적으로 다룰 것입니다. Klaytn dApp을 만드는 것에 대한 자세한 내용은 [CountDApp 튜토리얼](count-dapp/)을 참고하세요.

> **소스 코드** 전체 소스 코드는 GitHub에서 확인할 수 있습니다. [https://github.com/klaytn/countbapp](https://github.com/klaytn/countbapp)

#### 튜토리얼 대상 <a href="#intended-audience" id="intended-audience"></a>

* 본 튜토리얼은 [React](https://reactjs.org/)에 대한 기본 지식이 있다고 가정하고 진행합니다. 샘플 코드는 리액트로 작성되어 있습니다.
* 블록체인 애플리케이션에 대한 기본적인 지식과 경험이 필요하지만 Klaytn에 대한 사용 경험은 필요치 않습니다.

#### 테스트 환경 <a href="#testing-environment" id="testing-environment"></a>

CountDApp은 다음의 환경에서 테스트 되었습니다.

* MacOS Mojave 10.14.5
* Node 10.16.0 (LTS)
* npm 6.9.0
* Python 2.7.10

## 2. Klaytn has Ethereum compatibility <a href="#2-klaytn-has-ethereum-compatibility" id="2-klaytn-has-ethereum-compatibility"></a>

Klaytn의 런타임 환경은 이더리움 가상머신과 호환되어 솔리디티로 작성된 스마트 컨트랙트를 실행할 수 있습니다. Klaytn의 RPC API 및 기타 클라이언트 라이브러리들은 가능한 한 거의 동일하게 이더리움과 동일한 API 사양을 유지하고 있습니다. 따라서 이더리움 애플리케이션에서 Klaytn으로 이전하는 것은 매우 간단합니다. 이러한 점들은 개발자들이 새로운 블록체인 플랫폼으로 쉽게 옮길 수 있도록 합니다.

## 3. Change node connection from Ethereum to Klaytn <a href="#3-change-node-connection-from-ethereum-to-klaytn" id="3-change-node-connection-from-ethereum-to-klaytn"></a>

우선 노드에 연결하는 라이브러리를 변경해야 합니다. 그리고 'rpcURL'에 노드 URL을 지정합니다. (참고: [이더리움의 Ropsten 테스트넷은 Q4 2022에 종료됩니다.](https://blog.ethereum.org/2022/06/21/testnet-deprecation) )

* Ethereum
  * `web3` 라이브러리는 이더리움 노드에 연결하고 통신합니다.
  * `Ropsten testnet` URL이 'rpcURL'에 할당되어 있습니다.
* Klaytn
  * `caver-js` 라이브러리는 Klaytn 노드에 연결하고 통신합니다.
  * `Baobab testnet` URL이 'rpcURL'에 할당되어 있습니다.

`src/klaytn/caver.js`

```javascript
// import Web3 from 'web3'
import Caver from 'caver-js'

// const ROPSTEN_TESTNET_RPC_URL = 'https://ropsten.infura.io/'
const BAOBAB_TESTNET_RPC_URL = 'https://api.baobab.klaytn.net:8651/'

// const rpcURL = ROPSTEN_TESTNET_RPC_URL
const rpcURL = BAOBAB_TESTNET_RPC_URL

// const web3 = new Web3(rpcURL)
const caver = new Caver(rpcURL)

// export default web3
export default caver
```

## 4. Interact with Klaytn node: `BlockNumber` component <a href="#4-interact-with-klaytn-node-blocknumber-component" id="4-interact-with-klaytn-node-blocknumber-component"></a>

![blocknumber 컴포넌트](../../bapp/tutorials/count-bapp/images/blocknumber-component.gif)

BlockNumber 컴포넌트는 1초(1000ms)마다 현재 블록 번호를 가져옵니다.

간단히 `web3` 라이브러리를 `caver-js`로 대체하여 이더리움 블록 번호 대신 Klaytn의 블록 번호를 실시간 동기화할 수 있습니다.

> Ethereum: [`web3.eth.getBlockNumber()`](https://web3js.readthedocs.io/en/v1.2.1/web3-eth.html#getblocknumber)\
  Klaytn: [`caver.klay.getBlockNumber()`](../sdk/caver-js/v1.4.1/api-references/caver.klay/block.md#getblocknumber)

```js
// import web3 from 'ethereum/web3'
import caver from 'klaytn/caver'

class BlockNumber extends Component {
  state = { currentBlockNumber: '...loading' }

  getBlockNumber = async () => {
    // const blockNumber = await web3.eth.getBlockNumber()
    const blockNumber = await caver.klay.getBlockNumber()

    this.setState({ currentBlockNumber: blockNumber })
  }
  // ...
}

export default BlockNumber
```

For more detail about `BlockNumber` component, see [CountDApp tutorial - Blocknumber Component](count-dapp/5.-frontend-code-overview/5-1.-blocknumber-component.md).

## 5. Interact with the contract: `Count` component <a href="#5-interact-with-the-contract-count-component" id="5-interact-with-the-contract-count-component"></a>

![count 컴포넌트](../../bapp/tutorials/count-bapp/images/count-component.gif)

스마트 컨트랙트와 상호작용하기 위해서는 배포된 컨트랙트의 인스턴스를 생성해야 합니다. 생성한 인스턴스를 통해 컨트랙트의 데이터를 읽어오거나 컨트랙트에 데이터를 쓸 수 있습니다.

Let's learn step by step how to migrate `CountDApp` from Ethereum to Klaytn!

* 5-1. Klaytn에 `Count` 컨트랙트 배포
* 5-2. Create a contract instance
* 5-3. Interact with contract

### 5-1. Deploy `Count` contract on Klaytn <a href="#5-1-deploy-count-contract-on-klaytn" id="5-1-deploy-count-contract-on-klaytn"></a>

첫 번째 단계는 Count 컨트랙트를 Klaytn에 배포하고 컨트랙트 주소를 가져오는 것입니다. 대부분의 경우 Klaytn에서 이더리움 컨트랙트를 수정 없이 사용할 수 있습니다. 자세한 내용은 [이더리움 컨트랙트 포팅](../../smart-contract/porting-ethereum-contract.md)을 참고하세요. 이 가이드에서는 트러플을 사용하여 컨트랙트를 배포하겠습니다.

1. `truffle-config.js`의 네트워크 속성을 변경하여 Klaytn에 컨트랙트를 배포합니다.
2. Top up your account using [KLAY faucet](https://baobab.wallet.klaytn.foundation/access?next=faucet).
3. `$ truffle deploy --network baobab --reset`를 입력하세요.
4. `Count` 컨트랙트는 Klaytn의 Baobab 테스트넷에 배포됩니다.

`truffle-config.js`

```js
// const HDWalletProvider = require("truffle-hdwallet-provider")
const HDWalletProvider = require("truffle-hdwallet-provider-klaytn")

// const NETWORK_ID = '3' // Ethereum, Ropsten testnet's network id
const NETWORK_ID = '1001' // Klaytn, Baobab testnet's network id

// const RPC_URL = 'https://ropsten.infura.io/'
const RPC_URL = 'https://api.baobab.klaytn.net:8651'

// Change it to your own private key that has enough KLAY to deploy contract
const PRIVATE_KEY = '0x3de0c90ce7e440f19eff6439390c29389f611725422b79c95f9f48c856b58277'


module.exports = {
  networks: {
    /* ropsten: {
      provider: () => new HDWalletProvider(PRIVATE_KEY, RPC_URL),
      network_id: NETWORK_ID,
      gas: '8500000',
      gasPrice: null,
    }, */

    baobab: {
      provider: () => new HDWalletProvider(PRIVATE_KEY, RPC_URL),
      network_id: NETWORK_ID,
      gas: '8500000',
      gasPrice: null,
    },
  },
  compilers: {
    solc: {
      version: '0.5.6',
    },
  },
}
```

For more details about deploying contracts, See [CountDapp tutorial - Deploy Contract](count-dapp/6.-deploy-contract.md).

### 5-2. Create a contract instance <a href="#5-2-create-a-contract-instance" id="5-2-create-a-contract-instance"></a>

`caver-js` API를 사용하여 컨트랙트 인스턴스를 생성할 수 있습니다. 컨트랙트 인스턴스는 `Count` 컨트랙트와의 연결을 생성합니다. 즉 이 인스턴스를 통해 컨트랙트 메서드를 호출할 수 있습니다.

> Ethereum : [`web3.eth.Contract(ABI, address)`](https://web3js.readthedocs.io/en/v1.2.1/web3-eth-contract.html#new-contract)\
  Klaytn : [`caver.klay.Contract(ABI, address)`](../sdk/caver-js/v1.4.1/api-references/caver.klay.Contract.md#new-contract)

`src/components/Count.js`

```javascript
// import web3 from 'ethereum/web3'
import caver from 'klaytn/caver'

class Count extends Component {
  constructor() {
    /* const CountContract = DEPLOYED_ABI
      && DEPLOYED_ADDRESS
      && new web3.eth.Contract(DEPLOYED_ABI, DEPLOYED_ADDRESS) */

    this.countContract = DEPLOYED_ABI
      && DEPLOYED_ADDRESS
      && new cav.klay.Contract(DEPLOYED_ABI, DEPLOYED_ADDRESS)
  }

  // ...
}
export default Count
```

### 5-3. Interact with contract <a href="#5-3-interact-with-contract" id="5-3-interact-with-contract"></a>

The `ABI` (Application Binary Interface) used to create the Count contract instance allows the `caver-js` to invoke contract's methods as below. 자바스크립트 객체처럼 Count 컨트랙트와 상호작용할 수 있습니다.

* Read data (call)\
`CountContract.methods.count().call()`
* Write data (send)\
`CountContract.methods.plus().send({ ... })`\
`CountContract.methods.minus().send({ ... })`

이전 단계에서처럼 컨트랙트 인스턴스를 생성하면, 컨트랙트 메서드를 사용하여 코드를 수정할 필요가 없습니다. dApp migration has been completed!

#### 전체 코드: `Count` 컴포넌트 <a href="#full-code-count-component" id="full-code-count-component"></a>

`src/components/Count.js`

```js
import React, { Component } from 'react'
import cx from 'classnames'

import caver from 'klaytn/caver'

import './Count.scss'

class Count extends Component {
  constructor() {
    super()
    // ** 1. 컨트랙트 인스턴스 생성 **
    // 예시: new caver.klay.Contract(DEPLOYED_ABI, DEPLOYED_ADDRESS)
    // 이 인스턴스를 통해 컨트랙트 메서드를 호출할 수 있습니다.
    // Now you can access the instance by `this.countContract` variable.
    this.countContract = DEPLOYED_ABI
      && DEPLOYED_ADDRESS
      && new caver.klay.Contract(DEPLOYED_ABI, DEPLOYED_ADDRESS)
    this.state = {
      count: '',
      lastParticipant: '',
      isSetting: false,
    }
  }

  intervalId = null

  getCount = async () => {
    // ** 2. Call contract method (CALL) **
    // ex:) this.countContract.methods.methodName(arguments).call()
    // You can call contract method (CALL) like above.
    // For example, your contract has a method called `count`.
    // You can call it like below:
    // ex:) this.countContract.methods.count().call()
    // It returns promise, so you can access it by .then() or, use async-await.
    const count = await this.countContract.methods.count().call()
    const lastParticipant = await this.countContract.methods.lastParticipant().call()
    this.setState({
      count,
      lastParticipant,
    })
  }

  setPlus = () => {
    const walletInstance = caver.klay.accounts.wallet && caver.klay.accounts.wallet[0]

    // 컨트랙트 메서드 호출을 위해 지갑을 연동해야 합니다.
    if (!walletInstance) return

    this.setState({ settingDirection: 'plus' })

    // 3. ** Call contract method (SEND) **
    // ex:) this.countContract.methods.methodName(arguments).send(txObject)
    // You can call contract method (SEND) like above.
    // For example, your contract has a method called `plus`.
    // You can call it like below:
    // ex:) this.countContract.methods.plus().send({
    //   from: '0x952A8dD075fdc0876d48fC26a389b53331C34585', // PUT YOUR ADDRESS
    //   gas: '200000',
    // })
    this.countContract.methods.plus().send({
      from: walletInstance.address,
      gas: '200000',
    })
      .once('transactionHash', (txHash) => {
        console.log(`
          Sending a transaction... (Call contract's function 'plus')
          txHash: ${txHash}
          `
        )
      })
      .once('receipt', (receipt) => {
        console.log(`
          Received receipt! It means your transaction(calling plus function)
          is in klaytn block(#${receipt.blockNumber})
        `, receipt)
        this.setState({
          settingDirection: null,
          txHash: receipt.transactionHash,
        })
      })
      .once('error', (error) => {
        alert(error.message)
        this.setState({ settingDirection: null })
      })
  }

  setMinus = () => {
    const walletInstance = caver.klay.accounts.wallet && caver.klay.accounts.wallet[0]

    // Need to integrate wallet for calling contract method.
    if (!walletInstance) return

    this.setState({ settingDirection: 'minus' })

    // 3. ** Call contract method (SEND) **
    // ex:) this.countContract.methods.methodName(arguments).send(txObject)
    // You can call contract method (SEND) like above.
    // For example, your contract has a method called `minus`.
    // You can call it like below:
    // ex:) this.countContract.methods.minus().send({
    //   from: '0x952A8dD075fdc0876d48fC26a389b53331C34585', // PUT YOUR ADDRESS
    //   gas: '200000',
    // })

    // It returns event emitter, so after sending, you can listen on event.
    // Use .on('transactionHash') event,
    // : if you want to handle logic after sending transaction.
    // Use .once('receipt') event,
    // : if you want to handle logic after your transaction is put into block.
    // ex:) .once('receipt', (data) => {
    //   console.log(data)
    // })
    this.countContract.methods.minus().send({
      from: walletInstance.address,
      gas: '200000',
    })
      .once('transactionHash', (txHash) => {
        console.log(`
          Sending a transaction... (Call contract's function 'minus')
          txHash: ${txHash}
          `
        )
      })
      .once('receipt', (receipt) => {
        console.log(`
          Received receipt which means your transaction(calling minus function)
          is in klaytn block(#${receipt.blockNumber})
        `, receipt)
        this.setState({
          settingDirection: null,
          txHash: receipt.transactionHash,
        })
      })
      .once('error', (error) => {
        alert(error.message)
        this.setState({ settingDirection: null })
      })
  }

  componentDidMount() {
    this.intervalId = setInterval(this.getCount, 1000)
  }

  componentWillUnmount() {
    clearInterval(this.intervalId)
  }

  render() {
    const { lastParticipant, count, settingDirection, txHash } = this.state
    return (
      <div className="Count">
        {Number(lastParticipant) !== 0 && (
          <div className="Count__lastParticipant">
            last participant: {lastParticipant}
          </div>
        )}
        <div className="Count__count">COUNT: {count}</div>
        <button
          onClick={this.setPlus}
          className={cx('Count__button', {
            'Count__button--setting': settingDirection === 'plus',
          })}
        >
          +
        </button>
        <button
          onClick={this.setMinus}
          className={cx('Count__button', {
            'Count__button--setting': settingDirection === 'minus',
          })}
        >
          -
        </button>
        {txHash && (
          <div className="Count__lastTransaction">
            <p className="Count__lastTransactionMessage">
              You can check your last transaction in klaytnscope:
            </p>
            <a
              target="_blank"
              href={`https://scope.klaytn.com/transaction/${txHash}`}
              className="Count__lastTransactionLink"
            >
              {txHash}
            </a>
          </div>
        )}
      </div>
    )
  }
}

export default Count
```
