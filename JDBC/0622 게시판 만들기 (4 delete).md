# 4번째 기능 - delete


## main

- **delete**
    - **(1)** 수정한 글 번호를 입력받는데 **inputNumber**를 통해 정수만 입력받는다
    - **(2)** 입력받은 **정수(num)**와 Dao 객체의 참조변수 **dao**를 인수로 **select** 메서드를 호출한다.
    - **(3,4,5,6)** select의 역할은 비밀번호 확인이며, **getDetail**을 통해 해당 row의 데이터를 **Board에 저장**하고 그 **참조변수를 반환**한다. 실제로 해당 row를 출력하기 위해 컬럼명을 출력하는 **printTitle**도 호출해준다
    - **(7)** **get** 를 이용하여 현재 **DB에 저장**되어 있는 비밀번호를 가져와서 **사용자가 입력**한 비밀번호와 비교한다
    - **(8)** 만약 비밀번호가 서로 일치한다면 board를 인수로 하여 **dao.boardDelete를** 호출한다 (반환값이 int)
    
    ```java
    private static void delete(Scanner sc, BoardDAO_seq dao) {
    		System.out.print("삭제할  글 번호를 입력하세요>"); 
    		int num = inputNumber(sc); 
    		Board board = select(dao, num);
    		
    		if (board != null) {
    			System.out.println("비밀번호>");
    			String pass = sc.nextLine();
    			if (pass.equals(board.getBOARD_PASS())) {
    				int count = dao.boardDelete(board);
    				System.out.println(count + "개 삭제되었습니다.");
    				} else {
    					System.out.println( "비밀번호가 다릅니다.");
    		
    			}
    		}
    	}
    ```
    
- **boardDelete**


- 만약 어떤글을 지운다고 했을때 그에 해당하는 **답글도 당연히 지워져야 할 것이다.**
- 뒤에 달리는 답글들은 원문번호인 **ref가 같을 것이다**. 또한, 답글이기 때문에 **lev, seq가 현재 글보다 높을 것이다**
- 그렇다면 seq는 어디까지 범위를 지정해야 할까?
    - 5번 글을 삭제한다고 했을때 같은 lev이면서 **다음 seq에 있는 글**이 7번글인 것을 알수 있다. **여기서 -1 만큼** 한 글까지 seq의 범위를 정해주면 된다
    - 근데 만약에 7번 글 처럼 **같은 lev이면서 다음 seq가 없는 글**이면 어떻게 할까?
    - 결국에는 “**같은 lev이면서 다음 seq가 없는 글”**  이말은 **null**값이라는 말과 같기 때문에 **nvl**을 이용하여 **같은 ref중에서 가장 마지막에 있는 글**로 지정해준다
- 위 조건들을 쿼리로 나타내면 다음과 같다
    - String board_delete_sql =
    "delete from board "
    + **" where  BOARD_RE_REF = ? "**
    + **" and    BOARD_RE_LEV >=? "**
    + **" and    BOARD_RE_SEQ >=? "**
    + " and    BOARD_RE_SEQ <=(  "
    + "                                             **nvl((SELECT min(board_re_seq)-1 "**
    + "                                                    **FROM   BOARD "**
    + "                                                    **WHERE  BOARD_RE_REF=? "**
    + "                                                    **AND    BOARD_RE_LEV=? "**
    + "                                                    **AND    BOARD_RE_SEQ>?) , "**
    + "                                                   **(SELECT max(board_re_seq) "**
    + "                                                    **FROM   BOARD "**
    + "	 		                                 **WHERE  BOARD_RE_REF=? )) "**
    + "                         )";

```java
public int boardDelete(Board board) {
		
		PreparedStatement pstmt = null;
		Connection conn = null;
		
		String board_delete_sql =
				"delete from board "
				+ " where  BOARD_RE_REF = ? "
				+ " and    BOARD_RE_LEV >=? "
				+ " and    BOARD_RE_SEQ >=? "
				+ " and    BOARD_RE_SEQ <=(  "
				+ "                         nvl((SELECT min(board_re_seq)-1 "
				+ "                             FROM   BOARD "
				+ "                             WHERE  BOARD_RE_REF=? "
				+ "                             AND    BOARD_RE_LEV=? "
				+ "                             AND    BOARD_RE_SEQ>?) , "
				+ "                            (SELECT max(board_re_seq) "
				+ "                             FROM   BOARD "
				+ "	 		                    WHERE  BOARD_RE_REF=? )) "
                + "                         )";
		int result = 0;
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			String url = "jdbc:oracle:thin:@localhost:1521:xe";
			conn = DriverManager.getConnection(url, "board", "1234");
			pstmt = conn.prepareStatement(board_delete_sql);
			pstmt.setInt(1,  board.getBOARD_RE_REF());
			pstmt.setInt(2,  board.getBOARD_RE_LEV());
			pstmt.setInt(3,  board.getBOARD_RE_SEQ());
			pstmt.setInt(4,  board.getBOARD_RE_REF());
			pstmt.setInt(5,  board.getBOARD_RE_LEV());
			pstmt.setInt(6,  board.getBOARD_RE_SEQ());
			pstmt.setInt(7,  board.getBOARD_RE_REF());

			//쿼리 실행 후 삭제된 로우(레코드) 개수가 반환된다
			result = pstmt.executeUpdate();
			
		} catch (Exception ex) {
			ex.printStackTrace();
			System.out.println("boardDelete() 에러: " + ex);
		} finally {
			if (pstmt != null)
				try {
					pstmt.close();
			} catch (SQLException ex) {
				ex.printStackTrace();
			}
				if (conn != null)
					try {
						conn.close();
			} catch (SQLException ex) {
				ex.printStackTrace();
			}
		}
		return result;
	}
```
