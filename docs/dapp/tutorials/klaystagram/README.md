# Klaystagram

## Table of Contents <a href="#table-of-contents" id="table-of-contents"></a>

* [1. Environment Setup](1.-environment-setup.md)
* [2. Clone Klaystagram DApp](2.-clone-klaystagram-dapp.md)
* [3. Directory Structure](3.-directory-structure.md)
* [4. Write Klaystagram Smart Contract](4.-write-klaystagram-smart-contract.md)
* [5. Deploy Contract](5.-deploy-contract.md)
* [6. Frontend Code Overview](6.-frontend-code-overview.md)
* [7. FeedPage](7.-feedpage/)
  * [7-1. Connect Contract to Frontend](7.-feedpage/7-1.-connect-contract-to-frontend.md)
  * [7-2. UploadPhoto Component](7.-feedpage/7-2.-uploadphoto-component.md)
  * [7-3. Feed Component](7.-feedpage/7-3.-feed-component.md)
  * [7-4. TransferOwnership Component](7.-feedpage/7-4.-transferownership-component.md)
* [8. Run App](8.-run-app.md)

## Testing Environment <a href="#testing-environment" id="testing-environment"></a>

Klaystagram DApp is tested in the following environment.

* MacOS Mojave 10.14.5
* Node 10.16.0 (LTS)
* npm 6.9.0
* Python 2.7.10

## Introduction <a href="#introduction" id="introduction"></a>

[![Klaystagram Introduction Video](../../../bapp/tutorials/klaystagram/images/klaystagram-video-poster.png)](https://vimeo.com/327033594)

In this tutorial, we will learn how to make `Klaystagram`, a Klaytn-based NFT photo licensing application. This simple web application requires basic knowledge of Solidity, JavaScript and React.

NFT refers to a non-fungible token, which is a special type of token that represents a unique asset. As the name non-fungible implies, every single token is unique. And this uniqueness of NFT opens up new horizons of asset digitization. For example, it can be used to represent digital art, game items, or any kind of unique assets and allow people to trade them. For more information, refer to this [article](https://coincentral.com/nfts-non-fungible-tokens/).

In `Klaystagram`, every token represents users' unique pictures. When a user uploads a photo, a unique token is created containing the image data and its ownership. All transactions are recorded on the blockchain, so even service providers do not have control over the uploaded photos. Considering the purpose of this tutorial, only core functions will be implemented. After finishing this tutorial, try adding some more cool features and make your own creative service.

There are three main features.

1. **Photo upload** 사용자는 설명이 첨부된 사진을 Klaytn 블록체인에 업로드할 수 있습니다. 업로드된 사진들은 토큰화됩니다.
2. **Feed** 사용자는 블록체인에 업로드된 모든 사진을 볼 수 있습니다.
3. **Transfer ownership** 사진에 대한 소유권은 다른 사람에게 양도될 수 있고, 이러한 소유권 양도 기록은 트랜잭션을 통해 확인할 수 있습니다.

> **Source Code**\
  Complete source code can be found on GitHub at [https://github.com/klaytn/klaystagram](https://github.com/klaytn/klaystagram)

## Intended Audience <a href="#intended-audience" id="intended-audience"></a>

We will build a web application that interacts with smart contracts. To complete this tutorial, the audience is expected to be familiar with the following concepts.

* We assume that you have basic knowledge on [React](https://reactjs.org/) and [Redux](https://redux.js.org/). This course is not for absolute beginners.
* Basic knowledge and experience in [Solidity](https://solidity.readthedocs.io/en/v0.5.10/) development are recommended. However, any experienced SW developer should be able to complete the task by following the step-by-step guideline of this tutorial.
* Anyone interested in [ERC-721 Tokens](http://erc721.org/).
