- **MVC 모델2**
    - 모델1은 뷰와 로직을 jsp페이지 하나에서 처리하는 구조인데 JSP 코드가 복잡해져 유지보수가 어렵다는 단점이 있다
    - 이 단점을 보완하여 로직처리는 서블릿이 하게 된 것이 모델2
    - [https://hsp1116.tistory.com/9](https://hsp1116.tistory.com/9)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/544d5141-2144-4253-bfea-1db85f529d97/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b6c915bd-0dda-4dc5-b7c9-4d8251c4ae23/Untitled.png)

- **MVC모델 기본환경 설정**
    - **web.xml** 파일(WEB_INF 폴더)에 **초기화면(http://localhost:8088/Board_Ajax)**이 **index.jsp**로 가게 함
        
        ```java
        <?xml version="1.0" encoding="UTF-8"?>
        <web-app version="4.0"
        	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
        						http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd">
        						
        <welcome-file-list>
        		<welcome-file>index.jsp</welcome-file>
        		<welcome-file>index.html</welcome-file>
        		<welcome-file>index.htm</welcome-file>	
        	</welcome-file-list>
        </web-app>
        ```
        
    - **index.jsp**는 바로 **login.net**으로 가게 forward시킨다
        
        ```java
        <%@ page language="java" contentType="text/html; charset=UTF-8"%>
        <jsp:forward page="/login.net"/>
        ```
        
    - **MemberFrontController**
        - **@WebServlet("*.net")**
            - *는 어떠한 값도 올 수 있때문에 **.net**은 member에 대한 값을 구분짓기 위함이다
        - login.net의 풀네임은 **http://localhost:8088/Board_Ajax/login.net** 이다
        - **FrontController**는 이러한 주소들을 받아서 **view**나 **action**클래스로 전달하는 **서블릿** 역할을 한다
        - 따라서, 서블릿 인터페이스를 구현시켜야 한다
        - **String command = RequestURI.substring(contextPath.length());**
            - 풀네임에서 필요한 부분인 **/login.net**만 분해해내는 코드
        - 전송방식에는 주소 전달에 따라 **Redirect, Dispatcher**과 데이터 전송 유무에 따라 **Get, Post**로 나눌 수 있다
        - **전체코드**
            
            ```java
            @WebServlet("*.net")
            public class MemberFrontController extends javax.servlet.http.HttpServlet {  //서블릿 구현 해줘야함
            	private static final long serialVersionUID = 1L;
            	
            	protected void doProcess(HttpServletRequest request, HttpServletResponse response)
            			throws ServletException, IOException {
            		/* 요청된 전체 URL중에서 포트 번호 다음부터 마지막 문자열까지 반환됩니다.
            		 	예) contextPath가 "/JspProject"인 경우
            		 	http://localhost:8088/JspProject/login.net로 요청하면 
            		 	RequestURI는 "/JspProject/login.net" 반환됩니다.
            		 */
            		
            		String RequestURI = request.getRequestURI();
            		System.out.println("RequestURI = " + RequestURI);
            		
            		//getContextPath() : 컨텍스트 경로가 반환됩니다.
            		//contextPath는 "/JspProject"가 반환됩니다.
            		String contextPath = request.getContextPath();
            		System.out.println("contextPath = " + contextPath);
            		
            		//RequestURI에서 컨텍스트 경로 길이 값의 인덱스 위치의 문자부터 마지막 위치 문자까지 추출합니다.
            		//command는 "/login.net" 반환됩니다.
            		String command = RequestURI.substring(contextPath.length());
            		System.out.println("command = " + command);
            		
            		//초기화
            		ActionForward forward = null;
            		Action action = null;
            		
            		// 상속 관계를 이용해서 객체를 생성 (action이 부모)
            		switch (command) {
            		case "/login.net":
            			action = new MemberLoginAction();
            			break;
            		case "/join.net":
            			action = new MemberJoinAction();
            			break;
            		case "/idcheck.net":
            			action = new MemberIdCheckAction();
            			break;
            		case "/joinProcess.net":
            			action = new MemberJoinProcessAction();
            			break;
            		case "/loginProcess.net":
            			action = new MemberLoginProcessAction();
            			break;
            		case "/memberUpdate.net":
            			action = new MemberUpdateAction();
            			break;
            		}
            		forward = action.execute(request, response);  // 추상메서드 구현
            		//action이 null이기 때문에 실행할 수 없음
            		
            		if(forward != null) {
            			if (forward.isRedirect()) {
            				response.sendRedirect(forward.getPath());	// 리다이렉트	
            			} else {  // 포워딩
            				RequestDispatcher dispatcher = request.getRequestDispatcher(forward.getPath());
            				dispatcher.forward(request,response);
            			}
            		}
            	}
            		//doProcess(request, response)메서드를 구현하여 요청이 GET방식이든 POST방식이든 전송되어 오든
            		//같은 메서드에서 요청을 처리할 수 있도록 하였습니다. (어떠한 방식이 올지 모르니깐)
            		protected void doGet(HttpServletRequest request, HttpServletResponse response)
            					throws ServletException, IOException {
            			doProcess(request, response);
            		}		
            		
            		protected void doPost(HttpServletRequest request, HttpServletResponse response)
            				throws ServletException, IOException {
            			request.setCharacterEncoding("utf-8"); // post는 직접 써줘야함
            			doProcess(request, response);
            		}
            	}
            ```
            
    - **ActionForward**
        - ActionForward 클래스는 이러한 **redirect, dispatcher**의 값과 **경로에 대한 값**을 가지고 있다
        - **Action의 추상메서드를 수행하고 반환**되는 값이며, 이 값들은 FrontController에서 그 값에 해당하는 요청 페이지로 이동한다
            
            ```java
            public class ActionForward {
            	private boolean redirect=false;
            	private String path=null;
            	
            	//property redirect의 is메서드
            	public boolean isRedirect() {  // 프로퍼티 타입이 boolean일 경우 get 대신 is를 앞에 붙일 수 있다
            		return redirect;
            	}
            	public void setRedirect(boolean b) {
            		redirect=b;
            	}
            	public String getPath() {
            		return path;
            	}
            	public void setPath(String string) {
            		path = string;
            	}
            }
            ```
            
    - **Action**
        - 인터페이스로서, ActionForward를 반환하는 excute() 라는 **추상메서드**가 있다
        - Action은 조상이고, 각각의 **~Action 자식**들이 이 **추상메서드를 구현**시킨다
            
            ```java
            public interface Action {
            		public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException;
            }
            ```
            

# 로그인과 회원가입

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d86471a9-5a96-4ee3-9ca1-3cd6576c1b48/Untitled.png)

- **/login.net**
    - **MemberLoginAction**
        - **쿠키에 저장된 id값**을 불러오고, 다시 **request객체**에 저장한다
        
        ```java
        public class MemberLoginAction implements Action{
        	@Override
        	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        		String id = "";
        		Cookie[] cookies = request.getCookies();
        		if(cookies != null) {
        			for(int i=0; i<cookies.length; i++	 ) {
        				if(cookies[i].getName().equals("id")) {
        					id=cookies[i].getValue();
        				}
        			}
        		}
        		
        		request.setAttribute("id", id);
        		ActionForward forward = new ActionForward();
        		forward.setRedirect(false);  // 주소 변경 없이 jsp페이지의 내용을 보여줍니다.
        		forward.setPath("member/loginForm.jsp");
        		return forward;
        	}
        }
        ```
        
    - **loginForm**
        - 로그인 **submit**할시 **action**으로 **loginProcess.net**으로 이동
        - 회원가입 **button** 누를시 **location.href**으로 **join**.**net**으로 이동
        - el을 사용하여 **request에 저장된 id값**을 불러오고, 값이 있다면 **remember에 check** 표시한다
        
        ```java
        <head>
        	<link href="css/login.css" type="text/css" rel="stylesheet">	
        	<script src = "http://code.jquery.com/jquery-latest.js"></script>	
        	<script>
        		$(function(){
        			$(".join").click(function() {
        				location.href = "join.net";
        			});
        			
        			var id = '${id}';  // el을 쓴것
        			if (id) {
        				$("#id").val(id);
        				$("#remember").prop('checked', true);
        			}
        		})
        	</script>
        </head>
        <body>
        		<form name="loginform" action="loginProcess.net" method="post">
        			<h1>로그인</h1>
        			<hr>
        			<b>아이디</b>
        			<input type="text" name="id" placeholder="Enter id" id="id" required>		
        			
        			<b>비밀번호</b>
        			<input type="password" name="pass" placeholder="Enter password" required>
        			<input type="checkbox" id="remember" name="remember" value="store">
        			<span>remember</span>
        			
        			<div class="clearfix">
        				<button type="submit" class="submitbtn">로그인</button>
        				<button type="button" class="join">회원가입</button>
        			</div>
        		</form>
        </body>
        ```
        
    - **/loginprocess.net**
        - **MemberLoginProcessAction**
            - request객체에 저장된 id, pass를 불러낸다
            - mdao.isId를 통해 id와 비밀번호가 맞는 확인한다
                - result가 **1**이면 아이디, 비밀번호가 모두 일치하는 것이기 때문에 **session에 id 저장**
                    - cookie에 id 저장하여 전송
                    - BoardList.bo로 이동
                - result가  **-1**이면 비밀번호가 일치하지 않은 것이기 때문에 alert 기능 + 다시 login 화면
                - result가  **0**이면 아이디가 일치하지 않은 것이기 때문에 alert 기능 + 다시 login 화면
            
            ```java
            public class MemberLoginProcessAction implements Action{
            	
            	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) 
            			throws ServletException, IOException {
            		
            		ActionForward forward = new ActionForward();	
            		String id = request.getParameter("id");
            		String pass = request.getParameter("pass");
            		MemberDAO mdao = new MemberDAO();
            		int result = mdao.isId(id, pass);
            		System.out.println("결과는 " + result);
            		
            		//로그인 성공
            		if(result == 1) {
            			HttpSession session = request.getSession();
            			session.setAttribute("id", id);
            			
            			String IDStore = request.getParameter("remember");
            			Cookie cookie = new Cookie("id", id);
            			
            			//ID 기억하기를 체크한 경우
            			if(IDStore != null && IDStore.equals("store")) {
            				//cookie.setMaxAge(60 * 60 * 24); // 쿠키의 유효시간을 24시간으로 설정합니다.
            				cookie.setMaxAge(2 * 60);
            				//클라이언트로 쿠키값을 전송합니다.
            				response.addCookie(cookie);
            			} else {
            				cookie.setMaxAge(0);
            				response.addCookie(cookie);
            			}
            			
            			forward.setRedirect(true);
            			forward.setPath("BoardList.bo");
            			return forward;
            			
            		} else {
            			String message = "비밀번호가 일치하지 않습니다.";
            			if(result == -1)
            				message = "아이디가 존재하지 않습니다.";
            			response.setContentType("text/html;charset=utf-8");
            			PrintWriter out = response.getWriter();
            			out.println("<script>");
            			out.println("alert('" + message + "');");
            			out.println("location.href='login.net';");
            			out.println("</script>");
            			out.close();
            			return null;
            		}
            	}
            }
            ```
            
    - **/join.net**
        - **MemberJoinAction**
            - dispatch방식으로 joinForm으로 전송
            
            ```java
            public class MemberJoinAction implements Action{
            	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            	
            		ActionForward forward = new ActionForward();
            		forward.setRedirect(false);    // 주소 변경없이 jsp페이지의 내용을 보여준다
            		forward.setPath("member/joinForm.jsp");
            		return forward;  // redirect로 갈지 forward로 갈지 정하는 것이 ActionForward 클래스 
            	}
            }
            ```
            
        - **joinForm**
            - **id**
                - id에 **keyup**할때 생기는 이벤트
                    - **pattern에 맞지 않은 id**일 경우 html로 보여주고, checkid=**false**
                    - **ajax**를 이용해서 입력받은 id값 저장해서 **idcheck.net**으로 전송
                        - id가 있으면 checkid=**true**
                        - id가 없으면 checkid=**false**
                - **코드**
                
                ```java
                $("input:eq(0)").on('keyup',
                				function() {
                				
                					$('#message').empty(); // 처음에 pattern에 적합하지 않은 경우 메시지 출력 후 적합한 데이터를 입력
                					//[A-Za-z0-9]의 의미가 \w
                					var pattern =/^\w{5,12}$/;
                					var id = $("input:eq(0)").val();
                					if(!pattern.test(id)) {
                						$("#message").css('color', 'red')
                									 .html("영문자 숫자 _로 5~12자 가능합니다.");
                						checkid=false;
                						return;	
                					}
                			
                					$.ajax({
                						url : "idcheck.net",
                						data : {"id" : id},
                						success : function(resp) {
                							if(resp == -1) {  // db에 해당 id가 없는 경우
                								$("#message").css('color', 'green').html("사용 가능한 아이디입니다");
                								checkid=true;
                							} else {  // db에 해당 id가 있는 경우(0)
                								$("#message").css('color', 'blue').html("사용중인 아이디입니다");
                								checkid=false;
                							}
                						}
                					});
                				})
                ```
                
            - **email**
                - email에 **keyup**할때 생기는 이벤트
                    - pattern에 맞는 email일 경우 checkmail=**true**
                    - pattern에 맞지 않은 email일 경우 checkmail=**false**
                - 코드
                    
                    ```java
                    $("input:eq(6)").on('keyup',
                    			function(){
                    				$("#email_message").empty();
                    				//[A-Za-z0-9_]와 동일한 것이 \w입니다.
                    				//+는 1회 이상 반복을 의미하고 {1,}와 동일합니다.
                    				//\w+ 는 [A-Za-z0-9_]를 1개이상 사용하라는 의미입니다.
                    				var pattern = /^\w+@\w+[.]\w{3}$/;
                    				var email = $("input:eq(6)").val();
                    				if(!pattern.test(email)) {
                    					$("#email_message").css('color', 'red')
                    									   .html("이메일형식이 맞지 않습니다")
                    					checkmail=false;
                    				} else {
                    					$("#email_message").css('color', 'green')
                    					   				   .html("이메일형식이 맞습니다")
                    					checkemail=true;
                    				}
                    });
                    ```
                    
            - **form**
                - form을 **submit**할때 생기는 이벤트에서 다음 3가지는 페이지가 넘어가지 않음
                    - age가 숫자가 아닐경우
                    - checkid= false인 경우
                    - checkmail= false인 경우
                - **코드**
                    
                    ```java
                    $('form').submit(function() {
                    			if(!$.isNumeric($("input[name='age']").val())) {
                    				alert("나이는 숫자를 입력하세요");
                    				$("input[name'age']").val('').focus();
                    				return false;
                    			}
                    			
                    			if(!checkid){
                    				alert("사용가능한 id로 입력하세요");
                    				$("input:eq(0)").val('').focus();
                    				return false;
                    			}
                    			
                    			if(!checkemail){
                    				alert("email 형식을 확인하세요");
                    				$("input:eq(6)").focus();
                    				return false;
                    			}
                    		})
                    ```
                    
            - **html 코드**
                
                ```java
                <form name="joinform" action="joinProcess.net" method="post">
                			<h1>회원가입</h1>
                			<hr>
                			<b>아이디</b>
                			<input type="text" name="id" placeholder="Enter id" required maxLength="12">
                			<span id="message"></span>
                			
                			<b>비밀번호</b>
                			<input type="password" name="pass" placeholder="Enter password" required>
                			
                			<b>이름</b>
                			<input type="text" name="name" placeholder="Enter name" maxLength="5" required>
                			
                			<b>나이</b>
                			<input type="text" name="age" placeholder="Enter age" maxLength="2" required>
                			
                			<b>성별</b>
                			<div>
                				<input type="radio" name="gender" value="남" checked><span>남자</span>
                				<input type="radio" name="gender" value="여"><span>여자</span>
                			</div>
                			
                			<b>이메일 주소</b>
                			<input type="text" name="email" placeholder="Enter email"  maxLength="30" required>
                			<span id="email_message"></span>
                			
                			<div class="clearfix">
                				<button type="submit" class="submitbtn">회원가입</button>
                				<button type="reset" class="cancelbtn">다시작성</button>
                			</div>
                		</form>
                ```
                
    - **/idcheck.net**
        - **MemberIdCheckAction**
            - joinForm에서 **ajax**를 통해 /idcheck.net으로 이동한 결과이다
            - ajax를 쓴 이유는
                - 일단 db에 저장된 데이터값과 비교해야 한다.
                - 전체 페이지를 바꾸는 것이 아니라 일부 데이터만 바꾸는 목적이다(비동기적)
            - **response.getWriter().println(Integer.toString(result));**
                - 아래 두 문장을 합친것과 같다
                    - PrintWriter out = response.getWriter();
                    - out.prinln(Integer.toString(result));
                - **out.print**는 파라미터로 설정된 string을 **클라이언트에게** 보내는 기능 (response의 body로 저장해서 전송)
                - 그 값을 받아서 **ajax의 function의 매개변수**로 받아온다
                - Integer.toString는 **숫자를 String으로 변환**시키는 것이다 (out.print로 보내기 위해)
            - 반환값이 ActionForward인데 null이라는 것은 redirect나 forward 방식이 아닌 response로 바로 갔을 보내준다는 의미
            - **코드**
                
                ```java
                public class MemberIdCheckAction implements Action {
                	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                		MemberDAO dao = new MemberDAO();
                		int result = dao.isId(request.getParameter("id"));
                		response.getWriter().println(Integer.toString(result));
                		System.out.println(result);
                		return null;  // null이면 redirect와 foward형식이 아니라 response.getWriter().append(Integer.toString(result));값이 직접 전달됨
                
                	}
                }
                ```
                
        - **DAO - IsId**
            - 입력받은 id가 실제 DB에 있는지 체크하는 기능
            
            ```java
            public int isId(String id)	{
            		Connection con = null;
            		PreparedStatement pstmt = null;
            		ResultSet rs = null;
            		int result = -1; //DB에 해당 id가 없습니다.
            		try {
            			con = ds.getConnection();
            			
            			String sql = "select id from member where id=?";			
            			pstmt = con.prepareStatement(sql);
            			pstmt.setString(1, id);
            			rs = pstmt.executeQuery();
            			
            			if (rs.next()) { 
            				result = 0;
            			}
            		} catch (Exception se) {
            			se.printStackTrace();
            		} finally {
            			try {
            				if (rs != null)
            					rs.close();
            			} catch (SQLException e) {
            				System.out.println(e.getMessage());
            			}
            			try {
            				if (pstmt != null)
            					pstmt.close();
            			} catch (SQLException e) {
            				System.out.println(e.getMessage());
            			}
            			try {
            				if (con != null)
            					con.close();
            			} catch (SQLException e) {
            				System.out.println(e.getMessage());
            			}
            		}
            		return result;
            		}
            ```
            
    - **/joinProcess.net**
        - **MemberJoinProcess**
            - 입력받은 값들을 getParameter로 불러온 다음 insert쿼리를 이용하여 DB에 저장한다
            - result값이 1이면 성공 alert → login.net으로 이동
            - result값이 -1이면 아이디 중복 alert → history.back(join.net으로 돌아감)
            - result값이 0이면 회원가입 실패 alert → error.jsp로 이동  // **message 저장한거는 잘 모르겠음**
        - **코드**
            
            ```java
            public class MemberJoinProcessAction implements Action{
            	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            		String id = request.getParameter("id");
            		String pass = request.getParameter("pass");
            		String name = request.getParameter("name");
            		int age = Integer.parseInt(request.getParameter("age"));
            		String gender = request.getParameter("gender");
            		String email = request.getParameter("email");
            		
            		Member m = new Member();
            		m.setAge(age);
            		m.setEmail(email);
            		m.setGender(gender);
            		m.setId(id);
            		m.setName(name);
            		m.setPassword(pass);
            		
            		response.setContentType("text/html;charset=utf-8");
            		PrintWriter out = response.getWriter();
            		
            		MemberDAO mdao = new MemberDAO();
            		
            		int result = mdao.insert(m);
            //		result = 0;  회원가입 실패
            		if(result == 0) {
            			System.out.println("회원 가입 실패입니다.");
            			ActionForward forward = new ActionForward();	
            			forward.setRedirect(false);
            			request.setAttribute("message", "회원 가입 실패입니다.");
            			forward.setPath("error/error.jsp");
            			return forward;		
            		}
            		
            		out.println("<script>");
            		if(result == 1) { //삽입이 된 경우
            			out.println("alert('회원가입을 축하합니다');");
            			out.println("location.href='login.net';");
            		} else if (result == -1) {
            			out.println("alert('아이디가 중복되었습니다. 다시 입력하세요');");
            			//새로고침되어 이전에 입력한 데이터가 나타나지 않습니다.
            			//out.println("location.href='join.net';");
            			out.println("history.back()"); // 비밀번호를 제외한 다른 데이터는 유지되어 있습니다.
            		}
            		out.println("</script>");
            		out.close();
            		return null;
            	}
            }
            ```
            
        - **DAO - insert**
            - **java.sql.SQLIntegrityConstraintViolationException**
                - 무결성 제약조건에 위배 (아이디가 중복됨)
                - result = -1값으로 지정
            - **코드**
                
                ```java
                public int insert(Member m)	{
                		Connection con = null;
                		PreparedStatement pstmt = null;		
                		int result = 0;
                		
                		try {
                			con = ds.getConnection();
                			System.out.println("getConnection : insert()");
                			
                			String sql = "INSERT INTO member (id, password, name, age, gender, email) "
                					   + " VALUES (?,?,?,?,?,?) ";
                							
                			pstmt = con.prepareStatement(sql);
                			pstmt.setString(1, m.getId());
                			pstmt.setString(2, m.getPassword());
                			pstmt.setString(3, m.getName());
                			pstmt.setInt(4, m.getAge());
                			pstmt.setString(5, m.getGender());
                			pstmt.setString(6, m.getEmail());
                			result = pstmt.executeUpdate(); //삽입 성공시 result는 1
                			
                		//primary key 제약조건 위반했을 경우 발생하는 에러	
                		} catch (java.sql.SQLIntegrityConstraintViolationException e) {			
                			result = -1;
                			System.out.println("멤버 아이디 중복 에러입니다.");
                		} catch (Exception e) {
                			e.printStackTrace();			
                		} finally {
                			if (pstmt != null)
                			try {
                				pstmt.close();
                			} catch (SQLException e) {
                				e.printStackTrace();
                			}
                		}
                		return result;
                	}
                ```
