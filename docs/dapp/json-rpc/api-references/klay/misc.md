# 기타 <a id="miscellaneous"></a>

## klay_sha3 <a id="klay_sha3"></a>

입력된 데이터의 Keccak-256(이 해시 함수는 표준 SHA3-256가 아닙니다) 해시를 반환합니다.

**Parameters**

| Name | Type | Description          |
| ---- | ---- | -------------------- |
| data | DATA | SHA3 해시로 변환할 데이터입니다. |

**Return Value**

| Type         | Description                 |
| ------------ | --------------------------- |
| 32-byte DATA | 입력으로 받은 데이터의 SHA3 해시 결과입니다. |


**Example**

```shell
// Request
curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"klay_sha3","params":["0x11223344"],"id":1}' https://public-en-baobab.klaytn.net

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x36712aa4d0dd2f64a9ae6ac09555133a157c74ddf7c079a70c33e8b4bf70dd73"
}
```
