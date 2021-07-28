## 1. 오늘의 SQL

##### 1. 특정 테이블들의 조회권한 가진 신규 계정 생성
```
-- USER 생성 여부 확인
SELECT * FROM ALL_USERS WHERE USERNAME LIKE 'U_PEM';

-- USER 생성 (특수문자 등 넣을 경우 "" 사용)
 CREATE USER U_PEM IDENTIFIED "qwer098!";

-- USER role 확인
select * from dba_role_privs where GRANTEE = 'U_PEM';

-- PEM_INQ USER에 connect role 부여 (이 권한이 없으면 이 유저로 접속 불가)
grant connect to U_PEM;

--pemxxxx 형식의 모든 테이블 권한 부여 쿼리 생성
select 'GRANT select on '||owner||'.'||table_name||' to PEM_INQ;' from dba_tables where table_name like 'pem____'

```

## 2. SQL 최적화
##### 1. SELECT 시에는 필요한 컬럼만 선택해서 가져온다
  ```
	SELECT * FROM USER_TABLE;

	SELECT USR_NO, USR_NM FROM USR_TABLE:
  ```

##### 2. DISTINCT, UNION과 같이 중복 값을 제거하는 함수를 최소화한다.
  ```
	SELECT DISTINCT USR_NM FROM USR_TABLE U, DEPT_TABLE D
		WHERE U.DEPT_NO = D.DEPT_NO;

	SELECT USR_NM FROM USR_TABLE U
		FROM (SELECT 1 FROM DEPT_TABLE D WHERE U.DEPT_NO = D.DEPT_NO);
  ```

##### 3. LIKE 사용 시 %를 가능한 STRING 앞에 배치하지 않는다.
 - LIKE '%...'의 경우 인덱스를 타지 않고 FULL SCAN을 사용하게 된다.

