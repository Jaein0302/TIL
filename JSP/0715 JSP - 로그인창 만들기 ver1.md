- **JSP-로그인창 만들기**
    - **로그인 화면단**
        - **action=”login_ok” method=”post”**
        
        ```java
        <body>
        	<form action="login_ok" method="post">
        		아이디 : <input type="text" name="id" required><br>
        		비밀번호 : <input type="password" name="passwd" required><br>
        		<div class="clearfix">
        			<button type="submit" class="submitbtn">전송</button>
        			<button type="reset" class="cancelbtn">취소</button>		
        			</div>
        	</form>
        </body>
        ```
        
    - **로그인 - servlet**
        - 아이디, 비밀번호 둘다 “java”를 만족하면 id를 **session**에 저장하고, **loginsucess.jsp**로 이동
            - **session**으로 저장하기 위한 코드
                - **HttpSession session = request.getSession();
                session.setAttribute("id", id);**
                - 이 작업에 의해 브라우저 새창으로도 로그아웃이 안되어있음 (저장되어있음)
            - **loginsucess.jsp**로 이동 (sendRedirect 방식)
                - **response.sendRedirect("loginSuccess.jsp");**
        - 조건에 만족하지 못하면 **alert창 띄우기**
            - html를 쓰기 위해 필요한 코드
                - **response.setContentType("text/html;charset=euc-kr");
                PrintWriter out = response.getWriter();**
        
        ```java
        public void doPost (HttpServletRequest request,
        					   HttpServletResponse response) throws ServletException, IOException {
        		//post방식 요청 한글 처리
        		request.setCharacterEncoding("euc-kr");
        
        		//응답의 컨텐츠 형식 : 캐릭터 셋 설정
        		response.setContentType("text/html;charset=euc-kr"); 
        		
        		//출력 스트림 생성
        		PrintWriter out = response.getWriter();		
        
        		//파라미터 id와 passwd의 값 가져오기
        		String id = request.getParameter("id");
        		String passwd = request.getParameter("passwd");
        	
        		if(id.equals("java") && passwd.equals("java")) {
        			//세션 생성
        			HttpSession session = request.getSession();
        			
        			//세션 객체에 id라는 속성으로 id값 저장
        			session.setAttribute("id", id);
        			response.sendRedirect("loginSuccess.jsp");
        		}
        
        		else if(!id.equals("java")) {
        			out.println("<script>");
        			out.println("alert('아이디가 일치하지 않습니다')");
        			out.println("history.back()");
        			out.println("</script>");
        			out.close();
        		}
        		
        		else {
        			out.println("<script>");
        			out.println("alert('비밀번호가 일치하지 않습니다')");
        			out.println("history.back()");
        			out.println("</script>");
        			out.close();
        		}
        ```
        
        - **로그인성공 - jsp**
        
        ```java
        <body>
        	<%=session.getAttribute("id") %>님 로그인에 성공하셨습니다.
        	<a href="logout.jsp">로그아웃</a>
        </body>
        ```
        
        - **로그아웃 - jsp**
            - session은 브라우저를 끄지 않은이상 정보가 살아있다
            - 세션의 모든 속성을 제거하기 위한 코드
                - **<%session.invalidate(); %>**
                - **<%session.removeAttibute("id") %>** → id 속성만 제거
        
        ```java
        <body>
        	<%=session.getAttribute("id") %>님이 로그아웃 되셨습니다.
        	<%session.invalidate(); %>  //세션의 모든 속성을 제거
        	<%-- session.removeAttibute("id") // id 속성만 제거 --%>
        
        	<br>
        	<a href="login.jsp">로그인</a>
        </body>
        ```
