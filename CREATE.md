모든 출처는 MariaDB에 있습니다.

링크 : https://mariadb.com/docs/server/reference

## 1.CREATE DATABASE

```jsx
데이터베이스를 만드는 SQL
OR REPLACE : 원래 있던 DB를 DROP후 새로 만든다.
IF NOT EXISTS : 같은 db_name의 DB가 존재한다면 경고를 보냄.
COMMENT : 길이제한 1024byte, 초과 시 error 4144 발생, 
COMMENT는 db.opt 파일 뿐만 아니라 INFORMATION_SCHEMA.SCHEMATA에 저장

CREATE [OR REPLACE] {DATABASE | SCHEMA} [IF NOT EXISTS] db_name
    [create_specification] ...

create_specification:
    [DEFAULT] CHARACTER SET [=] charset_name
  | [DEFAULT] COLLATE [=] collation_name
  | COMMENT [=] 'comment'
```

## 2. CREATE CATALOG

```jsx
카탈로그와 mysql안의 sys안에 있는 performance_schema를 만드는 SQL

CREATE CATALOG [IF NOT EXISTS] catalog_name
    [create_specification] ...

create_specification:
    [DEFAULT] CHARACTER SET [=] charset_name
  | [DEFAULT] COLLATE [=] collation_name
  | COMMENT [=] 'comment'
```

## 3. CREATE EVENT

```jsx
새 이벤트를 생성하고 예약하는 SQL, EVENT 권한 필요
ON SCHEDULE : 언제, 얼마나 실행될지 결정
DO : 실행될 SQL문을 포함하는 절
ON COMPLETION [NOT] PRESERVE : 이벤트 완료 후 삭제 여부 결정, 기본적으로 삭제
ENABLE|DISABLE : 이벤트 활성화 또는 비활성화, 기본적으론 ENABLE

CREATE [OR REPLACE]
    [DEFINER = { user | CURRENT_USER | role | CURRENT_ROLE }]
    EVENT 
    [IF NOT EXISTS]
    event_name    
    ON SCHEDULE schedule
    [ON COMPLETION [NOT] PRESERVE]
    [ENABLE | DISABLE | DISABLE ON SLAVE]
    [COMMENT 'comment']
    DO sql_statement

AT : 일회성 이벤트에 사용
STARTS~ENDS : 이벤트가 반복되는 경우

schedule:
    AT timestamp [+ INTERVAL interval] ...
  | EVERY interval 
    [STARTS timestamp [+ INTERVAL interval] ...] 
    [ENDS timestamp [+ INTERVAL interval] ...]

interval:
    quantity {YEAR | QUARTER | MONTH | DAY | HOUR | MINUTE |
              WEEK | SECOND | YEAR_MONTH | DAY_HOUR | DAY_MINUTE |
              DAY_SECOND | HOUR_MINUTE | HOUR_SECOND | MINUTE_SECOND}
```
