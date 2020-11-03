# 용량 큰 인덱스 Reindex 하는 법

  ## 어떻게?
    - 리인덱스 전 레플리카의 개수과 refresh_interval 옵션을 주지않고 데이터 이동
    - 리인덱스 완료 후 옵션값 복원
    
  ## refresh?
    - ES는 새로운 문서가 들어왔을 때 인덱싱을 하고 그 결과를 세그먼트 단위로 저장, 
      이때 ES가 세그먼트를 생성하는 것을 refresh라고 함
    - 세그먼트를 생성하는 주기를 refresh_interval이라고 함
    
  

```
## reindex할 인덱스 설정 수정
PUT v1-dqms_prod_step_doc-0003/_settings
{
  "refresh_interval": "-1",
  "number_of_replicas": "0"
}

## reindex
POST _reindex
{
  "source": {
    "index": "source-index"
  },
  "dest": {
    "index": "target-index"
  }
}

## 적재 상태 체크
GET target-index/_stats

## Forcemerge
POST target-index/_forcemerge?max_num_segments=1

## Replica와 refresh interval 수정
PUT target-index/_settings
{
  "refresh_interval": "3s",
  "number_of_replicas": "2"
}
```
