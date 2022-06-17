# 0616

- **JDBC**
    - **자바에서 오라클 연결하기** **(Select)**
        - **JDBC 드라이버**를 로드한다
            - **String driver = "oracle.jdbc.driver.OracleDriver";**
            **Class.forName(driver);**
        - **DB에 연결**한다
            - **String url =** "jdbc:oracle:thin:@localhost:1521:xe";
            **conn =** **DriverManager.getConnection**(**url**, "scott", "tiger");
            **stmt** = **conn.createStatement();   →  데이터베이스에 있는 테이블의 데이터를 읽어오기 위해 Statement 객체 필요**
        - **명령문** 작성
            - **String select_sql** = "select * from dept";
            **rs** = **stmt.executeQuery(select_sql)**;      **→   ResultSet 객체는 Select절에만 쓰임**
        - **데이터 읽어오기**
            - int i = 0;
            **while (rs.next())** {   →   **다음 행 위치로 이동하는 메서드 (boolean형) (더이상 읽을 데이터가 없을때까지 반복)**
                int deptno = rs.getInt**(1);      →    출력하는 컬럼 순서대로 인덱스 사용 가능**
                String dname = rs.getString**(2);**
                String loc = rs.getString**(3);**
                System.out.printf("%5d\t%5d\t%-15s%s\n", ++i, deptno, dname, loc);
            - 날짜 형식 불러오기
                - String hiredate = rs.**getString**("hiredate").**substring**(0,10);  →  제일 깔끔하고 형식을 자유롭게 바꿀 수 있음
                - Date hiredate = rs.getDate("hiredate");
        - DB 연결을 끊는다     →    연결한 순서와 반대로 끊는다
            - **if (rs != null)**
            rs.close();
            - **if (stmt != null)
            stmt.close();**
            - **if (conn != null)
            conn.close();**
    - **자바에서 오라클 연결하기** **(DML)**
        - select문과 비슷하지만 **executeQuery**를 쓰지 않고, **executeUpdate**를 쓴다
            - int **rowNum** = **stmt**.**executeUpdate**(sql); 를 추가한다
            - **row count**를 return 하거나 nothing일때는 **0**을 return한다
        - 예외처리할때도 rs는 쓰지 않는다
- **DAO 활용하기**
    - **Emp**
        - emp 테이블에 있는 **컬럼명을 필드로 저장**하고 **getter, setter를 생성**한다
        - **toString** 오버라이딩  →  출력하는 문장 생성
            - public String **toString()** {
            return  String.format("%-8s%-8s%-16s%s\t%-16s%s\t%s\t%s\t", empno,  ename, job, mgr, hiredate, sal, comm, deptno);
            }
    - **DAO**
        - **Data Transfer Objec**t : 데이터를 저장하고 운반
        - **ArrayList<Emp>**을 반환하는 **SelectAll**이라는 메서드를 만든다
        - 오라클에 연결하고 **select절 명령문을 수행**시킨다
        - 출력을 하는것이 아니라, **Emp클래스의 필드**에 **set메서드**를 이용하여 하나씩 **저장**시킨다
            - int i = 0;
            while (rs.next()) {
                **if(i++ == 0)** {   →   **0만족하고 i가 증가하니깐 그다음부터는 만족하지 않음 (1회용)** 
                    **list = new ArrayList<Emp>();    →   - Arraylist 객체 한번만 만듬**
            }
            **Emp st = new Emp();     →   Emp 객체를 만들고 그안에 데이터를 하나씩 저장**
            st.setEmpno(rs.getInt(1));
            st.setEname(rs.getString(2));
            st.setJob(rs.getString(3));
            st.setMgr(rs.getInt(4));
            st.setHiredate(rs.getDate(5));
            st.setSal(rs.getInt(6));
            st.setComm(rs.getInt(7));
            st.setDeptno(rs.getInt(8));
            **list.add(st);**	**→   Emp 객체를 ArrayList에 저장**		
            }
        - **list**라는 ArrayList<Emp>의 참조변수를 return시킨다
            - return **list**;
    - **CRUD**
        - **CRUD** : 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능
            - C(**Create** - Insert)
            - R(**read** - Select)
            - U(**Update** - update)
            - D(**Delete** - delete)
        - **emp의 모든 정보를 조회**하는 클래스를 통해 가져온 **데이터를 출력**
        - **DAO 객체**를 만들고, 그안에 **selectAll() 메서드** 호출 (return타입이 ArrayList<Emp>) ⇒ **list**가 반환됨
            - DAO **dao** = new DAO();
            ArrayList<Emp> **e** = **dao.selectAll()**;
        - for문을 이용하여 출력
            - **for (Emp s : e)** {
            System.out.println(**s.toString()**);    →   Emp클래스에서 toString을 오버라이딩 했음
