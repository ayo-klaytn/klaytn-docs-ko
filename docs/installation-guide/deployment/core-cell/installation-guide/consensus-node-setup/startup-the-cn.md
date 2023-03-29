# CN 실행 <a id="startup-the-cn"></a>

## CN 시작/중지  <a id="cn-start-stop"></a>

다음 `systemctl` 명령을 사용하여 Klaytn 서비스를 시작/중지할 수 있습니다.

**참고**: 루트 권한이 필요합니다.

**실행**

```bash
$ systemctl start kcnd.service

```

**중지**

```bash
$ systemctl stop kcnd.service

```

**status**

```bash
$ systemctl status kcnd.service

```

## Troubleshooting <a id="troubleshooting"></a>

다음 오류가 발생하면,

```bash
Failed to start kcnd.service: Unit not found.
```

다음 명령으로 시스템 관리자 환경설정을 다시 로드하세요.

```bash
$ systemctl daemon-reload
```


