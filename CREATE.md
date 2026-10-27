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

## 4. CREATE FUNCTION

```jsx
사용자 설정 함수를 만드는 SQL

CREATE [OR REPLACE]
    [DEFINER = {user | CURRENT_USER | role | CURRENT_ROLE }]
    [AGGREGATE] FUNCTION [IF NOT EXISTS] func_name ([func_parameter[,...]])
    RETURNS type
    [characteristic ...]
    func_body

func_parameter:
    [ OUT | INOUT | IN OUT ] param_name type |
    [ IN ] param_name type [DEFAULT value or expression]
    ex) (IN a INT, IN b INT, OUT c INT)

type:
    Any valid MariaDB data type

characteristic:
    LANGUAGE SQL
  | [NOT] DETERMINISTIC
  | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
  | SQL SECURITY { DEFINER | INVOKER }
  | COMMENT 'string'

func_body:
    Valid SQL procedure statement
    ex)
    DELIMITER $ ....
			  BEGIN
				  DECLARE c, res INT;
				  SET res = add_func3(a, b, c) + d;
				  if (c > 99) then
					    return  3;
				  else
					    return res;
				  end if;
				END;
		$$
		DELIMITER ;
	DELIMITER을 이용하여 내부의SQL문에서 코드처럼 ;를 여러번 넣어도 한 문장으로 인식 
```

## 5. CREATE INDEX

```jsx
CREATE [OR REPLACE] [UNIQUE|FULLTEXT|SPATIAL|VECTOR] INDEX
    [IF NOT EXISTS] index_name
    [index_type]
    ON tbl_name (index_col_name,...)
    [WAIT n | NOWAIT]
    [index_option]
    [algorithm_option | lock_option] ...

index_col_name:
    col_name [(length)] [ASC | DESC]

index_type:
    USING {BTREE | HASH | RTREE}

index_option:
    KEY_BLOCK_SIZE [=] value
  | index_type
  | WITH PARSER parser_name
  | COMMENT 'string'
  | CLUSTERING = {YES | NO}
  | IGNORED | NOT IGNORED
  | DISTANCE = {EUCLIDEAN | COSINE}
  | M = number

algorithm_option:
    ALGORITHM [=] {DEFAULT|INPLACE|COPY|NOCOPY|INSTANT}

lock_option:
    LOCK [=] {DEFAULT|NONE|SHARED|EXCLUSIVE}
```