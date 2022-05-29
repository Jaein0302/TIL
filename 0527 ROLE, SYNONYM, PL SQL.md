
# ROLE

- **ROLE**
    - ROLE이란?
        - 사용자에게 보다 효율적으로 권한을 부여할 수 있도록 여러 개의 **권한을 묶어 놓은 것**
    - 종류
        - **CONNECT ROLE**
            - 데이터베이스에 **접속 가능**
        - **RESOURCE ROLE**
            - 사용자가 **객체를 생성 가능**
        - **DBA ROLE**
            - 시스템 관리에 필요한 모든 권한
    - **USER_ROLE_PRIVS**
        - **현재 user**에게 부여된 롤을 확인
    - **ROLE_TAB_PRIVS**
        - 해당 테이블에 접근 가능한 **객체 권한**에 대한 정보 확인
    - **ROLE_SYS_PRIVS**
        - 해당 테이블에 접근 가능한 **시스템 권한**에 대한 정보 확인
    - **시스템 권한** 부여 (**생성** → **롤**에 권한 부여 → **사용자**에 롤 부여)
        - **create** role MROLE;
        - **grant** connect, resource **to** MROLE;
        - **grant** MROLE **to** user;
    - **객체 권한** 부여
        - 객체에 대한 권한은 해당 **스키마** 또는 **system**에서 가능
        - **grant** update, delete, select **on** emp **to** def_role;
    - **REVOKE**
        - **revoke** mrole02 **from** user;
        - user_role_privs에서 사라지지 않음
    - **DROP**
        - **drop role** mrole;
        - uesr_role_privs에서 사라짐

# SYNONYM

- 동의어
    - **객체의 소유자**를 **간단한 이름으로 접근**할 수 있게 함
- 권한 부여
    - **grant create synonym** **to** scott;
- 종류
    - 비공개 동의어
        - 해당 계정에서 동의어를 만들면 **그 계정에서만** 동의어를 쓸수 있음
        - **create sysnonym** prisystbl **for** system.systbl;
        - 삭제할때도 그 **계정에 접속**후 삭제가능 → **drop synonym** dept;
    - 공개 동의어
        - system 계정에서 동의어를 만들면 **다른 계정에서도** 쓸 수 있음
        - dual도 원래 sys.dual로 표현되어야 하지만 공개 동의어로 지정되었어서 생략
        - **create public synonym** pubDEPT **for** scott.dept;
        - **system 계정**에 접속후 가능 → **drop public synonym** dept;

# PL/SQL

- 왜 쓰는가?
    - 변수 선언, 비교 처리, 반복 처리 기능은 절차적 언어! 효율적으로 SQL문을 사용할 수 있음
    - 출력을 위해선 **set serveroutput on**을 써줘야 함
- 블록구조
    - **선언부**(DECLARE SECTION):선택항목
        - PL/SQL에서 사용하는 모든 변수나 상수를 선언하는 부분으로서 **DECLARE**로 시작합니다.
        - **스칼라 변수**
            
            declare
            
            vempno **number(4);**
            
            vdeptno number(2) **not null** **:= 20;   →  not null은 초기화 필수**
            
        - **레퍼런스 변수**
            
            declare
            
             vempno **emp.empno%type**;
            
        - **%ROWTYPE**
            
            declare
            
            vemp **emp%rowtype;**
            
    - **실행부**(EXECUTABLE SECTION) : 필수항목
        - 절차적 형식으로 SQL문을 실행할 수 있도록 절차적 언어의 요소인 제어문, 반복문, 함수 정의 등 로직을 기술할 수 있는 부분으로 **BEGIN**으로 시작합니다. 끝에는 **END;**를 적어줘야함
        - begin
            
                 select empno, ename
            
            **into vempno, vename**
            
            from emp
            
            where ename = ‘SCOTT’;
            
            end;
            
    - **예외 처리**(EXCEPTION SECTION):선택항목
        - PL/SQL 문이 실행되는 중에 에러가 발생할 수 있는데 이를 예외사항이라고 합니다. 이러한 예외 사항이 발생했을 때 이를 해결하기 위한 문장을 기술할 수 있는 부분으로 **EXCEPTION** 으로 시작합니다.
            - EXCEPTION
              **WHEN NO_DATA_FOUND THEN**
                 DBMS_OUTPUT.PUT_LINE('조회된 결과가 없습니다.');
              **WHEN TOO_MANY_ROWS THEN**
                DBMS_OUTPUT.PUT_LINE('자료가 2건 이상입니다.');
              **WHEN OTHERS THEN**
                DBMS_OUTPUT.PUT_LINE('기타 에러입니다.');
            END;
- 선택문
    - **IF**
        - declare
            
                vdeptno emp.deptno%type
            
               vdname varchar2(20) := null;
            
            begin
            
                select deptno
            
                into vdeptno
            
                emp
            
                empno = 7788;
            
                **if (vempno = 10) then**
            
                  **vdname := ‘ACCOUNGTING’;**
            
                **end if;**
            
            end;
            
    - **IF ~ ELSE**
        - declare
            
                vemp emp%rowtype;
            
                annsal number(7,2);
            
            begin
            
                select *
            
                into vemp
            
                from emp
            
                where ename = ‘SCOTT’;
            
                **if (vemp.comm is null) then**
            
                  **annsal := vemp.sal*12;**
            
                **else**
            
                  **annsal := vemp.sal*12 + vemp.comm;**
            
                **end if;**
            
            end;
            
    - **IF ~ ELSIF ~ ELSE**
        - BEGIN
            
            **IF** (VEMP.DEPTNO = 10) **THEN**
                VDNAME := 'ACCOUNTING';
            **ELSIF** (VEMP.DEPTNO = 20) **THEN**
                VDNAME := 'RESEARCH';
            **END IF;**
            
    - **LOOP**
        - DECLARE
            
                N NUMBER := 1;
            
            BEGIN
            
                **LOOP**
            
                  DBMS_OUT.PUT_LINE (N);
            
                  N := N+1;
            
                  IF N>5 THEN
            
                     EXIT;
            
                  END IF;
            
                **END LOOP;**
            
            END;
            
    - **FOR LOOP**
        - declare
            vdept dept%rowtype;
        begin
            **for** N **in** **1..4 loop     → N 선언 안해도 됨**
                select * into vdept
                from dept
                where deptno = 10*N;
                DBMS_OUTPUT.PUT_LINE(vdept.deptno||'/'||vdept.dname||'/'||vdept.loc/);
            end loop;
        end;
    - **WHILE LOOP**
        - declare
            
                **N number := 1;**
            
            begin
            
                **while N ≤ 5      → N 선언 해야함**
            
                    **loop**
            
                        DBMS_OUTPUT.PUT_LINE( N );
            
                        N := N+1;
            
                    **end loop;**
            
            end;        
            
        - declare
            N number := 1;
            S varchar2(10) := null;
        begin
            **while N<5 loop**
                S := S || '*';
                DBMS_OUTPUT.PUT_LINE(S);
                N := N+1;
            **end loop;**
        end;
- 대체 변수
    - 종류
        - **&변수명** : PL/SQL문이 실행될 때 마다 입력을 받는다
        - **&&변수명** : 한 번 입력하면 세션안에서 계속 입력된 내용으로 사용
    - DECLARE
        
            VDAN NUMBER := **&DAN;**
            VEND NUMBER := **&&INEND**;
        BEGIN
            FOR VCNT IN 1..VEND
                LOOP
                    DBMS_OUTPUT.PUT_LINE( VDAN || '*' || VCNT ||'=' || (VDAN * VCNT));
                END LOOP;
        END;
        
- **ACCEPT 변수**
    - ACCEPT.SQL에 저장해야함
    - **ACCEPT P_DEPTNO PROMPT** ‘입력’
    DECLARE
    V_DEPTNO EMP.DEPTNO%TYPE := **&P_DEPTNO;**
    BEGIN
    DBMS_OUTPUT.PUT_LINE (V_DEPTNO);
    END;
