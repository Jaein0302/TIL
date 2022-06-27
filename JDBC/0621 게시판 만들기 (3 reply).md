# 3번째 기능 - reply


## main

- **reply**
    - **(1)** 답변을 달 글 번호를 입력받는데 **inputNumber**를 통해 정수만 입력받는다
    - **(2)** 입력받은 **정수(num)**와 Dao 객체의 참조변수 **dao**를 인수로 **select** 메서드를 호출하여 해당 row에 있는 데이터를 가져온다
    - (3) **board**에 **답변의 데이터**(글쓴이, 제목, 내용, 비밀번호)를 **저장**한다
    - (4) **boardReply**를 호출하여 입력한 데이터들을 DB에 저장시키고 반환값 int형을 반환시킨다
        
        ```java
        private static void reply(Scanner sc, BoardDAO_seq dao) {
        		System.out.println("답변을 달 글 번호를 입력하세요>");
        		int num = inputNumber(sc);
        		Board board = select(dao, num);
        		
        		if (board != null) {		
        			System.out.println("글쓴이>");
        			board.setBOARD_SUBJECT(sc.nextLine());
        			
        			System.out.println("제목>");
        			board.setBOARD_SUBJECT(sc.nextLine());
        			
        			System.out.println("글 내용>");
        			board.setBOARD_CONTENT(sc.nextLine());
        		
        			System.out.println("비밀번호>");
        			board.setBOARD_PASS(sc.nextLine());
        			
        		int result = dao.boardReply(board);
        		if ( result != 0) {
        			System.out.println("답변 달기 성공");
        		} else
        			System.out.println("답변 달기 실패");
        		}
        	}
        ```
        
- **boardReply**
    - 답변을 다는 기능 중에 제일 중요한 것은 어느 위치에 들어가는지이다
        - **re_ref** : 원문 글 그룹 번호
        - **re_lev** : 답글의 깊이
        - **re_seq** : 원문 글 그룹에서의 순서 (원문은 0)
    - **update**와 **insert** 두가지가 모두 만족해야 commit이 되는 transaction을 활용할 것이다
       
        
        - **update**
            - 답글이 들어가는 원문 글에 **다른 답글이 있을 경우** 다른 답글들은 **re_seq를 +1** 해줘야 한다
            - 위 그림처럼 **8번 글을 답글로 추가**한다고 했을때 **같은 원문글에 포함**되고, **8번 답글보다 seq가 높은 글**들은 **re_seq를 1씩 더해줘야 한다**
            - 위와 같은 조건을 쿼리로 나타내면 다음과 같다
                - sql = " update board "
                          + "set    BOARD_RE_SEQ = **BOARD_RE_SEQ + 1 "**
                          + "**where  BOARD_RE_REF = ?** "
                          + **"and    BOARD_RE_SEQ > ?";**
            
            		pstmt = conn.prepareStatement(sql);
            		pstmt.setInt(1, re_ref);
            		pstmt.setInt(2, re_seq);
            
        - **insert**
            - 주변 환경이 정리가 되었으니깐 답글의 당사자인 8번 글을 입력해보도록 한다
            - 답글은 원글 바로 아래에 오는 것이기 때문에 8번글의 **lev, seq는 원글보다 1씩 커야 한다**
            - 또한, **(3)에서 board에 입력한 수정된 데이터값**들을 가져와서 쿼리문을 작성한다
            - **update**와 **insert** 작업이 모두다 끝났을때 **commit**을 해준다
            - 이와 같은 조건을 방영한 커리문은 다음과 같다
                - **re_seq = re_seq + 1;
                re_lev = re_lev + 1;**
            
            		sql = "insert into board "
            		     + "(BOARD_NUM,BOARD_NAME,BOARD_PASS,BOARD_SUBJECT,"
            			 + " BOARD_CONTENT, BOARD_FILE, BOARD_RE_REF,"
            		     + " BOARD_RE_LEV, BOARD_RE_SEQ,"
            			 + " BOARD_READCOUNT,BOARD_DATE) "
            		     + "values(board_seq.nextval,?,?,?,?,?,?,?,?,?,sysdate)";
            
            		pstmt = conn.prepareStatement(sql);
            		pstmt.setString(1, **board.getBOARD_NAME()**);
            		pstmt.setString(2, **board.getBOARD_PASS()**);
            		pstmt.setString(3, **board.getBOARD_SUBJECT()**);
            		pstmt.setString(4, **board.getBOARD_CONTENT()**);
            		result = pstmt.executeUpdate();
            		conn.**commit();**
            
        
        ```java
        public int boardReply(Board board) {
        		Connection conn = null;
        		PreparedStatement pstmt = null;
        		String sql = "";
        		int result = 0;
        
        		int re_ref = board.getBOARD_RE_REF();
        
        		int re_lev = board.getBOARD_RE_LEV();
        
        		int re_seq = board.getBOARD_RE_SEQ();
        
        		try {
        			Class.forName("oracle.jdbc.driver.OracleDriver");
        			String url = "jdbc:oracle:thin:@localhost:1521:xe";
        			conn = DriverManager.getConnection(url, "board", "1234");
        			
        			// 트랜잭션을 이용하기 위해서 setAutoCommit을 false로 설정합니다.
        			conn.setAutoCommit(false);
        
        			// BOARD_RE_REF, BOARD_RE_SEQ 값을 확인하여 원문 글에 다른 답글이 있으면
        			// 다른 답글들의 BOARD_RE_SEQ값을 1씩 증가시킵니다.
        			// 현재 글을 다른 답글보다 앞에 출력되게 하기 위해서 입니다.
        			sql = " update board " 
        			     + "set    BOARD_RE_SEQ = BOARD_RE_SEQ + 1 " 
        				 + "where  BOARD_RE_REF = ? "
        			  	 + "and    BOARD_RE_SEQ > ?";
        
        			pstmt = conn.prepareStatement(sql);
        			pstmt.setInt(1, re_ref);
        			pstmt.setInt(2, re_seq);
        			pstmt.executeUpdate();
        			pstmt.close();
        			
        			// 등록할 답변 글의 BOARD_RE_LEV, BOARD_RE_SEQ 값을 원문 글보다 1씩 증가시킵니다.
        			re_seq = re_seq + 1;
        			re_lev = re_lev + 1;
        
        			sql = "insert into board " 
        			     + "(BOARD_NUM,BOARD_NAME,BOARD_PASS,BOARD_SUBJECT,"
        				 + " BOARD_CONTENT, BOARD_FILE, BOARD_RE_REF," 
        			     + " BOARD_RE_LEV, BOARD_RE_SEQ,"
        				 + " BOARD_READCOUNT,BOARD_DATE) " 
        			     + "values(board_seq.nextval,?,?,?,?,?,?,?,?,?,sysdate)";
        
        			pstmt = conn.prepareStatement(sql);
        			pstmt.setString(1, board.getBOARD_NAME());
        			pstmt.setString(2, board.getBOARD_PASS());
        			pstmt.setString(3, board.getBOARD_SUBJECT());
        			pstmt.setString(4, board.getBOARD_CONTENT());
        			pstmt.setString(5, ""); // 답변에는 파일을 업로드하지 않습니다.
        			pstmt.setInt(6, re_ref);
        			pstmt.setInt(7, re_lev);
        			pstmt.setInt(8, re_seq);
        			pstmt.setInt(9, 0); // BOARD_READCOUNT(조회수)는 0
        			result = pstmt.executeUpdate();
        			conn.commit(); // commit합니다.
        
        		} catch (Exception ex) {
        			ex.printStackTrace();
        			System.out.println("boardReply() 에러 : " + ex);
        				try {
        					if (conn != null)
        					   conn.rollback();
        				} catch (SQLException e) {
        					e.printStackTrace();
        				}
        		} finally {
        			
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
        		return result;
        	}
        ```
