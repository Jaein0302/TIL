# 05.18 / 오라클 SELECT

# 데이터베이스 관리 시스템(DBMS)

- **데이터베이스(DataBase)**
    - 지속적으로 유지 관리해야 하는 데이터의 집합
- **데이터베이스 관리 시스템(DataBase Management System)**
    - 방대한 양의 데이터를 편리하게 저장하고 효율적으로 관리하고 검색할 수 있는 환경을 제공해주는 시스템 소프트웨어
    - 데이터를 공유하여 정보의 체계적인 활용을 가능하게 합니다
    - 응용 프로그램과 데이터베이스의 중재자로서 모든 응용 프로그램들이 데이터베이스를 공용할 수 있게끔 관리해 주는 소프트웨어 시스템입니다.

# 데이터베이스의 특징

- **실시간 접근성(Real-time accessability)**
    - 다수의, 사용자의 요구에 대해서 처리 시간이 몇 초를 넘기지 말아야 한다는 의미입니다.
- **지속적인 변화(Continuos evolution)**
    - 데이터벤이스에 저장된 데이터는 최신의 정보가 정확하게 저장되어 처리되어야 합니다.
- **동시공유(Concurrent sharing)**
    - 동일 데이터를 동시에 서로 다른 목적으로 사용할 수 있어야 합니다.
- **내용에 대한 참조**
    - 데이터베이스 내에 있는 데이터,레코드들은 주소나 위치에 의해 참조되는 것이 아니라 가지고 있는 값에 따라 참조해야 합니다.

# 관계형 데이터베이스 관리 시스템

- 관계형 데이터베이스 정보를 테이블 형태로 저장합니다.
- 테이블은 2차원 형태의 표처럼 볼 수 있도록 로우(ROW:행)와 칼럼(COLUMN: 열)으로 구성합니다.
- 관계형 데이터베이스 관리시스템(RDBMS: Relational DataBase Management System)은 가장일반적인 형태의 DBMS 입니다.
- 오라클(Oracle), 사이베이스(Sybase), 인포믹스(Infomix), MYSQL, Access, SQL Server
- 장점
    - 작성과 이용이 비교적 쉽고 확장이 용이하다.
    - 처음 데이터베이스를 만든 후 관련되는 응용 프로그램들을 변경하지 않고도, 새로운 데이터 항목을 데이터베이스에 추가할 수 있다.
- **데이터 딕셔너리(Data Dictionary: DD)**
    - 관계형 데이터베이스에서 객체를 정의하게 되면 그 객체가 가진 메타 데이터(metadata-객체에 대한 정보들, 예를 들면 테이블 객체일 경우에는 컬럼, 도메인 및 제약 조건에 대한 내용)의 정보가 저장되는 곳입니다.
    - 사용자에 의해서 추가, 삭제, 수정되지 못하며 오로지 오라클 시스템에 의해서만 가능합니다
- **SQL(Structured Query Language)**
    - 사용자와 관계형 데이터베이스를 연결시켜 주는 표준 검색 언어

# 데이터베이스 시스템을 사용하는 사용자

1. **데이터 베이스 관리자(DBA)**
    
    데이터베이스 설계와 정의, 관리 및 운영 등 데이터베이스 시스템을 관리하고 제어하는 사용자
    
2. **응용 프로그래머(Application Programmer)**
데이터베이스를 실제적으로 설계하여 최종 사용자들의 요구에맞는 인터페이스와 응용 프로그램을 개발합니다.
3. **최종 사용자(End User)**
데이터 베이스를 실질적으로 사용하는 사용자 입니다.

# SQL plus 환경설정

- 계정을 2개 만듬
    - sys계정 (SYS)
        - **sqlplus sys/1234 as sysdba**
    - 그외 계정 (SYSTEM)
        - **sqlplus system/1234**
- scott 계정 활성화
    - 일단 system계정으로 들어와서
    - **@scott.sql** 입력
- oracle 폴더에 **login.sql**제목으로 메모장 저장
    - 내용은 **set sqlprompt '_user>’**
    - 기본값으로 정하고 싶은 것들을 적으면 됨
- 계정전환
    - sys 계정 전환 방법 : **connect sys/비밀번호 as sysdba**
    - 그 외 계정 전환 방법 : **connect 계정/비밀번호**
- 테이블 목록 출력
    - **select * from tab;**
- 종료하기
    - **quit** 또는 **exit**
- scott.spl 실행하는 방법
    - system 계정에 접속한 상태에서 **@scott.sql**
- scott 비밀번호 변경하기 (system이나 sys 계정에서 변경)
    - **alter user scott identified by tiger;**
- scott계정 lock 해제  (system이나 sys 계정에서 변경)
    - **alter user scott account unlock;**
- 쓰레기통 확인
    - **show recyclebin;**
- 쓰레기통 비우기
    - **purge recyclebin;**
- login.sql에 **“set linesize 200”**을 저장하면 나중에 다시 켜도 그대로 볼 수 있음
- DEPT 테이블의 구조 살피기
    - **DESC DEPT**
- **NULL**의 의미
    - 컬럼에 어떠한 값도 정해지지 않을 때 갖는 값, 즉 데이터가 없음을 의미
    - Null값은 연산처리를 안함
- *****의 의미
    - 모든 컬럼

# 데이터 형

- **number**
    - **n1 number (a,b)**
        
        변수, 자료형, a=전체자리수, b=소수점 자리수
        
- **date**
    - **‘YY/MM/DD’**
- **char**
    - 고정 길이 문자 데이터를 저장
- varchar2
    - 가변적인 길이의 문자열을 저장

# SELECT문

- **select [column 종류] from [table_name]**
- **select sal + comm from emp;**
    - sal과 comm column을 합함
- **select nvl(comm,0) from emp;**
    - comm column에서 null을 0으로 바꿔주는 함수
- **select sal+nvl(comm,0) as annsal**
    - sal과 nvl의 제목을 annal로 바꿈
    - 대소문자 구분하고 싶거나 한글로 쓰고 싶을 때는 **“ ”**
- **select ename || ‘ is a ‘ || job from emp;**
    - 중간에 공백이 아니라 문자를 적어서 하나로 연결할때 쓰임
- **select distinct dpetno from emp;**
    - 중복되는 번호를 한번씩만 출력

# SQL plus 명령어

- SQL plus 명령어 VS SQL 명령어
    - **SQL**은 데이터베이스에서 자료를 검색하고 수정하고 삭제하는 등을 위한 데이터베이스 언어인 반면 **SQL*Plus 명령어**는 툴에서 출력 형식을 지정하는 등 환경을 설정합니다.
- 편집 명령어
    - **LIST (L**) : 버퍼에 저장된 모든 SQL 문 또는 검색한 라인의 SQL 문을 나타낸다.
    - **/** :  SQL 문을 보여주지 않고 바로 실행한다. **(가장 최근)**
    - **RUN (R)** : 버퍼에 저장된 SQL 문을 보여주고 실행한다
- 파일 명령어
    - **EDIT (ED) :** 파일의 내용을 vi(유닉스)나 notepad (윈도우즈) 와 같은 에디터로 읽어 편집
    할 수 있도록 한다.
        - ed만 입력하면 파일 이름이 “afiedt.buf”로 된다
    - **HOST** : 오라클을 종료하지 않고 OS 명령을 수행할 수 있도록 OS 환경으로 잠시 빠져 나갈 수 있도록 한다. OS Prompt 상에서 Exit 하면 다시 오라클 환경으로 돌아온다.
    - **SAVE** : SQL 버퍼 내의 현재 내용을 실제 파일로 저장한다.
        - **REPLACE** : 내용을 덮어쓸때
        - **APPEND** : 이미 존재하는 파일에 내용 추가하기
    - **@** : SQL 파일에 저장된 내용을 실행한다.
    - **SPOOL** : 오라클 화면을 갈무리하여 파일로 저장한다.
        - **spool off**를 하지 않고 오라클을 종료하게 되면 지금까지 갈무리한 내용이 저장되지 않고 날아가 버린다
    - **GET** : 파일의 내용을 SQL 버퍼로 읽어 들인다.
    - **EXIT** : 오라클을 종료한다.
- SET 명령어
    - **HEADING (HEA) on | off** : SELECT 명령어를 수행한 후 실행결과가 출력될 때 컬럼의 제목을 출력할 것인지의 여부를 제어한다.
    - **LINESIZE (LIN) n** : SELECT 명령어를 수행한 후 결과를 출력할 때 **한 라인**에 출력할 최대 문자(Character) 수를 결정한다.
    - **PAGESIZE (PAGES) n** : SELECT 명령어를 수행한 후 결과를 출력할 때 **한 페이지**에 출력할 최대 라인 수를 결정한다.
    - **COLUMN FORMAT** : 칼럼 데이터에 대한 출력 형식을 다양하게 지정하기 위한 명령
        - 99999 : 적게 출력시 **공란**
        - 00000 : 적게 출력시 **0으로 채움**
- WHERE 조건과 비교연산자
    - **select [column] from [table_name] where 조건절;**
        - 자바와 다르게 ‘=’가 대등조건문 (자바에서는 ==)
    - 문자 데이터 조회
        - 단일 따옴표로 표시 (where ename = ‘ford’)
        - 대소문자 구분해서 조회
