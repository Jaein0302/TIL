
- decode, nvl2 사용
    - **decode**
        
        [1]
        
        select rpad(e.ename,6) ||'의 매니저는 ' ||
        **rpad(m.ename,6) || decode(m.ename,null,'없습니다','입니다') " 사원별 매니저"**
        from emp e left outer join emp m
        on e.mgr=m.empno;
        
        [2] 
        
        select rpad(e.ename,6) ||'의 매니저는 ' ||
        **decode(m.ename,null,'없습니다',rpad(m.ename,6)||'입니다') " 사원별 매니저"**
        from emp e left outer join emp m
        on e.mgr=m.empno;
        
    - **nvl2**
        
        select rpad(e.ename,6) ||'의 매니저는 ' ||
        **rpad(m.ename,6) || nvl2(m.ename,'입니다','없습니다') " 사원별 매니저"**
        from emp e left outer join emp m
        on e.mgr=m.empno;
        
- EXISTS
    - 괄호 안의 쿼리(서브쿼리)에 어떤 것이든 **결과 반환되면 참**이 됩니다.
    - SELECT ENAME, SAL
    FROM EMP
    **WHERE EXISTS (SELECT SAL
                             FROM EMP
                             WHERE DEPTNO = 30 );**
- 다중 열 서브쿼리
    - 서브 쿼리의 결과가 **두 개 이상의 컬럼**으로 반환되어 메인 쿼리에 전달되는 쿼리
    - 쓰는 이유? group by 때문에 원하는 값을 구할수가 없음 → where 절에 서브쿼리를 넣어서 구하면 됨
    - 부서별 가장 작은 급여를 받는 직원 조회
        - 이렇게 하면 각각의 depnto, ename의 min(sal)이 추출 (옳은 방법 X)
            
            select deptno, ename, min(sal)
            from emp
            group by deptno, ename;
            
        - where절에서 부서별 min(sal)을 구하고 그 값을 sal을 통해 출력
            
            SELECT DEPTNO, ENAME, SAL
            FROM EMP
            **WHERE (DEPTNO, SAL) IN (SELECT DEPTNO, MIN(SAL)
                                                       FROM EMP
                                                       GROUP BY DEPTNO)**
            ORDER BY DEPTNO;
            
- **UNION (합집합)**
    - 두 테이블의 결합을 나타내며, 결합시키는 두 테이블의 **중복 데이터는 제거**한다.
    - SELECT 문장의 열의 개수가 반드시 같아야 한다.
    - 결합시 반드시 같은 자료형이어야 한다.
    - 중복을 제거하기 위해 SORT 함
    - 자료가 많거나 INDEX가 되어있지 않는 칼럼을 대상으로 하면 쿼리시간이 길어질수 있음
    - **UNION ALL** : 두 테이블의 **중복**되는 값까지 반환한다.
- **INTERSECT (교집합)**
    - 여러 개의 SQL문의 결과에 대한 교집합을 나타내고 중복 행은 하나의 결과로 보여줍니다.
- **MINUS (차집합)**
    - 여러 개의 SQL문의 결과에 대한 차집합을 나타냅니다. 공통된 항목을 제외한 결과만 추출합니다.

# DDL (데이터 정의어)

→ 데이터베이스 관리자나 응용 프로그래머가 데이터베이스의 **논리적 구조를 정의**하기 위한 언어로서 **데이터 사전(Data Dictionary)에 저장** 됩니다.

- **CREATE (테이블 생성)**
    - **create** table dept01(
      detno number(2),
      dname varchar2(14),
      loc varchar2(13)
    )
    - **create** table emp02
        
        **as**
        
        select * from emp **where 1=0;** → 빈 껍데기만 생성 (데이터X)
        
- **ALTER (기존 테이블을 변경)**
    - **alter** table dept02
    **add**(dmgr varchar2(10));
    - **alter** table dept02
    **modify**(dmgr number(4));
    - **alter** table dept02
    **drop** column dmgr
- **DROP (기존 테이블 제거)**
    - **drop** table emp01;
- **TRUNCATE (테이블의 데이터만 삭제)**
    - **truncate** table emp02;
- **RENAME (테이블 명 변경)**
    - **rename** emp **to** test;

# DML (데이터 조작어)

→ 데이터베이스에 **저장된 데이터를 조작**하기 위해 사용하는 언어로서 데이터 검색(Retrieval), 추가(Insert), 삭제(Delete), 갱신(Update) 작업 수행 합니다.

- **INSERT (테이블에 새로운 데이터 입력)**
    - **insert into** sam01
        
        **(empno, ename, job, sal)      → 생략가능**
        
        **values** (1000,'APPLE','POLICE', NULL);
        
    - **insert into** dept01
        
        select * from dept;
        
- **UPDATE (테이블에 저장된 데이터 수정)**
    - **update** emp01
        
        **set** sal = sal-5000
        
        **where** sal ≥ 10000;
        
    - **update** sam02
    **set** sal=sal+1000
    **where deptno** = (select deptno
                               from dept
                               where loc='DALLAS');
    - **update** sam02
    **set** **(sal,hiredate)** = (select sal, hiredate
                                   from sam02
                                   where ename = 'KING');
- **DELETE (테이블에 저장된 데이터 삭제)**
    - **delete** from sam02
    **where deptno** = (select deptno
                               from dept
                               where dname = 'RESEARCH');

# 오류처리

- ORA-00942: table or view does not exist
    - drop하고났을때
- no rows selected
    - Truncate하고났을때
- ORA-00913: too many values
    - values를 더 많이 줬을때
- ORA-00907:missing right parenthesis
    - 괄호나 콤마가 빠졌을때
- ORA-00907: missing right parenthesis
    - 콤마 빠졌을때
