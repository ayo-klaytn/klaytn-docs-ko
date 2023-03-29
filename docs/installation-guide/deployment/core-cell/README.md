# 코어 셀 개요 <a id="core-cell-overview"></a>

## 튜토리얼 대상  <a id="intended-audience"></a>

- 코어 셀 연산자
- Klaytn에서 블록체인 애플리케이션을 만들고 실행하는 데 관심이 있다면, 코어 셀을 유지할 필요가 없습니다. 대신 애플리케이션이 Klaytn 네트워크와 상호작용하도록 [엔드포인트 노드](../endpoint-node/README.md)를 실행해야 합니다.


## Core Cell Overview <a id="core-cell-overview"></a>

코어 셀(Core Cell, CC)은 합의 프로세스에 참여하고, 트랜잭션 수행 및 블록 생성을 담당하는 요소입니다. Klaytn 코어 셀(CC)은 다음 요소로 구성됩니다.

-  컨센서스 노드(CN): 컨센서스 노드는 블록 생성 프로세스에 참여하고 있습니다.
-  프록시 노드(PN): 프록시 노드는 네트워크에 인터페이스를 제공합니다. PNs transmit the transaction requests to the Consensus Nodes, and propagate the blocks down to the Endpoint Nodes.

코어 셀은 하나의 CN과 두 개 이상의 PN으로 구성하는 것이 좋습니다. CN은 코어 셀 네트워크 내의 다른 CN에 연결하여 합의를 수행합니다. CN은 동일한 코어 셀에있는 PN의 연결만 수락하여 트랜잭션 요청을 수신하고 블록을 네트워크에 전파합니다. PN은 엔드포인트 노드 네트워크 내의 모든 EN에서 연결을 허용합니다.

![코어 셀 개요](images/cn_set.png)

| Name | Description                                                                                                                                                                                                                         | 네트워크 보안                                                                                                                                                                                                  | Quantity                         |
|:---- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |:-------------------------------- |
| CN   | 코어 셀 네트워크에서 다른 CN과 새로운 블록을 생성하는 노드                                                                                                                                                                                                  | 네트워크는 허가된 CN으로 구성됩니다. (IP 액세스 제어 필요).                                                                                                                                                                    | 1개                               |
| PN   | - Klaytn 엔드포인트 노드 네트워크에서 받은 트랜잭션을 CN에 제출하는 노드. <br>- It propagates the created blocks to Klaytn Endpoint Node Network. <br>- It can scale out horizontally depending on the number of ENs in the Endpoint Node Network. | - 코어 셀의 CN에 연결되어 있으며, 인터넷의 다른 Klaytn 노드에서 연결을 수락하기 위해 IP 및 포트가 공용이어야 합니다. <br>- It can connect to other PNs in other Core Cell via PN bootnode. <br>- It can connect to ENs via EN bootnode. | 적어도 하나는 필요합니다. 2개 이상의 PN이 권장됩니다. |



