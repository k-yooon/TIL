# java heap memory 설정

## 설정법
 1. config 폴더 아래 jvm.options 파일 수정
  
 ```
-Xms8g -- 최소값
-Xmx8g -- 최대값

-XX:+UseG1GC
-XX:MaxGCPauseMillis=200
 ```

 2. 실행 시 환경 변수 이용
    ** jvm.options 파일 내 옵션 주석 필요

 ```
./bin/elasticsearch ES_JAVA_OPTS="-Xms2g -Xmx2g"
 ```

## 설정 시, 주의사항
 - 최소값과 최대값은 같게 
 - 힙의 최소값을 물리적 RAM의 50% 이상으로 설정하지 말 것.

## 설정 후, 재시작
  - 이미 인덱스 존재하고 있는 경우
    - _cluster/settings의 allocation.enable=none으로 변경 해준 뒤, 다시 all로 변경
    - none으로 변경 후 node를 정지 -> 해당 node에 있던 primary shard가 다른 node에 있던 replica와 교환 -> 교환된 replica는 unassigned 상태로 변경 -> node 재기동 -> allocation.enable=all로 변경 -> unassigned replica가 재기동된 node에 할       
```
PUT _cluster/settings
{
    "transient" : {
        "cluster.routing.allocation.enable" : "none"
    }
}

//재시작 후
PUT _cluster/settings
{
    "transient" : {
        "cluster.routing.allocation.enable" : "all"
    }
}
```

###### reference https://springboot.cloud/17 / https://doubly12f.tistory.com/89