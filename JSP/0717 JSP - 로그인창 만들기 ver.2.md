- **JSP-로그인창 만들기 ver.2**
    - **첫화면단**
        - **header.jsp**와 **footer.jsp**로부터 **include** 해온다
        
        ```java
        <body>
        		<header>
        			<%@ include file="header.jsp" %>
        		</header>
        		<h1> template.jsp파일입니다.</h1>
        		<footer>
        			<%@ include file="footer.jsp" %>
        		</footer>		
        	</body>
        ```
        
    - **header.jsp**
        - 현재 session에 저장된 **id**가 **null**이고, **빈문자열**이라는 얘기는 아직 **로그인창에 입력하기 전**이라는 뜻이다  →  **로그인창**으로 넘어갈 수 있는 링크
        - 반대로, 속성값이 있다는 얘기는 이미 로그인이 되었다는 뜻이다  →  **로그아웃창**으로 넘어갈 수 있는 링크
        
        ```java
        <div>
        	<% String id = (String)session.getAttribute("id");
        	   if(id != null && !id.equals("")) {
        	%>
        		<%=id %>님이 로그인 되었습니다.
        		<a href="logout.jsp">(로그아웃)</a>
        	<%
        	   }else {
        	%>
        		<a href="login.jsp">로그인</a>
        	<% 
        	   }
        	%>
        </div>
        ```
        
    - **login.jsp**
        - action="**login**" method="**get**"
        
        ```java
        <body>
        	<form action="login" method="get">
        		아이디 : <input type="text" name="id" required><br>
        		비밀번호 : <input type="password" name="passwd" required><br>
        		<div class="clearfix">
        			<button type="submit" class="submitbtn">전송</button>
        			<button type="reset" class="cancelbtn">취소</button>		
        			</div>
        	</form>
        </body>
        ```
        
    - **loginServlet.java**
        - **session**에 id값을 저장(header에서 로그인 여부를 확인할 수 있음)
        - **template.jsp**로 전송한다
    - **logout.jsp**
        - **session**에 있는 값 **제거**
        - 다시 **template.jsp**로 이동
        
        ```java
        <body>
        	<% session.invalidate(); %>
        	<script>
        		alert("로그아웃이 되었습니다");
        		location.href = "template.jsp"
        	</script>
        </body>
        ```
