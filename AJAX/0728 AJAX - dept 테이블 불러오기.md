- **dept테이블 불러오기- ajax**
    - **$("button+div").remove()**
        - 버튼을 눌렀을때 이벤트에 의해 기존에 출력되었던 자료 아래에 **중복되어 출력**되는 것을 **막기 위해 필요한 코드**이다
        - 여기서 **button+div**는 **div id=”result”**를 말한다
    - **전체코드**
        
        ```java
        <body>
        <div class="container">
        	<button class="btn btn-info">JSON 데이터 불러오기</button>	
        </div>
        <script>
        		$("button").click(function(){
        			$.ajax({
        				type : "post",
        				url : '${pageContext.request.contextPath}/dept_lib',
        				dataType : 'json', 
        				success : function(rdata) {
        					console.log("성공" + rdata)
        					$("button+div").remove()						
        					if(rdata.length > 0) {								
        						var output = '<div id="result"><table class="table">'
        									+ '<thead><tr><th>부서번호</th><th>부서명</th><th>지역</th></tr></thead>'
        									+ '<tbody>';
        						
        					$(rdata).each(function(index,item) {
        						output += '<tr>';
        						output += '	  	<td>' + item.deptno + '</td>';
        						output += '	  	<td>' + item.dname + '</td>';
        						output += '	  	<td>'+ item.loc+ '</td>';			
        						output += '</tr>';
        					});
        						output += "</tbody></table></div>"
        						$(".container").append(output);	
        					} else {
        						$(".container").append("<div>데이터가 존재하지 않습니다.</div>");			
        					}					
        					
        				}, 
        				
        				error : function(request, status, error) {
        					$("body").append("<div id='error'>code :"
        										+ request.status + "<br>"
        										+ "받은 데이터 :" + request.responseText + "<br>"
        										+ "error status :" + status + "<br>"
        										+ "error 메시지 :" + error + "</div>");			
        				},
        				
        				complete : function(){	//요청의 실패, 성공과 상관없이 완료될 경우 호출
        					$(".container").append("<div id='com'>Ajax가 완료되었습니다.</div>")
        				}				
        			});
        		});
        		
        </script>
        </body>
        ```
        
- **DeptGet - Servlet**
    - **DAO** 객체에서 **JsonArray를 생성**하여 **out.print**로 html에 **출력**한다
    - **전체코드**
    
    ```java
    @WebServlet("/dept_lib")
    public class DeptGet_lib extends HttpServlet {
    	private static final long serialVersionUID = 1L;
    	
    	protected void doPost(HttpServletRequest request, HttpServletResponse response)   //링크로 왔으니깐 doGet?
    			throws ServletException, IOException {	
    		
    			DAO dao = new DAO();
    			JsonArray array = dao.getList_lib();			
    			response.setContentType("text/html;charset=utf-8");
    			PrintWriter out = response.getWriter();
    			out.print(array);
    			System.out.println(array);
    	}
    }
    ```
    
- **DAO**
    - **JsonObject 이용하기**
        - **JsonObject**을 생성해서 속성들을 추가한다
        - 그리고 이 JsonObject객체를 **array**에 넣는다
            - while (rs.next()) { 
            	**JsonObject** obj = new JsonObject();
            	obj.addProperty("deptno", rs.getInt(1));
            	obj.addProperty("dname", rs.getString(2));
            	obj.addProperty("loc", rs.getString(3));
            	**array.add(obj);**
            	}
        - **oracle 연결만 따로 빼내서 구할 수 있다**
            - private DataSource ds;
                
                try {
                     Context init = new InitialContext();
                     ds = (DataSource) init.lookup("java:comp/env/jdbc/OracleDB");
                } catch (Exception ex) {
                    System.out.println("DB 연결 실패 : " + ex);
                }
                }
                
        - **전체코드**
            
            ```java
            public JsonArray getList_lib()	{
            		Connection con = null;
            		PreparedStatement pstmt = null;
            		ResultSet rs = null;
            		JsonArray array = new JsonArray();
            		
            		try {
            
            			con = ds.getConnection();
            			
            			String select_sql = "select * from dept order by deptno";			
            			pstmt = con.prepareStatement(select_sql.toString());
            			rs = pstmt.executeQuery();
            			
            			while (rs.next()) { 
            				JsonObject obj = new JsonObject();
            				obj.addProperty("deptno", rs.getInt(1));
            				obj.addProperty("dname", rs.getString(2));
            				obj.addProperty("loc", rs.getString(3));
            				array.add(obj);
            
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
            		return array;
            		}
            ```
            
    - **GSON 이용하기**
        - **JsonElement j = new Gson().toJsonTree(dept);**
            - This method serializes the specified **object** into its equivalent representation as **a tree of  JsonElements**
            - 결국엔 자바의 **object**를 **JsonElement**로 바꿔준다는 의미이다
        - **해당 코드**
            
            ```java
            while (rs.next()) { 
            				Dept dept = new Dept();
            				dept.setDeptno(rs.getInt(1));
            				dept.setDname(rs.getString(2));
            				dept.setLoc(rs.getString(3));
            				
            				//JsonElement com.google.gson.Gson.toJsonTree(Object src);
            				JsonElement j = new Gson().toJsonTree(dept);
            				array.add(j);  //JsonElement형을 요구한다
            			}
            ```
