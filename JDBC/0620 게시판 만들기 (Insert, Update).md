# 게시판 만들기

- **insert, update, reply, delete, selectAll, selectPage, end** 이렇게 7가지 기능이 있는 게시판을 만들어본다

## 메서드 (보조 역할)

- **menuselect**
    - **menuselect**는 위 7가지 기능을 고르는 메서드이다
    - menus라는 String배열에 기능들을 나열하고, 숫자와 함께 출력한다
    - 숫자를 선택하는 기능은 **inputNumber**로 넘겨준다
    - inputNumber의 인수로 Scanner의 참조변수, 숫자의 최소값과 최대값을 쓴다
        
        ```java
        private static int menuselect(Scanner sc) { 
        		String[] menus = {"글쓰기", "수정", "답변달기", "글삭제", "조회", "페이지 선정", "종료"};
        		int i = 0;
        		System.out.println("========================================================");
        		for (String m : menus) {
        			System.out.println(++i + "." + m);
        		}
        		System.out.println("========================================================");
        		System.out.print("메뉴를 선택하세요");
        		return inputNumber(sc, 1, menus.length);
        		}
        ```
        
- **inputNumber**
    - **inputNumber** 는 매개변수로 입력한 숫자의 범위안에 있는지 확인
    - 또한, 숫자가 아닌 문자로 입력했을때 예외처리로 알려준다
        
        ```java
        private static int inputNumber(Scanner sc, int start, int end) {
        		int input = 0;
        		while(true)
        			try {
        				input = Integer.parseInt(sc.nextLine());
        				if (input <= end && input >= start) {
        					break;					
        				} else {
        					System.out.println(start + "~" + end + "사이의 숫자를입력하세요");
        				}
        			} catch (NumberFormatException e) {
        				System.out.println("숫자로 입력하세요>");
        			}
        		return input;
        	}
        ```
        
- **select**
    - **dao.getDetail**를 이용하여 해당 row를 조회하고 그 데이터값들을 **Board에 저장**시킨다 (반환값 Board)
    - **PrintTitle**로 컬럼값을 출력하고, 전달 받은 Board에 있는 데이터들을 **toString으로 출력**한다(확인용)
    - **board를** 반환한다
    
    ```java
    private static Board select(BoardDAO_seq dao, int num) {
    		Board board = dao.getDetail(num);
    		if (board != null) {
    			printTitle();
    			System.out.println(board.toString());
    		} else {
    			System.out.println("해당 글이 존재하지 않습니다");
    		}
    		return board;   // 수정, 삭제시에는 비밀번호 확인을 위해 필요
    						// 답변의 경우 원문글의 BOARD_RE_REF, BOARD_RE_LEV, BOARD_RE_SEQ 값이 필요
    	}
    ```
    
- **printTitle**
    - **컬럼명들을 출력**하기 위해 사용
    
    ```java
    private static void printTitle() {
    		System.out.printf("%s\t%s\t\t%s\t\t\t%s\t\t\t%s\t%s\t%s\t%s\n", 
    				        "글번호", "작성자", "제목", "내용", "ref", "lev", "seq",	"date");
    	}
    ```
    
- **dao.getDetail**
    - update, delete시에는 **비밀번호 확인**을 위해 필요하기 때문에 현재 DB에 저장되어 있는 데이터를 가져온다

# **1번째 기능 - insert**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/81a3e96e-44d8-4c6a-9e48-f89b042196b9/Untitled.png)

## main

- **insert**
    - **(1)** 입력하고자 하는 컬럼들을 입력받는다 (name, pass, subject, content)
    - **set**를 이용하여 입력받은 데이터들은 **Board**에 저장한다
    - **(2)** 이렇게 저장된 **Board의 참조변수**는 **dao.boardInsert의 매개변수**로 사용된다 (반환값 int)
    - 결과값이 1이면 글이 한줄 잘 입력되었다는 의미
        
        ```java
        private static void insert(Scanner sc, BoardDAO_seq dao) {
        		Board board = new Board();
        		
        		System.out.println("작성자>");
        			board.setBOARD_NAME(sc.nextLine());
        			
        		System.out.println("비밀번호>");
        			board.setBOARD_PASS(sc.nextLine());
        			
        		System.out.println("제목>");
        			board.setBOARD_SUBJECT(sc.nextLine());
        			
        		System.out.println("글 내용>");
        			board.setBOARD_CONTENT(sc.nextLine());
        			
        		if(dao.boardInsert(board) == 1)
        			System.out.println("글이 작성되었습니다");
        		else
        			System.out.println("오류가 발생되었습니다");	
        	}
        ```
        

## **Dao**

- **boardInsert**
    - **(3)** **get**으로 **Board** 에 있는 입력한 데이터들을 가져온다
    - **(4) set**를 이용하여 **sql쿼리**에 저장한다
    - executeUpdate로 sql문장을 실행한다
    - 반환값은 int형으로써 실행했을때 row count를 한 결과값이다
        
        ```java
        Class.forName("oracle.jdbc.driver.OracleDriver");
        			String url = "jdbc:oracle:thin:@localhost:1521:xe";
        			conn = DriverManager.getConnection(url, "board", "1234");
        
        			sql = "insert into board " 
        			    + "(BOARD_NUM,BOARD_NAME,BOARD_PASS,BOARD_SUBJECT,"
        				+ " BOARD_CONTENT, BOARD_RE_REF,"
        			    + " BOARD_RE_LEV,BOARD_RE_SEQ,BOARD_READCOUNT,"
        				+ " BOARD_DATE) " 
        			    + " values(board_seq.nextval,?,?,?,"
        			    + "        ?,  board_seq.nextval,"
        			    + "        ?,?,?,"
        			    + "        sysdate)";
        
        			// 새로운 글을 등록하는 부분입니다.
        			pstmt = conn.prepareStatement(sql);
        			pstmt.setString(1, board.getBOARD_NAME());
        			pstmt.setString(2, board.getBOARD_PASS());
        			pstmt.setString(3, board.getBOARD_SUBJECT());
        			pstmt.setString(4, board.getBOARD_CONTENT());
        
        			// 원문의 경우 BOARD_RE_LEV, BOARD_RE_SEQ 필드 값은 0 입니다.
        			pstmt.setInt(5, 0); // BOARD_RE_LEV 필드
        			pstmt.setInt(6, 0); // BOARD_RE_SEQ 필드
        			pstmt.setInt(7, 0); // BOARD_READCOUNT 필드
        
        			result = pstmt.executeUpdate();
        ```
        

# 2**번째 기능 - update**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/79a26713-19e2-4a96-94a7-65d7f4cec70f/Untitled.png)

## main

- **update**
    - **(1)** 수정한 글 번호를 입력받는데 **inputNumber**를 통해 정수만 입력받는다
    - **(2)** 입력받은 **정수(num)**와 Dao 객체의 참조변수 **dao**를 인수로 **select** 메서드를 호출한다.
    - **(3,4,5,6)** select의 역할은 비밀번호 확인이며, **getDetail**을 통해 해당 row의 데이터를 **Board에 저장**하고 그 **참조변수를 반환**한다. 실제로 해당 row를 출력하기 위해 컬럼명을 출력하는 **printTitle**도 호출해준다
    - **(7)** **set** 를 이용하여 바꾸고자 하는 데이터(제목, 내용)를 **Board**에 저장한다
    - **(8)** **get** 를 이용하여 현재 **DB에 저장**되어 있는 비밀번호를 가져와서 **사용자가 입력**한 비밀번호와 비교한다
    - **(9)** 만약 비밀번호가 서로 일치한다면 board를 인수로 하여 **dao.boardModify** 메서드를 호출한다 (반환값이 int)
    
    ```java
    private static void update(Scanner sc, BoardDAO_seq dao) {
    		System.out.println("수정할 글 번호를 입력하세요>");
    		int num = inputNumber(sc);
    		Board board = select(dao, num);
    		if (board != null) {			
    			System.out.println("제목>");
    			board.setBOARD_SUBJECT(sc.nextLine());
    				
    			System.out.println("글 내용>");
    			board.setBOARD_CONTENT(sc.nextLine());
    		
    			System.out.println("비밀번호>");
    			String pass = sc.nextLine();
    
    		if (pass.equals(board.getBOARD_PASS())) {
    			int result = dao.boardModify(board);
    			if (result == 1) {
    				System.out.println("수정에 성공했습니다");
    			} else {
    				System.out.println("수정에 실패했습니다");
    			}
    		} else {
    			System.out.println("비밀번호가 다릅니다");
    		}
    	}
    }
    ```
    

## Dao

- **boardModify**
    - (10,11) **수정된 board의 참조변수**를 이용하여 **get**를 통해 데이터를 가져와서 **set**를 통해 **DB에 다시 저장**한다
    - 반환값은 int형으로서 실행했을때 row count를 한 결과값이다
    - 반환값이 **1**이면 글을 한줄 잘 수정했다는 의미
    
    ```java
    public int boardModify(Board modifyboard) {
    		PreparedStatement pstmt = null;
    		Connection conn = null;
    		String sql = "update board " 
    		           + "set    BOARD_SUBJECT= ?, " 
    				   + "       BOARD_CONTENT= ? " 
    		           + "where  BOARD_NUM=? ";
    		int result = 0;
    		try {
    			Class.forName("oracle.jdbc.driver.OracleDriver");
    			String url = "jdbc:oracle:thin:@localhost:1521:xe";
    			conn = DriverManager.getConnection(url, "board", "1234");
    			pstmt = conn.prepareStatement(sql);
    			pstmt.setString(1, modifyboard.getBOARD_SUBJECT());
    			pstmt.setString(2, modifyboard.getBOARD_CONTENT());
    			pstmt.setInt(3, modifyboard.getBOARD_NUM());
    			result = pstmt.executeUpdate();
    		} catch (Exception ex) {
    			ex.printStackTrace();
    			System.out.println("boardModify() 에러: " + ex);
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
