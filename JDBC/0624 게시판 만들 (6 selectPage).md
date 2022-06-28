# 6번째 기능 - selectPage


**Main**

- **selectPage**
    - **(1), (2)** **dao.count**를 통해 현재 저장되어 있는 **row의 개수**를 구한다
    - **(3)** **한 페이지에 보여줄 목록의 숫자(limit)**를 입력받고, 이 limit을 통해 페이지의 수(maxpage)를 구한다
    - 1~maxpage 중 사용자가 **보고 싶어하는 페이지의 숫자(page)**를 입력받는다
    - (4) 위에서 구해진 **page**와 **limit**를 인자로 하여 **dao.getBoardList**를 호출한다
    - (8) **printTitle**을 통해 컬럼들을 출력하고 dao.getBoardlist로부터 반환받은 Arraylist의 참조변수를 통해 출력한다
    - **전체문장**
    
    ```java
    private static void selectPage(Scanner sc, BoardDAO_seq dao) {
    		int listcount =  dao.count();
    		
    		if (listcount > 0) {
    			System.out.println("한 페이지에 보여줄 목록을 입력하세요>");
    	        int limit = inputNumber(sc);
    		    int maxpage = (listcount-1) / limit + 1;
    		    System.out.println("선택할 페이지를 입력하세요(1~" + maxpage + ")" );
    		    
    		    int page = inputNumber(sc, 1, maxpage);
    		    List<Board> arrs = dao.getBoardList(page, limit);
    		    System.out.println("글의 총 개수 :" + listcount);
    		    if (arrs != null) {
    		    	printTitle();
    		    	for (Board b : arrs) {
    		    		System.out.println(b.toString());
    		    	}
    		    } else {
    	    	System.out.println("테이블에 데이터가 없습니다.");
    		    }
    		}
    	}
    ```
    

**DAO**

- **count**
    - 현재 저장되어 있는 **row의 개수**를 구하는 쿼리문은 다음과 같다
        - **select count(*) from board**
    - count(*)이라는 컬럼이 첫번째 index이기 때문에 아래와 같이 count값을 가져올 수 있다
        - if (rs.next())
            count = **rs.getInt(1)**
    - **전체 문장**
    
    ```java
    public int count() {
    		PreparedStatement pstmt = null;
    		Connection conn = null;
    		ResultSet rs = null;
    		int count = 0;
    		
    		try {
    			Class.forName("oracle.jdbc.driver.OracleDriver");
    			String url = "jdbc:oracle:thin:@localhost:1521:xe";
    			conn = DriverManager.getConnection(url, "board", "1234");			
    			pstmt = conn.prepareStatement("select count(*) from board");
    			rs = pstmt.executeQuery();
    			
    			if (rs.next())
    				count = rs.getInt(1);   //  1 대신에 "COUNT(*)" 써도 됨
    		} catch (Exception ex) {
    			ex.printStackTrace();
    			System.out.println("getBoardList() 에러: " + ex);
    		} finally {
    			try {
    				if(rs!=null)
    					rs.close();
    			} catch (SQLException ex) {
    				ex.printStackTrace();
    			}
    			try {
    				if (pstmt != null)
    					pstmt.close();
    			} catch (SQLException ex) {
    				ex.printStackTrace();
    			}
    			try {
    				if (conn != null)
    					conn.close();
    			} catch (SQLException ex) {
    				ex.printStackTrace();
    			}
    		}
    		return count;
    	}
    ```
    
- **getBoardList**
    - getBoardList는 6번째 기능에서 했던 메서드이기 때문에 아래 링크를 참고하면 되겠다
        - [getBoardList 분석글](https://www.notion.so/0623-5-selectAll-7f10db4950c3489c973482b94e8d000f)
