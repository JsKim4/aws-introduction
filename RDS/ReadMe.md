# AWS-RDS 

## 목차
- RDS란?
- Database Back-ups
- Multi AZ 그리고 Read Replicas
- ElasticCache
- RDS 실습

## RDS 란?
- Relational DB Service

- Relation DB 종류 (AWS RDS 에서 사용 가능한)
    - Microsoft SQL, Oracle, Mysql, Postgre, Aurora(Free tier provide X), Maria DB

- Data Warehousing
    - Business Intelligence
    - 리포트 작성, 데이터 분석시 사용 (Production Database -> Data Warehousing)
    - 매우 방대한 분량의 데이터 로드시 사용

- OTLP VS OLAP
    - OLTP : INSERT 와 같이 종종 사용되어지는, 혹은 규모가 작은 데이터를 불러올 때 사용되는 SQL 쿼리가 필요할때 유용
    - OLAP : 매우 큰 데이터를 불러올때 사용, 주로 덩치가 큰 SELECT 쿼리가 사용됨

## Database Back-ups
- Automated Backups - 자동 백업
    - Retention Period(1~35일) 안의 어떤 시간으로 돌아가게 할 수 있음
    - AB는 그날 생성된 스냅샷과 Transcation Logs를 참고함
    - 디폴트로 AB 기능이 설정되어 있으며 백업 정보는 S3에 저장함
    - AB동안(S3 버킷에 저장할 때) 약간의 I/O suspension이 존재할 수 있음 - Latency(지연시간)
- DB Snapshots - 데이터 베이스 스냅샷
    - 주로 사용자에 의해 실행됨
    - 원본 RDS Instance를 삭제해도 스냅샷은 존재함 ( vs AB(AB는 Instance 삭제시 X))

- 데이터베이스 백업
    - Original Instance 를 복제함
        - ex) `original`.[region].rds.amazonaws.com ->  `restored`.[region].rds.amazonaws.com

## Multi AZ 그리고 Read Replicas
- Multi AZ - Multi Availability Zone
    - 원래 존재하는 RDS DB에 변화가 일어날때 다른 AZ에 동일한 복제본이 생성됨
    - AWS에 의해 자동으로 관리가 이루어짐
    - 원본 RDS DB에 문제가 생길 경우 자동으로 다른 AZ의 복제본이 사용됨
    - Multi AZ의 유용성은 Disaster Recovery Only, 성능상 이점 때문에 사용하는 것이 아님
- Read Replica
    - Production DB의 읽기 전용 복제본이 생성됨
    - 주로 Read-Heavy DB 작업시 효율성의 극대화를 위해 사용됨 (Scaling)
    - Disaster Recovery 용도가 아님
    - 최대 5개의 Read Replica DB 허용
    - Read Replica 의 Read Replica생성이 가능함 (Letency 존재함)
    - 각각의 Read Replica 는 자기만의 EndPoint로 구분 가능함
    