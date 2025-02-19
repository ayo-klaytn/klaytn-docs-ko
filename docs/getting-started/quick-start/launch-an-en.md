# Launch an Endpoint Node

## Download and Initialize an Endpoint Node (EN) <a href="#download-and-initialize-an-endpoint-node-en" id="download-and-initialize-an-endpoint-node-en"></a>

Unzip the provided [ken binary package](../../installation-guide/download/#get-the-packages) and copy the files into the klaytn folder.\
**Note**: Please download appropriate package starting with `ken`.

Mac 사용자의 경우, 다음 명령으로 다운로드한 파일을 압축 해제합니다.

```bash
$ tar zxf ken-baobab-vX.X.X-X-darwin-amd64.tar.gz
$ export PATH=$PATH:$PWD/ken-darwin-amd64/bin
```

Linux 사용자의 경우, 다음 명령으로 다운로드한 파일을 압축 해제합니다.

```bash
$ tar zxf ken-baobab-vX.X.X-X-linux-amd64.tar.gz
$ export PATH=$PATH:$PWD/ken-linux-amd64/bin
```

블록체인 데이터를 저장할 데이터 디렉토리를 만들어야 합니다. 이 튜토리얼에서는 홈 디렉터리에 `kend_home` 폴더를 생성하겠습니다.

```bash
$ mkdir -p ~/kend_home
```

## EN 환경설정 <a href="#configuring-the-en" id="configuring-the-en"></a>

설정 파일인 `kend.conf`는 `ken-xxxxx-amd64/conf/`에 위치합니다. For the details of configurable parameters, you can refer to the [EN Configuration Guide](../../operation-guide/configuration.md). Baobab 테스트넷의 EN을 실행하려면, 다음과 같이 `kend.conf` 파일을 업데이트하시기 바랍니다.

```
# cypress, baobab은 NETWORK_ID를 명시하지 않은 경우에만 사용할 수 있습니다.
NETWORK = "baobab"
# NETWORK_ID를 명시하면 개인(private) 네트워크가 생성됩니다.
NETWORK_ID=
...
RPC_API="klay,net" # 추후 truffle을 위해 net 모듈을 열어야 합니다.
...
DATA_DIR=~/kend_home
```

## EN 실행하기 <a href="#launching-the-en" id="launching-the-en"></a>

EN을 시작하려면 다음 명령을 실행합니다.

```bash
$ kend start
 Starting kend: OK
```

## EN 확인하기<a href="#checking-the-en" id="checking-the-en"></a>

EN이 구동 중인지 확인하려면 다음 명령을 실행합니다.

```bash
$ kend status
kend is running
```

## EN의 로그 확인 <a href="#checking-the-log-of-the-en" id="checking-the-log-of-the-en"></a>

EN의 로그를 확인하려면 다음 명령을 실행합니다.

```bash
$ tail -f ~/kend_home/logs/kend.out
...
INFO[03/26,15:37:49 +09] [5] Imported new chain segment                blocks=1    txs=0  mgas=0.000  elapsed=2.135ms   mgasps=0.000    number=71340 hash=f15511…c571da cache=155.56kB
...
```

## 문제 해결 <a href="#troubleshooting" id="troubleshooting"></a>

Please refer to the [Troubleshooting](../../operation-guide/errors-and-troubleshooting.md) if you have trouble in launching the Klaytn Endpoint Node.
