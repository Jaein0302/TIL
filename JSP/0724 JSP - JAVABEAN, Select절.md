- **자바빈**
    - 자바빈(javabean)은 자바빈 규약에 따라 작성된 자바 클래스입니다
    - 예를 들어 회원정보, 게시판 글 등의 정보를 출력할때 정보를 저장하고 이는 자바빈 객체를 사용하게 됩니다.
    - **자바빈의 규약**
        - 필드 마다 별도의 **get/set 메서드**가 존재해야 한다.
        - **get** 메서드는 **파라미터가 존재하지 않아야 한다**.
        - **set** 메소드는 반드시 **하나 이상의 파라미터**가 존재해야 한다.
        - 빈즈 컴포넌트의 속성은 반드시 읽기 또는 쓰기가 가능해야 한다.
        단, 읽기 전용인 경우 **get** 또는 **is 메서드**만 정의할 수 있다.
        - 생성자는 **파라미터가 없는 기본 생성자**를 반드시 작성해 주어야 한다.
        - **필드의 접근 제어자**는 **private**이고 각 **set/get 메서드의 접근 제어자**는
         **public**으로 정의되어야 하며 **클래스의 접근 제어자**는 **public**으로 정의한다.
    - 자바빈 클래스는 데이터를 저장하는 필드, 테이터를 읽어올 때 사용되는  메서드(**getter**메서드), 값을 저장할 때 사용되는 메서드(**setter**메서드)로 구성됩니다.
    - 메서드 이름을 사용해서 **프로퍼티(property)**의 이름을 결정하게 됩니다.
    - 예를 들어 **프로퍼티의 이름이 name**이고 **값의 타입이 int**인 경우 프로퍼티와 관련된 메서드의 이름은 다음과 같이 결정됩니다.
      public void setName(int value){};
      public int getName(){};
    - setName()과 getName()를 통해서 관리되는 데이터를 **프로퍼티(property)**라고 부릅니다.
    - 프로퍼티의 타입이 **boolean**인 경우 **get**대신 **is**를 붙일 있습니다.
    - 즉, 프로퍼티의 값을 설정하는 메서드의 경우 프로퍼티의 이름 중 첫 글자를 대문자로 변환한 문자열 앞에 **set**을 붙이고 프로퍼티의 값을 읽어오는 메서드의 경우 프로퍼티의 이름 중 첫 글자를 대문자로 변환한 문자열 앞에 **get**을 붙입니다.
    - 프로퍼티의 이름과 필드의 이름은 같지 않아도 됩니다.
    - 예를 들어 name 프로퍼티의 값을 실제로 저장하는 필드가 _name인 경우
      	private String **_name**;
    
      	public String **getName()** {
      		return _name;
      	}
    
      	public void **setName**(String name) {
      		_name = name;
      	}
    - **데이터 빈(DataBean) 클래스** 작성
        - 여러개의 정보들을 데이터 빈이라는하나의 객체에 저장하게 되고 이 객체를 전달하면 각 정보를 하나씩 전달할 필요가 없으며 한꺼번에 모든 정보가 전달됩니다. 이런 클래스를 **DTO(Data Transfer Object)**, **VO(Value Object)**라고 합니다. DB에서 만들었던 컬럼명과 동일하게 프로퍼티들을 생성합니다.
- **dept - select이용해서 data 불러오기**
    - **Dept - 데이터빈**
        
        ```java
        public class Dept {
        	private int deptno;
        	private String dname;
        	private String loc;
        	
        	public Dept() {
        		
        	}
        	
        	public int getDeptno() {
        		return deptno;
        	}
        
        	public void setDeptno(int deptno) {
        		this.deptno = deptno;
        	}
        
        	public String getDname() {
        		return dname;
        	}
        
        	public void setDname(String dname) {
        		this.dname = dname;
        	}
        
        	public String getLoc() {
        		return loc;
        	}
        
        	public void setLoc(String loc) {
        		this.loc = loc;
        	}
        }
        ```
        
    - **dept_select - servlet**
        - 서버에서 DAO에 접근하여 데이터를 추출하여 ArrayList로 반환
        - /ex8_db/_2.list/list.jsp를 **dispatcher**로 전송 (주소 안바뀜)
        - 반환된 list를 **request**에 저장한다.
        
        ```java
        public class Dept_select extends HttpServlet {
        	private static final long serialVersionUID = 1L;
        	
        	protected void doGet(HttpServletRequest request,
        						 HttpServletResponse response) throws ServletException, IOException {
        		DAO dao = new DAO();
        		ArrayList<Dept> list = dao.selectAll();
        		RequestDispatcher dispatcher = request.getRequestDispatcher("/ex8_db/_2.list/list.jsp"); // view
        		request.setAttribute("list",list);
        		dispatcher.forward(request, response);
        	}
        }
        ```
        
    - **DAO**
        - select * from dept 쿼리문을 통해 데이터 추출
        
        ```java
        public class DAO {	
        	public ArrayList<Dept> selectAll()	{
        		Connection conn = null;
        		PreparedStatement pstmt = null;
        		ResultSet rs = null;
        		ArrayList<Dept> list = null;
        		
        		try {
        			//context.xml에 생성해 놓은(JNDI에 설정해 놓은) 리소스 jdbc/OracleDB를 참조하여 Connection 객체를 얻어옵니다.
        			Context init = new InitialContext();
        			DataSource ds = (DataSource) init.lookup("java:comp/env/jdbc/OracleDB");
        			conn = ds.getConnection();
        			
        			String select_sql = "select * from dept";
        			
        			//PreparedStatement 객체 얻기
        			pstmt = conn.prepareStatement(select_sql.toString());
        			rs = pstmt.executeQuery();
        			
        			int i = 0;
        			while (rs.next()) { //더이상 읽을 데이터가 없을때까지 반복
        				int deptno = rs.getInt("deptno");
        				String dname = rs.getString("dname");
        				String loc = rs.getString("loc");
        				
        				Dept dept = new Dept();
        				dept.setDeptno(deptno);
        				dept.setDname(dname);
        				dept.setLoc(loc);
        				if (i++ == 0) {
        					list = new ArrayList<Dept>();
        				}
        				list.add(dept);
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
        				if (conn != null)
        					conn.close();
        			} catch (SQLException e) {
        				System.out.println(e.getMessage());
        			}
        		}
        		return list;
        		}
        	}
        ```
        
    - **list - 화면단**
        - get과 set 메서드를 사용해야 하기 때문에 **<%@ page import = “ex8.Dept" %>**가 필요하다
        - request로 데이터를 전송했기 때문에 **getAttribute**를 사용하여 list를 구한다
            - <%
                 ArrayList<Dept> list = **(ArrayList<Dept>) request.getAttribute("list");**
                 if(list != null) {
            %>
        - **전체코드**
        
        ```java
        <%@ page language="java" contentType="text/html; charset=EUC-KR"
            pageEncoding="EUC-KR"%>
        <%@ page import = "java.util.*, ex8.Dept" %>
        <!DOCTYPE html>
        <html>
        <head>
        <link rel = "stylesheet"
        	  href="http://maxcdn.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
        </head>
        <body>
        	<div class= "container">
        	<%
        		ArrayList<Dept> list = (ArrayList<Dept>) request.getAttribute("list");  
        		if(list != null) {
        	%>
        		<table class="table">
        			<thead>
        				<tr>
        					<td>DEPTNO</td>
        					<td>DNAME</td>
        					<td>LOC</td>
        				</tr>
        			</thead>
        			<tbody>
        				<%
        					for (Dept dept : list) {
        				%>
        				<tr>
        					<td><%=dept.getDeptno() %></td>						
        					<td><%=dept.getDname() %></td>						
        					<td><%=dept.getLoc() %></td>	
        				</tr>			
        				<%
        					}
        				%>		
        			</tbody>		
        		</table>
        		<% 
        		} else {
        			out.print("<h4>조회된 데이터가 없습니다</h4>");
        		}
        		%>	
        	</div>
        </body>
        </html>
        ```
        
- **dept - select이용해서 data 불러와서, 검색하기**
    - **emp - 데이터빈**
        
        ```java
        package ex8_emp;
        
        import java.sql.*;
        
        public class Emp {
        	private int empno;
        	private String ename;
        	private String job;
        	private int mgr;
        	private Date hiredate;
        	private int sal;
        	private int comm;
        	private int deptno;
        	
        	public int getEmpno() {
        		return empno;
        	}
        	public void setEmpno(int empno) {
        		this.empno = empno;
        	}
        	public String getEname() {
        		return ename;
        	}
        	public void setEname(String ename) {
        		this.ename = ename;
        	}
        	public String getJob() {
        		return job;
        	}
        	public void setJob(String job) {
        		this.job = job;
        	}
        	public int getMgr() {
        		return mgr;
        	}
        	public void setMgr(int mgr) {
        		this.mgr = mgr;
        	}
        	public Date getHiredate() {
        		return hiredate;
        	}
        	public void setHiredate(Date hiredate) {
        		this.hiredate = hiredate;
        	}
        	public int getSal() {
        		return sal;
        	}
        	public void setSal(int sal) {
        		this.sal = sal;
        	}
        	public int getComm() {
        		return comm;
        	}
        	public void setComm(int comm) {
        		this.comm = comm;
        	}
        	public int getDeptno() {
        		return deptno;
        	}
        	public void setDeptno(int deptno) {
        		this.deptno = deptno;
        	}
        }
        ```
        
    - **emp_select - servlet**
        - 서버에서 DAO에 접근하여 데이터를 추출하여 ArrayList로 반환
        - /ex8_db/_2.list.emp/list.jsp를 **dispatcher**로 전송 (주소 안바뀜)
        - 반환된 list를 **request**에 저장한다.
        
        ```java
        @WebServlet("/emp_select")
        public class Emp_select extends HttpServlet {
        	private static final long serialVersionUID = 1L;
        	
        	protected void doGet(HttpServletRequest request,
        						 HttpServletResponse response) throws ServletException, IOException {
        		DAO dao = new DAO();
        		ArrayList<Emp> list = dao.selectAll();
        		RequestDispatcher dispatcher = request.getRequestDispatcher("/ex8_db/_2.list.emp/list.jsp"); // view
        		request.setAttribute("list",list);
        		dispatcher.forward(request, response);
        	}
        }
        ```
        
    - **DAO**
        - select * from emp 쿼리문을 통해 데이터 추출
        
        ```java
        **public class DAO {
        	
        	public ArrayList<Emp> selectAll()	{
        		Connection conn = null;
        		PreparedStatement pstmt = null;
        		ResultSet rs = null;
        		ArrayList<Emp> list = null;
        		
        		try {
        			//context.xml에 생성해 놓은(JNDI에 설정해 놓은) 리소스 jdbc/OracleDB를 참조하여 Connection 객체를 얻어옵니다.
        			Context init = new InitialContext();
        			DataSource ds = (DataSource) init.lookup("java:comp/env/jdbc/OracleDB");
        			conn = ds.getConnection();
        			
        			String select_sql = "select * from emp";
        			
        			//PreparedStatement 객체 얻기
        			pstmt = conn.prepareStatement(select_sql.toString());
        			rs = pstmt.executeQuery();
        			
        			int i = 0;
        			while (rs.next()) { //더이상 읽을 데이터가 없을때까지 반복
        				if (i++ == 0) {
        					list = new ArrayList<Emp>();
        				}
        				Emp st = new Emp();
        				st.setEmpno(rs.getInt(1));
        				st.setEname(rs.getString(2));
        				st.setJob(rs.getString(3));
        				st.setMgr(rs.getInt(4));
        				st.setHiredate(rs.getDate(5));
        				st.setSal(rs.getInt(6));
        				st.setComm(rs.getInt(7));
        				st.setDeptno(rs.getInt(8));
        				list.add(st);			
        	
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
        				if (conn != null)
        					conn.close();
        			} catch (SQLException e) {
        				System.out.println(e.getMessage());
        			}
        		}
        		return list;
        		}
        	}**
        ```
        
    - **List - 불러오기, 검색하기**
        - **불러오기**
            - get과 set 메서드를 사용해야 하기 때문에 **<%@ page import = “ex8_emp.emp" %>**가 필요하다
            - request로 데이터를 전송했기 때문에 **getAttribute**를 사용하여 list를 구한다
                - <%
                     ArrayList<Emp> list = **(ArrayList<Emp>) request.getAttribute("list");**
                     if(list != null) {
                %>
            
            ```java
            <div class= "container">
            	<p>검색어를 입력하세요</p>
            	<input class="form-control" id="myInput" type="text" placeholder="Search..">
            	<%
            		ArrayList<Emp> list = (ArrayList<Emp>) request.getAttribute("list");  
            		if(list != null) {
            	%>
            		<table class="table table-striped">
            			<thead>
            				<tr>
            					<td>사원번호</td>
            					<td>사원이름</td>
            					<td>직급</td>
            					<td>매니저</td>
            					<td>입사일자</td>
            					<td>급여</td>
            					<td>커미션</td>
            					<td>부서번호</td>
            				</tr>
            			</thead>
            			<tbody id="myTable">
            				<%
            					for (Emp emp : list) {
            				%>
            				<tr>
            					<td><%=emp.getEmpno() %></td>						
            					<td><%=emp.getEname() %></td>						
            					<td><%=emp.getJob() %></td>	
            					<td><%=emp.getMgr() %></td>	
            					<td><%=emp.getHiredate() %></td>	
            					<td><%=emp.getSal() %></td>	
            					<td><%=emp.getComm() %></td>	
            					<td><%=emp.getDeptno() %></td>	
            				</tr>			
            				<%
            					}
            				%>		
            			</tbody>		
            		</table>
            		<% 
            		} else {
            			out.print("<h4>조회된 데이터가 없습니다</h4>");
            		}
            		%>	
            	</div>
            ```
            
        - 검색하기
            - 검색창에 글씨를 작성하면 반응하는 메서드
                - **on("keyup", function() {})**
            -
