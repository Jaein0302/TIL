| 해당 주소로 이동하는 이유 | 서블릿 주소 | 이동할 페이지 주소 | 기능 | 다른 페이지로 이동하는 태그 |
| --- | --- | --- | --- | --- |
| 처음시작 |  @WebServlet("/templatetest") | /ex8_db/_4.join/templatetest.jsp | 메인페이지로 이동 |       |
| 회원가입 메뉴 클릭 시 |  @WebServlet("/join") | /ex8_db/_4.join/join.jsp | 회원 가입 폼으로 이동 | <form name="myform" method="post" action="join_ok"> |
| 회원가입 폼에서 회원가입 버튼 클릭시 |  @WebServlet("/join_ok") | templatetest | 회원가입 성공시 |       |
| templatetest | 회원가입 실패시 |       |  |  |
| 로그인 메뉴 클릭시 |  @WebServlet("/login") | /ex8_db/_4.join/login.jsp | 로그인 폼으로 이동 | <form name="myform" method="post" action="login_ok"> |
| 로그인 Submit 버튼 클릭시 |  @WebServlet("/login_ok") | /ex8_db/_4.join/templatetest.jsp | 로그인 성공시 메시지 |  |
| /login | 로그인 실패시 메시지 |  |  |  |
| 정보수정 메뉴 클릭시 |  @WebServlet("/update_form") | /ex8_db/_4.join/update_from.jsp | 정보수정 폼으로 이동 | <form name="myform" method="post" action="update"> |
| 정보수정 Submit 버튼 클릭시 |  @WebServlet("/update") | templatetest | 정보수정 성공 메시지 |  |
| templatetest | 정보수정 실패 메시지 |  |  |  |
| 회원정보 메뉴 클릭시 |  @WebServlet("/list") | /ex8_db/_4.join/list.jsp | 회원 목록 보여주기 | <a href="detail?id=Abcde">Abcde</a> |
| <button class="btn btn-danger btn-sm">삭제</button> |  |  |  |  |
| <a href="detail?id=Abcde">Abcde</a> 클릭시 | @WebServlet("/detail") | /ex8_db/_4.join/detail_form.jsp | 특정 회원의 정보 보여주기 |  |
| <button class="btn btn-danger btn-sm">삭제</button> | @WebServlet("/delete") | /list | 특정 회원 삭제하기 |       |
- 메인화면 **(include 이용해서 페이지 바꾸기)**
    - **left.jsp**
        - "newitem", "bestitem", "useditem”  중 하나를 클릭했을때 a 태그를 이용해서 get방식으로 page값을 넘겨주고, 그 page값은 servlet에서 받는다
            - <a class="nav-link active" **href="templatetest?page=newitem"**>신상품</a>
        - **전체코드**
        
        ```java
        <%@ page language="java" contentType="text/html; charset=EUC-KR"
            pageEncoding="EUC-KR"%>
            
        <ul class="nav nav-pills flex-column">
        	<li class="nav-item">
        	  <a class="nav-link active" href="templatetest?page=newitem">신상품</a>
        	</li>
        	<li class="nav-item">
        	  <a class="nav-link" href="templatetestpbestitem">인기상품</a>
        	</li>
        	<li class="nav-item">
        	  <a class="nav-link" href="templatetest?page=useditem">중고상품</a>
        	</li>
        </ul>
        ```
        
    - **Template.java**
        - **left.jsp**에서 **page**에 대한 값을 넘겨줌 **(get방식)** → "templatetest?page=newitem”
        - 넘겨받은 page가 null일 경우에는 초기값을 **newitem**으로 지정한다
        - 넘겨받은 page값을 **request 객체에 pagfile로 저장**한다
        - **전체코드**
            
            ```java
            @WebServlet("/templatetest")
            public class Template extends HttpServlet {
            	private static final long serialVersionUID = 1L;
            	
            	protected void doGet(HttpServletRequest request, HttpServletResponse response)
            			throws ServletException, IOException {
            		String go = request.getParameter("page");
            		if(go==null) {
            			go="newitem";
            		}
            		
            		RequestDispatcher dispatcher = request.getRequestDispatcher("/ex8_db/_4.join/templatetest.jsp");
            		request.setAttribute("pagefile", go);
            		dispatcher.forward(request, response);
            	}
            }
            ```
            
    - **TemplateTest.jsp**
        - **request** 객체로부터 **pagefile**을 불러온다
            - <%
                String pagefile = (String)request.**getAttribute("pagefile");**  
            %>
        - 넘겨받은 pagefile를 이용해서 화면이 바뀌게 한다 (**include** 이용)
            - **<jsp:include page='<%= pagefile + ".jsp" %>' />**
        - "newitem", "bestitem", "useditem” 중에서 하나를 누를때 그 화면이 나타나게 하려면, 클릭한 해당  클래스는 **active**, 나머지 클래스는 **remove**해주면 된다
            
            <script>
            var pagefile='<%=pagefile%>';
            var filelist = ["newitem", "bestitem", "useditem"];
            for(var index=0; index<filelist.length; index++) {
            	if(pagefile==filelist[index]){
            		$('.nav-pills> .nav-item > .nav-link').eq(index).**addClass**('active');
            	} else {
            		$('.nav-pills> .nav-item > .nav-link').eq(index).**removeClass**('active');
            	}
            }
            </script>
            
        - **회원가입 후 알림창 기능**
            - 넘겨받은 join_result의 값이 **1**인 경우 **회원가입 성공**
            - 다 사용한 속성값이니깐 **session**을 삭제해줘야 함
                
                var result1 = **'<%=session.getAttribute("join_result")%>';**
                if(result1 != 'null') { //세션 객체에 "join_result" 속성이 없으면 result는 'null'입니다.
                     if(result1 === '1') {
                          alert('회원가입을 축하합니다');
                     } else {
                          alert('회원 가입에 실패하셨습니다');
                     }
                **<%session.removeAttribute("join_result"); %>**  
                }
                
        - **로그인 후 알림창 기능**
            - Login_OK.java, Update.java으로부터 넘겨받은 message를 이용한다
                
                var message='**<%=request.getAttribute("message")%>**'; 
                    if(message != 'null') {
                          alert(message)
                }
                
        - **전체코드**
            
            ```java
            <%@ page language="java" contentType="text/html; charset=EUC-KR"
                pageEncoding="EUC-KR"%>
            <!DOCTYPE html>
            <html>
            <head>
            <meta name="viewport" content="width=device-width, initial-scale=1">
            <link rel="stylesheet"
            	href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
            <script
            	src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
            <script
            	src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
            <script
            	src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
            
            <style>
            footer {
            	position: fixed;
            	bottom: 0px;
            	height: 3rem;
            	background: #ccc;
            	width: 100%;
            	text-align: center;
            	line-height: 3rem;
            }
            
            section li {
            	height: 2rem;
            }
            </style>
            <% 
            	String pagefile = (String)request.getAttribute("pagefile");  //getParameter에서 getAttribute로 바뀜
            %>
            <script>   // 회원가입 알림창 추가
            	var result1 = '<%=session.getAttribute("join_result")%>';
            	if(result1 != 'null') { //세션 객체에 "join_result" 속성이 없으면 result는 'null'입니다.
            		if(result1 === '1') {
            			alert('회원가입을 축하합니다');
            		} else {
            			alert('회원 가입에 실패하셨습니다');
            		}
            	<%session.removeAttribute("join_result"); %>  //다 쓴 속성값이니깐 삭제해야함
            	}
            	
            	//로그인시 알림창
            	var message='<%=request.getAttribute("message")%>'; /* Login_OK.java, Update.java에서 설정 */
            		if(message != 'null') {
            			alert(message)
            		}
            
            </script>
            </head>
            <body>
            	<header>
            		<div class="jumbotron text-center" style="margin-bottom:0">
            			<h1>상품목록</h1>
            		</div>
            	</header>
            	<nav>
            		<jsp:include page="top.jsp" /><br><br>
            	</nav>
            	<div class="container" style="margin-top:10px">
            	  <div class="row">
            	      <div class="col-sm-4">      
            	  		<aside>
            	  			<jsp:include page="left.jsp" />
            	  		</aside>
              		  </div>
               		  <div class="col-sm-8" style="margin-bottom: 5rem">	
               		  	<section>
            	  			<jsp:include page='<%= pagefile + ".jsp" %>' /> 
               		  	</section>
              		  </div>
             		</div>
            	</div>
            	
            	<footer>
             		<jsp:include page="bottom.jsp" />
            	</footer>
            
            	<script>
            		var pagefile='<%=pagefile%>';
            		var filelist = ["newitem", "bestitem", "useditem"];
            		
            		for(var index=0; index<filelist.length; index++) {
            			if(pagefile==filelist[index]){
            				$('.nav-pills> .nav-item > .nav-link').eq(index).addClass('active');
            			} else {
            				$('.nav-pills> .nav-item > .nav-link').eq(index).removeClass('active');
            			}
            		}	
            	</script>
            </body>
            ```
            
- **로그인**
    - **TOP.jsp**
        - **로그인**
            - **Login.OK**에서 **session에 저장한 id값**을 불러온다 (**아이디와 비밀번호가 일치**하는 경우 session에 저장함)
            -
