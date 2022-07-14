- **용어 정리**
    - **웹프로그래밍 언어**
        - 웹상에서 **사용자와 사용자들간의 연결**을 가능하게 하는 프로그래밍 언어
        - 클라이언트/서버의 방식 구축
        - **JSP**
    - **JSP**
        - 스크립트 기반으로 개발되어 서버 페이지를 쉽게 작성할 수 있으며 서블릿과 함께 구동함으로써 **서블릿의 기능을 그대로 사용**할수 있음
    - **Servlet**
        - **웹 서버** 상에서 실행되는 **자바의 클래스 파일**
        - **HttpServlet 클래스를 상속**받아야 Selvlet 클래스를 정의할 수 있다
        - 입력과 출력을 HTTP 프로토콜의 **요청(request)**와 **응답(response)**의 형태로 처리
        - **뷰**는 단순히 **클라이언트가 보는 화면**으로서 클라이언트로부터 요청 받거나 처리된 결과를 보여주는 페이지
        - 뷰에서 들어온 **요청을 받아서 처리하는 페이지**가 **컨트롤러 페이지**인데 이 페이지를 **Servlet으로 구현**
    - **웹서버**
        - 웹에서 서버 기능을 수행하는 프로그램
        - **HTTP라는 프로토콜을 기반**으로 하여 클라이언트로부터 **요청을 서비스하는 기능**을 담당 (HTTP 서버라고도 함)
    - **HTTP프로토콜**
        - 인터넷상에서 데이터를 주고 받기 위한 **서버/클라이언트 모델**을 따르는 프로토콜
    - **웹 어플리케이션 서버 (WAS)**
        - 웹서버는 클라이언트로부터 **요청 받은 일**과 **화면에 표현**하는 로직까지만 담당
        - 다양한 기능을 수행하는 **business logic**은 컨테이너가 담당하도록 **WAS**에서 일을 나누어 역할을 분담한다
        - **톰캣**
    - **톰캣**
        - **JSP**와 **서블릿**을 실행하는 컨테이너와 웹서버만 제공한다
    - **GET방식**
        - 브라우저 주소 표시줄에 주소를 직접 입력해서 요청을 전송하는 방식
    - **POST방식**
        - body영역에 데이터를 실어서 전송하는 방식
- **서블릿 정의 방법**
    - **annotation으로 실행하기 (main/java)**
        - 요청받으려는 url을 적어준다
            - **@WebServlet(urlPatterns= {"/currentTime"})**
        - 서블릿 클래스를 정의하려면 반드시 **HttpServlet클래스를 상속**받는다
            - **public class ServletTest extends HttpServlet**
        - 클라이언트 요청 방식 결정하여 매개별수로 **request, response**를 둔다
            - public void **doGet** (HttpServletRequest **request**,
                                           HttpServletResponse **response**) throws IOException
        - 응답 데이터의 MIME 타입을 HTML타입의 text로 지정 (out.println을 쓸때 사용)
            - **response.setContentType("text/html");**
        - 응답 타입의 문자 인코딩 타입을 한글이 제대로 출력되도록 "euc-kr"로 지정
            - **response.setCharacterEncoding("euc-kr");**
        - **전체코드**
            
            ```java
            @WebServlet(urlPatterns= {"/currentTime"})
            public class ServletTest extends HttpServlet{
            	private static final long serialVersionUID = 1L;
            	public void doGet (HttpServletRequest request,
            					   HttpServletResponse response) throws IOException {
            		response.setContentType("text/html");
            		response.setCharacterEncoding("euc-kr");
            		
            		//Calendar객체를 생성하여 객체로부터 시간, 분, 초 값을 얻어온다
            		Calendar c=Calendar.getInstance();
            		int hour = c.get(Calendar.HOUR_OF_DAY) ;  // 시간
            		int minute = c.get(Calendar.MINUTE);	  // 분
            		int second = c.get(Calendar.SECOND);	  // 초
            		
            		//응답에 내용을 출력할 출력 스트림을 생성한다
            		PrintWriter out=response.getWriter();
            		
            		//클라이언트로 응답할 내용을 HTML타입의 데이터로 출력하는 부분이다
            		out.write("<HTML><HEAD><TITLE>Servlet Test</TITLE></HEAD>");
            		out.write("<BODY><H1>");
            		out.write("현재시각은");
            		out.write(Integer.toString (hour));
            		out.write("시 ");
            		out.write(Integer.toString(minute));
            		out.write("분 ");
            		out.write(Integer.toString(second));
            		out.write("초입니다. ");
            		out.write("</H1></BODY></HTML>");
            		out.close();
            	}
            }
            ```
            
    - **annotation없이 실행하기 (WEB-INF)**
        - annotation이 없이 web.xml에서 요청받는다
        - web.xml의 위치는 **WEB-INF**에 저장되어 있고, 첫줄에는 **XML선언**이 들어가야 한다(톰캣 web.xml에서 복사)
            - **<?xml version="1.0" encoding="UTF-8"?>**
            - <web-app version="4.0" xmlns="[http://xmlns.jcp.org/xml/ns/javaee](http://xmlns.jcp.org/xml/ns/javaee)"
            xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
            xsi:schemaLocation="[http://xmlns.jcp.org/xml/ns/javaee](http://xmlns.jcp.org/xml/ns/javaee)[http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd](http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd)">
        - **<servlet></servlet>** 태그로 묶인 부분이 각각의 서블릿에 대응하는 서블릿 클래스를 지정하는 부분
            - **<servlet>**
                <servlet-name>S</servlet-name>
                <servlet-class>_1.ServletTest2</servlet-class>
            **</servlet>**
        - **<servlet-mapping></servlet-mapping>**태그 부분은 URL 상의 요청명과 서블릿을 연결해 주는 부분
        - <servlet-mapping>태그 안의 <servlet-name>은 반드시 <servlet>태그 안의 <servlet-name>과 일치해야 한다
        - <url-pattern>태그로 묶인 부분이 <servlet-name>태그에 명시된 서블릿이 요청되기 위한 주소의 패턴을 입력하는 부분
            - **<servlet-mapping>**
                <servlet-name>S</servlet-name>
                <url-pattern>/test</url-pattern>
            **</servlet-mapping>**
        - **전체 코드**
            
            ```html
            <?xml version="1.0" encoding="UTF-8"?>
            <web-app version="4.0" xmlns="http://xmlns.jcp.org/xml/ns/javaee" 
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee                       
            http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd">
            	
            	<servlet>
            		<servlet-name>S</servlet-name>
            		<servlet-class>_1.ServletTest2</servlet-class>
            	</servlet>
            	
            	<servlet-mapping>
            		<servlet-name>S</servlet-name>
            		<url-pattern>/test</url-pattern>
            	</servlet-mapping>
            </web-app>
            ```
            
- **로그인창 요청 받아오기**
    - **login.html**
        - 화면단을 만들어주고, **action에 요청할 url주소**를 적어준다
        - 여기서는 “LifeCycleTest”
        
        ```java
        <form action="LifeCycleTest" method="get">
        	<h1>로그인</h1>
        		<hr>
        		<b>아이디</b>
        			<input type="text" name="id" placeholder="Enter id" required>
        			<b>Password</b>
        			<input type="text" name="passwd" placeholder="Enter password" required>
        			<div class="clearfix">
        				<button type="submit" class="submitbtn">Submit</button>
        				<button type="reset" class="cancelbtn">Cancel</button>			
        			</div>
        </form>
        ```
        
    - **LifeCycleServlet.java**
        - 웹사이트에서 설정한 주소로 넘어가는 순간 서버에 요청이 들어온다
            - **@WebServlet("/ex1/_1.login/LifeCycleTest")**
        - Servlet객체를 상속받아 **Servlet에서 자동으로 객체**를 생성해준다
            - **public class LifeCycleTest_get extends HttpServlet**
        - **doGet방식, doPost방식** 중 선택해서 request와 response 매개변수로 하는 **메서드**를 만든다
            - **public void doGet (HttpServletRequest request,
                                          HttpServletResponse response) throws ServletException, IOException**
        - **getParameter() 메서드** →  **html의 태그** 속성 중에서 입력한 후 전송되어 온 파라미터 값을 반환해 주는 메서드이다
            - String id = **request.getParameter**("id");
        - 응답되는 데이터들의 한글처리
            - **response.setContentType("text/html;charset=euc-kr");**
        - 응답하는 reponse객체에 내용을 출력할 수 있는 출력 스트림 생성
            - PrintWriter out = **response.getWriter();**
        - **전체코드**
            
            ```java
            @WebServlet("/ex1/_1.login/LifeCycleTest")
            public class LifeCycleTest_get extends HttpServlet{
            	private static final long serialVersionUID = 1L;
            
            	public void doGet (HttpServletRequest request,
            					   HttpServletResponse response) throws ServletException, IOException {
            
            		String id = request.getParameter("id");
            		String passwd = request.getParameter("passwd");
            		
            		response.setContentType("text/html;charset=euc-kr");
            		PrintWriter out = response.getWriter();
            		
            		out.println("웹 애플리케이션 경로 정보: " + request.getContextPath());
            		out.println("<br>" + "아이디=" + id + "<br>");
            		out.println("비밀번호=" + passwd + "<br>");
            		out.close();
            	}
            }
            ```
            
- **멤버 인적사항 받아오기**
    - **memReg.html**
        - 같은 내용의 웹페이지를 **get방식, post방식** 2개의 형식으로 요청하는 방법을 해보기로 한다
        - **전체코드 - get**
            
            ```java
            <form action="memReg" method="get">
            <h1>회원가입</h1>
            <hr>
            	<b>회원명</b>
            	<input type="text" name="name" placeholder="Enter name" required>
            	
            	<b>주소</b>
            	<input type="text" name="addr" placeholder="Enter address" required>
            	
            	<b>전화번호</b>
            	<input type="text" name="tel" placeholder="Enter tel" required
            						pattern="[0-9]{2,3}-[0-9]{3-4}-[0-9]{4}" required>
            						
            	<b>취미</b>
            	<input type="text" name="hobby" placeholder="Enter hobby" required>
            	<div class="clearfix">
            		<button type="submit" class="submitbtn">가입</button>
            		<button type="reset" class="cancelbtn">취소</button>	
            	</div>
            </form>
            ```
            
        - **전체코드 - post**
            
            ```java
            <form action="memReg" method="post">
            <h1>회원가입</h1>
            <hr>
            	<b>회원명</b>
            	<input type="text" name="name" placeholder="Enter name" required>
            	
            	<b>주소</b>
            	<input type="text" name="addr" placeholder="Enter address" required>
            	
            	<b>전화번호</b>
            	<input type="text" name="tel" placeholder="Enter tel" required
            						pattern="[0-9]{2,3}-[0-9]{3-4}-[0-9]{4}" required>
            						
            	<b>취미</b>
            	<input type="text" name="hobby" placeholder="Enter hobby" required>
            	<div class="clearfix">
            		<button type="submit" class="submitbtn">가입</button>
            		<button type="reset" class="cancelbtn">취소</button>	
            	</div>
            </form>
            ```
            
    - **memServlet**
        - 처음에는 **get방식**으로 요청을 받아와서 html을 작성한다
        - **doPost방식**으로 요청을 받아올때는 **doGet메서드를 호출**만 하면 된다
            - **한글화시키기**
                - **get방식**
                    - **request →** server.xml에 Connector태그에 **URIEncoding="euc-kr” 추가**
                    - **response** → **response.setContentType("text/html;charset=euc-kr");**
                - **post방식**
                    - **request** → **request.setCharacterEncoding("euc-kr");**
                    - **response → response.setContentType("text/html;charset=euc-kr");**
        - **전체코드 - get, post 구현**
            
            ```java
            public class memReg extends HttpServlet{
            	private static final long serialVersionUID = 1L;
            
            	public void doGet (HttpServletRequest request,
            					   HttpServletResponse response) throws ServletException, IOException {
            		
            		String name = request.getParameter("name");
            		String addr = request.getParameter("addr");
            		String tel = request.getParameter("tel");
            		String hobby = request.getParameter("hobby");
            		
            		response.setContentType("text/html;charset=euc-kr");  // 서블릿에서 웹페이지로 내보낼때 한글화
            		PrintWriter out = response.getWriter();
            		
            		out.println("<html><head>"
            					+ "<style>table{border-collapse:collapse;width:50%;margin:0 auto} "
            					+ "tr{height:3em;border-bottom:1px solid black} "
            					+ "td{width:60%}</style></head>");
            		out.println("<body><table><tbody>");
            		out.println("<tr><th>회원명</th><td>" + name + "</td></tr>");
            		out.println("<tr><th>주소</th><td>"+ addr + "</td></tr>");
            		out.println("<tr><th>전화번호</th><td>"+ tel + "</td></tr>");
            		out.println("<tr><th>취미 </th><td>"+ hobby + "</td></tr></tbody></table></body></html");
            		out.close();
            	}
            	
            	//write나 prinln은 같은 기능인데 println은 줄바꿈
            	protected void doPost (HttpServletRequest request,
            			   HttpServletResponse response) throws ServletException, IOException {
            		request.setCharacterEncoding("euc-kr");	  // 웹페이지에서 받을때 미리 한글화
            		doGet(request, response);
            	}
            }
            ```
