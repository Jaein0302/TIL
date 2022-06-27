# 5번째 기능 - selectAll


## main


- **selectAll**
    - 전체 데이터 중 **첫 페이지만 보여주는 기능**이다
    - (1) **getBoardList**를 호출할때 인수값으로 **조회하고 싶은 페이지**와 **페이지 당 목록의 수**를 넣고 호출한다
    - (5) getBoardList에서 반환된 **arraylist**를 가지고 **toString**으로 하나씩 출력해준다
        
        ```java
        private static void selectAll(BoardDAO_seq dao) {
        		//1 - 페이지
        		//10 - 페이지 당 목록의 수(limit)
        		List<Board> arrs = dao.getBoardList(1,10);
        		if (arrs != null) {
        			printTitle();
        			for (Board b : arrs) {
        				System.out.println(b.toString());
        			}
        		} else {
        			System.out.println("테이블에 데이터가 없습니다");
        		}
        	}
        ```
        

## DAO

- **getBoardList**
    - (2) getBoardlist는 매개변수로 **조회하고 싶은 페이지를 page, 페이지당 목록의 수를 limit로 전달받았다**
    - **page** = **0**, **limit** = **10**으로 전달받았기 때문에 그 의미는 첫 페이지의 10개까지만 출력한다는 얘기이다.
        - 이것을 이용하여 목록의 첫 시작인 **startrow = (page - 1) * limit + 1**
        - 목록의 마지막인 **endrow = startrow + limit - 1** 으로 구한다
    - 정해진 데이터에서 단순히 임시적으로 **순번만을 정하는 rownum**의 특징을 잘 이용해야 한다
    - 정해진 데이터란 순서를 이미 정해놓은 상태에서 rownum은 거기에 순번만 만든다는 의미이다
    - 가장 안쪽 쿼리를 분석해보면 원글 번호라고 할수 있는 **ref**는 **내리차순**,  같은 ref 속에서 seq가 생기기 때문에 **seq**는 **오름차순**으로 쿼리문을 짜야한다.
        - select * from board 
        order by **BOARD_RE_REF desc**,
        **BOARD_RE_SEQ asc**
    - 위 커리를 **A**라고 했을때 from절에 넣고 **rownum을 설정**하는 쿼리문을 작성할것이다.
    - **rownum의 범위**를 **endrow**까지 지정한다
        - select **rownum rnum**,BOARD_NUM,BOARD_NAME,
                   BOARD_SUBJECT, BOARD_CONTENT,BOARD_FILE,
                   BOARD_RE_REF,BOARD_RE_LEV,BOARD_RE_SEQ,
                   BOARD_READCOUNT,BOARD_DATE 
        **from  A**
        where rownum<= **endrow**
    - 위 커리를 통해 글번호가 생겼고, 이 커리를 **B**라고 하자. 여기서 우리는 다시한번 범위를 지정해줘야한다
    - rownum의 별칭을 **rnum**으로 지정했고, rnum의 범위를 **startrow**에서 **endrow**로 지정한다
    - 이제 위 조건을 이용하여 마지막 쿼리문을 작성해보자
        - select *
        from **B
        where rnum ≥ startrow and rnum ≤ endrow**
    - **왜 별칭을 두어야 하는지**는 다음 링크를 참고하자
        - [rownum 분석](https://www.notion.so/ROWNUM-b86e4ab0b3be489e921c08f1da5a592c)
    - (3) 작성한 쿼리문을 가지고  **DB에서 조회**하고 **Board에 저장**시킨다
    - (4) 저장된 Board를 하나씩 **Arraylist에 저장**시킨다
    - 전체 코드
        
        ```java
        public List<Board> getBoardList(int page, int limit) {
        		PreparedStatement pstmt = null;
        		Connection conn = null;
        		ResultSet rs = null;
        		// page : 페이지
        		// limit : 페이지 당 목록의 수
        		// BOARD_RE_REF desc, BOARD_RE_SEQ asc에 의해 정렬한 것을
        		// 조건절에 맞는 rnum의 범위 만큼 가져오는 쿼리문입니다.
        		String board_list_sql = 
        				  "select * "
        				+ "from " 
        		        + "     (select rownum rnum,BOARD_NUM,BOARD_NAME,"
        				+ "             BOARD_SUBJECT, BOARD_CONTENT,BOARD_FILE," 
        		        + "             BOARD_RE_REF,BOARD_RE_LEV,BOARD_RE_SEQ,"
        				+ "             BOARD_READCOUNT,BOARD_DATE from " 
        		        + "                                           (select * from board "
        				+ "                                            order by BOARD_RE_REF desc,"
        				+ "                                            BOARD_RE_SEQ asc)"
        				+ "      where rownum<=?) " 
        				+ "where rnum>=? and rnum<=?";
        
        		List<Board> list = null;
        		// 한 페이지당 10개씩 목록인 경우                              1페이지, 2페이지, 3페이지, 4페이지 ...
        		int startrow = (page - 1) * limit + 1; // 읽기 시작할 row 번호(1    11     21      31 ...
        		int endrow = startrow + limit - 1;    // 읽을 마지막 row 번호(10    20     30      40 ...
        		
        		try {
        			Class.forName("oracle.jdbc.driver.OracleDriver");
        			String url = "jdbc:oracle:thin:@localhost:1521:xe";
        			conn = DriverManager.getConnection(url, "board", "1234");
        			pstmt = conn.prepareStatement(board_list_sql);
        			pstmt.setInt(1, endrow);
        			pstmt.setInt(2, startrow);
        			pstmt.setInt(3, endrow);
        			rs = pstmt.executeQuery();
        
        			int count=0;
        			while (rs.next()) {
        				if(count++ == 0)
        					list = new ArrayList<Board>();
        				Board board = new Board();
        				board.setBOARD_NUM(rs.getInt("BOARD_NUM"));
        				board.setBOARD_NAME(rs.getString("BOARD_NAME"));
        				board.setBOARD_SUBJECT(rs.getString("BOARD_SUBJECT"));
        				board.setBOARD_CONTENT(rs.getString("BOARD_CONTENT"));
        				board.setBOARD_FILE(rs.getString("BOARD_FILE"));
        				board.setBOARD_RE_REF(rs.getInt("BOARD_RE_REF"));
        				board.setBOARD_RE_LEV(rs.getInt("BOARD_RE_LEV"));
        				board.setBOARD_RE_SEQ(rs.getInt("BOARD_RE_SEQ"));
        				board.setBOARD_READCOUNT(rs.getInt("BOARD_READCOUNT"));
        				board.setBOARD_DATE(rs.getString("BOARD_DATE"));
        				list.add(board); // 값을 담은 객체를 리스트에 저장합니다.
        			}
        			return list; //  리스트를 호출한 곳으로 가져갑니다.
        		} catch (Exception ex) {
        			ex.printStackTrace();
        			System.out.println("getBoardList() 에러 : " + ex);
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
        		return null;
        	}
        ```
