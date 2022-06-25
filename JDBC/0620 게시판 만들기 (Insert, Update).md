
# 게시판 만들기

- **insert, update, reply, delete, selectAll, selectPage, end** 이렇게 7가지 기능이 있는 게시판을 만들어본다

## main - 메서드 (보조 역할)

- **menuselect**
    - **menuselect** 메서드는 이 7가지 기능을 고르는 메서드이다
    - menus라는 String배열에 기능들을 나열하고, 숫자와 함께 출력한다
    - 숫자를 선택하는 기능은 **inputNumber** 메서드로 넘겨준다
    - inputNumber의 매개변수로 Scanner의 참조변수, 숫자의 최소값과 최대값을 쓴다
        
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
    - **inputNumber** 메서드는 매개변수로 입력한 숫자의 범위안에 있는지 확인
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

# **1번째 기능 - insert**

## main

- **insert**
    - 입력하고자 하는 컬럼들을 입력받는다 (name, pass, subject, content)
    - 입력받은 데이터들은 **Board 객체**에 저장한다
    - 이렇게 저장된 **Board의 참조변수**는 **dao.boardInsert 메서드의 매개변수**로 사용된다
        
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
    - **get메서드**로 **Board 객체**에 있는 입력한 데이터들을 가져온다
    - **set메서드**를 이용하여 **sql쿼리**에 저장한다
    - executeUpdate로 sql문장을 실행한다
        
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
        
