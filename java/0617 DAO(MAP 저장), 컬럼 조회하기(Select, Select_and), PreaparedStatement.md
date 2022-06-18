# 0617

- **DAO - MAP에 저장하기**
    - **DAO**
        - ArrayList <Map<String, Object>>  → ArrayList에 **Map형식**을 하나씩 저장
        - Map형식에 데이터 넣을때는 **put**을 쓰면 됨
        - ArrayLIst에 넣을때는 add를 씀
            - int i = 0;
            while (rs.next()) {
                if(i++ == 0) { 
                    list = new ArrayList<Map<String, Object>>();
                }
                **Map <String, Object> hs = new HashMap <String, Object>();**
                hs.**put**(**"Empno", rs.getInt(1)**);
                hs.put("Ename", rs.getString(2));
                hs.put("Job", rs.getString(3));
                hs.put("Mgr", rs.getInt(4));
                hs.put("Hiredate", rs.getDate(5));
                hs.put("Sal", rs.getInt(6));
                hs.put("Comm", rs.getInt(7));
                hs.put("Deptno", rs.getInt(8));
                list.add(hs);
            }
    - **CRUD**
        - **DAO 객체**를 생성해서 **selectAll() 메서드**를 통해 **ArrayList<Map<String, Object>>**의 **참조형 변수 e**를 얻는다
            - DAO dao = **new DAO();**
            ArrayList<Map<String, Object>> **e** = **dao.selectAll();**
        - **for문**을 통해 List에 저장된 각각의 **Map**형식의 데이터를 불러오고, **get(”키값”)**을 이용하여 **데이터값**을 출력한다
            - for (Map<String, Object> s : e) {
                System.out.printf("%-8s%-8s%-16s%s\t%-16s%s\t%s\t%s\n",
                s.get("Empno"),  s.get("Ename"), s.get("Job"), s.get("Mgr"), s.get("Hiredate"), s.get("Sal"), s.get("Comm"), s.get("Deptno"));
            }
- **Emp 컬럼 조회하기 (Select)**
    - **CRUD**
        - **menuselect**
            - 기능 : emp의 컬럼 중 하나를 선택하는 기능
            - **매개변수로 Scanner 타입**을 넣어서 **inputNumber** 메서드 **호출**할때 쓴다
                - private static int menuselect(Scanner sc) {
                    int i = 0;
                    for (String m : menus) {
                        System.out.println(++i + "." + m);
                    }
                    System.out.print("조회할 컬럼을 선택하세요");
                    return **inputNumber(sc);**
                    }
        - **inputNumber**
            - 기능 : Scanner를 통해 **숫자를 입력**받고 그 숫자를 **반환**
            - **무한반복문**을 만들고
            - **숫자 이외**의 것이 입력되면 **예외처리**
            - l**owNumber와 HiNumber**을 지정하고 **그안에 숫자**가 입력되면 **반복문 벗어나기**
            - **그 외에 숫자**가 입력되면 **다시 반복문** 수행
                - private static int inputNumber(Scanner sc) {
                    int input = 0;
                    int lowNumber = 1;
                    int hiNumber = menus.length; //9
                    **while(true)**
                        try {
                            input = **Integer.parseInt**(sc.nextLine());
                            if (input <= hiNumber && input >= lowNumber) {
                                break;					
                            } else {
                                System.out.println(lowNumber + "~" + hiNumber + "사이의 숫자를입력하세요");
                            }
                        } **catch (NumberFormatException e)** {
                            System.out.println("숫자로 입력하세요>");
                        }
                    return input;
                }
        - **Input**
            - 기능 : 입력받은 값이 9일 경우 빈 문자열 반환
            - 9가 아닐 경우 **searchData 메서드 호출**
                - private static String input(int menu, Scanner sc) {
                    String search_word = "";
                    if (menu != 9)
                        search_word = **searchData(sc,menu);**
                    return search_word;
                }
        - **searchData**
            - 기능 : menus 배열을 이용하여 검색어를 입력받는다
                - private static String searchData(Scanner sc, int menu) {
                    System.out.println("조회할" + menus[menu-1] + "를 입력하세요");
                    return sc.nextLine();
                }
        - **search**
            - 기능 : **dao 객체**를 생성하고 **dao의 search메서드** 호출 (매개변수는 **컬럼번호**와 **검색어**)
            - 출력은 똑같음
                - private static void search(int menu, String search_word) {
                    DAO dao = new DAO();
                    ArrayList<Emp> e = dao.search(menu - 1, search_word);
                }
    - **DAO**
        - 컬럼에 따라  ‘ 를 쓸지 안쓸지 골라써야 한다 + menu 8일때는 기본 문장만 출력
            - menu = 8
                - sql = “select * from emp”
            - menu = 1~7
                
                → sqpl = sql + “where’ + columns[menu] + “=” + single + search_word + single
                
                - menu = 1, 2, 4
                    - single = “ ’ ”
                - else
                    - single = “”
        - String sql = "select * from emp";
        **if (menu != 8)** {
            **String single = ""**;
            **if (menu == 1 || menu == 2 || menu == 4)**
               **single = "'"**;
            sql = sql + " where " + columns[menu] + "=" + **single** + search_word + **single;**
        }
- **Emp 컬럼 조회하기 (Select_and)**
    - **CRUD**
        - **menuselect**
            - 위 예제처럼 menu와 search_word 2가지 모두를 구하는 것이 아니라 **search_word**만 추출한다
            - 추출된 search_word를 **search 배열**에 넣는다
            - search배열을 **DAO 객체로 호출**한다
                - private static String[] menuselect() {
                    Scanner sc = new Scanner([System.in](http://system.in/));
                    String search[] = new String[8];
                    int i = 0;
                    for (String m : menus) {
                        System.out.println(++i + "." + m);
                    }                
                	    int menu = 0;
                	    do {
                		System.out.print("조회할 컬럼을 선택하세요");
                		menu = **inputNumber(sc);**
                		if (menu == 9) {    **→   9일때는 search[8]에 null로 저장된다**
                	            sc.close();
                		    break;
                		}
                		**search[menu-1]** = **searchData(sc, menu);**
                	} while(true);
                	return search;
                }
                
    - **DAO**
        - 위 예제에서 조건이 하나 더 붙는다고 생각하면 된다
        - **where**과 **and**를 앞에 언제 쓰는냐를 결정하면 된다
            - String sql = "select * from emp ";
            String columns[] = { "empno", "ename", "job", "mgr", "hiredate", "sal", "comm", "deptno"};
            for(int i=0; i<searchs.length; i++) {
                if (searchs[i] != null) {
                    String single = "";
                    if (i==1 || i==2 || i==4)
            	    single = "'";            
            	**if (sql.contains("where"))
            	    sql += " and ";
            	else
            	    sql += "where ";**
                    sql += columns[i] + "=" + single + searchs[i] + single;
                }
            }
- **PreparedStatement**
    - **사전에 컴파일**을 함으로써 조건문이 바뀌지 않고 **조건값**이나 **입력값**만 **바뀌는 경우**에 쓰여서 Statement보다 속도가 빠르다
        - Statement는 같은 쿼리라도 매번 컴파일 과정을 거친다
    - 자료형에 따라서 자동으로 **Single quitation(')**을 처리해준다
- **PreparedStatement 형식**
    - Connection conn = null;
    **PreparedStatement pstmt = null;    →   Statement 대신에 PreparedStatement를 쓴다**
    ResultSet rs = null;
    
    	try {
    		Class.forName("oracle.jdbc.driver.OracleDriver");    **→**   **JDBC 드라이버 도르**
    		String url = "jdbc:oracle:thin:@localhost:1521:xe";    **→**    **DB에 연결**
    		conn = DriverManager.getConnection(url, "scott", "tiger");     
            String **sql** = "insert into test (no, name) " + **"values (?, ?)";**
            **pstmt = conn.prepareStatement(sql);   →   원래는 sql문을 다 만들어놓고 excuteQuery()에서 컴파일 했는데, 여기서는 prepareStatement에서 미리 컴파일함**
            **pstmt.setInt(1, 900302);**     →    **컴파일한 후에 ? 부분에 대한 내용을 set메서드를 이용하여 입력한다**
            **pstmt.setString(2, “조재인”);
            rs = pstmt.executeQuery();     →   문장 실행**
