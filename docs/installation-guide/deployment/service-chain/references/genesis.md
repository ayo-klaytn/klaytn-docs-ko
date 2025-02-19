이 페이지는 `genesis.json`의 세부 사항을 설명합니다.

# Genesis JSON 파일 구조 <a id="genesis-json-file-structure"></a>

`genesis.json` 파일 구조는 다음 표에 설명되어 있습니다.

| Field Name | Description                                                                                   |
| ---------- | --------------------------------------------------------------------------------------------- |
| config     | 블록 체인 환경설정. [Config](#config) 장을 확인하세요.                                                       |
| nonce      | (더 이상 사용되지 않음) 이 필드는 이더리움으로부터 파생되었지만, Klaytn에서는 사용되지 않습니다.                                    |
| timestamp  | 블록이 생성된 유닉스 시간.                                                                               |
| extraData  | 서명자 베니티(vanity) 그리고 검증자 리스트, 제안자 씰(seal) 및 커밋 씰을 포함하는 RLP-인코딩된 이스탄불 추가 데이터를 위한 서명자 데이터 통합 필드. |
| gasLimit   | 블록에 사용된 최대 가스량.                                                                               |
| difficulty | (deprecated) This field is derived from the Ethereum, but not used in Klaytn.                 |
| mixhash    | (deprecated) This field is derived from the Ethereum, but not used in Klaytn.                 |
| coinbase   | 채굴자가 보상을 받는 주소. 이 필드는 Clique 컨센서스 엔진에서만 사용됩니다.                                                |
| alloc      | 사전 정의된 계정.                                                                                    |
| number     | 블록 번호 필드.                                                                                     |
| gasUsed    | 블록에서 사용된 가스량.                                                                                 |
| parentHash | 이전 블록의 해시값.                                                                                   |

## Config <a id="config"></a>

`config` 필드는 체인과 관련된 정보를 저장합니다.

| Field Name              | Description                                  |
| ----------------------- | -------------------------------------------- |
| chainId                 | 현재 체인을 식별하고 리플레이 공격을 방지하는 데 사용됩니다.           |
| istanbulCompatibleBlock | 이스탄불 업그레이드가 적용될 블록 번호.                       |
| istanbul, clique        | 합의 엔진의 유형.                                   |
| unitPrice               | 단가.                                          |
| deriveShaImpl           | 트랜잭션 해시 및 영수증 해시를 생성하는 방법을 정의합니다.            |
| governance              | 네트워크의 거버넌스 정보. [거버넌스](#governance) 장을 참조하세요. |


## extraData <a id="extradata"></a>

`extraData` 필드는 제안자 베니티와 RLP-인코딩된 이스탄불 추가 데이터의 통합입니다:

  - 제안자 베니티는 임의의 제안자 베니티 데이터를 포함하는 32바이트 데이터입니다.
  - 나머지 데이터는 다음을 포함하는 RLP-인코딩된 이스탄불 추가 데이터입니다:
     - 검증자: 오름차순의 검증자 리스트.
     - 씰: 헤더의 제안자 서명. `genesis.json`의 경우, 65개의 `0x0`로 초기화된 바이트 배열입니다.
     - 커밋된 씰: 합의 증명으로서의 커밋 서명 씰의 리스트. `genesis.json`의 경우, 빈 배열입니다.

**Example**
| Field         | Type           | Value                                                                                   |
| ------------- | -------------- | --------------------------------------------------------------------------------------- |
| Vanity        | 32바이트 16진수 문자열 | 0x0000000000000000000000000000000000000000000000000000000000000000                      |
| Validators    | []address      | [0x48009b4e20ec72aadf306577cbe2eaf54b0ebb16,0x089fcc42fd83baeee4831319375413b8bae3aceb] |
| Seal          | 65개 원소의 바이트 배열 | [0x0,...,0x0]                                                                           |
| CommittedSeal | [][]byte       | []                                                                                      |

위의 데이터에서 `extraData`는 다음으로부터 생성됩니다
```
concat('0x',Vanity,RLPEncode({Validators,Seal,CommittedSeal}))
```
여기서 `concat`은 문자열 연결 함수, `RLPEncode`는 주어진 구조체를 RLP-인코딩된 문자열로 변환하는 함수입니다.

이 함수로부터 예제의 출력 `extraData`는 0x0000000000000000000000000000000000000000000000000000000000000000f86fea9448009b4e20ec72aadf306577cbe2eaf54b0ebb1694089fcc42fd83baeee4831319375413b8bae3acebb8410000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c0입니다.


# 합의 엔진 <a id="consensus-engine"></a>

클레이튼 네트워크에 사용 가능한 합의 엔진은 Clique와 Istanbul입니다. 각 엔진의 설명은 다음과 같습니다.

## Clique <a id="clique"></a>

`clique` 필드는 Proof-Of-Authority(POA) 기반 씰링을 위한 환경설정을 저장합니다.

| 필드     | Description                 |
| ------ | --------------------------- |
| period | 연속된 블록 사이의 최소 시간 간격(단위: 초). |
| epoch  | 투표를 재설정하고 체크포인트로 표시될 블록의 수  |

## Istanbul <a id="istanbul"></a>

`istanbul` 필드는 이스탄불 기반 씰링을 위한 환경설정을 저장합니다.

| Fields | Description                                |
| ------ | ------------------------------------------ |
| epoch  | 체크포인트가 되기 위해 투표를 재설정할 블록 수.                |
| policy | 블록 제안자 선출 정책. [0: 라운드 로빈, 1: 고정, 2: 가중 랜덤] |
| sub    | 위원회 규모.                                    |

# Governance <a id="governance"></a>

`governance` 필드는 네트워크를 위한 거버넌스 정보를 저장합니다.

| Fields         | Description                                                             |
| -------------- | ----------------------------------------------------------------------- |
| governanceMode | 세 가지 거버넌스 모드 중 하나입니다. `none`, `single`, `ballot` 등 세 가지 모드 중 하나를 선택합니다. |
| governingNode  | Designated governing node's address. 거버넌스 모드가 `single` 인 경우에만 작동합니다.    |
| reward         | 보상 환경설정을 저장합니다. [Reward](#reward) 장을 확인하세요.                             |

## Reward <a id="reward"></a>

`reward` 필드는 네트워크의 토큰 이코노미에 대한 정보를 저장합니다.

| Fields                 | Description                                                           |
| ---------------------- | --------------------------------------------------------------------- |
| mintingAmount          | 블록이 생성될 때 발행되는 peb의 양. Double quotation marks are needed for a value. |
| ratio                  | `/`으로 구분되는 `CN/KIR/PoC`의 분포율 모든 값의 합은 100이어야 합니다.                     |
| useGiniCoeff           | 지니(GINI) 계수 사용 여부                                                     |
| deferredTxFee          | 블록에 대한 TX 수수료를 분배하는 방법.                                               |
| stakingUpdateInterval  | 스테이킹 정보를 업데이트하기 위한 블록 높이의 시간 간격.                                      |
| proposerUpdateInterval | 제안자 정보를 업데이트하기 위한 블록 높이의 시간 간격.                                       |
| minimumStake           | 코어 셀 운영자에 참여하기 위한 최소량의 peb.                                           |

# Example <a id="example"></a>

```
{
    "config": {
        "chainId": 2019,
        "istanbulCompatibleBlock": 0,
        "istanbul": {
            "epoch": 604800,
            "policy": 2,
            "sub": 13
        },
        "unitPrice": 25000000000,
        "deriveShaImpl": 2,
        "governance": {
            "governingNode": "0x46b0bd6380005952759f605d02a6365552c776f3",
            "governanceMode": "single",
            "reward": {
                "mintingAmount": 6400000000000000000,
                "ratio": "50/40/10",
                "useGiniCoeff": true,
                "deferredTxFee": true,
                "stakingUpdateInterval": 86400,
                "proposerUpdateInterval": 3600,
                "minimumStake": 5000000
            }
        }
    },
    "nonce": "0x0",
    "timestamp": "0x5c9af60e",
    "extraData": "0x0000000000000000000000000000000000000000000000000000000000000000f89af85494aeae0ab623d4118ac62a2decc386949b5ce67ce29446b0bd6380005952759f605d02a6365552c776f394699b607851c878e29499672f42a769b71f74be8e94e67598eb5831164574c876994d53f63eab4f36d7b8410000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c0",
    "gasLimit": "0xe8d4a50fff",
    "difficulty": "0x1",
    "mixHash": "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
    "coinbase": "0x0000000000000000000000000000000000000000",
    "alloc": {
        "0000000000000000000000000000000000000400": {
            "code": "0x6080604052600436106101505763ffffffff60e00a165627a7a7230582093756fe617053766b158f7c64998c746eb38f0d5431cc50231cc9fb2cd1fd9950029",
            "balance": "0x0"
        },
        "46b0bd6380005952759f605d02a6365552c776f3": {
            "balance": "0x446c3b15f9926687d2c40534fdb564000000000000"
        },
        "699b607851c878e29499672f42a769b71f74be8e": {
            "balance": "0x446c3b15f9926687d2c40534fdb564000000000000"
        },
        "aeae0ab623d4118ac62a2decc386949b5ce67ce2": {
            "balance": "0x446c3b15f9926687d2c40534fdb564000000000000"
        },
        "e67598eb5831164574c876994d53f63eab4f36d7": {
            "balance": "0x446c3b15f9926687d2c40534fdb564000000000000"
        }
    },
    "number": "0x0",
    "gasUsed": "0x0",
    "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000"
}
```
