# **AJAX 개념**

- AJAX란?
    - **Asynchronous JavaScript and XML**
    - JavaScript를 사용한 비동기 통신, 클라이언트와 서버간 데이터를 주고받는 기술(**비동기적** 정보 교환 기법)
    - 서버로부터 데이터를 가져와 전체 페이지를 새로 고치지 않고 **일부 데이터**만 바꾸어 웹페이지를 **비동기적으로 변경**하기 위한 것
- 장점
    - 페이지 이동 없이 고속으로 화면을 전환할 수 있다.
    - 클라이언트에게 처리를 위임할 수도 있다.
    - 수신하는 데이터의 양을 줄임
- 단점
    - 요청을 남발하면 역으로 서버 부하가 늘 수 있음.
    - Ajax를 지원하지 않는 브라우저가 있다.
- 기존 방식 VS AJAX 방식
    - 기존 방식
        - 웹 브라우저가 웹 서버에 요청 전송
        - 웹 서버는 JSP 등의 서버 어플리케이션을 사용해 사용자의
        요청 처리 후 결과를 HTML로 생성해서 웹 브라우저에 전
        송
        - 결과적으로 웹 브라우저가 웹 서버와 통신을 하고 요청 결
        과는 HTML로 생성되고 사용자 입장에서는 페이지 이동
        이 발생함
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7e232f04-e6fb-41d1-ae95-56be2f465fa2/Untitled.png)
        
    - AJAX 방식
        - 사용자가 이벤트를 발생 -> 자바스크립트는 DOM을 사용해서
        필요한 정보를 구한 뒤, XMLHttpRequest 객체를 통해서 웹 서
        버에 요청을 전달
        - 웹 서버는 XMLHttpRequest로부터의 요청을 알맞게 처리 후 결
        과를 XML이나 단순 Text을 생성해서 XMLHttpRequest에 전송
        - 결과적으로 사용자 입장에서는 페이지 이동이 발생되지 않고 페
        이지 내부 변화만 일어남
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b2cd6dec-ecae-4792-b518-74dbdad2bb1c/Untitled.png)
        
        # AJAX 실습
        
        - **LOAD() 메서드**
            - 서버로부터 **데이터를 받아서** 일치하는 요소 안에 **HTML을 추가**
            - 형식
                - **load( url , data , complete )**
                    - **url  →**  정보를 요청할 URL
                    - **data  →**  서버에 보낼 데이터
                    - **complete  →**  요청이 완료되면 실행될 콜백 함수
                - **function(responseTxt, status, xhr)**
                    - **responseTxt  →**  서버로부터 리턴된 텍스트 데이터를 가져옵니다.
                    - **status  →**  서버로부터의 리턴된 상태를 가져옵니다
                    (성공인 경우 "success", 실패한 경우 "error")
                    - **xhr  →**  XMLHttpRequest 객체
                        - **xhr**.**statusText  →**  상태 설명 문자열을 가져옵니다.
                        (주로 오류 내용을 가져오기 위해 사용)
                        - **xhr**.**status  →**  상태 코드를 가져옵니다
                            - **200**  →  서버가 응답을 완료했으며 아무런 문제가 없는 경우
                            - **404**  →  파일을 찾지 못하는 경우
                            - **500**  →  서버에서 클라이언트 요청을 처리 중에 에러 발생하는 경우 (보통 오타)
            - **txt파일로부터 데이터 가져오기**
                - **p태그, pre태그에 load하기**
                    - **$("p").load("sample1.txt");
                    $("pre").load("sample1.txt");**
                    - 전체코드
                        
                        ```java
                        <head>
                        <link href="../css/ex1.css" rel="stylesheet" type="text/css">
                        <script src = "http://code.jquery.com/jquery-latest.js"></script><script>
                        	$(function(){
                        		$("button").click(function() {  // 변경을 클릭하면
                        			$("button").text('로딩완료').css('color','red');
                        			
                        			$("p").load("sample1.txt");
                        			$("pre").load("sample1.txt");		
                        		})
                        	})
                        </script>
                        </head>
                        <body>
                        	<button>변경</button>
                        	<p>변경전 : 줄이 안바뀌어요</p>
                        	<br>
                        	<pre>변경전 : 줄이 바뀝니다. (입력한대로 출력됩니다)</pre>
                        </body>
                        ```
                        
                - **오류 처리 하기**
                    - **status**는 리턴된 상태, **xhr.status**는 오류코드, **xhr.statusText**는 상태를 나타내므로 이것을 활용하면 오류상태를 확인할 수 있다
                        
                        ```java
                        $(function(){
                        	 	function error(responseTxt, status, xhr) {
                        	 		if(status == "success") {
                        	 			alert("status == success\n" + responseTxt);
                        	 		} else if (status == "error") {  // 파일명을 sample2.txt로 변경 // 브라우저 콘솔로 에러 확인
                        	 			alert("xhr.status : " + xhr.status	//404
                        	 			+ "\n xhr.statusText : " + xhr.statusText);
                        	 		}
                        	 	}
                        ```
                        
                    - 특정 오류에 대해서 alert할 수 있다. **xhr.status는 특정 오류코드**를 나타내기 때문에 이걸 활용하면 된다
                        
                        ```java
                        $(function(){
                        	 	function error(responseTxt, status, xhr) {
                        	 		console.log(responseTxt);
                        	 		if(xhr.status == "404") {  // 파일명을 sample2.txt로 변경 // 브라우저 콘솔로 에러 확인
                        	 			alert("해당파일이 존재하지 않습니다.");
                        	 		}
                        	 	}
                        ```
                        
            - **HTML파일로부터 데이터 가져오기**
                - **html 파일(menu.html)**에서 특정 **태그(li)**를 가져오는 작업을 해보자
                - **$("#message2").load("menu.html li");**
                - **전체코드**
                    
                    ```java
                    <script>
                    	$(function(){
                    		$("#menu1").click(function() {  
                    			//load()메서드를 이용해서 munu.html 문서전체를 로드하여 id가 message1인 엘리먼트에 넣는다
                    			$("#message1").load("menu.html");
                    		});
                    		
                    		$("#menu2").click(function() {  
                    			//load()메서드를 이용해서 munu.html 내용중 li 엘리먼트만 읽어 id가 message2인 엘리먼트에 넣는다
                    			$("#message2").load("menu.html li");
                    		});
                    		
                    	})
                    </script>
                    </head>
                    <body>
                    	<div>
                    		<a href="#" id="menu1">메뉴 보기 1</a>  <!-- #없으니깐 계쏙 첫화면으로 가려고 하네 -->
                    		<p>
                    			<span id="message1"><</span>
                    		</p>	
                    	</div>
                    	<div>
                    		<a href="#" id="menu2">메뉴 보기 2</a>
                    		<p>
                    			<span id="message2"><</span>
                    		</p>	
                    	</div>
                    </body>
                    ```
                    
            - **form태그부터 데이터 가져오기**
                - **val() 메서드 이용**
                    - form태그의 Action이 없을경우 **같은 페이지를 반복해서 접속**한다. 이걸 방지하기 위해 **기본 이벤트를 제거**시킨다
                        - $("form").submit(function(e){  
                            **e.preventDefault();**
                    - **val() 메서드**를 이용해 저장된 value값을 구한다
                        - var data_name = "name=" + $("#name").**val();**
                    - **div태그**에 **process.jsp를 로드**시키는데, **data값**을 쿼리 스트링 형식으로 파라미터의 이름과 값의 형태로 전달한다
                        - data = name=love&age=21&address=서울시
                        - **$("div").load("process.jsp",data);**
                    - **전체코드**
                        
                        ```java
                        <head>
                        <script src = "http://code.jquery.com/jquery-latest.js"></script>
                        <link href="../css/form.css" rel="stylesheet" type="text/css">
                        <script>
                        	$(function(){
                        		$("form").submit(function(e){  // 전송 버튼 클릭시
                        			e.preventDefault(); // a태그나 action태그는 기본 이벤트 제거
                        			var data_name = "name=" + $("#name").val(); // 쿼리스트리밍 작성시 매개변수와 "="사이에 공백없이 작성합니다
                        			var data_age = "age=" + $("#age").val(); 
                        			var data_address = "address=" + $("#address").val();
                        			var data = data_name + "&" + data_age + "&" + data_address;
                        			//쿼리스트링 형식으로 파라미터의 이름과 값의 형태로 전달합니다.
                        			//$("div").load("process.jsp", "name=love&age=21&address=서울시");
                        			$("div").load("process.jsp",data);	
                        		})
                        	})
                        </script>
                        </head>
                        <body>
                        	<%-- form태그의 action 속성 없습니다. 이상태에서 전송을 클릭하면 현재 URL를 다시 불러온다 --%>
                        	<form>
                        		<span>이름</span><input type="text" placeholder="이름" id="name" name="name" required><br>
                        		<span>나이</span><input type="text" placeholder="나이" id="age" name="age" required><br>
                        		<span>주소</span><input type="text" placeholder="주소" id="address" name="address" required><br>
                        		<input type="submit" value="전송">	
                        	</form>
                        		<div></div>
                        </body>
                        ```
                        
                - **object 이용**
                    - 자바스크립트의 object를 이용해서 자료값을 담아서 전달한다
                        - **var data =** { name		: $("#name").val(),
                                          age		: $("#age").val(),
                                          address	        : $("#address").val()
                        };
                        - **전체코드**
                            
                            ```java
                            <head>
                            <script src = "http://code.jquery.com/jquery-latest.js"></script>
                            <link href="../css/form.css" rel="stylesheet" type="text/css">
                            <script>
                            	$(function(){
                            		$("form").submit(function(e){  // 전송 버튼 클릭시
                            			e.preventDefault(); //기본 이벤트 제거
                            			var data = { name		: $("#name").val(),
                            						       age		: $("#age").val(),
                            						     address	: $("#address").val()
                            						};	
                            			$("div").load("process.jsp",data);	
                            		});
                            	});
                            </script>
                            </head>
                            <body>
                            	<%-- form태그의 action 속성 없습니다. 이상태에서 전송을 클릭하면 현재 URL를 다시 불러온다 / name이 필요없음--%>
                            	<form>
                            		<span>이름</span><input type="text" placeholder="이름" id="name"  required><br>
                            		<span>나이</span><input type="text" placeholder="나이" id="age"  required><br>
                            		<span>주소</span><input type="text" placeholder="주소" id="address" required><br>
                            		<input type="submit" value="전송">	
                            	</form>
                            		<div></div>
                            </body>
                            ```
                            
                - **Serialize 이용**
                    - 서버로 보낼 데이터를 **폼에서 얻어와서** 입력 양식에 적힌 값을 **문자열로 바꾼다**
                        - **var data=$('form').serialize();**
                    - 전체코드
                        
                        ```java
                        <head>
                        <script src = "http://code.jquery.com/jquery-latest.js"></script>
                        <link href="../css/form.css" rel="stylesheet" type="text/css">
                        <script>
                        	$(function(){
                        		$("form").submit(function(e){  // 전송 버튼 클릭시
                        			e.preventDefault(); //기본 이벤트 제거
                        			//서버로 보낼 데이터를 폼에서 얻어온다
                        			//입력 양식에 적힌 값을 쿼리 문자열로 바꾼다
                        			var data=$('form').serialize();
                        			console.log(data);
                        			//$("div").load("process.jsp", "name=love&age=21&address=서울시");
                        			$("div").load("process.jsp",data);	
                        		});
                        	});
                        </script>
                        </head>
                        <body>
                        	<%-- form태그의 action 속성 없습니다. 이상태에서 전송을 클릭하면 현재 URL를 다시 불러온다 / name이 필요없음--%>
                        	<form>
                        		<span>이름</span><input type="text" placeholder="이름" id="name" name="name" required><br>
                        		<span>나이</span><input type="text" placeholder="나이" id="age" name="age" required><br>
                        		<span>주소</span><input type="text" placeholder="주소" id="address" name="address" required><br>
                        		<input type="submit" value="전송">	
                        	</form>
                        		<div></div>
                        </body>
                        ```
                        
                - **form 태그 아니어도 가능**
                    - 만약 form태그가 없다면 어떻게 해야 할까
                    - submit이 없어졌기 때문에, button으로 대체한다.
                    - button에는 기본이벤트가 없기때문에 **기본이벤트를 제거해줄 필요가 없다**
                    - **form에서만 serialiaze가 사용 가능**하기 때문에 다른 방법(object)을 이용한다
                    - **전체코드**
                        
                        ```java
                        <head>
                        <script src = "http://code.jquery.com/jquery-latest.js"></script>
                        <link href="../css/form.css" rel="stylesheet" type="text/css">
                        <script>
                        	$(function(){
                        		$("button").click(function(e){  // 전송 버튼 클릭시
                        			//e.preventDefault(); //기본 이벤트 제거 필요없음
                        			var data = { name		: $("#name").val(),
                        					 	 age		: $("#age").val(),
                        					 	 address	: $("#address").val()
                        						};	
                        			$("div").load("process.jsp",data);	
                        		});
                        	});
                        </script>
                        </head>
                        <body>
                          <span>이름</span><input type="text" placeholder="이름" id="name" name="name" required><br>
                          <span>나이</span><input type="text" placeholder="나이" id="age" name="age" required><br>
                          <span>주소</span><input type="text" placeholder="주소" id="address" name="address" required><br> 
                          <button type="button">전송</button>
                          <div></div>
                        </body>
                        ```
