# 0525 / DML, CONSTRAINT, VIEW

# 과제 리뷰

- from 절에 서브쿼리 쓰기 (4번) → 만들어진 테이블을 불러와서 select
    
    select *
    f**rom (select name, kor, eng, mat, kor+eng+mat "총점", round((kor+eng+mat)/3) "평균", rank() over(order by (kor+eng+mat)desc) "등수" from jumsu)**
    where 등수 = 2;
    
- union 활용하기 (7번) → 행으로 정렬해서 하나의 테이블을 만들때 쓰임
    
    select '국어' 과목, max(kor) 점수 from jumsu
    **union**
    select '수학' 과목, max(mat) 점수 from jumsu
    **union**
    select '영어' 과목, max(eng) 점수 from jumsu;
    
- from 절에 서브쿼리 쓰기 (8번) → 테이블에 이름을 붙여줌 (VIEW개념과 비슷)
    
    **with B**
    as (select '국어' 과목, max(kor) 점수 from jumsu
    union
    select '수학' 과목, max(mat) 점수 from jumsu
    union
    select '영어' 과목, max(eng) 점수 from jumsu)
    
    select 과목, 점수
    from **B**
    where 점수 = (select max(점수)
    from **B**);
    
- 껍데기만 만들고 안에 채우기 (9번)
    
    create table jumsu2
    as
    select * from jumsu
    **where 1=0;**
    
    insert into jumsu2
    values('홍길동',100,100,100);
    
    insert into jumsu2
    values('박혜영',100,100,100);
    

# DML

- **MERGE**
    - 구조가 같은 두개의 테이블을 하나의 테이블로 합치는 기능
    - 
    - EMP01 테이블에 EMP02 테이블을 합병
    **MERGE INTO** EMP01
    **USING** EMP02
    **ON**(EMP01.EMPNO=EMP02. EMPNO)
    **WHEN MATCHED THEN
    UPDATE SET**
      EMP01.ENAME=EMP02.ENAME,
      EMP01.JOB=EMP02.JOB,
      EMP01.MGR=EMP02.MGR,
      EMP01.HIREDATE=EMP02.HIREDATE,
      EMP01.SAL=EMP02.SAL,
      EMP01.COMM=EMP02.COMM,
      EMP01.DEPTNO=EMP02.DEPTNO
    **WHEN NOT MATCHED THEN
    INSERT VALUES**(
      EMP02.EMPNO, EMP02.ENAME, EMP02.JOB,
      EMP02.MGR, EMP02.HIREDATE, EMP02.SAL,
      EMP02.COMM, EMP02.DEPTNO);
- **TRANSACTION**
    - **Transaction**
        - 데이터 처리의 한단위
        - 여러개의 SQL 명령문들을 하나의 **논리적인 작업 단위**로 처리
        - 하나의 트랜잭션은 **ALL-OR-Nothing**
        - 트랜잭션 제어를 위한 명령어는 COMMIT, SAVEPOINT, ROLLBACK이 있다
    - **COMMIT**
        - DML 작업이 성공적으로 처리되도록 하기 위해 쓰임 (하드에 저장)
        - 자동 commit 하는 경우
            - DDL **: CREATE, ALTER, DROP, RENAME, TRUNCATE**
            - DCL : **GRANT, REVOKE**
    - **ROLLBACK**
        - 작업을 취소하기 위해 쓰임
    - **SAVEPOINT**
        - 트랜잭션을 작게 분할할 수 있음
        - savepoint 불러올때 : **rollback to save point**
        - 여러 savepoint가 있는데 특정 지정 없이 rollback 한다면 가장 맨앞 savepoint로 이동
- 데이터 읽기 일관성과 락
    - **LOCK**
        - 파일이나 레코드를 누군가가 열어서 작업하고 있는 도중에 **다른 사용자가 이를 열지 못하도록** 하고 먼저 오픈한 사용자가 닫기 전까지 사용할 수 없도록 하는 것
    - **DEAD LOCK (교착상태)**
        - 양쪽이 lock이 걸린 상태

# CONSTRAINT

- 무결성 제약 조건

| NOT NULL  | NULL을 허용하지 않는다. |
| --- | --- |
| UNIQUE  | 중복된 값을 허용하지 않는다. 항상 유일한 값을 갖도록 한다, NULL 가능 |
| PRIMARY KEY
 | NULL을 허용하지 않고 중복된 값을 허용하지 않는다.
NOT NULL 조건과 UNIQUE 조건을 결합한 형태이다. |
| FOREIGN KEY  | 참조되는 테이블의 칼럼의 값이 존재하면 허용한다. |
| CHECK | 저장 가능한 데이터 값의 범위나 조건을 지정하여 설정한 값만 을 허용한다. |
- 제약조건 살피기
    - **user_constraints** 테이블
        - select constraint_name, constraint_type, table_name from **user_constraints**;
        - constraint_type
            
            
            | P | PRIMARY KEY |
            | --- | --- |
            | R | FOREIGN KEY |
            | U | UNIQUE |
            | C | CHECK, NOT NULL |
    - **user_cons_columns;**
        - select * from **user_cons_columns;**
- **컬럼 레벨** 정의 방법
    - **UNIQUE KEY**
        - **NULL값을 중복**으로 저장할 수 있다
        - CREATE TABLE EMP03(
        EMPNO NUMBER(4)
            
            **CONSTRAINT** EMP03_EMPNO_UK **UNIQUE** );
            
    - **PRIMARY KEY**
        - CREATE TABLE EMP050 (
        EMPNO NUMBER(4)
            
            **CONSTRAINT EMP05_EMPNO_PK PRIMARY KEY );**
            
    - **FOREIGN KEY**
        - 반드시 **부모 테이블에 존재하는 column만** 입력하도록 참조하는 방법
        - **먼저** 정의되어야 하는 테이블이 **부모 테이블**이고 나중에 정의되어야 하는 테이블이 자식 테이블이 된다
        - 부모 키가 되기 위한 칼럼은 반드시 부모 테이블의 **기본키(PRIMARY KEY)**나 **유일키(UNIQUE)**로 설정되어 있어야 한다
        - CREATE TABLE EMP06 (
        DEPTNO NUMBER(2)
           **CONSTRAINT EMP06_DEPTNO_FK REFERENCES DEPT(DEPTNO) );**
    - **CHECK**
        - 설정된 값 이외의 값이 들어오면 오류 수행
        - CREATE TABLE EMP07(
        GENDER VARCHAR2(1)
           **CONSTRAINT EMP07_GENDER_CK
           CHECK (GENDER IN('M', 'F')) );**
    - **DEFAULT**
        - 아무런 값도 입력 안했을때 디폴트값으로 입력
- **테이블 레벨** 정의
    - CREATE TABLE EMP03(
       EMPNO NUMBER(4),
       ENAME VARCHAR2(10) **CONSTRAINT** EMP03_ENAME_NN **NOT NULL,**
       JOB VARCHAR2(9),
       DEPTNO NUMBER(4),
       **CONSTRAINT** EMP03_EMPNO_PK **PRIMARY KEY(**EMPNO),
       **CONSTRAINT** EMP03_JOB_UK **UNIQUE**(JOB),
       **CONSTRAINT** EMP03_DEPTNO_FK **FOREIGN KEY**(DEPTNO)
       **REFERENCES DEPT**(DEPTNO)
    );
        - NOT NULL은 **테이블 레벨 정의로 지정할 수 없음**
    - **복합키**를 **기본키**로 지정
        - CREATE TABLE MEMBER01(
        
          CONSTRAINT MEMBER01_COMBO_PK **PRIMARY KEY(NAME, HPHONE)**
        );
        
        - POSITION 1,2,3… 으로 나타남
- **ON DELETE CASCADE**
    - 부모키를 삭제 → 자식 테이블의 참조 컬럼도 삭제됨 **(NULL값으로 채워짐)**
    - on delete cascade 없이 부모키 삭제하려고 하면 오류뜸
    - CREATE TABLE CHILD2(
      **CONSTRAINT** CHILD2_ID_FK **FOREIGN KEY**(C_ID)
      **REFERENCES** PARENT2(P_ID) **ON DELETE CASCADE** );

# VIEW

- **VIEW**
    - **물리적인 테이블 (기본 테이블)**을 근거한 논리적인 **가상테이블**
    - view도 만들 수 있는 권한이 필요함 (Grant create view to scott)
    - insert문에 의해 **view**에 추가한 행이 **테이블에도 추가**된다
    - 쓰는 이유? 제한적으로 정보를 조회할 수 있도록 해야하기 때문이다
    - **USER_VIEWS**를 조회하면 지금까지 생성한 뷰를 확인할 수 있다
    - 생성
        - **create view** sal_view
        **as**
        select dname, max(sal) "MAX_SAL", min(sal) "MIN_SAL"
        from emp,dept
        where emp.deptno = dept.deptno
        group by dname;
    - 수정
        - **create or replace** view emp_view20
        **as**
        select empno EMP_NO, ename EMP_NAME, deptno DEPT_NO, mgr MANAGER, sal SAL
        from emp
        where deptno=20;
    
    # 오류종류
    
- ORA-00001: unique constraint (SCOTT.PK_DEPT) violated →PK_DEPT (제약조건 이름)
    - dept는 primary key라서 중복 X, null X
- ORA-01400: cannot insert NULL into ("SCOTT"."EMP02"."EMPNO")
    - SCOTT이라는 owner, EMP02라는 테이블, EMPNO라는 컬럼
- ORA-00001: unique constraint (SCOTT.SYS_C007194) violated
    - 이름을 정해주지 않아서 이렇게 뜸 (unique)
- ORA-01756: quoted string not properly terminated
    - null이나 중복된 값이 나와서 (primary)
- ORA-02291: integrity constraint (SCOTT.EMP06_DEPTNO_FK) violated - parent key not found
    - 부모키에 해당되는게 없을때 (foreign)
- ORA-01031: insufficient privileges
    - 권한이 없다 →
