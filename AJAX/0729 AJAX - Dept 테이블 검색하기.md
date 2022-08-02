- **form - ajax**
    - **설계 방향**
        - 첫화면에 **dept 테이블**이 조회될 수 있도록 한다
        - 검색을 누르면 검색된 화면이 조회테이블을 대신해서 나온다
    - **getData()**
        - getData()는 화면을 조회해주는 메서드
        - 인자값을 **go, senddata**로 두어 url과 data에 사용한다
            - url : '${pageContext.request.contextPath}/' + **go**,  
            data : {"deptno":**senddata**}
    - **getData('dept_lib') 호출**
        - dept 테이블 조회화면을 담당한다
        - getData의 가변인자인 go, senddata 중 **go**만 인자로 사용된다
            - **getData('dept_lib');**
    - **getData('dept_lib_search', senddata) 호출**
        - **senddata**에 **dept의 속성값**을 저장하고, **getData**메서드의 인자값으로 설정한다
        - 그렇게 되면 getData에서 ajax에서 data : **{"deptno":senddata}**으로 설정이 되고 이값을 Servlet과 DAO에서 사용할 수 있다
        - **var senddata = $("input[name='deptno']").val();
        getData('dept_lib_search', senddata);**
    - **전체코드**
        
        ```java
        <body>
        	<div class="container">
        		<h3 class="mt-5">검색할 부서 번호를 입력하세요</</h3>
        		<form class="mb-3">
        			<div class="row">
        				<input type='search' name='deptno' required pattern="[0-9]{2}" class=form-control col-8 ml-3">
        				<button class="btn btn-info">검색</button>   <!-- type=submit 기본 -->
        			</div>
        		</form>
        	</div>
        <script>
        	function getData(go, senddata){  // 가변변수 dept_lib->go 로 들어감
        		console.log(senddata);
        		$.ajax({
        			type : "post",
        			url : '${pageContext.request.contextPath}/' + go,  //수정
        			data : {"deptno":senddata},		//여기서는 사용안됨
        			dataType : 'json', 
        			success : function(rdata) {						
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
        				$(".container").append("<div id='error'>code :"
        									+ request.status + "<br>"
        									+ "받은 데이터 :" + request.responseText + "<br>"
        									+ "error status :" + status + "<br>"
        									+ "error 메시지 :" + error + "</div>");			
        			}		
        		});
        	}
        	
        	getData('dept_lib');
        	
        	$("form").submit(function(event) {
        		event.preventDefault();
        		$("#result").remove();
        		var senddata = $("input[name='deptno']").val();
        		getData('dept_lib_search', senddata);
        	})	//form submit
        	
        </script>
        </body>
        ```
