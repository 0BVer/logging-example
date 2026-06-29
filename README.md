# ELK Logging with Docker

[![Elastic Stack version](https://img.shields.io/badge/Elastic%20Stack-8.5.3-00bfb3?style=flat&logo=elastic-stack)](https://www.elastic.co/blog/category/releases)

Drop-in ELK logging sidecar for Docker Compose projects — ships Nginx access logs through Filebeat → Logstash → Elasticsearch and surfaces them in Kibana, without touching your existing compose setup.

Nginx 액세스 로그를 ELK 스택으로 수집·시각화하는 Docker Compose 사이드카입니다. 기존 compose 파일을 수정하지 않고 `-f` 옵션으로 붙여서 사용합니다.

## Architecture

```
Nginx ──(access.log)──► Filebeat ──► Logstash ──► Elasticsearch ◄── Kibana
```

## Stack

| Component | Version |
|-----------|---------|
| Elasticsearch | 8.5.3 |
| Logstash | 8.5.3 |
| Kibana | 8.5.3 |
| Filebeat | 8.5.3 |

## Usage

Run alongside your existing compose file:

```shell
docker compose -f docker-compose.prod.yml -f docker-compose.logging.yml up --build
```

Kibana: http://localhost:5601  
Log index pattern: `weblogs-yyyy.MM.dd`

### Kibana Visualization

1. Go to http://localhost:5601 → search **Index Management**
2. Confirm `weblogs-*` indices exist
3. Analytics → Dashboard → **Create data view** → pattern: `weblogs-*`
4. **Create Visualization** → drag fields onto the canvas

→ [Kibana demo](https://demo.elastic.co/app/dashboards)

## Project Structure

```
logging-example/
├── elasticsearch/
│   ├── config/elasticsearch.yml
│   └── Dockerfile
├── filebeat/
│   ├── config/filebeat.yml
│   └── Dockerfile
├── kibana/
│   ├── config/kibana.yml
│   └── Dockerfile
└── logstash/
    ├── config/logstash.yml
    ├── pipeline/logstash.conf
    └── Dockerfile
```

### Required host project layout

```
your-project/
├── nginx/
│   └── log/          ← auto-created on first run
│       ├── access.log
│       └── error.log
├── logging-example/  ← this repo
├── docker-compose.prod.yml
└── docker-compose.logging.yml
```
