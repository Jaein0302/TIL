
# 과제 리뷰

- 부모키, 외래키 구하기 (view 3개를 이용하여 join하기)

처음에 2개의 테이블만 조인하는것만 배워서 생각을 확장하지 못했다

아래와 같이 코드를 짰고, 당연히 column 하나를 구하지 못했다.

**[테이블 2개 조인]**

```sql
select  U.table_name, **column_name**, C.table_name, C.column_name
from user_constraints U, user_cons_columns C
where U.r_constraint_name=C.constraint_name
and U.table_name = C.table_name;.
```

**[테이블 3개 조인]**

**1) view 3개를 만들고 시작**

```sql
create view constraint
as
select table_name, constraint_name, r_constraint_name
from user_constraints
```

```sql
create view p
as
select constraint_name, table_name, column_name
from  user_cons_columns
```

```sql
create view c
as
select constraint_name, table_name, column_name
from  user_cons_columns
```

**2) 답 구하기**

```sql
select  c.table_name "자식테이블", c.column_name "외래키", p.table_name "부모테이블", p.column_name"부모키"
from constraint, p, c
where constraint.r_constraint_name=p.constraint_name
and constraint.constraint_name = c.constraint_name;
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c9637288-488a-4788-b9d3-51d307c08baf/Untitled.png)

# VIEW

- **WITH CHECK OPTION**
    - VIEW에서 insert, update, delete를 통해서 기본테이블이 변경되는 것을 방지 하기 위한 옵션
    - **where절의 조건**으로 설정하지 않은 컬럼에 대해서는 변경 가능!
    - CREATE OR REPLACE VIEW VIEW_CHK30
    AS
    SELECT EMPNO, ENAME, SAL, COMM, DEPTNO
    FROM EMP_COPY
    WHERE DEPTNO=30 **WITH CHECK OPTION;**
- **WITH READ ONLY**
    - with check option과 다른점은 뷰를 통해서 기본테이블 절대 변경 불가
- **인라인 뷰**
    - TOP-N 구하기
        - select *
        from (select **rownum** as ranking, empno, ename, sal
                  **from (select empno, ename, sal
                            from emp
                            order by sal desc )
                  where rownum<=3)**
        where ranking between 2and 3;
    - 순서 정하기
        - insert into emp_max
            
            values (**(select nvl(max(empno),0)+1 from  emp_max**), ‘AAA’) → 괄호 필수
            

# SEQUENCE

- 시퀀스
    - 테이블 내의 유일한 숫자를 자동으로 생성하는 **자동 번호 발생키**
    - **기본키**로 사용
    - 생성
        - create **sequence** dept_example_seq
        **increment by** 10
        **start with** 10;
    - **USER_SEQUENCES** : 시퀀스 객체에 대한 정보
    - **Pseudo Columns**
        - **CURRVAL** : 현재 값을 반환
        - **NEXTVAL** : 현재 시퀀스값의 다음값 반환 (nextval로 먼저 생성하고 currval)
    - 시퀀스를 테이블의 **기본키에 접목** (순서 정하기)
        - 시퀀스를 사용하기 위해 empno에 기본키 설정
        create table emp01 (
            
            empno number(4) **primary key** );
            
        - 시퀀스 생성
            
            **create sequence** emp_seq
            
            increment by 1
            
            start with 1;
            
        - 기본키 입력
            
            **insert into** emp01
            
            values (**emp_seq.nextval**, ‘JULIA’, sysdate);
            
    - **ALTER SEQUENCE**
        - **start with**는 alter sequence를 써서 **변경 X**
    - **시퀀스 순환**하기 **(10 → 20 → 30 → 40 → 10 → 20 →..)**
        - CREATE SEQUENCE DEPT_SEQ
        START WITH 10
        INCREMENT BY 10
        MAXVALUE 40
        MINVALUE 10
        **cache 2   → 1보다는 크고 4보다는 작아야함**
        CYCLE;

# INDEX

- INDEX
    - SQL 명령문의 **처리속도를 향상**시키기 위해서 컬럼에 대해서 생성하는 오라클 객체
    - **기본키**나 유일키와 같은 제약 조건을 지정하면 자동으로 생성
    - **USER_IND_COLUMNS** : 인덱스에 대한 정보를 확인
    - 자동 생성되는 인덱스 이름 = **제약조건명**
    - 생성
        - **create index** idx_emp01_ename
            
            **on** emp01(ename);
            
    - 인덱스를 사용해야 하는 경우
        
        
        | 인덱스를 사용해야 하는 경우 | 인덱스를 사용하지 말아야 하는 경우 |
        | --- | --- |
        | 테이블에 행의 수가 적을 때 | 테이블에 행의 수가 많을 때 |
        | WHERE 문에 해당 컬럼이 많이 사용될 때 | WHERE 문에 해당 컬럼이 자주 사용되지 않을 때 |
        | 검색 결과가 전체 데이터의 2%~4% 정도 일 때  | 검색 결과가 전체 데이터의 10%~15% 이상 일 때 |
        | JOIN에 자주 사용되는 컬럼이나 NULL을 포함하는 컬럼이 많은 경우 | 테이블에 DML 작업이 많은 경우 즉, 입력 수정 삭제 등이 자주 일어 날 때 |
    

# 권한

- **시스템 권한**
    - DBA(SYS,SYSTEM)에 의해 부여되는 권한
    - 종류
        - **CREATE USER** : 새롭게 사용자를 생성하는 권한
        - **DROP USER** : 사용자를 삭제하는 권한
        - **DROP ANY TABLE** : 임의의 테이블을 삭제할 수 있는 권한
        - **QUERY REWRITE** : 함수 기반 인덱스를 생성하는 권한
        - **BACKUP ANY TABLE** : 임의의 테이블을 백업할 수 있는 권한
        - **CREATE SESSION** : 데이터베이스에 접속할 수 있는 권한
        - **CREATE TABLE** : 사용자 스키마에서 테이블을 생성할 수 있는 권한
        - **CREATE VIEW** : 사용자 스키마에서 뷰를 생성할 수 있는 권한
        - **CREATE SEQUENCE** : 사용자 스키마에서 시퀀스를 생성할 수 있는 권한
        - **CREATE PROCEDURE** : 사용자 스키마에서 프로시저를 생성할 수 있는 권한
    - 권한 부여 방법
        - **grant** create session **to** user01;
        - **grant** create table **to** user01;
        - **ALTER** USER USER01
        QUOTA 2M **ON SYSTEM;   → 테이블스페이스(테이블이나 뷰등이 저장되는 공간)를 만들어줌**
    - **WITH ADMIN OPTION**
        - 사용자에게 시스템 권한을 WITH ADMIN OPTION과 함께 부여하면 그 사용자는 데이터베이스 관리자가 아닌데도 불구하고 부여받은 **시스템 권한**을 **다른 사용자에게 부여할 수 있는 권한**도 함께 부여 받게 됩니다 (관리자 모드를 부여)
    - 
- **객체 권한**
    - DML문**(SELECT, INSERT, DELETE**)를 사용할수 있는 권한
    - **grant** select **on** emp **to** user01
    - **권한조회**
        - 스키마 ****
            - 객체를 소유한 사용자명
        - **USER_TAB_PRIVS_MADE** : 현재 사용자가 다른 사용자에게 부여한 권한 정보
        - **USER_TAB_PRIVS_RECD** :  자신에게 부여된 사용자 권한을 알고 싶을 때
    - **권한철회**
        - **revoke** select **on** emp **from** user01;
    - **WITH GRANT OPTION**
        - **객체 권한**을 **다른 사용자에게 부여할 수 있는 권한**을 부여 받음
