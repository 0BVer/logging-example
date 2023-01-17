# docker-example Repo용 로깅 예제

[![Elastic Stack version](https://img.shields.io/badge/Elastic%20Stack-8.5.3-00bfb3?style=flat&logo=elastic-stack)](https://www.elastic.co/blog/category/releases)

https://github.com/litsynp/docker-example 프로젝트를 위한 로깅 세팅입니다.

## ELK 로깅

- *단일 compose 파일 실행시 동작하지 않습니다.*

### 동작 방식
1. NGINX의 로그파일을 Filebeat로 수집
2. 수집한 로그를 Logstash에 전달
3. 전달 받은 로그를 Elasticsearch에 저장
4. 저장된 로그를 Kibana를 통해 분석

### 사용법

  ```sh
 $ docker compose -f docker-compose.prod.yml -f docker-compose.logging.yml up --build
  ```

### Kibana를 통한 로그 시각화 방법
1. localhost:5601 접속
2. 상단 메뉴에서 index management 검색
3. 수집된 로그 인덱스 확인 weblogs-yyyy.MM.dd 형식
4. 좌측 메뉴의 Analytics의 Dashboard 클릭
5. Create data view를 통해 인덱스 선택
    - 전체 조회 : Index Pattern에 `weblogs-*` 입력 및 저장
    - 선택 조회 : Index Pattern에 보고싶은 날짜입력 (eg. `weblogs-2023.01.01`)
6. Create Visualization 클릭
7. 보고 싶은 필드를 화면에 드롭다운 하여 시각화

### 필수 파일
```
docker-example
├── backend
│   └── ...
├── frontend
│   └── ...
├── logging-example
│   ├── elasticsearch
│   │   ├── config
│   │   │   └── elasticsearch.yml
│   │   └── Dockerfile
│   ├── filebeat
│   │   ├── config
│   │   │   └── filebeat.yml
│   │   └── Dockerfile
│   ├── kibana
│   │   ├── config
│   │   │   └── kibana.yml
│   │   └── Dockerfile
│   └── logstash
│       ├── config
│       │   └── logstash.yml
│       ├── pipeline
│       │   └── logstash.conf
│       └── Dockerfile
├── nginx
│   ├── nginx.conf
│   └── log     # 실행시 자동 생성
│       ├── access.log
│       └── error.log
├── docker-compose.logging.yml
└── docker-compose.prod.yml
```