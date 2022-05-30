
- **PROCEDURE**
    - 저장 프로시저
        - **PL/SQL을 저장**해놓고 필요한 경우 호출하여 사용
    - **IN**
        - CREATE OR REPLACE **PROCEDURE** DEL_ENAME
        **(VENAME in EMP01.ENAME%TYPE) → in 생략가능**
        **IS**
        **BEGIN**
          DELETE FROM EMP01
          WHERE ENAME=VENAME;
        **END;**
        /
    - **OUT (순서)**
        - **프로시저 생성**
        CREATE OR REPLACE **PROCEDURE** SEL_EMPNO
        ( VEMPNO **IN** EMP.EMPNO%TYPE,
        VENAME **OUT** EMP. ENAME%TYPE,
        VSAL **OUT** EMP.SAL%TYPE,
        VJOB **OUT** EMP.JOB%TYPE
        )
        **IS**
        **BEGIN**
            SELECT ENAME, SAL, JOB
            INTO VENAME, VSAL, VJOB
            FROM EMP
            WHERE EMPNO = VEMPNO;  → 단일행만 가능
        **END;**
        - **바인드 변수 선언**
            
            SCOTT>**VARIABLE** VAR_ENAME **VARCHAR2(15);**
            SCOTT>**VARIABLE** VAR_SAL **NUMBER**;
            SCOTT>**VARIABLE** VAR_JOB **VARCHAR2(9);**
            
        - **프로시저 호출**
            
            **EXECUTE** SEL_EMPNO(7788, **:VAR_ENAME**, **:VAR_SAL**, **:VAR_JOB**)
            
        - **바인드 변수 활용**
            
            SELECT * FROM EMP WHERE ENAME = **:VAR_ENAME;**
            
    - **DESC USER_SOURCE**
        - 저장 프로시저에 대한 정보
    - **SET AUTOPRINT ON**
        - 바로 **바인드 변수** 호출
- **FUNCTION**
    - 저장 함수
        - 함수는 결과를 되돌려 받기 위해서 RUTURN문을 씀
    - 순서
        - **함수 생성**
            
            CREATE OR REPLACE **FUNCTION** CAL_BONUS (
                **VEMPNO IN EMP. EMPNO%TYPE )**
            **RETURN NUMBER**
            **IS**
                 **VSAL NUMBER(7, 2);
            BEGIN**
                SELECT SAL
                INTO VSAL
                FROM EMP
                WHERE EMPNO = VEMPNO; → 단일행만 가능
            
                **RETURN (VSAL * 2);** 
            END;
            
            /
            
        - **저장할 변수 선언 (바인드 변수와 같음)**
            
            **VARIABLE VAR_RES** NUMBER
            
        - **함수 호출**
            
            **EXECUTE  :VAR_RES**  := CAL_BONUS(7788);
            
        - 변수의 활용
            
            SELECT SAL, **CAL_BONUS(7788)**
            FROM EMP
            WHERE EMPNO = 7788;
            
- **CURSOR**
    - CURSOR
        - **여러개의 행**으로 구해지는 SELECT문을 처리할 때 필요함
        - ORA-01422: exact fetch returns more than requested number of rows
    - **FETCH 문**
        
        CREATE OR REPLACE **PROCEDURE** CURSOR_SAMPLE01
        **IS**
            **VDEPT DEPT%ROWTYPE;**
            **CURSOR** C1
            IS
            SELECT * FROM DEPT;
        **BEGIN**
            **OPEN C1;**
        
            **LOOP**
                **FETCH C1 INTO VDEPT.DEPTNO, VDEPT. DNAME, VDEPT.LOC;**
                EXIT WHEN C1%NOTFOUND;
                DBMS_OUTPUT.PUT_LINE (VDEPT.DEPTNO||' ‘||VDEPT. DNAME ||' '||VDEPT.lOC);
            **END LOOP;**
        
            **CLOSE C1;**
        END;
        /
        
    - **FOR LOOP 1 (커서 버전)**
        
        CREATE OR REPLACE **PROCEDURE** CURSOR_SAMPLE02
        **IS**
            **VDEPT DEPT%ROWTYPE;   → 생략가능**
            **CURSOR** C1
            IS
                SELECT FROM DEPT;
        **BEGIN
            FOR** VDEPT **IN** C1 **LOOP**          
        
            DBMS_OUTPUT.PUT_LINE(RPAD(VDEPT.DEPTNO,10)||RPAD(VDEPT.DNAME,15));
            **END LOOP;
        END**;
        
    - **FOR LOOP 2 (커서 생략 버전)**
        
        CREATE OR REPLACE **PROCEDURE** CURSOR_SAMPLE03
        IS
        BEGIN
            **FOR** VDEPT **IN (SELECT * FROM DEPT) LOOP**
        
                DBMS_OUTPUT.PUT_LINE(RPAD(VDEPT.DEPTNO,10)||RPAD(VDEPT.DNAME,15));
        **END LOOP;
        END:**
        
    - create or replace **procedure** CURSOR_TEST(
    vdeptno dept.deptno%type)
    **is**
        su number := 0;
    **begin**
        **for V in** (
                       select dname, count(dname) CNT, max(sal) MAX, round(avg(sal),2) AVG
                       from emp, dept
                       where emp.deptno = dept.deptno
                       and emp.deptno = vdeptno
                       group by dname)
         **loop**
             DBMS_OUTPUT.PUT_LINE('부서명   :' || V.dname);
             DBMS_OUTPUT.PUT_LINE('사원수   :' || V.CNT);
             DBMS_OUTPUT.PUT_LINE('최고급여   :' || V.MAX);
             DBMS_OUTPUT.PUT_LINE('평균급여   :' || V.AVG);
             su := su +1;
          **end loop;**
          if(su = 0) then
             DBMS_OUTPUT.PUT_LINE ('해당부서의 사원이 존재하지 않습니다');
          **end if;**
    **end;**
    /
- **PACKAGE**
    - 패키지
        - 관련 있는 프로시저를 보다 효율적으로 관리하기 위해서 사용
    - **선언부**
        
        CREATE OR REPLACE **PACKAGE** EXAM_PACK
        **IS**
            **FUNCTION** CAL_BONUS(VEMPNO IN EMP.EMPNO%TYPE)
                **RETURN** NUMBER;
            **PROCEDURE** CURSOR_SAMPLE02;
        **END;**
        /
        
    - **몸체부**
        
        **CREATE OR REPLACE PACKAGE BODY EXAM_PACK**
        IS
        
            **FUNCTION CAL_BONUS** (VEMPNO IN EMP. EMPNO%TYPE )
            **RETURN NUMBER**
            IS
                VSAL NUMBER(7, 2);
            BEGIN
                SELECT SAL INTO VSAL
                FROM EMP
                WHERE EMPNO = VEMPNO;
                RETURN (VSAL * 200);
        END;
        
        **PROCEDURE CURSOR_SAMPLE02**
        IS
            VDEPT DEPT%ROWTYPE;
            CURSOR C1
            IS
            SELECT * FROM DEPT;
        BEGIN
            DBMS_OUTPUT.PUT_LINE('부서번호 / 부서명 / 지역명 ');
            DBMS_OUTPUT.PUT_LINE('———————————--');
                FOR VDEPT IN C1
                    LOOP
                        EXIT WHEN C1%NOTFOUND;
                        DBMS_OUTPUT.PUT_LINE(VDEPT. DEPTNO||’ '||VDEPT. DNAME || ' '||VDEPT.LOC);
                    END LOOP;
                END;
        END;
        /
        
- **TRIGGER**
    - TRIGGER
        - **어떤 이벤트**가 발생하면 **자동**으로 다른 테이블이 변경되도록 하기 위해 사용
        - 테이블을 삭제하면 트리거도 삭제됨
    - **행 레벨**
        - 행마다 트리거를 발생. **FOR EACH ROW**를 기술
        - CREATE OR REPLACE **TRIGGER** ROW_TRIG
        AFTER UPDATE
        ON EMP02
        **FOR EACH ROW**
        BEGIN
            DBMS_OUTPUT.PUT_LINE('급여가 10%가인상되었습니다.');
        END;
        /
    - **문장 레벨**
        - 단 한번만 트리거를 발생
        - create or replace **trigger** stat_trig
        after update
        on emp01
        begin
            DBMS_OUTPUT.PUT_LINE('10% 44244.');
        end;
        /

- 오류
    - PL/SQL procedure successfully completed
        - set serveroutput on를 안써서!!
